# wingspan-ml — Contexto del proyecto para Claude Code

## Descripción del proyecto

Proyecto final del Diplomado en Transformación Digital Empresarial.
Análisis estratégico y modelado no supervisado aplicado a las cartas del juego de mesa Wingspan.

**Pregunta central:** ¿Qué grupos naturales emergen de los textos de poder de las cartas de Wingspan, y cómo se relacionan esos grupos con el tipo de activación, las mecánicas especiales y el hábitat del ave?
**Enfoque:** Clustering no supervisado sobre texto vectorizado (TF-IDF) con reducción de dimensionalidad (PCA + t-SNE).

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

## Skills disponibles

| Skill | Ubicación | Cuándo usarla |
|---|---|---|
| `wingspan-domain` | `.claude/skills/wingspan-domain.md` | Antes de construir la variable objetivo, formular hipótesis o interpretar resultados. Contiene reglas del juego, señales de cada motor, función `classify_engine()` recomendada y notas de modelado. |

> Siempre preferir la lógica de `wingspan-domain` sobre heurísticas ad-hoc.

---

## Estructura del repositorio

```
wingspan-ml/
├── .claude/
│   ├── commands/
│   │   └── status.md                    ← slash command /status
│   └── skills/
│       └── wingspan-domain.md           ← reglas del juego y lógica de clasificación
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
- **Variable de análisis principal:** `Power text` — texto libre que describe el poder de cada carta. Se vectoriza con TF-IDF para descubrir grupos naturales.
- **Variables de enriquecimiento:** `Color`, `Predator`, `Flocking`, `Forest`, `Grassland`, `Wetland`, `Egg limit`, `Victory points` — se usan para describir el perfil de cada cluster, no como features de entrada.

---

## Esqueleto de referencia: `fraude_detection_pipeline.ipynb`

El docente (Rosmer) recomienda usar este script como base del proyecto. Al adaptar el notebook de Wingspan, seguir la misma filosofía de flujo profesional:

| Sección del pipeline de fraude | Equivalente en Wingspan |
|---|---|
| Imports y configuración | Imports y configuración |
| Carga de datos | Carga con `skiprows=[1, 2]`, separar cartas con y sin poder |
| Análisis exploratorio rápido | EDA del `Power text`: longitud, palabras frecuentes, nulos |
| Preparación de datos | TF-IDF sobre `Power text`, reducción con PCA |
| Definición de modelos | K-means vs DBSCAN |
| Búsqueda de hiperparámetros | Método del codo + silhouette score para K-means; eps + min_samples para DBSCAN |
| Evaluación | Silhouette score, Davies-Bouldin index, visualización t-SNE |
| Comparación visual | t-SNE coloreado por cluster, top palabras por cluster |
| Importancia de features | Perfil de cada cluster: Color, Predator, Flocking, hábitat |
| Resumen ejecutivo | ¿Los clusters son motores reconocibles? Recomendación al jugador |

**Diferencias clave respecto al pipeline de fraude:**
- No hay `train_test_split` — el clustering es no supervisado, usa todos los datos.
- Las métricas de evaluación son internas: silhouette score y Davies-Bouldin index.
- La "interpretación" viene del perfil de cada cluster, no de SHAP.

---

## Requisitos académicos (rúbrica)

El notebook debe cubrir estas cinco secciones en orden:

| Sección | Peso | Contenido mínimo |
|---|---|---|
| 1. Problema y datos | 15% | Pregunta, justificación, carga reproducible, EDA sobre `Power text`, limpieza |
| 2. Modelado y análisis | 35% | TF-IDF + PCA + t-SNE, K-means vs DBSCAN, justificación del número de clusters |
| 3. Interpretación | 25% | Perfil de cada cluster, mínimo 3 visualizaciones con título e interpretación textual |
| 4. Conclusiones | 15% | Respuesta a la pregunta, limitaciones, recomendación concreta al jugador |
| 5. Uso de IA | 10% | Celda Markdown con prompts usados, respuestas recibidas y ajustes propios |

**Requisitos del repositorio:**
- Mínimo 3 commits significativos con mensajes descriptivos.
- README con descripción e instrucciones de ejecución.
- `requirements.txt` con todas las dependencias.
- Notebook limpio, sin errores de ejecución, con celdas Markdown explicativas.

**Prácticas penalizables a evitar:**
- Entregar gráficos sin interpretación textual.
- Copiar código de IA sin ajuste ni criterio propio.
- No documentar el uso de IA en la sección 5.
- No describir el perfil de los clusters encontrados.

---

## Entorno de desarrollo

- Python 3.11
- Entorno virtual en `.venv/` (ya configurado, ignorado por git)
- Dependencias: `pandas`, `numpy`, `openpyxl`, `scikit-learn`, `shap`, `matplotlib`, `seaborn`, `jupyter`
