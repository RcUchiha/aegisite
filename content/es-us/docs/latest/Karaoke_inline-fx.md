---
title: Tutorial de efectos de línea
menu:
  docs:
    parent: tutorials
weight: 2720
---

Efectos de línea para karaoke es una forma de marcar [karaoke sincronizado]({{< relref "Timing#karaoketiming" >}}) para asignar distintos efectos a diferentes partes de una línea.

Por sí mismo, el marcado de efectos de línea no hace nada, solo tiene efecto cuando se aplica un [script de efectos de karaoke]({{< relref "Automation" >}}) que lo interprete en el karaoke sincronizado.

## El marcado

Las etiquetas de efectos de línea son etiquetas ASS de formato de la forma `\-nombreefecto`, donde _nombreefecto_ es el nombre del efecto de línea definido.

Al igual que las etiquetas de formato normales, una etiqueta de efecto de línea afecta a la sílaba en la que está colocada y a todas las sílabas siguientes, hasta la siguiente sílaba con una nueva etiqueta de efecto de línea.

Al inicio de cada línea, el efecto de línea se restablece a vacío.

{{<example-box>}}
Aquí hay una línea de karaoke sincronizado con marcado de efecto de línea:

```ass
{\k40}zu{\k20}t{\k42}to {\k32\-color}e{\k17}ga{\k45}i{\k32}te{\k26}ta {\k24\-sombra}yu{\k55}me
```

Estas sílabas reciben los siguientes efectos de línea:

| Sílaba           | FX de línea |
| ---------------- | ----------- |
| zu               | (vacío)     |
| t                | (vacío)     |
| to               | (vacío)     |
| e                | `color`     |
| ga               | `color`     |
| i                | `color`     |
| te               | `color`     |
| ta               | `color`     |
| yu               | `sombra`    |
| me               | `sombra`    |

{{</example-box>}}

## Uso en el Karaoke Templater

Si utilizas [Karaoke Templater]({{< relref "Automation/Karaoke_Templater" >}}) para crear efectos, puedes usar el modificador _fx_ en las plantillas para hacer que una plantilla afecte solo a las sílabas con un efecto de línea específico. No es posible (directamente) hacer coincidir solo sílabas con efecto de línea vacío.

{{<example-box>}}
Con el karaoke sincronizado del ejemplo anterior, podrías tener las siguientes plantillas:

```plaintext
template syl: {efecto base aplicado a todas las sílabas}
template syl fx color: {efecto adicional aplicado solo a las sílabas con el efecto 'color'}
template syl fx sombra: {efecto adicional aplicado solo a las sílabas con el efecto 'sombra'}
```

La idea aquí es tener un efecto base y luego aplicar efectos adicionales a algunas sílabas específicas.

{{</example-box>}}

{{<example-box>}}
Es posible hacer coincidir solo sílabas con efecto de línea vacío en Karaoke Templater utilizando un _fxgroup_ que habilite o deshabilite los efectos según el efecto de línea. También puedes usar _fxgroup_ para aplicar plantillas a múltiples efectos de línea.

```plaintext
code syl: fxgroup.blankfx = (syl.inline_fx == "")
template syl fxgroup blankfx: {efecto aplicado solo a sílabas sin efecto de línea}
```

Lo importante es que la línea de código se ejecute por sílaba y antes de cualquier plantilla por sílaba que la necesite.

{{</example-box>}}

## Uso en scripts de Lua

Las etiquetas de efecto de línea son analizadas por [`karaskel.preproc_line_text`]({{< relref "Automation/Lua/Modules/karaskel.lua.md#karaskel.preproc_line_text" >}}), por lo que solo funcionarán si has aplicado al menos ese nivel de preprocesamiento de karaskel en tus líneas de subtítulo.

El efecto de línea de una sílaba está disponible como `syl.inline_fx`, lo que permite compararlo con una cadena de texto para aplicar efectos de manera condicional.

{{<example-box>}}
En un código que se ejecuta por sílaba en tu script:

```lua
if syl.inline_fx == "" then
    apply_base_effect(subs, meta, line, syl)
elseif syl.inline_fx == "color" then
    apply_paint_effect(subs, meta, line, syl)
elseif syl.inline_fx == "sombra" then
    apply_cloud_effect(subs, meta, line, syl)
end
```

Simplemente compara el nombre del efecto de línea con las diferentes opciones y ejecuta el código del efecto correspondiente.

{{</example-box>}}

{{<example-box>}}
En un código que se ejecuta por sílaba en tu script:
A nivel superior de tu script:

```lua
effects = {}
effects[""] = function(subs, meta, line, syl)
    -- código del efecto base aquí
end
effects.paint = function(subs, meta, line, syl)
    -- código del efecto 'color' aquí
end
effects.cloud = function(subs, meta, line, syl)
    -- código del efecto 'sombra' aquí
end
```

Luego, en algún código de procesamiento por sílaba:

```lua
effects[syl.inline_fx](subs, meta, line, syl)
```

Primero, se crea una tabla con funciones que aplican los distintos efectos. Las claves de la tabla son los nombres de los efectos de línea posibles. Cuando debe aplicarse un efecto, se busca la función correspondiente en la tabla y se ejecuta.

{{</example-box>}}
