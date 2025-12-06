# Codelabs-opcionales-proyecto-integrador-II

Resumen
-------
Colección de codelabs y demos prácticos sobre aprendizaje automático ligero, clasificación de texto, detección con YOLO, web scraping y un demo IoT con Node-RED y MCP. Contiene scripts Python, pipelines serializados, notebooks y un ejemplo con Docker Compose para integrar una IA con Node-RED.

Estructura principal
-------------------
- `codelab1/`
  - `model-training.py` — ejemplo mínimo con `ultralytics.YOLO` para evaluar un modelo.
  - `model-webcam.py` — inferencia en webcam con visualización.
  - `roboflow.py` — descargar dataset desde Roboflow (usa `ROBOFLOW_API_KEY`).
- `codelab2/`
  - `Traige_de_mensajes/triage.py` — pipeline TF-IDF + LinearSVC; guarda `pipeline_triage.joblib`.
- `codelab3/`
  - `Detector_de_estafa/estafa.py` — detector spam/estafa; guarda `pipeline_spam.joblib`.
- `codelab4/`
  - `web-scrapping/web-scraping-basico/dlib_hog.ipynb` — notebook de scraping y visión.
- `codelab5/`, `codelab6/`
  - Copias de ejercicios de e-commerce en formato `.ipynb`.
- `codelab7/`
  - `respuestas.md` — notas sobre ESP32, servidor web y `WiFi.h`.
- `codelab8/`
  - `mcp-nodered-mcp-nodered-demo/` — demo Docker Compose con Node-RED y un MCP server.
    - `docker-compose.yml`
    - `mcp-server/server.mjs`
    - `mcp-server/Dockerfile`
    - `nodered/data/flows.json`

Requisitos generales
--------------------
- Python 3.8+ (3.10+ recomendado)
- Node.js 20+ (para `codelab8` MCP server)
- Docker & Docker Compose (para `codelab8` demo)
- Herramientas adicionales según codelab:
  - `ultralytics`, `opencv-python` (codelab1)
  - `roboflow` (codelab1)
  - `scikit-learn`, `pandas`, `numpy`, `joblib`, `matplotlib` (codelab2/3)
