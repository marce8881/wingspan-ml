# **Propuesta de Proyecto Final \- Diplomado en Transformación Digital Empresarial**

**Tema:** Análisis Estratégico y Modelado Predictivo del Juego de Mesa *Wingspan*

## **1\. Objetivo, Estrategia del Dataset y Visión a Largo Plazo**

Desarrollar un proyecto de Machine Learning reproducible aplicado a un problema analítico y estratégico.

* **Visión del Proyecto (Etapa 1 de un Proyecto Mayor):** Este entregable académico constituye la **Etapa 1** de un proyecto estadístico personal a largo plazo desarrollado en conjunto con mi esposo (también estadístico). Como ávidos fanáticos de los juegos de mesa, siendo *Wingspan* nuestro gran favorito, nuestro objetivo final es extraer un histórico de partidas reales y analizar empíricamente qué motores otorgan mayor cantidad de puntos. Este proyecto de clase sentará las bases metodológicas, de limpieza de datos y de modelado inicial para esa meta mayor.  
* **Cumplimiento de Volumen:** Dado que el dataset exige un mínimo de 500 filas y 5 variables, el proyecto integrará tanto la base de datos de las **Cartas de Aves** como la de las **Cartas de Bonus/Objetivos** de *Wingspan* (incluyendo expansiones). Esto garantizará superar la barrera de las 500 filas de forma orgánica y con rigor estadístico.

## **2\. Enfoque Metodológico: Clasificación**

Se aplicará un enfoque de **Aprendizaje Supervisado (Clasificación)**.

* **Problema de negocio/estrategia:** *¿Qué tipo de estrategia de motor (ej. motor de huevos vs. motor de robo de cartas vs. motor de comida) es más probable que se consolide como ganadora o de alto valor basándose en los atributos de las cartas iniciales?*  
* **Requisito técnico:** Se dividirán los datos en conjuntos de *train/test* para validar la precisión del modelo.  
* **Comparación de Modelos:** Se entrenarán y compararán al menos dos algoritmos de clasificación (por ejemplo, Regresión Logística Multinomial vs. Random Forest Classifier o XGBoost).  
* **Estructura del código:** Todo el preprocesamiento (codificación de variables categóricas como hábitats, escalado de costos) y el modelado se construirá utilizando *Pipelines* de Scikit-learn y métricas de evaluación adecuadas (F1-Score, Precision, Recall). Se utilizará la estructura del script fraude\_detection\_pipeline.ipynb visto en clase como plantilla base.

## **3\. Entregables y Repositorio (Portafolio Profesional)**

El proyecto se entregará como un portafolio en un **repositorio público de GitHub**. El repositorio debe cumplir estrictamente con este checklist:

* Enlace público y funcional.  
* Historial de desarrollo con al menos **3 commits significativos** con mensajes descriptivos.  
* Archivo README.md con la descripción del proyecto e instrucciones de ejecución.  
* Archivo requirements.txt con todas las dependencias necesarias.  
* Jupyter Notebook limpio, ejecutado sin errores, estructurado y acompañado de texto en celdas Markdown.

## **4\. Estructura Obligatoria del Jupyter Notebook (Rúbrica)**

El desarrollo en el cuaderno seguirá la siguiente distribución porcentual:

1. **Problema y datos (15%):** Planteamiento de la pregunta estratégica sobre los motores de juego en Wingspan. Carga de datos reproducible, análisis exploratorio básico (EDA) identificando la distribución de tipos de cartas, y limpieza de datos nulos.  
2. **Modelado / Análisis (35%):** Implementación de *Pipelines* de Scikit-learn, entrenamiento de los modelos de clasificación y comparación de métricas.  
3. **Interpretación (25%):** Integración de la librería **SHAP** para explicar qué características (ej. costo de alimento, puntos base, tipo de nido) influyen más en la clasificación de una estrategia. Se incluirán al menos **3 visualizaciones clave**, cada una con su título e interpretación analítica textual.  
4. **Conclusiones (15%):** Respuesta a la pregunta estratégica, indicando qué atributos tempranos definen mejor un motor ganador. Se mencionarán las limitaciones estadísticas del modelo y recomendaciones para el jugador.  
5. **Uso de IA (10%):** Celda Markdown final que documente el uso de asistentes de IA (prompts, respuestas y ajustes manuales realizados con criterio estadístico).

## **5\. Logística y Presentación Final**

* **Registro:** Inscripción del equipo en el Google Sheets del curso.  
* **Video Explicativo:** Grabación de un video de máximo 5 minutos demostrando el impacto del análisis.  
* **Regla de oro de la presentación:** Enfoque en el *storytelling* estratégico y los hallazgos (ej. "Descubrimos que las cartas de bajo costo en bosque predicen un motor de comida exitoso con 80% de precisión"), sin leer el código del cuaderno.