# TaxAI - Desenvolvimento de um Modelo de Inteligência Artificial com Capacidade de Compreender o Conceito de Espécie Biológica

> **Um modelo de IA especializado em taxonomia que compreende e representa espécies biológicas (taxa) como tokens codificando características fenotípicas/genotípicas, relações hierárquicas e ecológicas**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/pytorch-2.0+-red.svg)](https://pytorch.org/)
[![Transformers](https://img.shields.io/badge/transformers-4.30+-blue.svg)](https://huggingface.co/transformers/)

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Objetivos](#-objetivos)
- [Arquitetura do Modelo](#-arquitetura-do-modelo)
- [Componentes Principais](#-componentes-principais)
- [Stack Tecnológico](#-stack-tecnológico)
- [Modelos e Repositórios Relevantes](#-modelos-e-repositórios-relevantes)
- [Referências Bibliográficas](#-referências-bibliográficas)
- [Contribuindo](#-contribuindo)
- [Licença](#-licença)

## 🌍 Visão Geral

O **TaxAI** é um projeto de pesquisa dedicado ao desenvolvimento de um modelo de inteligência artificial com capacidade de compreender o conceito de espécie biológica (taxa).

O modelo é treinado em uma base de dados robusta integrada que combina:
- **300.000 nomes de espécies** (fauna, flora e fungos) com metadados taxonômicos completos
- **11 milhões de registros de ocorrência** padronizados segundo Darwin Core
- **Literatura científica especializada** (monografias, revisões taxonômicas, descrições de novas espécies em PDF)

O objetivo é que o modelo aprenda a:
- **Representar taxa como tokens semânticos** que codificam características fenotípicas e genotípicas
- **Compreender relações hierárquicas** entre organismos (filo, classe, ordem, família, gênero)
- **Codificar relações ecológicas** e padrões de distribuição geográfica
- **Resolver ambiguidade nomenclatural** (sinonímias, homonímias, conceitos variáveis)
- **Generalizar para novos casos** de classificação e identificação taxonômica

## 🎯 Objetivos

1. **Aprender Representação Semântica de Taxa**
   - Codificar espécies como embeddings vetoriais que capturem características fenotípicas/genotípicas
   - Modelar relações hierárquicas entre taxa no espaço vetorial
   - Capturar padrões ecológicos e distribuição geográfica
   - Permitir operações de similaridade entre organismos

2. **Treinar Modelo Especializado em Taxonomia**
   - Fine-tuning de modelos transformer em literatura taxonômica científica
   - Otimização para compreensão de conceitos biológicos complexos
   - Múltiplas modalidades de entrada (texto, dados estruturados, metadados)

3. **Classificar e Identificar Organismos**
   - Identificar espécies a partir de descrições textuais e características
   - Matching automático de novos espécimes com taxa conhecidas
   - Resolução automática de sinonímias e nomes alternativos
   - Classificação em hierarquia taxonômica completa

4. **Generalizar para Novos Organismos**
   - Classificar espécies não vistas durante treinamento
   - Sugerir classificações com scores de confiança
   - Aplicar conhecimento para detecção de potenciais novas espécies

## 🏗️ Arquitetura do Modelo

```
┌──────────────────────────────────────────────────────────┐
│                   Input Processing                        │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────┐ │
│  │ Descrições     │  │ Metadados      │  │  Ocorrência│ │
│  │ Textuais       │  │ Taxonômicos    │  │  Espacial  │ │
│  └────────────────┘  └────────────────┘  └────────────┘ │
└──────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────────────────────────────────────┐
│              Tokenization & Embedding Layer              │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Transformer-based Encoder (BERT-like)             │ │
│  │ - Text encoder para descrições                     │ │
│  │ - Tabular encoder para metadados                   │ │
│  │ - Geospatial encoder para distribuição            │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────────────────────────────────────┐
│              Taxonomic Representation Layer              │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Hierarchical Embeddings                           │ │
│  │ - Embeddings de taxa (espécies como tokens)       │ │
│  │ - Embeddings hierárquicos (Reino → Espécie)      │ │
│  │ - Similarity learning entre taxa relacionadas    │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────────────────────────────────────┐
│              Classification & Output Layer               │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Species Classifier                                │ │
│  │ - Matching com taxa conhecidas                    │ │
│  │ - Scoring de confiança                            │ │
│  │ - Classificação hierárquica completa             │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────────────────────────────────────┐
│                    Training Data                         │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────┐ │
│  │ 300k Espécies  │  │ 11M Ocorrências│  │ Literatura │ │
│  │ (MongoDB)      │  │ (Darwin Core)  │  │ (PDFs)     │ │
│  └────────────────┘  └────────────────┘  └────────────┘ │
└──────────────────────────────────────────────────────────┘
```

### Componentes Principais

#### 1. **Input Processing**

- **Text Encoder**: Processa descrições textuais e literatura científica
- **Metadata Processor**: Extrai características dos dados estruturados (Darwin Core)
- **Geospatial Encoder**: Codifica padrões de distribuição e ocorrência
- **Multi-modal Fusion**: Integra múltiplas modalidades de entrada

#### 2. **Embedding & Representation Layer**

- **Transformer Encoder**: Modelo base (BERT-like) otimizado para taxonomia
- **Hierarchical Embedding**: Aprende hierarquias taxonômicas (Reino → Espécie)
- **Contrastive Learning**: Aprende similaridades entre taxa relacionadas
- **Species Token Embeddings**: Representa cada espécie como um vetor semântico

#### 3. **Classification Layer**

- **Species Matcher**: Matching de novos organismos com taxa conhecidas
- **Hierarchical Classifier**: Classifica em múltiplos níveis taxonômicos
- **Confidence Scorer**: Fornece scores de confiança nas predições
- **Synonym Resolver**: Resolve sinonímias e nomes alternativos

#### 4. **Training Framework**

- **Data Preparation**: Pipeline para limpeza e formatação dos dados
- **Batch Processing**: Processamento eficiente de 300k espécies + 11M ocorrências
- **Loss Functions**: Contrastive loss, triplet loss, taxon-aware losses
- **Evaluation Metrics**: Acurácia, ranking metrics, F1-score por nível hierárquico

## 🛠️ Stack Tecnológico

### Deep Learning & Model Development

- **Python 3.10+**: Linguagem principal
- **PyTorch 2.0+**: Framework de deep learning
  - DDP (Distributed Data Parallel) para treinamento em GPU
  - Mixed precision training (AMP)
  - TorchScript para serialização
- **Hugging Face Transformers**: Modelos pré-treinados base
  - BERT, RoBERTa, DeBERTa como backbones
  - DistilBERT para versões mais leves

### Data Management & Processing

- **Pandas**: Manipulação de dados estruturados (Darwin Core)
- **NumPy**: Operações numéricas em larga escala
- **GeoPandas**: Processamento de dados geoespaciais
- **Polars** (alternativa): Processamento rápido de big data
- **PyArrow**: Serialização eficiente de dados

### Input Data Processing

- **spaCy**: NLP para processamento de textos científicos
- **Docling**: Extração de conteúdo de PDFs
- **PyMuPDF**: Processamento alternativo de PDFs
- **NLTK**: Tokenização e análise linguística
- **scikit-learn**: Pré-processamento e transformação

### Model Training & Evaluation

- **PyTorch Lightning**: Abstração para treinamento organizado
- **Weights & Biases**: Rastreamento de experimentos
- **MLflow**: Versionamento de modelos e artefatos
- **Optuna**: Hyperparameter optimization
- **Scikit-learn**: Métricas de avaliação (F1, precision, recall, ranking metrics)

### Embedding & Vector Operations

- **FAISS**: Indexação rápida de vetores (Facebook AI)
- **Annoy**: Indexação aproximada de vizinhos
- **Sentence-Transformers**: Modelos de embedding pré-treinados
  - Para inicialização ou fine-tuning

### Data Sources & Databases

- **MongoDB 5.0+**: Armazenamento de 300k espécies com metadados
- **PostgreSQL 16+** (opcional): Backup relacional dos dados
  - Extensão `pgvector` (opcional): Armazenamento de embeddings

### Visualization & Analysis

- **Matplotlib/Seaborn**: Gráficos estáticos
- **Plotly**: Análise interativa de embeddings
- **UMAP/t-SNE**: Redução de dimensionalidade para visualização
- **Pandas Profiling**: Análise exploratória dos dados

### Development & Deployment

- **Jupyter**: Notebooks para experimentação e prototipagem
- **Git**: Controle de versão
- **DVC** (Data Version Control): Versionamento de datasets e modelos
- **Docker**: Containerização (opcional)
- **CUDA/cuDNN**: Aceleração GPU em NVIDIA

## 🤖 Modelos e Repositórios Relevantes

### Modelos Hugging Face

#### LLMs e Embeddings

- **[sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)**: Embedding rápido e eficiente
- **[sentence-transformers/paraphrase-multilingual-mpnet-base-v2](https://huggingface.co/sentence-transformers/paraphrase-multilingual-mpnet-base-v2)**: Embeddings multilíngues
- **[meta-llama/Llama-3.1-8B](https://huggingface.co/meta-llama/Llama-3.1-8B)**: LLM open-source base
- **[mistralai/Mistral-7B-v0.3](https://huggingface.co/mistralai/Mistral-7B-v0.3)**: Modelo eficiente para RAG

#### Modelos Especializados em Biologia

- **[InstaDeepAI/segment_nt_multi_species](https://huggingface.co/InstaDeepAI/segment_nt_multi_species)**: Segmentação de DNA
- **[microsoft/BioGPT](https://huggingface.co/microsoft/BioGPT)**: LLM treinado em literatura biomédica
- **[TimSchopf/nlp_taxonomy_classifier](https://huggingface.co/TimSchopf/nlp_taxonomy_classifier)**: Classificador de taxonomia NLP (exemplo de arquitetura)

#### Datasets Relevantes

- **[bioscan-ml/BIOSCAN-5M](https://huggingface.co/datasets/bioscan-ml/BIOSCAN-5M)**: 5M+ espécimes de insetos com imagens + DNA
- **[society-ethics/lila_camera_traps](https://huggingface.co/datasets/society-ethics/lila_camera_traps)**: Camera traps com taxonomia padronizada
- **[imageomics/rare-species](https://huggingface.co/datasets/imageomics/rare-species)**: Espécies raras com taxonomia Linneana completa

### Repositórios GitHub Relevantes

#### Biodiversity Informatics

- **[ropensci/taxize](https://github.com/ropensci/taxize)**: R package para interagir com bases taxonômicas
- **[8Ginette8/gbif.range](https://github.com/8Ginette8/gbif.range)**: Geração de mapas de distribuição com GBIF
- **[microsoft/CameraTraps](https://github.com/microsoft/CameraTraps)**: PyTorch Wildlife para classificação de fauna

#### RAG Systems

- **[langchain-ai/langchain](https://github.com/langchain-ai/langchain)**: Framework para aplicações LLM
- **[run-llama/llama_index](https://github.com/run-llama/llama_index)**: Data framework para RAG
- **[embedchain/embedchain](https://github.com/embedchain/embedchain)**: Framework RAG simplificado
- **[instructlab/instructlab](https://github.com/instructlab/instructlab)**: Sistema de RAG com taxonomia customizável

#### Search Engines

- **[meilisearch/meilisearch](https://github.com/meilisearch/meilisearch)**: Search engine com AI-powered hybrid search
- **[pgvector/pgvector](https://github.com/pgvector/pgvector)**: Extensão PostgreSQL para vetores

#### Machine Learning para Biodiversidade

- **[pytorch/pytorch](https://github.com/pytorch/pytorch)**: Framework de deep learning
- **[huggingface/transformers](https://github.com/huggingface/transformers)**: Modelos transformer
- **[explosion/spaCy](https://github.com/explosion/spacy)**: NLP pipeline para processamento de texto científico

## 📚 Referências Bibliográficas

### Biodiversity Informatics - Conceitos Fundamentais

1. **Guralnick, R., & Hill, A. (2009).** Biodiversity informatics: automated approaches for documenting global biodiversity patterns and processes. *Bioinformatics*, 25(4), 421-428. https://doi.org/10.1093/bioinformatics/btn659

2. **Hardisty, A. R., et al. (2013).** A decadal view of biodiversity informatics: challenges and priorities. *BMC Ecology*, 13(1), 16. https://doi.org/10.1186/1472-6785-13-16

3. **Freudenstein, J. V., et al. (2017).** Biodiversity and the Species Concept—Lineages are not Enough. *Systematic Biology*, 66(4), 644-656. https://doi.org/10.1093/sysbio/syw098

4. **Remsen, D. (2016).** The use and limits of scientific names in biological informatics. *ZooKeys*, 550, 207-223. https://doi.org/10.3897/zookeys.550.9447

### AI e Machine Learning em Taxonomia

5. **Karbstein, K., et al. (2024).** Species delimitation 4.0: integrative taxonomy meets artificial intelligence. *Trends in Ecology & Evolution*, 39(1), 39-50. https://doi.org/10.1016/j.tree.2023.11.002

6. **Wäldchen, J., & Mäder, P. (2018).** Machine learning for image based species identification. *Methods in Ecology and Evolution*, 9(11), 2216-2225. https://doi.org/10.1111/2041-210X.13075

7. **Nanni, L., et al. (2024).** AI-Powered Biodiversity Assessment: Species Classification via DNA Barcoding and Deep Learning. *AI*, 12(12), 240. https://doi.org/10.3390/ai12120240

8. **Garcez, J. D., et al. (2021).** Machine learning approach to support taxonomic species discrimination based on helminth collections data. *Parasites & Vectors*, 14, 267. https://doi.org/10.1186/s13071-021-04721-6

### RAG e LLMs para Biodiversidade

9. **Stevens, S., et al. (2025).** Taxonomic Reasoning for Rare Arthropods: Combining Dense Image Captioning and RAG for Interpretable Classification. *arXiv preprint* arXiv:2503.10886. https://arxiv.org/abs/2503.10886

10. **Leśniak, K., et al. (2024).** Large language models overcome the challenges of unstructured text data in ecology. *Ecological Informatics*, 82, 102784. https://doi.org/10.1016/j.ecoinf.2024.102784

### Computer Vision e Multimodal Learning

11. **Gharaee, Z., et al. (2024).** BIOSCAN-5M: A Multimodal Dataset for Insect Biodiversity. *Advances in Neural Information Processing Systems*, 37, 36285-36313.

12. **Kline, J., et al. (2025).** MMLA: Multi-Environment, Multi-Species, Low-Altitude Aerial Footage Dataset. *arXiv preprint* arXiv:2504.07744.

13. **Botella, C., et al. (2020).** An algorithm competition for automatic species identification from herbarium specimens. *Applications in Plant Sciences*, 8(6), e11365. https://doi.org/10.1002/aps3.11365

### Tecnologias e Ferramentas

14. **Ankile, L., et al. (2024).** Pytorch-Wildlife: A Collaborative Deep Learning Framework for Conservation. *arXiv preprint* arXiv:2405.12930.

15. **Schopf, T., et al. (2023).** Exploring the Landscape of Natural Language Processing Research. *Proceedings of RANLP 2023*, 1034-1045. https://aclanthology.org/2023.ranlp-1.111

16. **Lewis, P., et al. (2021).** Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. *NeurIPS 2020*. https://arxiv.org/abs/2005.11401

### Standards e Protocols

17. **Wieczorek, J., et al. (2012).** Darwin Core: An Evolving Community-Developed Biodiversity Data Standard. *PLoS ONE*, 7(1), e29715. https://doi.org/10.1371/journal.pone.0029715

18. **Page, R. D. M. (2008).** Biodiversity informatics: the challenge of linking data and the role of shared identifiers. *Briefings in Bioinformatics*, 9(5), 345-354.

## 🤝 Contribuindo

Este é um projeto open-source e contribuições são muito bem-vindas! Por favor, consulte nosso guia de contribuição (em desenvolvimento) para detalhes sobre:

- Code of Conduct
- Como reportar bugs
- Como sugerir melhorias
- Como submeter pull requests
- Guidelines de código

## 📄 Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 👥 Projeto

- **Nome**: TaxAI - Modelo de IA para Compreensão de Taxa Biológicas
- **Repositório**: https://github.com/biopinda/taxai
- **Licença**: MIT

---

**Status do Projeto**: 🚧 Em Planejamento

**Última Atualização**: Outubro 2025
