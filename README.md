# TaxAI - Sistema Inteligente para Biodiversity Informatics

> **Plataforma de IA para busca semântica, classificação e análise de dados taxonômicos baseada em RAG (Retrieval-Augmented Generation)**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![PostgreSQL](https://img.shields.io/badge/postgresql-16+-blue.svg)](https://www.postgresql.org/)
[![Meilisearch](https://img.shields.io/badge/meilisearch-1.8+-orange.svg)](https://www.meilisearch.com/)

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Contexto Científico](#-contexto-científico)
- [Objetivos](#-objetivos)
- [Arquitetura do Sistema](#-arquitetura-do-sistema)
- [Stack Tecnológico](#-stack-tecnológico)
- [Fontes de Dados](#-fontes-de-dados)
- [Etapas de Implementação](#-etapas-de-implementação)
- [Modelos e Repositórios Relevantes](#-modelos-e-repositórios-relevantes)
- [Referências Bibliográficas](#-referências-bibliográficas)
- [Contribuindo](#-contribuindo)
- [Licença](#-licença)

## 🌍 Visão Geral

O **TaxAI** é uma plataforma open-source de biodiversity informatics que combina técnicas avançadas de IA, incluindo Large Language Models (LLMs) e Retrieval-Augmented Generation (RAG), para resolver desafios complexos em taxonomia e análise de biodiversidade.

O projeto integra uma base robusta de **300.000 nomes de espécies** (fauna, flora e fungos) e **11 milhões de registros de ocorrência** seguindo o padrão Darwin Core, com capacidades de busca semântica, classificação automática de espécies e análise exploratória de dados.

### Desafios Abordados

A biodiversity informatics enfrenta desafios críticos que o TaxAI busca resolver:

- **Impedimento Taxonômico**: Escassez de taxonomistas vs. crescente volume de dados
- **Ambiguidade Nomenclatural**: Sinonímias, homonímias e mudanças conceituais
- **Conceitos de Espécie**: Múltiplos conceitos (biológico, morfológico, filogenético, evolutivo)
- **Integração de Dados**: Dados dispersos em diferentes formatos e repositórios
- **Escalabilidade**: Necessidade de processar milhões de registros eficientemente

## 🔬 Contexto Científico

### Conceitos de Espécie em Biodiversity Informatics

O projeto fundamenta-se em conceitos modernos de taxonomia:

- **Biological Species Concept (BSC)**: Comunidades reprodutivas isoladas
- **Morphological Species Concept (MSC)**: Agrupamentos por características morfológicas
- **Lineage Species Concept (LSC)**: Trajetórias evolutivas distintas em árvores filogenéticas
- **Integrative Unified Species Concept (iUSC)**: Fusão automática de múltiplas fontes de dados

### IA e Machine Learning em Taxonomia

Estudos recentes demonstram que:

- **Deep Learning** pode identificar espécies com >90% de acurácia em datasets balanceados
- **RAG systems** superam LLMs puros em classificação de taxa raros
- **Multimodal models** (visão + texto + DNA) melhoram significativamente a identificação
- **Semantic search** permite consultas complexas sobre conceitos taxonômicos

## 🎯 Objetivos

### Objetivos Principais

1. **Busca Semântica Avançada**
   - Consultas em linguagem natural sobre taxonomia
   - Busca por similaridade em descrições de espécies
   - Navegação hierárquica inteligente na taxonomia

2. **Classificação e Identificação Automática**
   - Identificação de espécies a partir de descrições morfológicas
   - Classificação baseada em dados de ocorrência e distribuição
   - Matching de nomes científicos com resolução de sinonímias

3. **Análise e Visualização de Dados**
   - Análise exploratória de padrões de biodiversidade
   - Visualização de distribuições geográficas
   - Análise de relações taxonômicas e filogenéticas

4. **Treinamento de Modelos Especializados**
   - Fine-tuning de LLMs em literatura taxonômica
   - Modelos multimodais para classificação
   - Transfer learning de modelos pré-treinados

## 🏗️ Arquitetura do Sistema

### Arquitetura Geral

```
┌─────────────────────────────────────────────────────────────┐
│                     Interface Layer                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Web UI  │  │ REST API │  │ GraphQL  │  │  Jupyter │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                    Application Layer                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   RAG Engine │  │  Taxonomy    │  │ Classification│     │
│  │   (LangChain)│  │  Resolver    │  │    Engine     │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                     AI/ML Layer                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │     LLM      │  │  Embedding   │  │   Fine-tuned │     │
│  │  (Llama 3.x) │  │    Models    │  │   Bio Models │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                      Data Layer                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │  PostgreSQL  │  │  Meilisearch │  │   MongoDB    │     │
│  │  + pgvector  │  │  (Search)    │  │  (Source)    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└─────────────────────────────────────────────────────────────┘
```

### Componentes Principais

#### 1. **Data Management Layer**

- **MongoDB**: Armazenamento dos dados originais (300k espécies + 11M ocorrências)
- **PostgreSQL + pgvector**: Banco de dados vetorial para embeddings
- **Meilisearch**: Motor de busca híbrida (full-text + semântica)

#### 2. **AI/ML Processing Layer**

- **LLM Engine**: Modelos de linguagem para geração e raciocínio
- **Embedding Generator**: Criação de embeddings para busca semântica
- **Fine-tuning Pipeline**: Treinamento de modelos especializados

#### 3. **RAG System**

- **Document Processor**: Chunking e processamento de documentos científicos
- **Retrieval System**: Busca contextual em múltiplas fontes
- **Generation System**: Síntese de informações com citações

#### 4. **Taxonomy Services**

- **Name Resolver**: Resolução de nomes científicos e sinonímias
- **Hierarchy Navigator**: Navegação na árvore taxonômica
- **Concept Matcher**: Matching entre diferentes conceitos de espécie

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

## 📊 Fontes de Dados

### Dados Internos (MongoDB)

- **300.000 nomes de espécies**:
  - Fauna (Animalia)
  - Flora (Plantae)
  - Fungos (Fungi)
  - Metadados taxonômicos completos
  
- **11 milhões de registros de ocorrência**:
  - Seguem padrão Darwin Core
  - Coordenadas geográficas
  - Dados temporais
  - Metadados de coleta

### Literatura Científica

- **Monografias de Espécies**: Descrições detalhadas
- **Revisões Taxonômicas**: Estudos de grupos taxonômicos
- **Artigos de Novas Espécies**: Descrições originais
- **Literatura Complementar**: Ecologia, biogeografia, etc.

### Integrações Externas (Futuro)

- **GBIF**: Global Biodiversity Information Facility
- **BOLD**: Barcode of Life Data System
- **EOL**: Encyclopedia of Life
- **WoRMS**: World Register of Marine Species
- **Catalogue of Life**: Lista global de espécies

## 🚀 Etapas de Implementação

### Fase 1: Infraestrutura e ETL

**Objetivo**: Estabelecer a base de dados e infraestrutura

1. **Setup de Ambiente**
   - Configuração de containers Docker no UNRAID
   - Deploy de PostgreSQL + pgvector
   - Deploy de Meilisearch
   - Setup de Ollama para modelos LLM

2. **ETL Pipeline**
   - Migração de dados do MongoDB para PostgreSQL
   - Normalização e limpeza de dados Darwin Core
   - Criação de índices e otimizações
   - Validação de integridade dos dados

3. **Processamento de Literatura**
   - Coleta e organização de documentos científicos
   - Extração de texto de PDFs
   - Chunking estratégico de documentos
   - Metadata extraction (autores, ano, DOI, taxa mencionados)

### Fase 2: Indexação e Busca

**Objetivo**: Implementar sistema de busca semântica

1. **Geração de Embeddings**
   - Seleção e avaliação de modelos de embedding
   - Geração de embeddings para descrições de espécies
   - Embeddings de chunks de literatura científica
   - Armazenamento em pgvector

2. **Configuração do Meilisearch**
   - Definição de índices (espécies, ocorrências, documentos)
   - Configuração de campos searchable e filterable
   - Setup de sinônimos taxonômicos
   - Configuração de embedders para busca híbrida

3. **Implementação de Busca**
   - API de busca full-text
   - API de busca semântica (embeddings)
   - Busca híbrida (combinação de ambas)
   - Ranking e relevância

### Fase 3: Sistema RAG

**Objetivo**: Implementar RAG para consultas inteligentes

1. **RAG Pipeline com LangChain**
   - Setup de chains básicas (retrieval + generation)
   - Implementação de prompt engineering
   - Configuração de contexto e memória
   - Sistema de citações e referências

2. **Retrieval Strategies**
   - Dense retrieval (embeddings)
   - Sparse retrieval (BM25, TF-IDF)
   - Hybrid search (fusion strategies)
   - Reranking com cross-encoders

3. **Advanced RAG Techniques**
   - Multi-query retrieval
   - Hypothetical document embeddings (HyDE)
   - Parent-child chunking
   - Recursive retrieval
   - Query decomposition

### Fase 4: Taxonomy Services

**Objetivo**: Resolver desafios nomenclaturais

1. **Name Resolution**
   - Matching fuzzy de nomes científicos
   - Resolução de sinonímias
   - Detecção de homonímias
   - Validação nomenclatural

2. **Taxonomy Hierarchy**
   - Navegação na árvore taxonômica
   - Queries ancestrais e descendentes
   - Visualização de hierarquias
   - Export em diferentes formatos

3. **Taxonomic Concepts**
   - Tracking de mudanças conceituais
   - Linking entre diferentes circumscrições
   - Histórico de revisões taxonômicas

### Fase 5: Classificação Automática

**Objetivo**: Modelos de classificação e identificação

1. **Text-based Classification**
   - Fine-tuning de BERT/RoBERTa em descrições
   - Classification de taxa a partir de texto
   - Confidence scoring
   - Explicabilidade das classificações

2. **Multimodal Classification (Futuro)**
   - Integração de modelos de visão computacional
   - Classification com DNA barcodes
   - Fusion de múltiplas modalidades
   - Transfer learning de modelos pré-treinados

3. **Active Learning Pipeline**
   - Sistema de feedback humano
   - Continuous learning
   - Detecção de casos difíceis
   - Priorização de anotações

### Fase 6: Fine-tuning de Modelos

**Objetivo**: Treinar modelos especializados

1. **Dataset Preparation**
   - Curation de dados de treinamento
   - Balanceamento de classes
   - Augmentation strategies
   - Train/val/test splits

2. **Model Fine-tuning**
   - Fine-tuning de Llama/Mistral em literatura taxonômica
   - LoRA/QLoRA para eficiência
   - Domain adaptation de embeddings
   - Evaluation e benchmarking

3. **Model Deployment**
   - Quantização de modelos (GGUF)
   - Optimization para inferência
   - Model serving com Ollama
   - A/B testing e monitoring

### Fase 7: APIs e Interfaces

**Objetivo**: Expor funcionalidades

1. **REST API (FastAPI)**
   - Endpoints de busca
   - Endpoints de classificação
   - Endpoints RAG (Q&A)
   - Documentação OpenAPI/Swagger

2. **GraphQL API (Opcional)**
   - Schema flexível para queries complexas
   - Resolvers otimizados
   - Subscriptions para updates

3. **Web Interface**
   - Interface de busca amigável
   - Visualização de resultados
   - Dashboard de analytics
   - Interface de annotation/feedback

4. **Jupyter Integration**
   - Notebooks de exemplo
   - Python client library
   - Data exploration tools

### Fase 8: Análise e Visualização

**Objetivo**: Tools para análise exploratória

1. **Geospatial Analysis**
   - Mapas de distribuição de espécies
   - Análise de hotspots de biodiversidade
   - Temporal analysis de ocorrências
   - Range maps

2. **Statistical Analysis**
   - Diversidade alfa/beta/gamma
   - Padrões de riqueza de espécies
   - Análises filogenéticas
   - Community composition

3. **Interactive Dashboards**
   - Dashboard de overview de dados
   - Exploratory data analysis tools
   - Custom query builder
   - Export capabilities

### Fase 9: Otimização e Escalabilidade

**Objetivo**: Performance e produção

1. **Performance Optimization**
   - Query optimization
   - Caching strategies
   - Index optimization
   - Batch processing

2. **Scalability**
   - Horizontal scaling de APIs
   - Database sharding strategies
   - Load balancing
   - CDN para assets

3. **Monitoring e Observability**
   - Métricas de performance
   - Error tracking
   - Usage analytics
   - Alerting

### Fase 10: Documentação e Community

**Objetivo**: Facilitar uso e colaboração

1. **Documentation**
   - User guides
   - API documentation
   - Tutorials e examples
   - Best practices

2. **Community Building**
   - Contributing guidelines
   - Code of conduct
   - Issue templates
   - Discussion forums

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

## 👥 Autores e Reconhecimentos

- **Projeto**: TaxAI - Sistema Inteligente para Biodiversity Informatics
- **Repositório**: https://github.com/biopinda/taxai
- **Contato**: [Adicionar informações de contato]

### Agradecimentos

- Comunidade de Biodiversity Informatics
- Desenvolvedores dos projetos open-source utilizados
- GBIF e outras iniciativas de dados abertos em biodiversidade
- Pesquisadores e taxonomistas que disponibilizam dados e literatura

---

**Status do Projeto**: 🚧 Em Planejamento

**Última Atualização**: Outubro 2025

Para mais informações, visite: https://github.com/biopinda/taxai
