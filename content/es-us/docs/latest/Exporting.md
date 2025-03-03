---
title: Exportación de subtítulos
menu:
  docs:
    parent: working-with-subtitles
weight: 3100
---

Además de las funciones normales de "Guardar" y "Guardar como", Aegisub también tiene una función de "Exportar", que permite transformar todo el guion a través de varios filtros de exportación. Esto se usa para diversas tareas, que van desde conversiones de velocidad de fotogramas y generación de efectos de karaoke hasta simplemente guardar en otros formatos y/o conjuntos de caracteres.

## La ventana de exportación

![Export](/img/3.2/Export.png#center)

La mitad superior de la ventana contiene los filtros disponibles. Marcar uno o más de ellos los aplicará en el orden en que aparecen en la lista; puedes usar los botones de mover arriba/abajo para cambiar el orden. La mitad inferior contiene una breve descripción del filtro seleccionado.

Algunos filtros tienen parámetros de configuración; en esos casos, la ventana se extenderá hacia la derecha para mostrar los controles de configuración correspondientes.

El menú desplegable en la parte inferior controla la codificación de texto que se utilizará para el archivo exportado. Esto puede ser útil para exportar a programas antiguos que no admiten Unicode.

Cuando hagas clic en el botón "Exportar", ten en cuenta que puedes elegir otros formatos además de ASS para guardar el archivo. También recuerda que esto casi siempre significa que muchas etiquetas de formato serán eliminadas.

## Filtros

Los siguientes filtros están disponibles en la instalación predeterminada:

### Limitar a líneas visibles

Exporta solo las líneas que son visibles en el fotograma de video activo. No tiene efecto si no hay video cargado. Los encabezados del guion y los estilos también se exportan.

### Plantilla de karaoke

Filtra el guion a través del script de automatización "karaoke templater" para generar efectos de karaoke. Consulta las páginas del [karaoke templater]({{< relref "Automation/Karaoke_Templater" >}}) y la [visión general de la automatización]({{< relref "Automation" >}}) para más detalles.

### Transformar velocidad de fotogramas

En el modo de salida "constante", recalcula todas las marcas de tiempo en el guion (incluidas las que están en [etiquetas de formato]({{< relref "ASS_Tags" >}})) para que funcionen con una nueva velocidad de fotogramas. Esto significa que todo el guion se "acelerará" o "ralentizará". Se puede utilizar para conversiones de NTSC a PAL o viceversa.

En el modo de salida "variable", se usa la velocidad de fotogramas del video cargado (o la especificada, si es diferente) y los códigos de tiempo cargados para recalcular todas las marcas de tiempo del guion. Esto permite que los subtítulos exportados puedan ser incrustados en el video y sigan sincronizados incluso después de aplicar los códigos de tiempo. No tiene efecto si no hay códigos de tiempo cargados. Consulta la sección sobre [video de velocidad de fotogramas variable]({{< relref "Video#variableframeratevideo" >}}) para más detalles.

### Limpiar etiquetas

Filtra el guion a través del script de automatización "clean tags", que intenta limpiar los bloques de etiquetas de formato concatenando bloques adyacentes y eliminando etiquetas redundantes (más específicamente, la segunda instancia de etiquetas que solo pueden especificarse una vez por línea).

### Limpiar información del guion

Limpia los encabezados del guion eliminando todas las líneas que no sean absolutamente esenciales para la visualización correcta del guion. Si eres paranoico con la privacidad, puedes considerar usar esta opción para los guiones que planeas distribuir en su forma original, ya que Aegisub almacena en los encabezados información como la ruta del último video/audio abierto.

### Corregir estilos

Recorre todas las líneas del guion y verifica qué estilo usan; cualquier línea que utilice un estilo que no esté disponible en el guion actual será reemplazada por "Default".
