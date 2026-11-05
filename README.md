BOE RAG Pipeline ⚖️🤖

Un pipeline modular de Retrieval-Augmented Generation (RAG) especializado en la recuperación semántica de información jurídica del Boletín Oficial del Estado (BOE).

BOE RAG Pipeline permite consultar información jurídica mediante búsqueda semántica, utilizando embeddings y una base de datos vectorial persistente para recuperar los fragmentos de texto más relevantes.

El proyecto está diseñado para ser modular, reproducible y portable, facilitando la sincronización de los datos, la inicialización del sistema de recuperación y su ejecución en entornos locales.

✨ Características principales

Sincronización desde Hugging Face: descarga y actualización de la base de datos vectorial y sus archivos asociados directamente desde el Hub.

Procesamiento multiplataforma: extracción de archivos comprimidos .rar mediante Python, utilizando patool, sin depender de herramientas externas como unrar.exe cuando el backend de extracción lo permite.

Almacenamiento vectorial persistente: integración con ChromaDB a través de langchain-chroma, permitiendo conservar y reutilizar las colecciones vectoriales.

Recuperación semántica: generación y comparación de embeddings para localizar fragmentos jurídicos relacionados con las consultas del usuario.

Filtrado por relevancia: configuración de similarity_score_threshold para descartar resultados que no superen un umbral mínimo de similitud y reducir el ruido en la recuperación.

Aceleración por GPU: compatibilidad con PyTorch y CUDA para aprovechar la capacidad de cálculo de las GPU NVIDIA durante la generación de embeddings.

🏗️ Arquitectura

El pipeline sigue un flujo de trabajo dividido en varias etapas:

Sincronización: obtención de los datos y archivos necesarios desde Hugging Face.

Extracción y preparación: descompresión y preparación de los datos para su procesamiento.

Inicialización vectorial: carga y conexión con la base de datos ChromaDB.

Recuperación semántica: transformación de consultas en embeddings y búsqueda de fragmentos relevantes.

Filtrado: aplicación de un umbral de similitud para mejorar la calidad de los resultados recuperados.

🛠️ Requisitos

Requisitos previos

Componente

Versión / Requisito

Python

3.12.x

PyTorch

Compatible con el entorno CUDA o CPU

CUDA

12.1 (opcional, para GPU)

GPU

NVIDIA compatible (opcional)

Nota: la ejecución en CPU es posible, aunque el rendimiento dependerá del volumen de datos y del modelo de embeddings utilizado.

🚀 Instalación

1. Clonar el repositorio

git clone https://github.com/mroscardd/boe-rag-pipeline.git

cd boe-rag-pipeline

2. Crear un entorno virtual

python -m venv .venv

Activar el entorno:

Windows (PowerShell):

.venv\Scripts\Activate.ps1

Linux / macOS:

source .venv/bin/activate

3. Instalar dependencias

pip install -r requirements.txt

4. Configurar PyTorch y CUDA (opcional)

Si dispones de una GPU NVIDIA, instala la versión de PyTorch compatible con tu entorno CUDA.

Por ejemplo, para CUDA 12.1:

pip install torch --index-url https://download.pytorch.org/whl/cu121

Puedes comprobar la disponibilidad de CUDA con:

import torch

print("PyTorch:", torch.__version__)
print("CUDA disponible:", torch.cuda.is_available())

📖 Uso

Una vez instalado el proyecto, podrás inicializar el pipeline para sincronizar los datos y preparar el sistema de recuperación.

Consulta la estructura del repositorio y los módulos correspondientes para conocer los puntos de entrada y la API del recuperador.

🧰 Tecnologías utilizadas

Python 3.12 — lenguaje principal.

PyTorch — procesamiento y aceleración por hardware.

Hugging Face Hub — distribución y sincronización de datos.

ChromaDB — almacenamiento y búsqueda vectorial.

LangChain — integración y abstracción del sistema de recuperación.

patool — gestión de archivos comprimidos.

🎯 Objetivo del proyecto

El objetivo es disponer de una infraestructura de recuperación semántica especializada en documentación jurídica española, que pueda utilizarse como componente de un sistema RAG más amplio.

El pipeline separa la gestión de los datos y la recuperación de información de la fase de generación de respuestas, permitiendo integrar diferentes modelos de lenguaje (LLM) sin acoplar el sistema a uno concreto.
