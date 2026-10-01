# BOE RAG Pipeline ⚖️🤖

Un sistema RAG (Retrieval-Augmented Generation) optimizado, modular y portable diseñado para realizar búsquedas semánticas y recuperar información jurídica sobre el **Boletín Oficial del Estado (BOE)** de España.

Este pipeline permite sincronizar automáticamente la base de datos vectorial desde Hugging Face, procesarla localmente sin dependencias pesadas de binarios del sistema y exponer un recuperador (*retriever*) de alta precisión basado en similitud semántica.

---

## 🎯 Características Principales

* **Actualización Automatizada:** Descarga las leyes actualizadas en la carpeta './leyes_pdf y ejecuta el script database.ipynb para actualizar
* **Base de Datos Vectorial Persistente:** Integración directa con **ChromaDB** mediante `langchain-chroma`.
* **Filtrado por Umbral de Similitud:** Configuración de `similarity_score_threshold` en el recuperador para eliminar el ruido semántico y reducir falsos positivos.
* **Soporte GPU / CUDA:** Aceleración por hardware mediante PyTorch y CUDA 12.1 para el cálculo rápido de embeddings.

---

## 🛠️ Requisitos e Instalación

### Requisitos Previos
* **Python:** 3.12.x
* **CUDA:** 12.1 (Opcional, recomendado para aceleración por GPU)

### Configuración del Entorno

1. **Clonar el repositorio:**

   git clone https://github.com/mroscardd/boe-rag-pipeline.git
