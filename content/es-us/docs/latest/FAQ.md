---
title: Preguntas frecuentes
menu:
  docs:
    parent: introduction
weight: 2600
---

Una pequeña colección de Preguntas Frecuentes sobre Aegisub; en su mayoría cosas que no encajan en ningún otro lugar.

### ¿Efectos de karaoke?

Consulta los [tutoriales de Karaoke Templater]({{< relref "Automation/Karaoke_Templater/Tutorial_1" >}}).

### ¿Puedo crear subtítulos para DVD con Aegisub?

No directamente, pero hay un programa llamado [MaestroSBT](http://sourceforge.net/projects/maestrosbt/) que puede convertir SSA a VOBSubs. Tiene varias restricciones sobre qué etiquetas y otros elementos pueden usarse, por lo que se recomienda leer primero su manual. También ten en cuenta que no acepta archivos ASS, solo SSA. Puedes usar la opción **Archivo -> Exportar...** en Aegisub para guardar archivos SSA reales.

### ¿Aegisub permite guardar en SRT?

Sí, pero solo si no se pierde información. En otras palabras, si tienes etiquetas de formato que no sean `\1c`, `\b` o `\i`, Aegisub no permitirá guardar directamente en SRT. Sin embargo, aún puedes exportar a SRT utilizando la opción **Archivo -> Exportar...**, simplemente desmarcando todas las casillas (información del script limpio, transformación VFR, etc.).

### ¡He encontrado un error!

Repórtalo en el [rastreador de errores](http://devel.aegisub.org/). ¡Incluye la mayor cantidad de detalles posible en tu informe! Recuerda que si un error no está en el rastreador de errores, _para nosotros no existe_.

### ¿Por qué Aegisub no tiene \<característica X>? ¡\<Programa Y> sí la tiene!

Posiblemente porque no sabíamos que la querías. Solicítala en el [rastreador de errores](http://devel.aegisub.org/) y ve qué sucede.

### ¿Dónde puedo encontrar más información o pedir ayuda?

Para temas relacionados con Aegisub, los [foros](http://forums.aegisub.org) y el [canal de IRC](irc://irc.rizon.net/aegisub) son buenos lugares para hacer preguntas. La [wiki de desarrollo de Aegisub](http://devel.aegisub.org) también contiene información más detallada que no está en el manual por diversas razones, al igual que los foros. Para preguntas generales sobre video, [Doom9.org](http://www.doom9.org) y sus [foros](http://forum.doom9.org) suelen ser el mejor lugar para consultar.

### ¿Hay errores en VSFilter que debería conocer?

En una palabra: [sí](https://web.archive.org/web/20110811220802/http://asa.diac24.net/VSFilter#BUGS).

