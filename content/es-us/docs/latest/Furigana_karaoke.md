---
title: Tutorial de Furigana
menu:
  docs:
    parent: tutorials
weight: 2730
---

![Furigana-demo-1](/img/3.2/Furigana-demo-1.png)

_Furigana_ (en Aegisub a menudo abreviado como _furi_) se refiere a pequeños caracteres guía fonéticos escritos junto al texto principal en japonés, específicamente usando el alfabeto fonético hiragana para indicar cómo se deben pronunciar los caracteres ideográficos kanji. En general, colocar texto más pequeño junto a una línea de texto principal se conoce como [_ruby text_](http://en.wikipedia.org/wiki/Ruby_character), pero dado que la implementación aquí está diseñada específicamente para el furigana japonés, este término se usará en toda la documentación.

Ninguno de los formatos de subtítulos que admite Aegisub tiene compatibilidad nativa con ruby text o furigana. Sin embargo, la implementación estándar de [karaskel]({{< relref "Automation/Lua/Modules/karaskel.lua.md" >}}) incluye un algoritmo que puede generar diseños básicos de furigana al calcular la posición de cada carácter individual.

Esta página describe la sintaxis que entiende el script de Automation 4, karaskel.lua, para el furigana y cómo utilizar la información de diseño que calcula para posicionar los caracteres correctamente.

[Karaoke Templater]({{< relref "Automation/Karaoke_Templater" >}}) también implementa soporte para furigana utilizando el algoritmo y la sintaxis de karaskel.lua.

Es importante señalar que la sintaxis está diseñada para karaoke y se basa en texto [sincronizado para karaoke]({{< relref "Karaoke_Timing_Tutorial" >}}). No es adecuada para la composición tipográfica de texto regular (por ejemplo, líneas de diálogo) con ruby text de propósito general. Para ello, se necesitaría una sintaxis más elaborada y un motor de diseño más complejo.

## Sintaxis de resaltado múltiple

Un requisito previo para una parte integral de la sintaxis del furigana es la sintaxis de resaltado múltiple.

Si el texto de una sílaba es un signo de número (#, ASCII 35, Unicode U+0023), esa sílaba se "unirá" con la anterior: el signo de número se eliminará y los tiempos de las dos sílabas se sumarán, produciendo una sola sílaba. Se pueden tener varias sílabas con signos de número en fila, sumando múltiples tiempos de esta manera.

Los tiempos de cada una de estas sílabas con signo de número aún se almacenan en la [tabla de resaltado]({{< relref "Automation/Lua/Modules/karaskel.lua.md#highlighttable" >}}) de la estructura de sílabas generada, pero el tiempo principal (`start_time` y `end_time`) reflejará solo la suma de los tiempos de estas sílabas.

{{<example-box>}}
Esta línea muestra cómo se usa la sintaxis de resaltado múltiple para marcar kanji y grupos de kanji que abarcan múltiples sílabas:

```ass
{\k5}明日{\k10}#{\k5}#{\k10}ま{\k7}た{\k10}会{\k4}う{\k6}時{\k14}#
```

Genera las siguientes estructuras de sílabas:

<table class="karatable">
    <tr><th>Texto</th><th>Duración de la sílaba</th><th>Duraciones de resaltado</th></tr>
    <tr><td rowspan="3">明日</td><td rowspan="3">20</td><td>5</td></tr>
    <tr><td>10</td></tr>
    <tr><td>5</td></tr>
    <tr><td>ま</td><td>10</td><td>10</td></tr>
    <tr><td>た</td><td>7</td><td>7</td></tr>
    <tr><td>会</td><td>10</td><td>10</td></tr>
    <tr><td>う</td><td>4</td><td>4</td></tr>
    <tr><td rowspan="2">時</td><td rowspan="2">20</td><td>6</td></tr>
    <tr><td>14</td></tr>
</table>
{{</example-box>}}

## Furigana básico

Para agregar furigana a una sílaba, se coloca un carácter de barra vertical (|, ASCII 124, Unicode U+007C) después del texto principal de la sílaba y luego se agrega el texto de furigana después de la barra. También se puede agregar furigana a sílabas repetidas (sílabas con signo de número para resaltado múltiple) para hacer que el furigana de una sola sílaba principal abarque múltiples sílabas de furigana.

Cuando varias sílabas consecutivas tienen furigana, el furigana de todas esas sílabas se agrupa y se centra encima de la cadena de sílabas principales a la que pertenecen. Si la cadena de furigana es más ancha que el texto principal, el furigana se alineará a la izquierda con el texto principal. Se puede controlar este comportamiento con caracteres especiales, como se explica a continuación.

{{<example-box>}}
Agregar furigana al ejemplo anterior:

```ass
{\k5}明日|あ{\k10}#|し{\k5}#|た{\k10}ま{\k7}た{\k10}会|あ{\k4}う{\k6}時|と{\k14}#|き
```

Las siguientes sílabas, resaltados y furigana se generan:

<table class="karatable">
    <tr><th>Texto</th><th>Duración de la sílaba</th><th>Duración de resaltado/furigana</th><th>Furigana</th></tr>
    <tr><td rowspan="3">明日</td><td rowspan="3">20</td><td>5</td><td>あ</td></tr>
    <tr><td>10</td><td>し</td></tr>
    <tr><td>5</td><td>た</td></tr>
    <tr><td>ま</td><td>10</td><td>10</td></tr>
    <tr><td>た</td><td>7</td><td>7</td></tr>
    <tr><td>会</td><td>10</td><td>10</td><td>あ</td></tr>
    <tr><td>う</td><td>4</td><td>4</td></tr>
    <tr><td rowspan="2">時</td><td rowspan="2">20</td><td>6</td><td>と</td></tr>
    <tr><td>14</td><td>き</td></tr>
</table>
{{</example-box>}}

## Control del diseño

A menudo, el diseño generado con la sintaxis básica de furigana no es exactamente el deseado o puede resultar confuso. Para solucionar esto, existen dos caracteres especiales que permiten controlar la disposición del furigana.

Ambos caracteres especiales se colocan antes del primer carácter del furigana en una sílaba, es decir, justo después del carácter de barra vertical.

El primero es el signo de exclamación (!, ASCII 33, Unicode U+0021), que marca una "separación de secuencia". Actúa como un divisor invisible que evita que el furigana de esta sílaba se fusione con el de la sílaba anterior. Esto se usa cuando hay dos palabras kanji adyacentes que tienen furigana y necesitan separarse. En ese caso, se coloca el signo de exclamación al inicio del furigana de la primera sílaba de la segunda palabra.

El segundo es el signo menor que (\<, ASCII 60, Unicode U+003C), que marca una "separación de secuencia con alineación a la izquierda". Tiene la misma función de separación que el signo de exclamación, pero también cambia el comportamiento de desbordamiento. Cuando una secuencia de furigana comienza con una sílaba marcada con el signo menor que y es más ancha que el texto principal al que se aplica, se centrará sobre el texto principal, incluso si eso significa que se extienda más allá del borde izquierdo del mismo.

En todos los casos, si dos secuencias de furigana se extienden más allá de su texto principal hasta el punto de superponerse, el texto principal se ajustará para evitar la superposición.

{{<example-box>}}
Aquí está el mismo texto de ejemplo mostrado sin control de diseño y con cada uno de los dos caracteres de control de diseño:

| Resultado                                          | Script                                                                                                              |
|----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| ![Furigana-demo-4](/img/3.2/Furigana-demo-4.png)   | `{\k10}`中\|ちゅ`{\k10}`#\|う`{\k10}`国\|ご`{\k10}`#\|く<br>`{\k10}`<u>魂\|た</u>`{\k10}`#\|ま`{\k10}`#\|し`{\k10}`#\|い   |
| ![Furigana-demo-3](/img/3.2/Furigana-demo-3.png)   | `{\k10}`中\|ちゅ`{\k10}`#\|う`{\k10}`国\|ご`{\k10}`#\|く<br>`{\k10}`<u>魂\|!た</u>`{\k10}`#\|ま`{\k10}`#\|し`{\k10}`#\|い  |
| ![Furigana-demo-2](/img/3.2/Furigana-demo-2.png)   | `{\k10}`中\|ちゅ`{\k10}`#\|う`{\k10}`国\|ご`{\k10}`#\|く<br>`{\k10}`<u>魂\|\<た</u>`{\k10}`#\|ま`{\k10}`#\|し`{\k10}`#\|い |

Es difícil notar la diferencia entre las dos primeras muestras, ya que la variación es solo de unos pocos píxeles, pero está ahí. En el primer ejemplo, el た se extiende ligeramente más allá del borde izquierdo de 魂 y por encima de 国, mientras que en el segundo se alinea exactamente con 魂. Además, en el segundo ejemplo, ちゅうごく también se centra sobre 中国, mientras que en el primero no.
{{</example-box>}}

## Resumen

| Carácter | ASCII | Unicode          | Ubicación                        | Significado                                                                                                                                                            |
|:--------:|:-----:|------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    #     |  35   | U+0023<br>U+FF03 | En lugar del texto principal     | Extiende la sílaba anterior con otro resaltado                                                                                                                         |
|    \|    |  124  | U+007C<br>U+FF5C | Entre texto principal y furigana | Separa el texto principal del texto furigana en una sílaba                                                                                                             |
|    !     |  33   | U+0021<br>U+FF01 | Primer carácter del furigana     | Separación de secuencia; evita que el furigana de esta sílaba se una con el de la sílaba anterior                                                                      |
|    \<    |  60   | U+003C<br>U+FF1C | Primer carácter del furigana     | Separación de secuencia con alineación izquierda; evita que el furigana se una con el de la sílaba anterior, pero permite que se extienda más allá del texto principal |

Cada carácter especial puede representarse mediante dos diferentes códigos Unicode. El primero es el carácter estándar correspondiente a ASCII, mientras que el segundo es su versión de ancho completo (full-width). Cuando se usa un IME (Editor de Método de Entrada) para escribir en japonés, a menudo es más fácil ingresar caracteres en modo de ancho completo en lugar de desactivar el IME solo para escribir uno o dos caracteres ASCII y luego volver a activarlo. Por esta razón, Aegisub acepta tanto las versiones de ancho medio (ASCII) como las de ancho completo de estos caracteres.

## Uso en Karaoke Templater

Furigana: [La clase de plantilla _furi_]({{< relref "Automation/Karaoke_Templater/Template_modifiers#furi" >}})

Resaltado múltiple: [El modificador _multi_]({{< relref "Automation/Karaoke_Templater/Template_modifiers#multi" >}})

{{<example-box>}}
Los ejemplos utilizados anteriormente en esta página se generaron con este fragmento de código de kara-templater:

```plaintext
Comment: 0,0:00:00.00,0:00:00.00,Default,,0000,0000,0000,template syl,{\pos(!line.left+syl.center!,!line.middle!)\an5\k!syl.start_time/10!\k$kdur}
Comment: 0,0:00:00.00,0:00:00.00,Default,,0000,0000,0000,template furi,{\pos(!line.left+syl.center!,!line.middle-line.height!)\an5\k!syl.start_time/10!\k$kdur}
Comment: 0,0:00:00.00,0:00:02.00,Default,,0000,0000,0000,karaoke,{\k15}二|ふ{\k15}#|た{\k10}人|り{\k15}だ{\k57}け{\k5}の{\k6}地|ほ{\k5}球|し{\k8}で
Comment: 0,0:00:02.00,0:00:04.00,Default,,0000,0000,0000,karaoke,{\k10}中|ちゅ{\k10}#|う{\k10}国|ご{\k10}#|く{\k10}魂|<た{\k10}#|ま{\k10}#|し{\k10}#|い
Comment: 0,0:00:04.00,0:00:06.00,Default,,0000,0000,0000,karaoke,{\k10}中|ちゅ{\k10}#|う{\k10}国|ご{\k10}#|く{\k10}魂|!た{\k10}#|ま{\k10}#|し{\k10}#|い
Comment: 0,0:00:06.00,0:00:08.00,Default,,0000,0000,0000,karaoke,{\k10}中|ちゅ{\k10}#|う{\k10}国|ご{\k10}#|く{\k10}魂|た{\k10}#|ま{\k10}#|し{\k10}#|い
```

La fuente utilizada es MS PMincho 30 pt, con el furigana en 15 pt.
{{</example-box>}}

## Uso en scripts de Lua

Todo está en [karaskel]({{< relref "Automation/Lua/Modules/karaskel.lua.md" >}}).

El diseño de furigana se invoca automáticamente con `karaskel.preproc_line_pos` si existe un estilo de furigana correspondiente a un estilo principal de línea. El estilo de furigana de un estilo principal es un estilo con el mismo nombre, pero con `-furigana` agregado al final. Por ejemplo, el estilo de furigana de `Default` es `Default-furigana`.

Karaskel puede generar estilos de furigana automáticamente si el argumento `generate_furigana` (segundo argumento) de la función `karaskel.collect_head` es `true`. Los estilos de furigana generados automáticamente son idénticos al estilo principal en el que se basan, excepto que el tamaño de la fuente se reduce a la mitad.

Las sílabas de furigana se almacenan en `line.furi` y siguen el mismo formato que las sílabas regulares. Es importante recordar asignar el estilo de las líneas generadas al estilo de furigana.

Los resaltados múltiples siempre se procesan, incluso cuando el diseño de furigana no se aplica. Los datos de resaltado múltiple se almacenan en `syl.highlights`.

{{<todo>}}Más detalles por añadir{{</todo>}}
