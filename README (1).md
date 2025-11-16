# Wikipedia-based RAG Summarizer

## 📚 Descripción

Sistema de **Recuperación y Generación Aumentada (RAG)** que utiliza contenido de Wikipedia para responder preguntas sobre temas específicos usando:

- **LangChain** - Framework para aplicaciones LLM
- **ChromaDB** - Base de datos vectorial para embeddings
- **SentenceTransformers** - Modelos de embeddings semánticos
- **Wikipedia API** - Extracción de contenido de Wikipedia

## 🎯 Funcionalidades

1. **Extracción de Datos**: Obtiene artículos completos de Wikipedia
2. **Procesamiento de Texto**: Divide el contenido en chunks semánticos
3. **Vectorización**: Genera embeddings usando SentenceTransformers
4. **Almacenamiento**: Guarda embeddings en ChromaDB
5. **Recuperación**: Busca información relevante basada en consultas
6. **Generación**: Responde preguntas usando contexto recuperado

## 🏗️ Arquitectura

```
Wikipedia API
     ↓
Extracción de Texto
     ↓
División en Chunks
     ↓
SentenceTransformer (Embeddings)
     ↓
ChromaDB (Vector Store)
     ↓
RAG Query System
     ↓
Respuestas Contextualizadas
```

## 📦 Instalación

```bash
pip install -r requirements.txt
```

## 🚀 Uso

### 1. Configuración Inicial

```python
import wikipediaapi
from sentence_transformers import SentenceTransformer
import chromadb

# Configurar Wikipedia API
wiki = wikipediaapi.Wikipedia(
    user_agent='RAG-Wikipedia-Project (educational@example.com)',
    language='en'
)

# Inicializar modelo de embeddings
embedding_model = SentenceTransformer('all-MiniLM-L6-v2')
```

### 2. Extraer Datos de Wikipedia

```python
# Obtener página
page = wiki.page("Federated_learning")

if page.exists():
    text = page.text
    print(f"✅ Página encontrada: {page.title}")
    print(f"📄 Longitud: {len(text)} caracteres")
```

### 3. Procesar y Almacenar

```python
# Dividir texto en chunks
chunks = split_text_into_chunks(text, chunk_size=500)

# Generar embeddings
embeddings = embedding_model.encode(chunks)

# Almacenar en ChromaDB
collection = client.create_collection("wikipedia_rag")
collection.add(
    documents=chunks,
    embeddings=embeddings,
    ids=[f"chunk_{i}" for i in range(len(chunks))]
)
```

### 4. Realizar Consultas

```python
# Hacer una pregunta
query = "What is federated learning?"

# Buscar chunks relevantes
results = collection.query(
    query_embeddings=[embedding_model.encode(query)],
    n_results=3
)

# Obtener respuesta
answer = generate_answer(query, results)
print(answer)
```

## 📂 Estructura del Proyecto

```
rag-wikipedia/
├── rag_wikipedia.ipynb     # Notebook principal
├── requirements.txt        # Dependencias
├── README.md              # Este archivo
├── data/                  # Datos extraídos
│   ├── wiki_page.txt
│   └── chunks.json
└── outputs/               # Resultados
    └── responses.json
```

## 🔧 Componentes Principales

### Wikipedia API
- Extrae contenido completo de artículos
- Soporta múltiples idiomas
- Acceso a metadatos y estructura

### SentenceTransformers
- Modelo: `all-MiniLM-L6-v2`
- Genera embeddings de 384 dimensiones
- Optimizado para similitud semántica

### ChromaDB
- Base de datos vectorial ligera
- Búsqueda por similitud eficiente
- Persistencia local

### LangChain
- Framework de orquestación
- Gestión de prompts
- Integración con LLMs

## 🎓 Casos de Uso

1. **Sistema de Q&A**: Responder preguntas sobre temas específicos
2. **Resumen Automático**: Generar resúmenes de artículos largos
3. **Búsqueda Semántica**: Encontrar información relacionada
4. **Chatbot Educativo**: Asistente virtual con conocimiento específico

## 📊 Ejemplo de Flujo

```python
# 1. Extraer datos
page = wiki.page("Machine_Learning")
text = page.text

# 2. Procesar
chunks = split_text_into_chunks(text)

# 3. Vectorizar
embeddings = model.encode(chunks)

# 4. Almacenar
collection.add(documents=chunks, embeddings=embeddings)

# 5. Consultar
query = "What are the main types of machine learning?"
results = collection.query(query_embeddings=[model.encode(query)])

# 6. Responder
context = results['documents'][0]
answer = generate_response(query, context)
```

## 🛠️ Configuración Avanzada

### Ajustar Tamaño de Chunks

```python
chunk_size = 500  # Caracteres por chunk
overlap = 50      # Solapamiento entre chunks
```

### Cambiar Modelo de Embeddings

```python
# Modelos alternativos
model = SentenceTransformer('paraphrase-multilingual-MiniLM-L12-v2')  # Multilenguaje
model = SentenceTransformer('all-mpnet-base-v2')  # Mayor precisión
```

### Configurar ChromaDB

```python
client = chromadb.Client(Settings(
    chroma_db_impl="duckdb+parquet",
    persist_directory="./chromadb"
))
```

## 📝 Notas Importantes

- **Rate Limiting**: Wikipedia API tiene límites de tasa
- **Tamaño de Chunks**: Ajustar según el tipo de contenido
- **Modelo de Embeddings**: Elegir según idioma y precisión necesaria
- **Persistencia**: ChromaDB guarda datos localmente

## 🔍 Temas de Ejemplo

Temas populares para probar el sistema:

- `Federated_learning` (Machine Learning)
- `Artificial_intelligence`
- `Neural_network`
- `Natural_language_processing`
- `Computer_vision`
- `Reinforcement_learning`

## ⚙️ Requisitos del Sistema

- **Python**: 3.8+
- **RAM**: Mínimo 4GB (recomendado 8GB)
- **Espacio**: ~2GB para modelos y datos
- **Internet**: Requerido para descargar contenido de Wikipedia

## 🚨 Solución de Problemas

### Error: "Page not found"
```python
# Verificar nombre exacto de la página
page = wiki.page("Federated_learning")  # Usar guiones bajos
```

### Error: Out of Memory
```python
# Reducir tamaño de chunks o procesamiento por lotes
chunk_size = 300
batch_size = 100
```

### ChromaDB no persiste
```python
# Asegurar directorio de persistencia
client = chromadb.Client(Settings(persist_directory="./db"))
```

## 📚 Referencias

- [Wikipedia API Docs](https://wikipedia-api.readthedocs.io/)
- [SentenceTransformers](https://www.sbert.net/)
- [ChromaDB](https://docs.trychroma.com/)
- [LangChain](https://python.langchain.com/)

## 📄 Licencia

MIT License - Proyecto educativo

## 🤝 Contribuciones

Este es un proyecto educativo. Siéntete libre de:
- Probar con diferentes temas de Wikipedia
- Experimentar con modelos de embeddings
- Mejorar el sistema de chunking
- Agregar más funcionalidades RAG

---

**Desarrollado con**: Wikipedia API, LangChain, ChromaDB, SentenceTransformers
