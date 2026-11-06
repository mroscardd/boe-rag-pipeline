# BOE RAG Pipeline ⚖️🤖

**Recuperación semántica de información jurídica del Boletín Oficial del Estado (BOE) 🇪🇸 mediante Retrieval-Augmented Generation (RAG).**

Pipeline modular que combina búsqueda semántica sobre **ChromaDB** con la generación de respuestas de **Llama 3.1 8B Instruct** afinado mediante **QLoRA** sobre un dataset de redacción jurídica. Diseñado para ser **reproducible y portable**, con sincronización de datos desde Hugging Face y recuperación desacoplada del LLM.

## ✨ Características principales

- 🔄 **Sincronización desde Hugging Face:** la base de datos vectorial y sus archivos se descargan y actualizan directamente desde el Hub, con extracción de `.rar` en Python vía `patool`.
- 🗃️ **Almacenamiento vectorial persistente:** colecciones ChromaDB reutilizables a través de `langchain-chroma`.
- 🔍 **Recuperación semántica filtrada:** embeddings de sentence-transformers y umbral de similitud (`similarity_score_threshold`) para descartar resultados irrelevantes.
- ⚡ **Aceleración por GPU:** PyTorch + CUDA para la generación de embeddings y la inferencia del LLM en GPU NVIDIA, con soporte también en CPU.

## 🏗️ Arquitectura

1. **Sincronización** → descarga de los datos desde Hugging Face y extracción local.
2. **Base vectorial** → conexión con la colección ChromaDB de fragmentos jurídicos.
3. **Recuperación** → la consulta se convierte en embedding y se buscan los fragmentos más relevantes.
4. **Filtrado** → se descartan resultados por debajo del umbral de similitud.
5. **Generación** → el LLM (base + adaptador QLoRA) redacta la respuesta a partir del contexto recuperado.

## 📓 Estructura del proyecto

| Notebook | Descripción |
|---|---|
| `database.ipynb` | Construcción de la base de datos vectorial ChromaDB a partir de los documentos. |
| `qlora.ipynb` | Afinado QLoRA (4-bit, NF4, bfloat16) de `Meta-Llama-3.1-8B-Instruct` con el dataset de redacción jurídica. |
| `rag.ipynb` | Pipeline RAG completo: sincronización, recuperación y generación con el adaptador QLoRA. |
| `testing.ipynb` | Evaluación de la calidad de la recuperación. |

Otros directorios clave: `chroma_db_boe_completo/` (base vectorial persistente), `adaptador_qlora/` (adaptador entrenado con PEFT/TRL), `dataset_qlora/` (dataset de instrucciones) y `leyes_pdf/` (documentación legal de referencia).

## 🛠️ Requisitos

| Componente | Versión / Requisito |
|---|---|
| Python | 3.12.x |
| PyTorch | 2.x, compatible con tu entorno CUDA o CPU |
| CUDA | 12.x (opcional, para GPU) |
| GPU | NVIDIA compatible (opcional) |

## 🚀 Instalación

```bash
# 1. Clonar el repositorio
git clone https://github.com/mroscardd/boe-rag-pipeline.git
cd boe-rag-pipeline

# 2. Crear y activar el entorno virtual
python -m venv .venv
.venv\Scripts\Activate.ps1        # Windows (PowerShell)
# source .venv/bin/activate       # Linux / macOS

# 3. Instalar dependencias
pip install -r requirements.txt
```

> 💡 `requirements.txt` ya instala PyTorch configurado para CUDA. Si tu entorno usa otra versión de CUDA, instala la build correspondiente, por ejemplo para CUDA 12.1:
>
> ```bash
> pip install torch --index-url https://download.pytorch.org/whl/cu121
> ```

Verifica la disponibilidad de CUDA:

```python
import torch

print("PyTorch:", torch.__version__)
print("CUDA disponible:", torch.cuda.is_available())
```

## 📖 Uso

Los notebooks son los puntos de entrada del pipeline:

1. **`database.ipynb`** — genera la base de datos vectorial desde los documentos del BOE.
2. **`qlora.ipynb`** — entrena el adaptador QLoRA sobre el dataset de redacción jurídica.
3. **`rag.ipynb`** — sincroniza la base vectorial desde Hugging Face, inicializa el recuperador y responde consultas jurídicas con el LLM afinado.
4. **`testing.ipynb`** — evalúa la calidad de la recuperación semántica.

## 🧰 Tecnologías

| Área | Herramientas |
|---|---|
| Lenguaje | Python 3.12 |
| RAG y embeddings | LangChain, ChromaDB, Sentence Transformers |
| LLM y afinado | PyTorch, Transformers, PEFT, TRL, bitsandbytes |
| Distribución de datos | Hugging Face Hub, patool |

## 🎯 Objetivo

Disponer de una infraestructura de recuperación semántica especializada en documentación jurídica española, reutilizable como componente de un sistema RAG más amplio. La gestión de datos y la recuperación están separadas de la generación de respuestas, de modo que el LLM puede sustituirse sin acoplar el sistema a un modelo concreto.
