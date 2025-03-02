---
title: Tutorial de sincronización de karaoke
menu:
  docs:
    parent: tutorials
weight: 2710
---

Este tutorial te enseñará cómo cargar una canción en Aegisub, cómo ingresar la letra de la canción y cómo agregar códigos de tiempo a las palabras para sincronizarlas con la canción.

No necesitas haber usado Aegisub antes para seguir este tutorial.

## Antes de comenzar

Hay algunas cosas que necesitas tener listas antes de empezar:

- La canción en sí. Puede ser un archivo MP3 o estar dentro de un video. Aegisub puede leer el audio de archivos de video, por lo que no es necesario crear un archivo de sonido separado si la canción está en un video.

<div></div>

- La letra de la canción. Es más fácil si la tienes en un archivo de texto plano (.txt) dividido en versos y estrofas.

<div></div>

Estoy usando una canción en inglés para esta demostración, pero muchas de las funciones avanzadas de Aegisub están diseñadas para canciones en japonés y otros idiomas que a menudo necesitan transcripción o transliteración al alfabeto latino. Mostraré cómo usar esas funciones en un tutorial en video.

## Cargar la canción

Comenzaremos creando un archivo nuevo. Ya lo tienes si acabas de iniciar Aegisub.

![Karatiming-1](/img/3.2/Karatiming-1.png)

Ahora abre tu canción. Selecciona **Abrir audio** en el menú **Audio**...

![Karatiming-2](/img/3.2/Karatiming-2.png)

... luego selecciona tu archivo de canción.

![Karatiming-3](/img/3.2/Karatiming-3.png)

Aegisub tomará un momento para leer el archivo de audio.

![Karatiming-4](/img/3.2/Karatiming-4.png)

Cuando termine, deberías ver la forma de onda del audio en la parte superior de la ventana de Aegisub. Si has usado Aegisub antes, puede que las cosas se vean un poco diferentes. Puede ser más fácil seguir el resto de este tutorial si ajustas la apariencia a la de esta imagen.

![Karatiming-5](/img/3.2/Karatiming-5.png)

Veremos cómo usar la visualización de audio para sincronizar en un momento, pero primero carguemos la letra de la canción.

### Consejos

**Cargar audio directamente desde archivos de video:** Puedes seleccionar archivos de video en el selector de archivos de audio. Esto no abrirá el video, solo leerá el audio del archivo de video, como si fuera un archivo de audio independiente.

**Carga instantánea de archivos WAV:** Si tienes un archivo WAV PCM sin comprimir, Aegisub puede abrirlo instantáneamente sin cargarlo completamente en memoria. Esto puede ahorrar tiempo, pero requiere espacio adicional en disco y, probablemente, trabajo previo para crear el archivo WAV. (Recuerda que esto solo funciona con archivos _PCM sin comprimir_. Formatos como ADPCM o MP3 dentro de archivos WAV no funcionarán y seguirán requiriendo precarga.)

## Ingresar la letra

Ahora, para agregar el texto, podríamos simplemente escribirlo...

![Karatiming-6](/img/3.2/Karatiming-6.png)

**¡Pero no hagas eso!** Será mucho más eficiente si lo tienes en un archivo de texto, lo copias desde allí y lo pegas en Aegisub. (A menudo, también puedes copiar y pegar directamente desde tu sitio web de letras favorito.)

Tengo la letra en un archivo de texto, así que lo abro, selecciono el texto y lo copio al portapapeles.

![Karatiming-7](/img/3.2/Karatiming-7.png)

Ahora las cosas se complican un poco, pero no te preocupes, no es difícil :-)

Hay dos lugares donde puedes pegar en Aegisub: la cuadrícula de subtítulos y la caja de edición de subtítulos. Si pegas en la cuadrícula de subtítulos, creas nuevas líneas en el archivo de subtítulos. Si pegas en la caja de edición de subtítulos, cambias la línea de subtítulo seleccionada actualmente.

Queremos asegurarnos de pegar en la cuadrícula de subtítulos, así que haz clic una vez dentro del área de la cuadrícula (en la parte inferior de la ventana) para establecer el foco de entrada allí.

![Karatiming-8](/img/3.2/Karatiming-8.png)

Ahora podemos pegar la letra.

![Karatiming-9](/img/3.2/Karatiming-9.png)

Deberían aparecer inmediatamente como líneas en la cuadrícula. Nota que todas tienen tiempos de inicio y fin en cero. Esto hace que sea más fácil cuando vayamos a sincronizar cada línea de la letra con la canción.

![Karatiming-10](/img/3.2/Karatiming-10.png)

Podría ser una buena idea guardar tu archivo ahora, para que más tarde puedas guardarlo fácilmente sin necesidad de asignarle un nombre.

Recuerda que Aegisub guarda automáticamente una copia de tu archivo cada minuto, incluso si aún no le has dado un nombre, por lo que rara vez perderás mucho trabajo si algo sale mal.

![Karatiming-11](/img/3.2/Karatiming-11.png)

Ahora estamos listos para sincronizar las líneas individuales de la letra.

## Sincronización aproximada: primero las líneas

Antes de empezar con la sincronización, debes saber que la forma presentada aquí es solo una de muchas. Hay varias maneras de sincronizar con audio en Aegisub, y esta puede que no sea la mejor para ti. Explora el programa y encuentra la que mejor se adapte a tu flujo de trabajo. Esta es solo la manera en que yo (jfs) lo hago habitualmente.

Primero veamos cómo navegar en la visualización de audio y reproducir el sonido. Probablemente ya notaste que hay al menos 6 botones de "Reproducir". Sin embargo, generalmente solo usarás uno: el que tiene los corchetes azules apuntando hacia afuera. Ese es el botón **Reproducir selección**, que reproduce la parte del audio que está actualmente resaltada.

![Karatiming-12](/img/3.2/Karatiming-12.png)

Intenta presionar el botón **Reproducir selección**, deberías escuchar los primeros 5 segundos de la canción. (Aegisub selecciona los primeros 5 segundos por defecto.)

Ahora intenta cambiar la selección: puedes hacer clic izquierdo y arrastrar en la visualización de audio para seleccionar una parte. Si haces clic y arrastras en el borde izquierdo o derecho de la selección, puedes cambiar solo el inicio o el final. También puedes hacer un solo clic izquierdo en cualquier parte para establecer el inicio de la selección en ese punto, y un clic derecho para establecer el final.

Vamos a sincronizar la primera línea. Encuentra el inicio y el final de la primera línea de la canción y asegúrate de que la selección de audio coincida exactamente. Al principio, la selección será gris, pero en cuanto la modifiques, se volverá roja y aparecerá la palabra "Modificado". Esto significa que has cambiado la selección pero no has guardado (confirmado) el nuevo tiempo.

Para confirmar la sincronización y guardarla en la línea de subtítulos, simplemente presiona el botón **Confirmar**, el de la marca de verificación verde.

![Karatiming-13](/img/3.2/Karatiming-13.png)

Cuando confirmes, pasarás automáticamente a la siguiente línea, para que puedas continuar sincronizando de inmediato.

Continúa sincronizando de esta manera hasta completar todas las líneas de la canción: encuentra el inicio y fin de cada línea, ajusta la selección y luego confírmala.

Cuando termines, guarda el archivo.

### Consejos

Sincronizar desde el audio no es difícil en absoluto, pero aquí tienes algunos consejos para hacerlo aún más fácil y rápido.

**Atajos de teclado:** Hay varias combinaciones de teclas que pueden hacer que la sincronización de audio sea mucho más rápida.

![Karatiming-14](/img/3.2/Karatiming-14.png)

Las más importantes son:

- **S** - Reproducir selección: Reproduce el fragmento de audio actualmente seleccionado.
- **A** y **F** - Desplazarse a la izquierda y derecha: Cambia la parte visible del audio.
- **G** - Confirmar: Copia los tiempos de inicio y fin de la selección actual de audio a la línea seleccionada en la cuadrícula de subtítulos y avanza a la siguiente línea.

<div></div>

**Reproducir cerca del inicio/final:** Hay cuatro botones (atajos de teclado **Q, W, E y D**) que reproducen medio segundo justo antes o después del inicio y el final de la selección. Puedes usarlos para ajustar con mayor precisión el inicio y el final exactamente donde comienza o termina el canto.

**Cambiar la selección mientras se reproduce:** Mientras el audio se está reproduciendo, aún puedes modificar la selección. No notarás una diferencia si cambias el inicio de la selección, pero si cambias el final, la reproducción terminará cuando llegue al nuevo final de la selección. Esto permite detener rápidamente la reproducción colocando el final cerca del cursor de reproducción (la línea blanca que se mueve cuando Aegisub está reproduciendo) o extender la reproducción aún más.

Por ejemplo, cuando buscas el inicio de la primera línea, puedes comenzar la reproducción con la selección inicial de 5 segundos e ir extendiéndola hasta encontrar la línea. Luego, mientras el audio sigue reproduciéndose, puedes establecer el inicio correcto y luego el final. Una vez que tengas la línea aproximadamente ajustada, puedes verificarla reproduciendo toda la selección nuevamente o utilizando las teclas Q/W/E/D para reproducir los segmentos justo alrededor de los tiempos de inicio y final.

**Modo espectro:** Normalmente, la visualización de audio está en modo de forma de onda, que es lo que se ha mostrado en las capturas de pantalla hasta ahora. Sin embargo, Aegisub tiene una forma mucho más avanzada de mostrar el audio: el modo espectro.

![Karatiming-15](/img/3.2/Karatiming-15.png)

El modo espectro consume más CPU y RAM que el modo de forma de onda, pero proporciona una mejor representación visual del audio. Con algo de práctica, puedes aprender a distinguir el canto de la música e incluso reconocer diferentes sonidos. Por ejemplo, los sonidos **S** son muy fáciles de identificar.

**Zoom y escala:** Puedes usar las barras deslizantes en el extremo derecho de la visualización de audio para acercar y alejar el audio, así como para ajustar el volumen.

## Sincronización precisa: primero las palabras, luego las sílabas

{{<todo>}}
Hacer clic en el botón **Karaoke**.<br>
Sincronizar palabras.<br>
Hacer clic en el botón **Dividir**. Colocar marcadores de división. Hacer clic en el botón **Aceptar división**.<br>
Sincronizar sílabas.<br>
Confirmar.<br>
Repetir.
{{</todo>}}

## Estilizado

{{<todo>}}Agregar información sobre estilos, cómo se ve un karaoke básico y los efectos \kf y \ko.{{</todo>}}

## Finalizando

{{<todo>}}Mencionar nuevamente el tutorial en video y señalar otros temas relevantes.{{</todo>}}
