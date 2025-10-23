# taxai
Desenvolvimento de uma inteligência artificial que entenda o conceito de espécie

### **Visão Geral do Projeto**
O projeto visa desenvolver um modelo de IA que compreenda o conceito de espécie biológica usando taxonomia integrativa combinada com inteligência artificial, permitindo extração automatizada de características e integração de dados de múltiplas fontes para identificação de unidades taxonômicas naturais.

### **🎯 Objetivos Principais**
1. Criar uma base vetorial que represente espécies como "tokens" semânticos
2. Extrair conhecimento de documentos taxonômicos (PDFs, revisões, descrições)
3. Compreender relações hierárquicas e ecológicas entre taxa
4. Implementar um sistema RAG (Retrieval Augmented Generation) especializado em biodiversidade

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


5. **Volume**: Quantos documentos/espécies pretende processar inicialmente?

Este planejamento fornece uma base sólida utilizando tecnologias open source e estado-da-arte em NLP para biodiversidade. O sistema será modular, escalável e poderá evoluir conforme suas necessidades específicas.
