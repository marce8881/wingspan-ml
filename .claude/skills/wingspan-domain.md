# Skill: Dominio de Wingspan (Juego de Mesa)

Este archivo provee el conocimiento de dominio del juego de mesa *Wingspan*
necesario para razonar con precisión sobre las variables del dataset, construir
la variable objetivo y formular hipótesis estratégicas fundamentadas en las
reglas reales del juego.

---

## 1. Estructura del juego (contexto mínimo)

Wingspan se juega en **4 rondas**. Cada jugador tiene un tablero con 3 hábitats
(filas): **Bosque**, **Pradera** y **Humedal**. En cada turno el jugador elige
**1 de 4 acciones**:

| Acción | Hábitat activado | Recurso principal |
|---|---|---|
| Jugar un ave | Ninguno (coloca la carta) | — |
| Obtén alimento | Bosque | Fichas de alimento (del comedero de dados) |
| Pon huevos | Pradera | Huevos (moneda para jugar aves en columnas 2–5) |
| Roba cartas de ave | Humedal | Cartas de ave (opciones de combo futuro) |

Cada ronda tiene menos turnos (8→7→6→5). El jugador con más puntos al final gana.

---

## 2. Anatomía de una carta de ave

Cada carta tiene estos atributos — todos presentes en el dataset:

| Atributo | Descripción | Variable en dataset |
|---|---|---|
| `common_name` | Nombre común del ave | identificador |
| `scientific_name` | Nombre científico | referencia taxonómica |
| `victory_points` | Puntos base (0–9 pts) | target numérico potencial |
| `total_food_cost` | Suma de fichas de alimento para jugarla | costo de entrada |
| `invertebrate/seed/fish/fruit/rodent` | Tipo específico de alimento requerido | booleanos de dieta |
| `any_food` | Acepta comodín de alimento | flexibilidad de costo |
| `nest_type` | Tipo de nido: plataforma, cuenco, cavidad, suelo, estrella | variable categórica |
| `egg_capacity` | Máximo de huevos que puede almacenar | relevante para motor de huevos |
| `wingspan` | Envergadura en cm | numérica continua |
| `forest/grassland/wetland` | Hábitats donde puede jugarse (puede ser múltiple) | booleanos de hábitat |
| `power_category` | Categoría del poder marrón | clave para clasificar motor |
| `power_color` | Color del poder: marrón, rosa, blanco, sin poder | cuándo se activa |
| `predator` | Es depredadora (caza otras aves) | booleano |
| `flocking` | Forma bandadas (solapa cartas) | booleano |
| `bonus_card` | Puede dar cartas de bonificación | booleano |
| `set` | Expansión de origen: core, european, oceania | categórica de origen |

---

## 3. Los tres tipos de motor y cómo se identifican

Un **motor** es la estrategia de combo que un jugador construye durante la
partida. Se define por qué acción repite más y qué poderes encadenan.
**Esta es la variable objetivo del proyecto de clasificación.**

### Motor de Alimento (Bosque)
El jugador prioriza la acción "Obtén alimento" y coloca aves en Bosque con
poderes marrones que generan más alimento o almacenan fichas.

**Señales en el dataset:**
- `forest = TRUE` como hábitat principal
- `power_category` incluye: `"get food"`, `"cache food"`, `"get food from supply"`
- `total_food_cost` bajo (aves baratas que se pueden jugar frecuentemente)
- Aves con `any_food = TRUE` (flexibilidad para sobrevivir con lo que salga en dados)

**Ejemplo de aves ancla:** Chupasavia Norteño, Perlita Grisilla, Carbonero de Carolina

### Motor de Huevos (Pradera)
El jugador prioriza "Pon huevos" para acumular huevos como moneda y puntuar
al final (1 pt/huevo).

**Señales en el dataset:**
- `grassland = TRUE` como hábitat principal
- `power_category` incluye: `"lay eggs"`, `"lay egg"`
- `egg_capacity` alta (≥ 4) — más espacio = más puntos
- `nest_type`: cuenco o cavidad suelen tener mayor `egg_capacity`
- Aves con poderes "UNA VEZ ENTRE TURNOS" que generan huevos cuando el oponente actúa

**Ejemplo de aves ancla:** Pradero Occidental, Codorniz Californiana, Zenaida Huilota

### Motor de Robo de Cartas (Humedal)
El jugador prioriza "Roba cartas" para tener muchas opciones y construir
combos con aves de alto puntaje.

**Señales en el dataset:**
- `wetland = TRUE` como hábitat principal
- `power_category` incluye: `"draw cards"`, `"draw bird cards"`
- `victory_points` alto (≥ 4 pts) — aves más costosas pero más valiosas
- `flocking = TRUE` — las bandadas solapan cartas (1 pt/carta solapada)
- `predator = TRUE` — depredadoras dan cartas solapadas como puntos

**Ejemplo de aves ancla:** Mascarita Común, Gaviota de Franklin, Pelícano Norteamericano

---

## 4. Fuentes de puntuación al final de la partida

Entender esto es crítico para interpretar qué hace que una estrategia sea
"ganadora". Los puntos vienen de 6 fuentes:

```
TOTAL = pts_cartas_de_ave
      + pts_cartas_de_bonificación
      + pts_objetivos_de_ronda (4 losetas, una por ronda)
      + 1 pt × huevos_en_tablero
      + 1 pt × fichas_de_alimento_almacenadas_en_aves
      + 1 pt × cartas_solapadas_bajo_aves
```

Esto implica que un motor de huevos compite directamente con un motor de
bandadas (cartas solapadas) en las fuentes de puntuación individual.

---

## 5. Cartas de bonificación (segunda tabla del dataset)

Las cartas de bonificación dan puntos al final por cumplir condiciones sobre
las aves jugadas. Son esenciales para el análisis porque **modulan qué tipo de
ave conviene jugar**.

Categorías principales (cada una es una fila en la tabla `bonus_cards`):

| Categoría | Condición de puntuación |
|---|---|
| Hábitat específico | Aves que *solo* viven en bosque / pradera / humedal |
| Tipo de alimento | Aves que comen semilla / pescado / fruta / roedor / invertebrado |
| Tipo de nido | Aves con nido plataforma / cuenco / cavidad / suelo |
| Depredadoras | Aves con `predator = TRUE` (Halconero) |
| Envergadura | Aves > 65 cm (grandes) o ≤ 30 cm (paseriformes) |
| Omnívoras | Aves con `any_food = TRUE` |
| Puntos bajos | Aves con `victory_points` < 4 (Aficionado de Jardín) |
| Bandadas | Implícito en `flocking` y cartas solapadas |

**Implicación para el modelo:** la carta de bonificación actúa como
multiplicador del motor. Un ave de bajo costo puede ser estratégicamente
superior si satisface la bonificación del jugador.

---

## 6. Poderes de las aves: colores y cuándo se activan

| Color | Cuándo se activa | Relevancia estratégica |
|---|---|---|
| **Marrón** ("AL ACTIVARLA") | Cuando activas ese hábitat | Son el corazón del motor; se encadenan de derecha a izquierda |
| **Rosa** ("UNA VEZ ENTRE TURNOS") | En el turno del oponente | Permiten acumular recursos pasivamente |
| **Blanco** ("AL JUGARLA") | Solo al colocar la carta | Efecto único, no repetible |
| **Sin color** | Nunca (son información pasiva) | Solo dan puntos base o bonificaciones |

**Regla de motor:** una carta ancla de motor tiene poder **marrón** que genera
el recurso principal de su hábitat con alta frecuencia (idealmente en las
primeras columnas para activarse más veces por ronda).

---

## 7. Reglas de negocio para construir la variable objetivo `engine_type`

La etiqueta de clasificación **no existe en el dataset crudo** y debe derivarse.
Lógica de derivación recomendada (orden de prioridad):

```python
def classify_engine(row):
    """
    Clasifica una carta de ave en el tipo de motor al que contribuye.
    Basado en reglas del manual de Wingspan (edición base española).
    Una carta puede contribuir a múltiples motores; se asigna el dominante.
    """
    scores = {'food': 0, 'egg': 0, 'card': 0}

    # Señales de motor de alimento
    if row['forest']:
        scores['food'] += 2
    if 'food' in str(row['power_category']).lower():
        scores['food'] += 3
    if row['any_food']:
        scores['food'] += 1
    if row['total_food_cost'] <= 1:
        scores['food'] += 1  # barata = fácil de encadenar

    # Señales de motor de huevos
    if row['grassland']:
        scores['egg'] += 2
    if 'egg' in str(row['power_category']).lower():
        scores['egg'] += 3
    if row['egg_capacity'] >= 4:
        scores['egg'] += 2

    # Señales de motor de cartas
    if row['wetland']:
        scores['card'] += 2
    if 'draw' in str(row['power_category']).lower():
        scores['card'] += 3
    if row['flocking']:
        scores['card'] += 2
    if row['predator']:
        scores['card'] += 1
    if row['victory_points'] >= 4:
        scores['card'] += 1  # aves de alto valor favorecen humedal

    # Asignar etiqueta dominante
    dominant = max(scores, key=scores.get)
    if scores[dominant] == 0:
        return 'neutral'
    return dominant
```

**Advertencia estadística:** esta derivación introduce sesgo de diseño (label
leakage conceptual). Documentar en el notebook como limitación y mencionar que
la etiqueta ideal vendría de datos reales de partidas.

---

## 8. Hipótesis estratégicas para guiar el EDA

Estas son las preguntas que el análisis debe responder, en orden de relevancia
para la rúbrica:

1. ¿Las aves de bajo `total_food_cost` en Bosque predicen mejor el motor de
   alimento que las de alto costo?
2. ¿Existe correlación entre `egg_capacity` y `victory_points`? (el juego
   compensa la capacidad de huevos con menor puntaje base)
3. ¿Las aves multihabitat (pueden jugarse en 2-3 hábitats) son más flexibles
   estratégicamente pero contribuyen menos a un motor específico?
4. ¿El `nest_type` cuenco tiene mayor `egg_capacity` promedio que los demás?
   (hipótesis de dominio: los cuencos son especializados en huevos)
5. ¿Las aves depredadoras (`predator = TRUE`) tienen sesgo hacia Humedal?

---

## 9. Glosario rápido de variables del dataset → concepto del juego

| Variable en dataset | Término en el juego | Qué mide |
|---|---|---|
| `victory_points` | Puntos del ave | Valor base al final de partida |
| `total_food_cost` | Coste en alimento | Inversión para jugar la carta |
| `egg_capacity` | Límite de huevos | Cuántos huevos puede acumular |
| `nest_type` | Tipo de nido | Plataforma/Cuenco/Cavidad/Suelo/Estrella |
| `power_color` | Color del poder | Cuándo se activa (marrón/rosa/blanco) |
| `power_category` | Categoría del poder | Qué hace cuando se activa |
| `predator` | Depredadora | Caza aves más pequeñas (solapado) |
| `flocking` | Bandada | Solapa cartas de la mano (1 pt c/u) |
| `forest/grassland/wetland` | Hábitat | Fila del tablero donde se juega |
| `set` | Expansión | core=base, european=Europa, oceania=Oceanía |

---

## 10. Notas para el modelado

- **Desbalance de clases esperado:** el motor de cartas (Humedal) suele estar
  subrepresentado en el juego base vs. las expansiones. Verificar con
  `value_counts()` y considerar `class_weight='balanced'` en los modelos.
- **Variables con nulos esperados:** `power_category`, `power_color` y
  `nest_type` pueden tener NaN para aves sin poder o nido inusual.
  Imputar como `'none'` / `'misc'` según el paquete R original.
- **No usar `victory_points` como feature si es parte de la lógica de
  derivación del target** — revisar leakage antes de entrenar.
- **La expansión importa:** aves de Oceanía tienen mecánica de néctar no
  presente en el juego base. Considerar filtrar `set != 'oceania'` para el
  modelo base o agregar `nectar` como feature booleana.
