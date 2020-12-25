---
title: "Multilingual Transformer Ensembles for Portuguese Natural Language Tasks"
categories:
  - Publications
---

![](https://raw.githubusercontent.com/ruanchaves/ruanchaves.github.io/master/assets/images/ensemble.png)

**Authors**: Ruan Chaves Rodrigues, Jéssica Rodrigues da Silva, Pedro Vitor Quinta de Castro, Nádia Félix Felipe da Silva, Anderson da Silva Soares
{: .notice--primary}

**Abstract:** Due to the technical gap between the language models available for low-resource languages and the state-of-the-art models available in English and Chinese, a simple approach that deploys automatic translation and ensembles predictions from Portuguese and English models is competitive with monolingual Portuguese approaches that may demand task-specific preprocessing and hand-crafted features. We performed our experiments on ASSIN 2 – the second edition of the Avaliação de Similaridade Semântica e Inferência Textual (Evaluating Semantic Similarity and Textual Entailment). On the semantic textual similarity task, we performed multilingual ensemble techniques to achieve results with higher Pearson correlation and lower mean squared error than BERT-multilingual, and on the textual entailment task, BERT-multilingual could be surpassed by automatically translating the corpus into English and then fine-tuning a large RoBERTa model over the translated texts.
{: .notice--info}

* [Read the Paper](http://ceur-ws.org/Vol-2583/3_DLB.pdf)

* [Github Repository](https://github.com/ruanchaves/assin)

* [ASSIN 2 Shared Task](https://sites.google.com/view/assin2/english)
* * [Proceedings](http://ceur-ws.org/Vol-2583/)
