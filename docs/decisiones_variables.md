# Decisiones sobre variables del dataset

Este documento registra las decisiones tomadas sobre las variables del dataset durante el análisis, incluyendo las exclusiones, sus razones y las mecánicas del juego que las explican.

---

## 1. Mecánica de costo de comida: columnas `/` y `*`

### Contexto

El costo de comida de una carta puede ser fijo, flexible o comodín. El dataset lo registra en tres capas:

| Columna | Qué representa |
|---|---|
| `Invertebrate`, `Seed`, `Fish`, `Fruit`, `Rodent`, `Nectar` | Cantidad de fichas de tipo específico requeridas |
| `Wild (food)` | Fichas comodín que aceptan cualquier tipo de alimento |
| `/ (food cost)` | Fichas con opción entre dos tipos (elige uno u otro) |
| `* (food cost)` | Fichas de tipo completamente libre (cualquier alimento) |
| `Total food cost` | Suma total de fichas necesarias, sin importar el tipo |

### Ejemplos

- **Costo fijo:** `Seed=1, Invertebrate=1, Total food cost=2` → pagas exactamente 1 semilla + 1 invertebrado.
- **Costo con `/`:** `/ (food cost)=1, Total food cost=1` → pagas 1 ficha que puede ser de uno u otro tipo específico.
- **Costo con `*`:** `* (food cost)=1, Total food cost=1` → pagas 1 ficha de cualquier tipo (máxima flexibilidad).

### Decisión

`/ (food cost)` y `* (food cost)` fueron **excluidas del modelo** por alto porcentaje de nulos. `Total food cost` se mantiene como feature porque captura el costo de entrada.

**Implicación no modelada:** la flexibilidad del costo (`/` y `*`) es una señal relevante para el motor de alimento — un ave con `*` es más fácil de jugar porque no depende de un dado específico. Queda como mejora potencial para versiones futuras del modelo.

---

## 2. Mecánica de dirección del pico: columna `Beak direction`

### Contexto

`Beak direction` indica hacia qué lado mira el pico del ave en la carta (`L` = izquierda, `R` = derecha). Afecta la acción **Roba cartas de ave** en el Humedal.

| Valor | Fuente de la carta |
|---|---|
| `L` ← | Roba del mazo boca abajo — carta aleatoria |
| `R` → | Roba de la bandeja boca arriba — el jugador elige |

Las aves con pico `R` son más valiosas en un motor de cartas porque permiten seleccionar la carta más conveniente para el combo. Las aves con pico `L` introducen aleatoriedad.

### Decisión

`Beak direction` fue **excluida del modelo** en esta versión por no estar codificada como variable numérica. Estratégicamente tiene valor predictivo para el motor de cartas y puede incorporarse fácilmente como feature binaria (`R=1, L=0`) en una versión futura.

---

## 3. Variables excluidas del modelo

### Eliminadas durante la limpieza

Estas columnas no llegan al modelo porque se eliminan en el paso de limpieza:

| Variable | Razón |
|---|---|
| `Scientific name` | Identificador taxonómico, sin valor predictivo |
| `Flavor text` | Texto decorativo sin relación con mecánicas |
| `Fan Art flavor text` | Texto decorativo de edición especial |
| `Fan Art Pack?` | Metadato editorial |
| `Fan art beak direction` | Metadato editorial de edición especial |
| `Swift Start` | Mecánica de variante de juego rápido, no relevante para motores |
| `Automa ban` | Indica si la carta está prohibida en modo solitario |
| `/ (food cost)` | Flexibilidad del costo — excluida por alto porcentaje de nulos |
| `* (food cost)` | Comodín del costo — excluida por alto porcentaje de nulos |

### Excluidas de las features al definir X

Estas columnas existen en el dataframe limpio pero no entran al modelo:

| Variable | Razón |
|---|---|
| `Common name` | Identificador del ave, no es una característica predictiva |
| `Power text` | Texto libre — requeriría procesamiento NLP para usarse como feature |
| `Beak direction` | Variable binaria `L/R` no codificada en esta versión |
| `engine_type` | Es el target (`y`), no puede ser feature |

---

## 4. Mejoras potenciales identificadas

| Mejora | Impacto esperado | Complejidad |
|---|---|---|
| Incluir `Beak direction` como binaria (`R=1`) | Mejor predicción del motor de cartas | Baja |
| Incluir `/ (food cost)` y `* (food cost)` con imputación | Capturar flexibilidad del costo para motor de alimento | Media |
| Procesar `Power text` con TF-IDF o embeddings | Capturar semántica del poder sin reglas manuales | Alta |
| Etiquetar `engine_type` con datos reales de partidas | Eliminar el sesgo de diseño en el target | Alta |
