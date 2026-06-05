Aquí tienes el resumen estructurado y detallado con todas las directrices, requisitos y consejos compartidos sobre el **Proyecto Final** de tu curso:

### 📌 Información General y Logística

El proyecto final consiste en aplicar Machine Learning a un problema real o conjunto de datos de alcance empresarial, documentado en un entorno reproducible 1, 2\.

* **Modalidad:** Puedes realizarlo de forma individual o en equipos de 2 a 3 personas 2, 3\.  
* **Inscripción obligatoria:** Debes registrar tu grupo en el documento de Google Sheets compartido por el profesor. Si aún no tienen el tema definido, pueden indicar "por definir" 4-6.  
* **Dataset (Conjunto de datos):** Es de libre elección (puedes buscar en Kaggle, datos.gov.co, UCI, etc.), pero debe tener un **mínimo de 500 filas y 5 variables (columnas)** 2, 7, 8\.  
* **Tiempo estimado:** El diseño del proyecto contempla entre 4 y 6 horas de trabajo autónomo 2, 3\.

### 🎯 Enfoques Permitidos

Debes elegir uno de los siguientes tres enfoques analíticos para tu proyecto 1, 7:

1. **Predicción (Supervisado):** Ej. Predecir abandono de clientes. Exige dividir los datos en *train/test*, comparar al menos 2 modelos (ej. regresión logística vs. XGBoost), usar métricas correctas y aplicar Pipelines de Scikit-learn 1, 9\.  
2. **Clustering (No supervisado):** Ej. Segmentación de clientes. Debes comparar al menos 2 algoritmos (ej. K-means vs. DBSCAN), justificar el número de clústeres y describir el perfil de los grupos encontrados 3, 9\.  
3. **Reducción de Dimensionalidad:** Comparar al menos 2 técnicas (ej. PCA vs. t-SNE/UMAP), explicando la varianza y visualizando/interpretando el espacio en 2D 3, 10\.

### 📂 Entregables y Requisitos Técnicos

La entrega principal no es solo el código, sino un **repositorio público en GitHub** que funcione como tu portafolio profesional 2, 7, 11\. El repositorio debe cumplir con este *checklist* 8, 11:

* El repositorio debe ser público y el enlace debe funcionar 8\.  
* Tener al menos **3 *commits* significativos** con mensajes descriptivos 8, 11\.  
* Incluir un archivo README que describa el proyecto e instrucciones para ejecutarlo 11, 12\.  
* Incluir el archivo requirements.txt con las dependencias 8, 11\.  
* El **Jupyter Notebook** debe estar limpio, sin errores de ejecución, ordenado y con celdas de texto (Markdown) explicativo 8, 11\.

### 📓 Estructura Obligatoria del Notebook (Rúbrica)

Tu cuaderno debe estar dividido estrictamente en estas secciones:

1. **Problema y datos (15%):** Pregunta a resolver, justificación, carga de datos reproducible, exploración básica y limpieza mínima 2, 7\.  
2. **Modelado / Análisis (35%):** Aplicación de técnicas (comparando al menos 2 métodos) usando Pipelines y métricas apropiadas 9, 13\.  
3. **Interpretación (25%):** Es clave no dejar el modelo como "caja negra". Si predices, usa **SHAP**; si agrupas, detalla los perfiles. Debes incluir al menos **3 visualizaciones relevantes con título e interpretación** 8, 10, 14\.  
4. **Conclusiones (15%):** Responder directamente la pregunta de negocio, indicar limitaciones y dar una recomendación concreta 2, 8, 14\.  
5. **Uso de IA (10%):** Una celda explícita documentando cómo usaste herramientas de Inteligencia Artificial (prompts usados, respuestas recibidas y ajustes con criterio propio). No documentarlo se penaliza 2, 11, 12\.

### 🎥 Formato de Presentación

Aunque la rúbrica escrita menciona una presentación en vivo de 10 minutos (7 de exposición y 3 de preguntas) 3, 13, el profesor aclaró en las últimas sesiones una modificación logística:

* **Video Explicativo:** El requisito principal solicitado es grabar un "mini video" explicativo de máximo 5 minutos demostrando el impacto analítico 5, 6\.  
* **Alternativa en Vivo:** Excepcionalmente, si la carga laboral lo impide, se permitirá a algunos grupos presentar en vivo durante la última sesión, bajo un estricto control de tiempo 5\.  
* *Regla de oro de la presentación:* **No leas el notebook**. Cuéntales el problema, qué técnicas usaste y qué hallazgos de negocio obtuviste 13\.

### 💡 Consejos Estratégicos del Docente (Rosmer)

* **Plantilla base:** Se recomienda fuertemente utilizar el script fraude\_detection\_pipeline.ipynb (visto en clases anteriores) como el "esqueleto" para tu proyecto, ya que contiene todo el flujo profesional exigido (escalado, balanceo, validación cruzada y comparación) 6, 15\.  
* **El "Plus" para destacar:** Integrar el código de explicabilidad **SHAP** a tu proyecto final para demostrar exactamente qué variables mueven la aguja en tu fenómeno es visto como un gran valor agregado 6\.  
* **Evita prácticas penalizables:** Entregar un repositorio con un solo *commit* la noche anterior, usar el *Accuracy* como métrica válida si tus datos están desbalanceados, copiar código de IA a ciegas, o entregar gráficos sin interpretación textual 16\.

