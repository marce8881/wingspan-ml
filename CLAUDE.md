# wingspan-ml — Contexto del proyecto para Claude Code

## Descripción del proyecto

Proyecto final del Diplomado en Transformación Digital Empresarial.
Análisis estratégico y modelado predictivo aplicado a las cartas del juego de mesa Wingspan.

**Pregunta central:** ¿Qué atributos de las cartas predicen el tipo de motor de juego que activan?
**Enfoque:** Clasificación supervisada con Scikit-learn Pipelines y explicabilidad SHAP.

---

## Reglas de colaboración

### Commits
- **Claude NO ejecuta commits.** Marce ejecuta todos los commits.
- Antes de sugerir un commit, explicar qué cambió y por qué merece un commit propio.
- Sugerir siempre un mensaje de commit descriptivo junto con la explicación.

### Lenguaje y estilo
- Títulos y subtítulos siguen las reglas de capitalización del español: solo se escribe con mayúscula la primera palabra y los nombres propios.
- Correcto: "Análisis exploratorio de datos"
- Incorrecto: "Análisis Exploratorio de Datos"

---

## Estructura del repositorio

```
wingspan-ml/
├── data/
│   └── raw/
│       ├── wingspan-20260128.xlsx       ← dataset principal (~907 cartas, 65 columnas)
│       └── wingspan_head15.csv          ← muestra de 15 filas para exploración rápida
├── docs/
│   ├── descripcion_dataset_wingspan.md  ← descripción técnica del dataset
│   ├── Guía Maestra_ Proyecto Final de Machine Learning.md
│   └── Propuesta de Proyecto Wingspan.md
├── notebooks/
│   ├── fraude_detection_pipeline.ipynb  ← esqueleto de referencia del curso
│   └── wingspan_analysis.ipynb          ← notebook principal del análisis
├── CLAUDE.md
├── README.md
└── requirements.txt
```

---

## Dataset

- **Fuente:** BoardGameGeek (BGG)
- **Hoja principal:** `Birds` — cartas de aves (juego base + expansiones)
- **Advertencia:** Las filas 1 y 2 del Excel son filas de estadísticas internas. Siempre cargar con `skiprows=[1, 2]`.
- **Variable objetivo:** `engine_type` — derivada del campo `Power text` mediante clasificación por palabras clave.

| Tipo de motor | Palabras clave |
|---|---|
| `motor_huevos` | lay, egg |
| `motor_cartas` | draw, tuck, card |
| `motor_comida` | cache, food, gain |
| `motor_puntos` | point, victory |
| `otro` | sin coincidencia |

---

## Esqueleto de referencia: `fraude_detection_pipeline.ipynb`

El docente (Rosmer) recomienda usar este script como base del proyecto. Contiene el flujo profesional completo que se exige en la rúbrica. Al adaptar el notebook de Wingspan, seguir la misma estructura:

| Sección del pipeline de fraude | Equivalente en Wingspan |
|---|---|
| Imports y configuración | Imports y configuración |
| Carga de datos | Carga con `skiprows=[1, 2]`, filtrar filas sin nombre |
| Análisis exploratorio rápido | Distribución de `engine_type`, VP, hábitats |
| Preparación de datos | Codificación de binarias (X→1), `train_test_split` estratificado |
| Definición de modelos | Regresión Logística Multinomial vs. Random Forest |
| Búsqueda de hiperparámetros con CV | `GridSearchCV` con `StratifiedKFold` |
| Evaluación en test | Métricas: F1-score, Precision, Recall por clase |
| Comparación visual de métricas | Gráficas comparativas de los dos modelos |
| Importancia de features | Sustituir por SHAP values |
| Resumen ejecutivo | Conclusión estratégica para el jugador |

**Diferencias clave respecto al pipeline de fraude:**
- El problema de Wingspan es **multiclase** (5 tipos de motor), no binario.
- No aplica balanceo con SMOTE (las clases no están tan desbalanceadas).
- En lugar de importancia de features del modelo, usar **SHAP** directamente.
- La métrica principal es **F1-score macro** (no average_precision).

---

## Requisitos académicos (rúbrica)

El notebook debe cubrir estas cinco secciones en orden:

| Sección | Peso | Contenido mínimo |
|---|---|---|
| 1. Problema y datos | 15% | Pregunta, justificación, carga reproducible, EDA, limpieza |
| 2. Modelado y análisis | 35% | Pipelines Scikit-learn, 2 modelos comparados, métricas F1/Precision/Recall |
| 3. Interpretación | 25% | SHAP values, mínimo 3 visualizaciones con título e interpretación textual |
| 4. Conclusiones | 15% | Respuesta a la pregunta, limitaciones, recomendación concreta al jugador |
| 5. Uso de IA | 10% | Celda Markdown con prompts usados, respuestas recibidas y ajustes propios |

**Requisitos del repositorio:**
- Mínimo 3 commits significativos con mensajes descriptivos.
- README con descripción e instrucciones de ejecución.
- `requirements.txt` con todas las dependencias.
- Notebook limpio, sin errores de ejecución, con celdas Markdown explicativas.

**Prácticas penalizables a evitar:**
- Usar accuracy como métrica principal si hay clases desbalanceadas.
- Entregar gráficos sin interpretación textual.
- Copiar código de IA sin ajuste ni criterio propio.
- No documentar el uso de IA en la sección 5.

---

## Entorno de desarrollo

- Python 3.11
- Entorno virtual en `.venv/` (ya configurado, ignorado por git)
- Dependencias: `pandas`, `numpy`, `openpyxl`, `scikit-learn`, `shap`, `matplotlib`, `seaborn`, `jupyter`
