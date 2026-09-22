[English](README.md) · **Español**

# RSVP — Rapid Serial Visual Presentation

**Un repositorio sobre RSVP (Rapid Serial Visual Presentation): qué es, por qué funciona y cómo se puede aplicar a la lectura en pantalla.**

Aquí iré reuniendo material y herramientas en torno a esta técnica de lectura. De momento el repo incluye un **lector web** completo y funcional; el resto es documentación sobre el propio concepto.

---

## ¿Qué es RSVP?

**RSVP** son las siglas de *Rapid Serial Visual Presentation*, una técnica de presentación de información en la que los estímulos se muestran **uno a uno, en el mismo punto de la pantalla**, a un ritmo controlado.

Aplicada a la lectura, consiste en mostrar una palabra cada vez, siempre en la misma posición, en lugar de presentar un bloque de texto que el ojo deba recorrer. El lector no mueve los ojos por la página: las palabras van apareciendo donde ya está mirando.

Es la base de muchas apps de "lectura rápida" y también se ha usado en investigación sobre atención, memoria de trabajo y percepción visual.

---

## ¿Por qué funciona?

### 1. Elimina los movimientos sacádicos

Cuando leemos texto normal, nuestros ojos no se deslizan de forma continua: avanzan a saltos (**sacadas**) y se detienen brevemente en cada punto (**fijaciones**). Cada salto cuesta tiempo y atención, y a veces obliga a retroceder (regresiones).

RSVP suprime ese coste: no hay saltos ni regresiones porque no hay que moverse por la página. La palabra llega al punto de fijación.

### 2. Ancla cada palabra en su ORP

El **ORP** (*Optimal Recognition Point*) es la posición de una palabra donde el ojo la identifica más rápido. Está cerca del principio de la palabra (normalmente la 2.ª–3.ª letra, según longitud).

Si una palabra se centra sin más, el ojo tiene que corregir su posición para caer sobre ese punto. Si se alinea **el ORP** con el centro de la pantalla, el reconocimiento es inmediato. Por eso los buenos lectores RSVP resaltan esa letra y desplazan la palabra para que quede en el eje.

### 3. Regula el ritmo de forma explícita

En lectura normal, el ritmo depende de la dificultad del texto y de la atención. En RSVP se fija con un parámetro (**WPM**, palabras por minuto). Eso permite:

- Leer **más rápido** de lo que permitirían las sacadas.
- Leer **más despacio y con foco** cuando el texto es denso.
- Mantener un ritmo **constante**, sin acelerones ni frenazos.

Los buenos lectores RSVP además **modulan** ese ritmo: dan más tiempo a palabras largas y a signos de puntuación, para que la lectura no suene mecánica.

### 4. Reduce la carga visual

Al no haber texto alrededor, se reduce la cantidad de información compitiendo por la atención. El ojo tiene un único objetivo y el cerebro no gasta recursos en filtrar líneas vecinas.

---

## ¿Para qué sirve?

- **Lectura rápida** de artículos, correos, informes o apuntes.
- **Repaso** de material ya conocido.
- **Estudio con foco**: forzar un ritmo evita la dispersión.
- **Accesibilidad**: útil para algunas personas con dificultades de seguimiento visual.
- **Investigación**: es un paradigma clásico en psicología experimental.

### Cuándo no es ideal

- Texto con **estructura visual** relevante (tablas, código, diagramas, fórmulas).
- Material que requiere **releer** o comparar pasajes.
- Textos donde el **formato** (negritas, listas, jerarquía) es parte del significado.
- Cuando se busca **comprensión profunda**, no velocidad.

RSVP acelera el acceso a la información; no sustituye a la lectura atenta cuando esta es necesaria.

---

## El ORP, en detalle

El ORP se calcula normalmente a partir de la longitud de la palabra, contando solo letras y números (la puntuación no cuenta):

| Longitud | Posición del ORP |
| --- | --- |
| 1 | 0 |
| 2–5 | 1 |
| 6–9 | 2 |
| 10–13 | 3 |
| 14–17 | 4 |
| 18–21 | 5 |
| 22–25 | 6 |
| 26+ | 6 + ⌊(n − 25) / 4⌋ |

La idea es que el pivote nunca quede demasiado a la izquierda en palabras muy largas, pero que tampoco se aleje tanto como para que la cola derecha crezca sin control.

En implementaciones serias, el pivote **salta la puntuación**: si una palabra empieza por `"`, `(`, `¿` o contiene signos internos, el ORP debe caer sobre una letra o número real, nunca sobre un símbolo. Esto es especialmente importante con URLs y tokens con puntuación interna.

---

## Ritmo: por qué no conviene ir a velocidad constante

Un lector RSVP que avanza siempre al mismo intervalo se siente artificial. El cerebro no procesa igual una palabra de tres letras que una de quince, ni una coma que un punto.

Un esquema habitual (y el que usa el lector de este repo) es:
```
intervalo = (60000 / WPM) × multiplicador
```

Donde `multiplicador` arranca en `1` y suma:

- `+0.15` si la palabra tiene más de 8 letras/números.
- `+0.15` adicional si tiene más de 12.
- `+0.80` si termina en `. ! ? …`.
- `+0.40` si termina en `, ; :`.

Esto hace que el texto "respire" donde debe y que las frases no se atropellen.

---

## Contenido del repositorio

De momento, este repo contiene:

- **`index.html`** — un lector RSVP web completo, en un solo archivo, sin dependencias ni build.

### El lector web

Un lector RSVP de una sola página que se puede abrir directamente en el navegador o servir desde GitHub Pages.

**Lectura**
- ORP resaltado en rojo y anclado al centro del escenario.
- Ajuste automático del tamaño de fuente midiendo el ancho real del texto (`canvas.measureText`).
- WPM ajustable de 60 a 1200.
- Pausas naturales por longitud y puntuación.
- *Chunking* de tokens muy largos (URLs, palabras compuestas) para que nunca se salgan de pantalla.

**Carga de texto**
- Pegar directamente en el textarea.
- Arrastrar y soltar archivos.
- Subir varios archivos a la vez.
- Soporte para muchas extensiones: `.txt`, `.md`, `.rst`, `.log`, `.csv`, `.json`, `.html`, `.xml`, `.css`, `.js`/`.ts`/`.tsx`, `.py`, `.rb`, `.go`, `.rs`, `.java`, `.c`/`.cpp`, `.sh`, `.yml`, `.toml`, `.ini`, `.sql`, `.tex` y más.
- Limpieza automática de HTML/XML/SVG (se quitan etiquetas, scripts y estilos).

**Reproducción**
- Play/pausa, ±1 y ±10 palabras.
- Barra de progreso clicable.
- Tiempo restante estimado.
- Contador de palabra actual / total.
- El WPM se guarda en `localStorage`.

**Exportación y compartir**
- Exportación a vídeo con `MediaRecorder` + `canvas` (intenta MP4; cae a WebM si el navegador no lo soporta).
- Generación de un enlace compartible con el texto y el WPM comprimidos en el hash de la URL (`#v1=…`).

---

## Contribuciones

Issues y PRs son bienvenidos. Cualquier mejora del lector, corrección de la documentación o referencia adicional sobre RSVP es bienvenida.

