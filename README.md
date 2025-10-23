# TaxAI - Modelo de IA para Compreensão de Taxa Biológicas

> **Um modelo de inteligência artificial que compreende o conceito de espécie biológica (taxa), representando-as como "tokens" codificando suas características, relações hierárquicas e ecológicas**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![PostgreSQL](https://img.shields.io/badge/postgresql-16+-blue.svg)](https://www.postgresql.org/)
[![MongoDB](https://img.shields.io/badge/mongodb-5.0+-green.svg)](https://www.mongodb.com/)

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Objetivos](#-objetivos)
- [Arquitetura Proposta](#-arquitetura-proposta)
- [Componentes Principais](#-componentes-principais)
- [Stack Tecnológico](#-stack-tecnológico)
- [Modelos e Repositórios Relevantes](#-modelos-e-repositórios-relevantes)
- [Referências Bibliográficas](#-referências-bibliográficas)
- [Contribuindo](#-contribuindo)
- [Licença](#-licença)

## 🌍 Visão Geral

O **TaxAI** é um projeto de pesquisa que visa desenvolver um modelo de inteligência artificial capaz de compreender o conceito de espécie biológica (taxa). Este modelo representa cada taxa como um "token" que encapsula:

- **Características fenotípicas e genotípicas** da espécie
- **Relações hierárquicas biológicas** (filo, classe, ordem, família, gênero)
- **Relações ecológicas** e distribuição geográfica
- **Ambiguidade nomenclatural** (sinonímias, homonímias)

O projeto integra:
- **300.000 nomes de espécies** da fauna, flora e fungos (MongoDB)
- **11 milhões de registros de ocorrência** seguindo o padrão Darwin Core
- **Literatura científica** (monografias, revisões taxonômicas, descrições de novas espécies em PDF)
- **Modelos de embedding vetorial** para representação semântica de taxa

O objetivo é criar uma **base de dados vetorial** que permita ao modelo:
- Associar tokens (nomes científicos) a características e atributos
- Compreender relações filogenéticas e ecológicas
- Realizar busca semântica e matching de espécies
- Classificar novos organismos com base em descrições textuais ou dados de ocorrência

## 🎯 Objetivos

### Objetivos Principais

1. **Desenvolver Representação Vetorial de Taxa**
   - Criar embeddings semânticos que capturem características biológicas
   - Codificar relações hierárquicas e ecológicas em espaço vetorial
   - Permitir operações de similaridade vetorial entre taxa

2. **Treinar Modelo Especializado em Taxonomia**
   - Fine-tuning de modelos de linguagem em literatura taxonômica
   - Modelos de embedding otimizados para conceitos biológicos
   - Integração de múltiplas modalidades (texto, DNA, imagem)

3. **Classificação e Identificação de Organismos**
   - Identificar espécies a partir de descrições textuais
   - Matching automático com resolução de sinonímias
   - Classificação baseada em características fenotípicas

4. **Busca e Recuperação Semântica**
   - Busca semântica em base de 300k espécies
   - Recuperação de literatura relevante (PDFs)
   - Consultas em linguagem natural sobre conceitos taxonômicos
   - Análise de relações filogenéticas e ecológicas

## 🏗️ Arquitetura Proposta

### Arquitetura Geral

```
┌──────────────────────────────────────────────────────────┐
│                   Interface & Application                 │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────┐ │
│  │   Search UI    │  │   REST API     │  │  Notebooks │ │
│  └────────────────┘  └────────────────┘  └────────────┘ │
└──────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────────────────────────────────────┐
│                   AI/ML Processing                        │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────┐ │
│  │ Embedding      │  │ LLM / RAG      │  │ Fine-tuned │ │
│  │ Models         │  │ (LangChain)    │  │ Bio Models │ │
│  └────────────────┘  └────────────────┘  └────────────┘ │
└──────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────────────────────────────────────┐
│                    Vector Store Layer                     │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────┐ │
│  │ PostgreSQL +   │  │ Meilisearch    │  │   FAISS    │ │
│  │ pgvector       │  │ (Hybrid Search)│  │   (Index)  │ │
│  └────────────────┘  └────────────────┘  └────────────┘ │
└──────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────────────────────────────────────┐
│                    Data Sources                          │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────┐ │
│  │ MongoDB        │  │ PDF Documents  │  │ Darwin Core│ │
│  │ (300k species) │  │ (Literature)   │  │ Records    │ │
│  └────────────────┘  └────────────────┘  └────────────┘ │
└──────────────────────────────────────────────────────────┘
```

### Componentes Principais

#### 1. **Data Sources**

- **MongoDB**: 300.000 nomes de espécies com metadados taxonômicos
- **Darwin Core Records**: 11 milhões de registros de ocorrência geoespaciais
- **PDF Library**: Monografias, revisões taxonômicas, descrições de novas espécies

#### 2. **Vector Store & Search**

- **PostgreSQL + pgvector**: Armazenamento de embeddings de taxa e documentos
- **Meilisearch**: Busca híbrida (full-text + semantic search)
- **FAISS/Annoy**: Índices para busca rápida de similaridade vetorial

#### 3. **Embedding & Representation Layer**

- **Embedding Generator**: Cria vetores para cada espécie/nome científico
- **Document Embedder**: Processa literatura científica em chunked embeddings
- **Fine-tuned Models**: Modelos otimizados para conceitos biológicos

#### 4. **LLM & RAG System**

- **LLM Engine**: Modelos de linguagem para raciocínio taxonômico
- **RAG Pipeline**: Recuperação contextual de literatura + síntese
- **Prompt Engineering**: Prompts especializados para taxonomia

#### 5. **Application Services**

- **Name Resolution**: Matching de nomes científicos e resolução de sinonímias
- **Taxonomic Hierarchy**: Navegação e queries na árvore taxonômica
- **Species Classifier**: Classificação de organismos descritos

## 🛠️ Stack Tecnológico

### Backend Core

- **Python 3.10+**: Linguagem principal
- **FastAPI**: Framework para APIs REST
- **Pydantic**: Validação de dados e schemas

### Bancos de Dados

- **PostgreSQL 16+**: Banco de dados relacional
  - Extensão `pgvector`: Armazenamento e busca de vetores
  - Extensão `pg_trgm`: Busca por similaridade de texto
- **MongoDB**: Dados originais em Darwin Core
- **Meilisearch 1.8+**: Motor de busca híbrida
  - Suporte a busca semântica (embeddings)
  - Typo tolerance e instant search
  - Filtros e faceted search

### AI/ML Stack

#### Large Language Models

- **Ollama**: Runtime para modelos locais
  - Llama 3.1/3.2 (8B, 70B)
  - Mistral 7B/Mixtral 8x7B
  - Qwen 2.5 (especializado em raciocínio)

#### Embedding Models

- **sentence-transformers**: Modelos multilíngues
  - `all-MiniLM-L6-v2`: Rápido e eficiente
  - `paraphrase-multilingual-mpnet-base-v2`: Multilíngue
- **BioLinkBERT**: Especializado em textos biomédicos
- **Custom embeddings**: Treinados em literatura taxonômica

#### Computer Vision (Opcional - Fase 2)

- **PyTorch Wildlife**: Detecção e classificação de animais
- **BIOSCAN-5M models**: Classificação de insetos
- **YOLO v11**: Detecção de objetos/espécies em imagens

#### RAG Framework

- **LangChain**: Orquestração de LLMs e RAG
- **LlamaIndex**: Indexação e recuperação de documentos
- **Chroma/FAISS**: Stores vetoriais alternativos (desenvolvimento)

### Processamento de Documentos

- **Docling**: Extração de conteúdo de PDFs científicos
- **PyMuPDF**: Processamento de PDFs
- **BeautifulSoup**: Parsing de HTML
- **python-docx**: Processamento de documentos Word

### Data Processing

- **Pandas**: Manipulação de dados estruturados
- **NumPy**: Operações numéricas
- **GeoPandas**: Dados geoespaciais
- **Shapely**: Geometrias e análises espaciais

### Visualização

- **Plotly**: Gráficos interativos
- **Folium**: Mapas interativos
- **Matplotlib/Seaborn**: Visualizações estáticas
- **Dash**: Dashboards interativos

### Containerização

- **Docker**: Containerização de serviços
- **Docker Compose**: Orquestração local
- **UNRAID**: Servidor host para containers

### Monitoramento e Logging

- **Prometheus**: Métricas
- **Grafana**: Dashboards de monitoramento
- **Loguru**: Logging estruturado

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
