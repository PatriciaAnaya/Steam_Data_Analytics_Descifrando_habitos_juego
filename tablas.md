# Base de Datos de Jugadores de Steam - Diccionario y Estructura

Este documento define la estructura, el origen y el propósito de las variables recolectadas a través de la API de Steam (Steamworks Web API) para el análisis de comportamiento de jugadores.

## 1. Identificadores y Metadatos del Juego
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`appid`** | `Integer` | Identificador único del juego en el ecosistema Steam. Es la clave primaria para cruzar con datos de la tienda. |
| **`name`** | `String` | Nombre oficial del título tal como aparece en la biblioteca del usuario. |
| **`img_icon_url`** | `String` | Hash hexadecimal que identifica el icono del juego. Se usa para construir la URL del recurso gráfico. |
| **`content_descriptor`** | `List / CSV` | Etiquetas de categorías de contenido (ej. "Violencia", "Lenguaje Fuerte"). Útil para segmentación por género. |

## 2. Información del Usuario y Actividad
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`idsuser_id`** | `String (64-bit)` | El **SteamID64** único del jugador. Se debe tratar como *String* para evitar pérdida de precisión numérica. |
| **`playtime_forever`** | `Integer` | Tiempo total acumulado de juego en **minutos** desde que el usuario adquirió el título. |
| **`playtime_2weeks`** | `Integer` | Tiempo de juego en los últimos 14 días en **minutos**. Es el principal indicador de "Engagement" actual. |

## 3. Atributos de Funcionalidad
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`has_community_visible_stats`** | `Boolean` | Flag que indica si el perfil del usuario permite la lectura pública de sus estadísticas detalladas. |
| **`has_leaderboards`** | `Boolean` | Indica si el juego posee tablas de clasificación globales integradas con Steam. |

## 4. Métricas de Progresión y Logros
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`achieved`** | `Integer` | Número total de logros que el usuario ha desbloqueado satisfactoriamente. |
| **`total_achievements`** | `Integer` | Número total de logros disponibles para ese `appid` específico. |
| **`completion_percentage`** | `Float` | Porcentaje de progreso calculado. Fórmula: $\frac{achieved}{total\_achievements} \times 100$. |

---

## 💡 Notas para el Análisis (Insights)

Como analista, puedes derivar las siguientes métricas calculadas a partir de esta tabla:

1.  **Ratio de Abandono:** Usuarios con un `playtime_forever` alto pero un `completion_percentage` bajo (indica juegos tipo "endless" o dificultad excesiva).
2.  **Churn Rate Potencial:** Si el `playtime_2weeks` es 0 pero el `playtime_forever` es alto, el jugador ha pausado su actividad en ese título.
3.  **Conversión a Horas:** Divide siempre los campos de *playtime* entre 60 para normalizar las visualizaciones en horas, lo cual es el estándar de la industria.

---

## 📊 Ejemplo de Visualización Recomendada

Para un análisis rápido de tu base de datos, te sugiero un gráfico de dispersión con una relación de aspecto de **(7, 4)**:

* **Eje X:** `playtime_forever` (en horas).
* **Eje Y:** `completion_percentage`.
* **Color:** `has_community_visible_stats` (para ver si los jugadores competitivos juegan más tiempo).

