---
title: Edición de subtítulos
menu:
  docs:
    parent: working-with-subtitles
weight: 3000
---

Aegisub está diseñado para la edición de subtítulos. Esta página trata sobre edición básica de líneas de subtítulos; para más información sobre tipografía de subtítulos, véase [composición tipográfica]({{< relref "Typesetting" >}}). Para información acerca de la sincronización de subtítulos, véase [trabajando con audio]({{< relref "Audio" >}}).

## Abrir subtítulos

En el menú _Archivo_ hay cinco opciones relacionadas con abrir o crear subtítulos:

Nuevos subtítulos
: Crear un nuevo guion en blanco (cierra el archivo actual).

Abrir subtítulos
: Abrir un archivo de subtítulos existente o importar subtítulos desde un [archivo contenedor Matroska](http://www.matroska.org).

Abrir subtítulos con conjunto de caracteres
: Abre subtítulos, permitiendo elegir el conjunto de caracteres que Aegisub usará para interpretar el archivo. Normalmente no es necesario, pero si el archivo tiene un conjunto de caracteres inusual, Aegisub podría detectarlo incorrectamente.

Abrir subtítulos desde video
: Abrir subtítulos multiplexados en el archivo de video actualmente abierto. Esta opción actualmente solo funciona con archivos de video Matroska.

Abrir [subtítulos autoguardados]({{< relref "Autosave" >}})
: Abrir un archivo creado por la función de autoguardado de Aegisub. Útil si Aegisub se cierra inesperadamente sin haber guardado cambios o si solo se quiere abrir una versión anterior de un archivo.

Cuando se abre un archivo de subtítulos que no se reconoce como Unicode, Aegisub intentará adivinar con qué conjunto de caracteres está codificado. Si no está seguro, le pedirá que escoja entre dos o más alternativas probables. Si el resultado se ve desordenado o incorrecto, intente volver a abrirlo con otro conjunto de caracteres.

### Formatos soportados

Aegisub admite los siguientes formatos de subtítulos:

- Advanced Substation Alpha, también conocido como SSA v4+ (.ass)
- Substation Alpha v4 (.ssa)
- [SubRip](http://zuggy.wz.cz/) Text (.srt)
- MPEG4 Timed Text (compatibilidad limitada en el mejor de los casos; roto en el peor), también conocido como ISO/IEC 14496-17, MPEG-4 Parte 17 o simplemente TTXT (.ttxt)
- MicroDVD (.sub)
- Texto simple con formato de "diálogo de guion" (vea abajo)

### Cargar subtítulos desde MKV

Es posible cargar subtítulos directamente desde archivos Matroska. Se admiten los siguientes CodecID:

- S_TEXT/UTF8 (SRT)
- S_TEXT/ASS (ASS/SSA v4+)
- S_TEXT/SSA (SSA v4)

### Importar guiones de texto simple

Aegisub también permite importar guiones de texto simple con formato de diálogo. Por ejemplo:

```plaintext
Actor 1: Bien comprendo su habla, pero pocos extraños lo hacen.
         ¿Por qué entonces no habla en la lengua común,
         como es costumbre en Occidente, si desea una respuesta?
# Verificación de traducción: Lo anterior parece ser una cita de El Señor de los Anillos. Debería buscarlo después.
Actor 2: ¿De qué locuras estás hablando?
```

Esto resultará en cinco líneas de subtítulos, una de ellas comentada. Las tres primeras tendrán el campo de actor como "Actor 1", y la quinta lo tendrá como "Actor 2" (el actor de la línea de comentario quedará en blanco).

Cuando se abre un archivo con la extensión .txt, Aegisub preguntará qué caracteres debe usar como separador de actor e inicio de comentario, respectivamente. En el ejemplo anterior, el separador de actor es el dos puntos ("`:`") y el inicio de comentario es el símbolo de almohadilla ("`#`").

## Editar subtítulos

La edición de subtítulos en Aegisub se realiza en dos áreas: la caja de edición de subtítulos (donde se ingresa y edita texto) y la cuadrícula de subtítulos. Los cambios realizados en ambas áreas generalmente afectan todas las líneas seleccionadas, no solo la línea mostrada en la caja de edición.

### La caja de edición de subtítulos

![subs_edit_box](/img/3.2/subs_edit_box.png)

La caja de edición es un área de edición sencilla con una serie de controles asociados.

1. Marca la línea como comentario. Las líneas de comentario no aparecerán en el video.
2. El [estilo]({{< relref "Styles" >}}) usado en esta línea.
3. El actor que habla esta línea. No tiene efecto en la visualización de subtítulos, pero puede ser útil para la edición.
4. Efecto aplicado a esta línea. Hay algunos efectos predefinidos que se pueden aplicar desde este campo, pero la compatibilidad con los renderizadores es inconsistente. Usar [etiquetas de formato]({{< relref "ASS_Tags" >}}) suele ser una mejor opción. Este campo se usa comúnmente como metadatos para scripts automatizados.
5. Número de caracteres en la línea más larga del subtítulo.
6. Capa de la línea. Si se usa una [etiqueta de formato]({{< relref "ASS_Tags" >}}) para superponer varias líneas, este campo controla la prioridad; los números de capa más altos se dibujan sobre los más bajos.
7. Tiempo de inicio de la línea.
8. Tiempo de finalización de la línea.
9. Duración de la línea. Si se modifica, el tiempo de finalización cambiará en consecuencia.
10. Margen izquierdo de la línea. 0 significa usar el margen especificado en el estilo.
11. Margen derecho de la línea. 0 significa usar el margen especificado en el estilo.
12. Margen vertical de la línea. 0 significa usar el margen especificado en el estilo.
13. Inserta una etiqueta de formato **negritas** (`\b1`) en la posición del cursor. Si el texto ya está en negritas, inserta una etiqueta de cierre correspondiente (`\b0`).
14. Inserta una etiqueta de formato _cursiva_ (`\i1`) en la posición del cursor. Si el texto ya está en cursiva, inserta una etiqueta de cierre correspondiente (`\i0`).
15. Inserta una etiqueta de formato _subrayado_ (`\u1`) en la posición del cursor. Si el texto ya está subrayado, inserta una etiqueta de cierre correspondiente (`\u0`).
16. Inserta una etiqueta de formato ~~tachado~~ (`\s1`) en la posición del cursor. Si el texto ya está tachado, inserta una etiqueta de cierre correspondiente (`\s0`).
17. Abre una ventana para elegir tipografía e inserta una etiqueta de nombre de fuente (`\fnFontName`) con la fuente seleccionada, además de las etiquetas de efectos elegidas.
18. Abre el [selector de color]({{< relref "Colour_Picker" >}}) y permite elegir un color; luego inserta una etiqueta de color principal (`\c`) con el color seleccionado en la posición del cursor.
19. Abre el [selector de color]({{< relref "Colour_Picker" >}}) y permite elegir un color; luego inserta una etiqueta de color secundario (`\2c`) con el color seleccionado en la posición del cursor.
20. Abre el [selector de color]({{< relref "Colour_Picker" >}}) y permite elegir un color; luego inserta una etiqueta de color de borde (`\3c`) con el color seleccionado en la posición del cursor.
21. Abre el [selector de color]({{< relref "Colour_Picker" >}}) y permite elegir un color; luego inserta una etiqueta de color de sombra (`\4c`) con el color seleccionado en la posición del cursor.
22. Pasa a la siguiente línea, creando una nueva al final del archivo cuando sea necesario. Nota: a diferencia de versiones anteriores de Aegisub, los cambios no necesitan ser confirmados con este botón.
23. Alterna entre visualización en tiempos y en fotogramas. Nota: esto no afecta cómo se almacenan los tiempos en el guion.

#### Mostrar original

Habilitar la opción "Mostrar original" cambia la caja de edición al siguiente modo:

![subs_edit_box_original](/img/3.2/subs_edit_box_original.png)

La parte superior de la caja de edición se vuelve de solo lectura y muestra el texto que tenía la línea seleccionada cuando fue elegida por primera vez. Esto puede ser útil para traducir subtítulos a otro idioma o simplemente para editarlos.

Revertir  
: Reemplaza el texto de la línea con el texto mostrado en la parte superior. Es una forma sencilla de deshacer todos los cambios realizados en la línea si cambia de opinión.

Borrar  
: Elimina la línea.

Borrar texto  
: Borra el texto de la línea pero conserva todas las etiquetas de formato. Puede ser útil para traducir carteles editados a otro idioma.

Insertar original  
: Inserta el texto original de la línea en la posición del cursor.

#### Menú contextual  

Si hace clic derecho en cualquier parte de la caja de edición, aparecerá el siguiente menú:

![Subs_Edit_Context](/img/3.2/Subs_Edit_Context.png)

Las opciones Seleccionar todo, Copiar, Cortar y Pegar funcionan como se espera.

Corrector ortográfico  
: Si hace clic derecho sobre una palabra detectada como mal escrita, el corrector ortográfico sugerirá algunas alternativas. También puede configurar el idioma de corrección desde este menú o agregar palabras desconocidas al diccionario si sabe que son correctas. Para más información sobre la corrección ortográfica en Aegisub, consulte la página del [Corrector Ortográfico]({{< relref "Spell_Checker" >}}).

Tesauro  
: Sugiere palabras alternativas similares a la palabra resaltada.

Dividir línea  
: Divide la línea en dos nuevas en la posición del cursor.  
  - "Preservar tiempos" mantiene la sincronización original para ambas líneas.  
  - "Estimar tiempos" intenta calcular la división del tiempo según la longitud del texto a cada lado del cursor.  
  - "En fotograma de video" hace que la primera mitad de la línea termine en el fotograma anterior y que la segunda mitad comience en el fotograma actual.
  
### Cuadrícula de subtítulos

![Subs_grid](/img/3.2/Subs_grid.png)

La cuadrícula de subtítulos muestra todas las líneas (comentarios y demás) en el archivo entero.

Algunos controles comunes:

- Para mover líneas arriba o abajo en la lista, selecciónelas, mantenga presionada la tecla `Alt` y use las teclas de flecha arriba o abajo.
- Para seleccionar múltiples líneas, mantenga presionada `Ctrl` o `Shift` y haga clic.  
  `Ctrl + clic` selecciona líneas individuales adicionales, mientras que `Shift + clic` selecciona todas las líneas entre la primera y la última clickeadas.
- Para cambiar la línea activa mostrada en la caja de edición sin cambiar la selección, mantenga presionada la tecla `Alt` y haga clic en la nueva línea.
- Para ordenar todas las filas de la cuadrícula, abra el menú _Subtítulos_ y, en _Ordenar líneas_, seleccione el campo por el cual ordenarlas.
- Para cambiar la manera en que se muestran las [etiquetas de formato]({{< relref "ASS_Tags" >}}) en la cuadrícula, haga clic en el botón "Recorrer los modos para ocultar etiquetas" en la barra de herramientas.

![Subs_grid_tags](/img/3.2/Subs_grid_tags.png)

Las líneas tienen diferentes colores (configurables) que representan distintos elementos; consulte la [sección de cuadrícula de subtítulos en la página de opciones]({{< relref "Options#cuadrícula-de-subtítulos" >}}) para más detalles sobre el significado de los colores.

Por defecto, las siguientes columnas son visibles:

**\#**
: El número de línea.

Inicio
: El tiempo de inicio de la línea.

Fin
: El tiempo de fin de la línea.

Estilo
: El estilo usado en esta línea.

Texto
: El texto de la línea (esto es lo que se mostrará en el video).

Las siguientes columnas se mostrarán si alguna línea del guion las usa:

L
: La capa de la línea (véase arriba).

Actor
: El actor que habla en la línea.

Efecto
: El efecto aplicado a esta línea.

Izquierda
: Margen izquierdo.

Derecha
: Margen derecho.

Vert
: Margen vertical.

También puede hacer clic derecho en la primera fila de la cuadrícula (donde aparecen los nombres de las columnas) para seleccionar manualmente cuáles desea mostrar.

Hacer clic derecho en cualquier otra línea de la cuadrícula abrirá el siguiente menú contextual (muchas de estas opciones también están disponibles en otros menús):

![grid_context_menu](/img/3.2/grid_context_menu.png)

Insertar (antes/después)
: Inserta una nueva línea vacía antes o después de la línea seleccionada. La nueva línea tendrá su tiempo de inicio en 0:00:00.00 y su tiempo de fin en 0:00:05.00.

Insertar en tiempo de video (antes/después)
: Lo mismo que la opción anterior, pero la nueva línea comenzará en el fotograma actual del video. No estará habilitado a menos que haya un video cargado.

Duplicar
: Duplica las líneas seleccionadas.

Dividir líneas antes del fotograma actual
: Duplica las líneas seleccionadas, establece el tiempo de fin de la línea original en el fotograma anterior al fotograma actual del video y ajusta el tiempo de inicio de la copia al fotograma actual.  
  Útil para edición de carteles fotograma por fotograma y para dividir una línea en un cambio de escena, evitando que colisione con una línea ya no visible. Solo se habilita si hay un video cargado.

Dividir líneas después del fotograma actual
: Similar a la opción anterior, pero separa la parte de la línea después del fotograma actual en lugar de la parte anterior. Útil cuando se hace edición de carteles fotograma por fotograma desde el último hasta el primer fotograma de una línea.

Dividir (por karaoke)
: Divide la línea en varias líneas, una por cada sílaba, de acuerdo con las etiquetas de formato de karaoke (`\k` y sus variantes).  
  La primera línea comenzará en el tiempo de inicio original y durará hasta que termine la primera sílaba; las siguientes líneas comenzarán donde terminó la anterior y durarán según el tiempo de cada sílaba.

Intercambiar
: Intercambia las posiciones de dos líneas seleccionadas en la cuadrícula.

Unir (mantener primera)
: Combina dos o más líneas, descartando el texto de todas excepto la primera.  
  La nueva línea tendrá su tiempo de inicio en el de la primera línea y su tiempo de fin en el de la última línea.  
  Solo se habilita si hay más de una línea seleccionada.

Unir (concatenar)
: Similar a la opción anterior, pero concatena el texto de todas las líneas seleccionadas. Se inserta un espacio entre el texto de cada línea.

Unir (como karaoke)
: Realiza la operación inversa a _Dividir (por karaoke)_: concatena el texto de las líneas seleccionadas e inserta etiquetas `\k` con el tiempo de cada línea en la línea final.

Hacer tiempos continuos (cambiar inicio/cambiar fin)
: Ajusta los tiempos de las líneas seleccionadas para que el tiempo de fin de cada línea sea igual al tiempo de inicio de la siguiente.  
  "Cambiar inicio/cambiar fin" determina si la función ajusta los tiempos de inicio o de fin de cada línea.  
  Solo se habilita si hay más de una línea seleccionada.

Recombinar líneas
: Si hay dos o más líneas con el mismo texto parcialmente presente en todas, las combina en líneas separadas por fragmento de texto.  
  Esta función es útil para corregir subtítulos extraídos de DVDs, que a menudo se ven así:

  ![Recombine_01](/img/3.2/Recombine_01.png)

  Después de recombinar las líneas, el resultado será:

  ![Recombine_02](/img/3.2/Recombine_02.png)

Crear clip de audio
: Guarda un segmento del audio cargado correspondiente al tiempo de las líneas seleccionadas (desde el tiempo de inicio más temprano hasta el tiempo de fin más tardío) como un archivo WAV sin comprimir.  
  Solo se habilita si hay audio cargado.

Cortar/Copiar/Pegar
: Corta, copia o pega líneas completas.  
  Tenga en cuenta que las líneas se copian como texto sin formato y pueden pegarse libremente en editores de texto, programas de chat, navegadores web, otras instancias de Aegisub, etc.

Pegar líneas sobre...
: Abre la ventana de [Pegar sobre]({{< relref "Paste_Over" >}}).

Eliminar
: Elimina las líneas seleccionadas.
