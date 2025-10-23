# taxai
Desenvolvimento de uma inteligência artificial que entenda o conceito de espécie
Vou ajudá-lo a desenvolver este fascinante projeto de IA para compreensão do conceito de espécie biológica. Primeiro, vou pesquisar sobre os fundamentos científicos e as soluções tecnológicas disponíveis.Excelente! Com base em minha pesquisa sobre o conceito de espécie, biodiversity informatics e soluções tecnológicas disponíveis, vou criar um planejamento detalhado para o seu projeto **TaxAI**.

## 📋 **Planejamento do Projeto TaxAI - Sistema de IA para Conceitos Taxonômicos**

### **Visão Geral do Projeto**
O projeto visa desenvolver um modelo de IA que compreenda o conceito de espécie biológica usando taxonomia integrativa combinada com inteligência artificial, permitindo extração automatizada de características e integração de dados de múltiplas fontes para identificação de unidades taxonômicas naturais.

### **🎯 Objetivos Principais**
1. Criar uma base vetorial que represente espécies como "tokens" semânticos
2. Extrair conhecimento de documentos taxonômicos (PDFs, revisões, descrições)
3. Compreender relações hierárquicas e ecológicas entre taxa
4. Implementar um sistema RAG (Retrieval Augmented Generation) especializado em biodiversidade

---

## **📊 Fase 1: Preparação e Análise**

### **1.1 Definição do Modelo Conceitual**
Dado que existem múltiplos conceitos de espécie (biológico, morfológico, filogenético), o sistema deve ser pluralístico e integrar diferentes linhas de evidência.

**Perguntas para esclarecer:**
- Qual grupo taxonômico será o foco inicial (plantas, animais, microrganismos)?
- Você tem acesso a bases de dados específicas além dos PDFs (GBIF, CoL, ITIS)?
- Qual o volume estimado de documentos para processamento inicial?

### **1.2 Arquitetura Técnica Proposta**
```
┌─────────────────────────────────────────┐
│         Entrada de Dados                │
│  (PDFs, Monografias, Revisões)         │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│     Processamento e Extração            │
│  (OCR, NER Taxonômico, Chunking)       │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│      Embeddings e Vetorização           │
│   (BioBERT/SciBERT + Ontologias)       │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│        Base de Dados Vetorial           │
│     (ChromaDB/Qdrant/Weaviate)         │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│         Interface RAG + LLM             │
│    (Query, Retrieval, Generation)       │
└─────────────────────────────────────────┘
```

---

## **🛠️ Fase 2: Configuração do Ambiente**

### **2.1 Stack Tecnológica Recomendada**

#### **Modelos de Linguagem e Embeddings:**
- **BioBERT** - Modelo BERT pré-treinado em corpora biomédicos que obteve resultados estado-da-arte em tarefas de NER biológico
- **SciBERT** - Para textos científicos gerais
- **Stella Embeddings** - Para vetorização eficiente

#### **Processamento de Documentos:**
- **Docling** (IBM) - Extração de texto de PDFs científicos
- **spaCy com TaxoNERD** - Sistema de NER específico para reconhecimento de entidades taxonômicas na literatura ecológica
- **LangChain** - Orquestração de pipelines

#### **Base de Dados Vetorial:**
- **ChromaDB** - Simples e eficiente para começar
- **Qdrant** - Mais robusto para produção
- **Neo4j** (Graph DB) - Para relações taxonômicas hierárquicas

### **2.2 Configuração Docker no UNRAID**

```yaml
# docker-compose.yml
version: '3.8'
services:
  chromadb:
    image: chromadb/chroma:latest
    ports:
      - "8000:8000"
    volumes:
      - ./chroma-data:/chroma/chroma
    environment:
      - IS_PERSISTENT=TRUE
      
  qdrant:
    image: qdrant/qdrant
    ports:
      - "6333:6333"
    volumes:
      - ./qdrant-data:/qdrant/storage
      
  neo4j:
    image: neo4j:latest
    ports:
      - "7474:7474"
      - "7687:7687"
    environment:
      - NEO4J_AUTH=neo4j/taxai2024
    volumes:
      - ./neo4j-data:/data
```

---

## **💻 Fase 3: Desenvolvimento do Pipeline**

### **3.1 Extração e Processamento de Texto**

```python
# Estrutura básica do processador
class TaxonomicDocumentProcessor:
    def __init__(self):
        self.nlp = self._load_taxonerd_model()
        self.pdf_extractor = DoclingPDFExtractor()
        
    def process_document(self, pdf_path):
        # 1. Extrair texto
        text = self.pdf_extractor.extract(pdf_path)
        
        # 2. Identificar entidades taxonômicas
        entities = self.nlp(text)
        
        # 3. Chunking inteligente
        chunks = self.smart_chunking(text, entities)
        
        return chunks, entities
```

### **3.2 Sistema de Embeddings Taxonômicos**

Implementar ontologias taxonômicas como SKOS Concepts, permitindo que conceitos taxonômicos sejam conectados hierarquicamente e semanticamente.

```python
class TaxonomicEmbedder:
    def __init__(self):
        self.biobert = AutoModel.from_pretrained("dmis-lab/biobert-v1.1")
        self.tokenizer = AutoTokenizer.from_pretrained("dmis-lab/biobert-v1.1")
        
    def create_taxon_embedding(self, taxon_data):
        # Combinar nome científico + descrição + características
        combined_text = self._prepare_taxon_text(taxon_data)
        
        # Gerar embedding
        embedding = self._encode(combined_text)
        
        # Adicionar metadados taxonômicos
        return {
            'vector': embedding,
            'metadata': {
                'scientific_name': taxon_data['name'],
                'rank': taxon_data['rank'],
                'parent_taxon': taxon_data['parent'],
                'synonyms': taxon_data['synonyms'],
                'ecological_traits': taxon_data['traits']
            }
        }
```

---

## **🔍 Fase 4: Implementação do RAG**

### **4.1 Sistema de Recuperação**

```python
class TaxonomicRAG:
    def __init__(self, vector_db, llm_model):
        self.vector_db = vector_db
        self.llm = llm_model
        self.reranker = CrossEncoder('cross-encoder/ms-marco-MiniLM-L-6-v2')
        
    def query(self, question, filters=None):
        # 1. Busca vetorial
        results = self.vector_db.similarity_search(
            query=question,
            k=20,
            filter=filters  # Ex: {'rank': 'species', 'kingdom': 'Plantae'}
        )
        
        # 2. Re-ranking
        reranked = self.reranker.predict(
            [(question, r.page_content) for r in results]
        )
        
        # 3. Construir contexto
        context = self._build_taxonomic_context(reranked[:5])
        
        # 4. Gerar resposta
        response = self.llm.generate(
            prompt=self._create_prompt(question, context)
        )
        
        return response
```

### **4.2 Ontologia e Grafo de Conhecimento**

Implementar taxonomias hierárquicas usando ontologias OWL/RDF, permitindo raciocínio automatizado sobre relações taxonômicas.

```python
# Estrutura do grafo taxonômico
class TaxonomicKnowledgeGraph:
    def __init__(self, neo4j_uri):
        self.driver = GraphDatabase.driver(neo4j_uri)
        
    def add_taxon(self, taxon_data):
        query = """
        MERGE (t:Taxon {name: $name})
        SET t.rank = $rank,
            t.authority = $authority,
            t.description = $description
        WITH t
        MATCH (p:Taxon {name: $parent_name})
        MERGE (t)-[:IS_CHILD_OF]->(p)
        """
        # Executar query...
        
    def get_taxonomic_path(self, taxon_name):
        # Retornar hierarquia completa
        pass
```

---

## **📚 Fase 5: Recursos Open Source Recomendados**

### **Repositórios GitHub Relevantes:**

1. **[TaxoNERD](https://github.com/nleguillarme/taxonerd)** - NER para entidades taxonômicas
2. **[BioBERT](https://github.com/dmis-lab/biobert)** - Modelo pré-treinado biomédico
3. **[OpenBiodiv-O](https://github.com/pensoft/OpenBiodiv-O)** - Ontologia para biodiversidade
4. **[txtai](https://github.com/neuml/txtai)** - Framework para RAG e busca semântica
5. **[ChromaDB](https://github.com/chroma-core/chroma)** - Base vetorial

### **Modelos HuggingFace:**
- `dmis-lab/biobert-v1.1` - BioBERT base
- `sentence-transformers/all-MiniLM-L6-v2` - Embeddings gerais
- `BAAI/bge-large-en` - Embeddings de alta qualidade

### **Bases de Dados e APIs:**
- **GBIF API** - Dados de ocorrência
- **CoL API** - Catalogue of Life
- **ITIS API** - Sistema Integrado de Informação Taxonômica com identificadores únicos para espécies

---

## **📊 Fase 6: Validação e Métricas**

### **6.1 Métricas de Avaliação**

```python
class TaxonomicEvaluator:
    def evaluate_ner(self, predictions, ground_truth):
        # Precision, Recall, F1 para entidades taxonômicas
        pass
        
    def evaluate_hierarchy(self, predicted_relations, true_relations):
        # Acurácia das relações hierárquicas
        pass
        
    def evaluate_retrieval(self, queries, relevant_docs):
        # MRR, NDCG para recuperação
        pass
```

### **6.2 Dataset de Teste**
- Criar conjunto de validação com ~100 espécies bem documentadas
- Incluir diferentes níveis taxonômicos
- Validar com especialistas em taxonomia

---

## **🚀 Fase 7: Deployment e Documentação (Semanas 11-12)**

### **7.1 API REST**

```python
# FastAPI endpoint
@app.post("/query")
async def query_taxon(request: TaxonQuery):
    result = rag_system.query(
        question=request.question,
        filters=request.filters
    )
    return {
        "answer": result.answer,
        "sources": result.sources,
        "confidence": result.confidence
    }
```

### **7.2 Documentação no GitHub**

Estrutura sugerida para `github.com/biopinda/taxai`:

```
taxai/
├── README.md
├── docs/
│   ├── installation.md
│   ├── usage.md
│   └── api.md
├── src/
│   ├── extractors/
│   ├── embedders/
│   ├── rag/
│   └── ontology/
├── docker/
│   └── docker-compose.yml
├── notebooks/
│   └── examples.ipynb
└── tests/
```

---
