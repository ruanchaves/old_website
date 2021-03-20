---
title: "Integrating Ray Tune, Hugging Face Transformers and W&B"
categories:
  - Blog
---

![](https://pbs.twimg.com/media/El1M-SpVcAAgxIq.jpg)

**Author**: Ruan Chaves Rodrigues
{: .notice--primary}

There are a few articles, notebooks and code samples that teach how to integrate Ray Tune and Hugging Face Transformers, but they either leave out W&B or 
do not work anymore due to changes made to the library.

* [Hyperparameter Optimization for 🤗Transformers: A guide](https://huggingface.co/blog/ray-tune)

* [Hyperparameter Search with Transformers and Ray Tune](https://huggingface.co/blog/ray-tune)

After some hours of experimentation, I figured out the right way to integrate them. First of all, there is a bug in the Trainer that won't
allow you to use W&B and Ray Tune at the same time. [I have already submitted a PR on this](https://github.com/huggingface/transformers/pull/10823), 
but regardless of whether they accept it or not, you can currently fix this bug by creating a subclass that inherits from the Trainer:   

```python

from typing import Any 
from transformers import Trainer

class CustomTrainer(Trainer):

    def __init__(self, *args, **kwargs):
        super(CustomTrainer, self).__init__(*args, **kwargs)

    def _hp_search_setup(self, trial: Any):
        try:
            trial.pop('wandb', None)
        except AttributeError:
            pass
        super(CustomTrainer, self)._hp_search_setup(trial)
```

Before you actually instantiate a CustomTrainer object, you'll have to create two functions: `model_init` and `hp_space_fn`. 
`model_init` has to simply return your model, and `hp_space_fn` has to return [the config that will be used by Ray Trace and W&B](https://docs.wandb.ai/integrations/ray-tune).

A few points regarding the code below:

* You can get your wandb api key at `wandb.ai/authorize`. I like to set it as an environment variable and run my scripts as `API_KEY=... WANDB_PROJECT=my_project_name python run_glue.py ...`.

* The `model_init` function is meant to run on a modified version of [run_glue.py](https://huggingface.co/transformers/v2.1.1/examples.html#glue). ( The link to `run_glue.py` in the docs is broken. [Here](https://github.com/huggingface/transformers/blob/master/examples/text-classification/run_glue.py) is its current location ).

```python
from ray import tune

def model_init():
    model = AutoModelForSequenceClassification.from_pretrained(
        model_args.model_name_or_path,
        from_tf=bool(".ckpt" in model_args.model_name_or_path),
        config=config,
        cache_dir=model_args.cache_dir,
        revision=model_args.model_revision,
        use_auth_token=True if model_args.use_auth_token else None
    )
    return model

def hp_space_fn(empty_arg):
    config = {
                "warmup_steps": tune.choice([50, 100, 500, 1000]),
                "learning_rate": tune.choice([2e-5, 3e-5]),
                "num_train_epochs": tune.quniform(0.0, 5.0, 0.5),
    }
    wandb_config = {
            "wandb": {
                    "project": os.environ.get(
                        'WANDB_PROJECT',
                        'wandb_project'),
                    "api_key": os.environ.get('API_KEY'),
                    "log_config": True
                    }
    }
    config.update(wandb_config)
    return config

trainer = CustomTrainer(
        model_init=model_init,
        args=training_args,
        train_dataset=train_dataset,
        eval_dataset=eval_dataset,
        compute_metrics=compute_metrics,
        tokenizer=tokenizer,
        data_collator=data_collator)
 ```
 
 After that, you're ready to run the hyperparameter search Trainer method. Any additional parameters ( such as `time_budget_s` ) will be passed directly to [tune.run](https://docs.ray.io/en/master/tune/api_docs/execution.html), 
 [as it is stated in the docs](https://huggingface.co/transformers/main_classes/trainer.html).
 
The best hyperparameters will be returned as a dictionary that can be accessed at `best_run.hyperparameters`.

```python
from ray.tune.logger import DEFAULT_LOGGERS
from ray.tune.integration.wandb import WandbLogger
from ray.tune.schedulers import PopulationBasedTraining

best_run = trainer.hyperparameter_search(
            direction="maximize",
            backend="ray",
            scheduler=PopulationBasedTraining(
                        time_attr='time_total_s',
                        metric='eval_f1_thr_0',
                        mode='max',
                        perturbation_interval=600.0
                    ),
            hp_space=hp_space_fn,
            loggers=DEFAULT_LOGGERS + (WandbLogger, ),
            time_budget_s=6000
    )
    
logger.info(json.dumps(best_run.hyperparameters, indent=4))
```

There may be better ways to do this, but this approach simply works. All code was tested on the `transformers` version `4.4.0.dev0`.
