# Wingspan ML — Análisis Estratégico y Modelado Predictivo

Proyecto final del Diplomado en Transformación Digital Empresarial.

## Pregunta de investigación

¿Qué atributos de las cartas de Wingspan predicen mejor el tipo de motor de juego (huevos, comida, robo de cartas) que una carta potencia?

## Dataset

Fuente: [BoardGameGeek](https://boardgamegeek.com/) — base de datos de cartas de Wingspan (juego base + expansiones).

- ~907 cartas de aves
- 65 variables por carta: hábitat, costo de comida, tipo de nido, puntos de victoria, texto de poder, región geográfica, entre otras.
- Archivo: `data/raw/wingspan-20260128.xlsx`

## Estructura del proyecto

```
wingspan-ml/
├── data/
│   └── raw/                    ← dataset original sin modificar
├── docs/                       ← propuesta, guía del curso y descripción del dataset
├── notebooks/
│   └── wingspan_analysis.ipynb ← análisis principal (EDA → modelos → SHAP → conclusiones)
├── requirements.txt
└── README.md
```

## Cómo ejecutar

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/marce8881/wingspan-ml.git
   cd wingspan-ml
   ```

2. Crear entorno virtual e instalar dependencias:
   ```bash
   python -m venv .venv
   source .venv/bin/activate   # En Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. Abrir el notebook:
   ```bash
   jupyter notebook notebooks/wingspan_analysis.ipynb
   ```

## Enfoque metodológico

- **Tipo:** Aprendizaje supervisado — Clasificación multiclase
- **Variable objetivo:** Tipo de motor de juego derivado del texto de poder de cada carta
- **Modelos comparados:** Regresión Logística Multinomial vs. Random Forest Classifier
- **Pipeline:** Scikit-learn (escalado + codificación + modelo)
- **Interpretabilidad:** SHAP values

## Autora

Marcela Cadena — Diplomado en Transformación Digital Empresarial, 2026
