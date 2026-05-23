# Guía de Usuario — MedellínVive

## ¿Qué es este sistema?

MedellínVive permite explorar datos de criminalidad e incidentes viales de las 16 comunas de Medellín, y hacer preguntas en lenguaje natural sobre ellos usando Inteligencia Artificial.

Para ver el sistema en acción antes de instalarlo: [ Video Demo](https://youtu.be/_XIa7Wb7RyE)

---

## Requisitos previos

- Python 3.13+
- Cuenta gratuita en [groq.com](https://groq.com)
- Git instalado

---

## Instalación paso a paso

**1. Clonar el repositorio**

```bash
git clone https://github.com/kevinqzg/ProyectoFinal_IA.git
cd ProyectoFinal_IA
```

**2. Crear entorno virtual**

```bash
python3 -m venv venv
source venv/bin/activate
```

**3. Instalar dependencias**

```bash
pip install -r requirements.txt
```

**4. Configurar API key de Groq**

- Ir a [groq.com](https://groq.com) → crear cuenta gratuita → API Keys → Create API Key
- Crear archivo `.env` en la raíz del proyecto con este contenido:

```
GROQ_API_KEY=tu_api_key_aqui
```

**5. Descargar los datasets**

- Descargar desde [Google Drive](https://drive.google.com/drive/folders/140SgmuC5wX05UDEZ-vrpldmsWGR0jQE-)
- Ubicarlos en `data/raw/` con esta estructura:

```
data/raw/
├── consolidado_cantidad_casos_criminalidad_en_comunas_por_anio.csv
└── Mede_Victimas_inci.csv
```

---

## Cómo ejecutar el proyecto

```bash
jupyter notebook
```

Se abrirá el navegador con la lista de notebooks. Ejecutarlos en este orden:

---

## Notebook 1 — EDA (`01_eda.ipynb`)

Analiza y visualiza los datos de criminalidad e incidentes viales de Medellín.

**Qué hace:**
- Carga y cruza los datasets de MEData por comuna y año
- Genera 8 visualizaciones: heatmap de criminalidad, evolución por año, distribución de la variable objetivo, correlaciones entre features, top comunas más peligrosas
- Analiza valores nulos (resultado: 0 nulos) y outliers por método IQR
- Guarda todas las figuras en `reports/figures/`

**Cómo correrlo:** abrir el notebook y hacer click en `Kernel → Restart & Run All`

---

## Notebook 2 — Modelado (`02_modeling.ipynb`)

Entrena y evalúa los modelos de Machine Learning para predecir el nivel de bienestar por comuna.

**Qué hace:**
- Divide el dataset en train/validación/test con proporción 70/15/15
- Entrena 4 modelos en orden: Baseline → Árbol de decisión → Random Forest → XGBoost
- Genera tabla comparativa de métricas y gráfica de comparación
- Calcula importancia de features con SHAP

**Resultados obtenidos:**

| Modelo | Accuracy | F1-macro | AUC-ROC |
|---|---|---|---|
| Baseline | 0.353 | 0.174 | 0.500 |
| Árbol de decisión | 0.824 | 0.817 | 0.968 |
| Random Forest | 0.941 | 0.939 | 0.989 |
| XGBoost | 0.941 | 0.937 | 0.973 |

**Cómo correrlo:** abrir el notebook y hacer click en `Kernel → Restart & Run All`

---

## Notebook 3 — RAG (`03_rag.ipynb`)

Sistema de preguntas y respuestas en lenguaje natural sobre los datos de las comunas.

**Qué hace:**
- Indexa todos los datos del dataset maestro en ChromaDB
- Genera embeddings con el modelo BAAI/bge-m3 de HuggingFace
- Conecta con LLaMA 3.1 vía Groq API para generar respuestas
- Permite hacer preguntas en español sobre cualquier comuna y año

**Cómo correrlo:**
1. Verificar que el archivo `.env` existe con el `GROQ_API_KEY`
2. Abrir el notebook
3. Hacer click en `Kernel → Restart & Run All`
4. Ir a la última celda e ingresar tu pregunta

**Ejemplos de preguntas:**

```
¿Cuál fue la comuna con más delitos en 2020?
→ "La comuna con más delitos en 2020 fue la comuna 10 con 9364 delitos."

¿Cómo evolucionó la accidentalidad vial en la comuna 10 entre 2015 y 2021?
→ "En 2015 se registraron 63 muertos en incidentes viales.
   En 2021, 41 muertos. La accidentalidad disminuyó en ese periodo."

¿Qué comunas tuvieron nivel de bienestar BAJO en 2019?
→ "Las comunas con nivel BAJO en 2019 fueron:
   Comuna 4 con índice de bienestar de -0.4419..."

¿En qué año tuvo la comuna 13 su mejor índice de bienestar?
→ "El mejor índice de bienestar de la comuna 13 fue en 2020
   con un valor de 0.7918."
```

---

## Solución de problemas comunes

**Error: `GROQ_API_KEY not found`**
Verificar que el archivo `.env` existe en la raíz del proyecto y que la key está escrita correctamente sin espacios.

**Error al instalar dependencias**
Asegurarse de tener el entorno virtual activado antes de correr `pip install -r requirements.txt`.

**Los notebooks no encuentran los datos**
Verificar que los CSVs están en `data/raw/` con los nombres exactos indicados arriba.

**ChromaDB tarda mucho en la primera ejecución**
Es normal — está generando los embeddings de todos los registros. Las ejecuciones siguientes son más rápidas porque usa la base de datos en caché.

---

## Equipo

| Integrante | Correo | Rol |
|---|---|---|
| Kevin Quiroz González | kquirozg@eafit.edu.co | EDA, preprocesamiento, modelado |
| Carlos Alberto Mazo Gil | camazog1@eafit.edu.co | RAG, evaluación, informe |

---

*Proyecto académico — Inteligencia Artificial, Universidad EAFIT 2026-1*
