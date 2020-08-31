# Responsive Web Design

## Inhalt

* [Intro](#intro)
* [Der Viewport](#der-viewport)
* [Media-Queries](#media-queries)
* [Testing](#testing)

## Setup

Für Übungen nutzen wir die folgende **CodeSandbox** als Startpunkt:

**[https://codesandbox.io/s/TODO](https://codesandbox.io/s/TODO)**

Die Übungen bauen immer aufeinander auf. Aber keine Angst! Für den Fall, dass bei einer Übung etwas nicht klappt, gibts bei jeder Übung einen Link zur CodeSandbox mit dem aktuellen Stand.

## Intro

Man kann nicht wissen, von welchem Device ein User auf eine Website geht, daher ist es wichtig, dass Websites responsive sind, sodass die Website sich dem Device anpassen kann um dem User immer eine möglichst gute User Experience zu bieten. Dies nennt man Responsive Web Design (RWD).

TODO Image

## Der Viewport

Damit eine Website responsive wird, wird vor allem eines benötigt. Dem Browser muss mitgeteilt werden, dass die Website auf die er zugreift auch wirklich responsive ist.
Dafür gibt es einen `meta`-Tag, den man nutzen kann.

**Beispiel**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### Verfügbare Attribute

|Attribut|Werte|Beschreibung|
|---|---|---|
|`width`|Positiver Integer oder der Wert `device-width`|Definiert die Breite des Viewports in `px`|
|`height`|Positiver Integer oder der Wert `device-height`|Definiert die Höhe des Viewports in `px`|
|`initial-scale`|Positiver Nummer zwischen `0.0` und `10.0`|Definiert die initiale Verhältnis zwischen Device und Viewport|
|`minimum-scale`|Positiver Nummer zwischen `0.0` und `10.0`|Definiert den minimalen Zoom|
|`maximum-scale`|Positiver Nummer zwischen `0.0` und `10.0`|Definiert den maximalen Zoom|
|`user-scalable`|`yes` (default) oder `no`|Bei `no` kann der User die Website nicht zoomen|

**Achtung Antipattern** 🚫

`user-scalable=no` anzugeben gilt als anitpattern, da User mit einer Sehschwäche die Möglichkeit genommen wird, die Website zu zoomen.  
`maximum-scale` sollte mindestens auf `5.0` (empfohlen von Google) gesetzt werden aus dem gleichen Grund.

**Beispiel mit HTML-Grundgerüst**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Beispiel mit HTML-Grundgerüst</title>
  </head>
  <body>
    <h1>Hello World</h1>
  </body>
</html>
```

**Demo** 🤯

- [TODO](https://codesandbox.io/s/TODO)

**Einschub: CSS Pixel und Device Pixel** 👀

Mitlerweile haben fast alle mobilen Geräte ein Pixelratio von mehr als 1:1. Das heisst grundsätzlich, dass 1 CSS Pixel 'mehr' ist als nur 1 Device Pixel.  
Ein iPhone 6 hat eine native Auflösung von `750px` x `1334px`, aber es besitzt ein Pixelratio von 2:1 (retina Display). Im Browser haben wir aber 'nur' `375px` Breite zur Verfügung. Das Betriebsystem gibt dann das Pixelratio vor, welches dann die CSS Pixel berechnet und diese Info an den Browser weitergibt. **Grundsätzlich sind alle Units die eine Weite beschreiben auf die CSS Pixel bezogen, und nicht auf die Device Pixel**.

**Hilfreiche Links**

* [TODO Liste von hilfreichen links device pixel](TODO)

## Media-Queries

### Vorwort

Grundsätzlich gibt es fürs RWD zwei Grundprinzipien.

1. Im Layout sollte vermehrt mit `%` gearbeitet werden
2. Mit **Media-Queries** ermöglichen wir, dass wir CSS-Deklarationen abhängig von bestimmten Kriterien wieder überschreiben können

### Syntax

Ein Media Query beinnt mit einem `@media`, auch 'at-rule' genannt. Danach folgen ein optionaler `media-type` und null bis mehrere `media-feature`.

```css
@media [media-type] [and [(media-feature)]] {
  /*...*/
}
```

**Beispiel**

```css
@media screen and (min-width: 600px) {
  /*...*/
}
```

Was dies genau heisst ist: "Wenn wir uns auf einem `screen` befinden, und dieser mindestens `600px` breit ist, wende das folgende CSS an"

`media-type` und `media-feature` sind beide optional, das heisst, dass die folgenden Media-Queries beide valid sind.

```css
@media screen {
  /*...*/
}
```

```css
@media (min-width: 600px) {
  /*...*/
}
```

### Media Types

TODO

### Media Features

Fürs Responsive Web Design werden vor allem das *dimension* Feature genutzt.

#### `width` und `height`

Es können die exakte `width`, `min-width` und `max-width`, und die exakte `height`, `min-height` und `max-height`.  
Am meisten werden jeweils `min-*` und `max-*` genutzt.

**Beispiele**

```css
@media (min-width: 600px) {
  /*...*/
}

@media (max-height: 1000px) {
  /*...*/
}
```

**Best Practises** ✅

Alle *dimension* Features unterstützen die regulären CSS Units wie `px`, `em`, usw. Es ist jedoch empfohlen, dass man `em` nutzt innerhalb von Media-Queries.
`em` Values skalieren mit, wenn der User im Borwser zoomt. `rem` hat in einem Mequa Query immer dieselbe Value wie `em`, aber `rem` wird nicht korrekt utnerstützt im Safari

**Hilfreiche Links**

* [Weitere Media Features](TODO)

### Logische Operatoren

#### Der `and` Operator

Man kann den `and` Operator zwischen verschiedenen `media-type` und `media-feature` nutzen, um diese beliebig zu verbinden.

**Beispiele**

```css
@media screen and (min-width: 600px) {
  /*...*/
}

@media (min-width: 400px) and (max-width: 800px) {
  /*...*/
}

@media screen and print and (min-width: 800px) and (max-width: 1200px) {
  /*...*/
}
```

#### Der **oder** Operator: `,` (ein Komma)

Der oder Operator wird genutzt um mehrere Media-Queries zu deklarieren, die das gleiche CSS anwenden. Jeder vom `,` getrennte Wert ist dabei ein eigenständiger Media Query und sobald ein Media Query davon zutrifft, wird das CSS angewendet.

**Beispiele**

```css
@media screen, print {
  /*...*/
}

@media screen and (min-width: 1200px), (max-width: 800px) {
  /*...*/
}

@media screen and (max-width: 400px), (min-width: 800px) and (max-width: 1200px) {
  /*...*/
}
```

#### Der `not` Operator

Mit dem `not` Operator kann man einen **ganzen** Media Query negieren. Solange dieser nicht zutrifft, wird das CSS angewendet.
Mit diesem Operator muss ein `media-type` angegeben werden, damit dieser valid ist

**Beispiel**

```css
@media not screen {
  /*...*/
}

@media not all and (max-width: 800px) {
  /*...*/
}
```

### Nesting

Media-Queries können auch ineinander verschachtelt werden.

**Beispiel**

```css
/* Anstatt */
@media (min-width: 800px) and (max-width: 1200px) {
  /*...*/
}

/* Kann dies auch so geschrieben werden */
@media (min-width: 800px) {
  @media (max-width: 1200px) {
    /*...*/
  }
}
```

> **Note:** Das verschachteln von Media-Queries wird grundsätzlich nicht genutzt, wird aber von Browsern unterstützt

### Media-Queries im HTML

In einem `<link>` können Media-Queries ebenfalls verwendet werden.

**Beispiel**

```html
<link href="style.css" rel="stylesheet" media="screen and (min-width: 400px)" />
```

**Achtung Antipattern** 🚫

Dies ist zwar möglich, sollte aber trotzdem nicht verwendet werden. Jede verlinkte CSS-Datei wird heruntergeladen, egal ob der Media Query zutrifft oder nicht. Der Media Query wird dabei erst evaluiert, wenn die Datei heruntergeladen ist.

### Usage

TODO

### Practice 🔥

Öffne diese [**CodeSandbox**](https://codesandbox.io/s/TODO) als Startpunkt.

- [ ] TODO

Zeit: ~ TODO min

**Solution**: [https://codesandbox.io/s/TODO](https://codesandbox.io/s/TODO)

## Testing

Die einfachste Form vom Testing von Responsive Websites ist den Browser kleiner und grösser zu machen.

![Responsive testing](./assets/responsive-testing.png)

Die effektivste Form vom Testing von Responsive Websites ist jedoch mit dem Dev-Tools. Dort kann man den Device-Modus aktivieren um div. Sachen nachzustellen.  
Custom `width` und `height` sind nur die Oberfläche, man kann damit ein Geräte simulieren, das Pixelratio verändern und noch vieles mehr.

![Responsive testing devtools](./assets/responsive-testing-devtools.png)
_Beispiel: Chrome Dev-Tools_

### Practice 🔥

Öffne diese [**CodeSandbox**](https://codesandbox.io/s/TODO) als Startpunkt.

- [ ] Simuliere ein iPhone10
- [ ] Simuliere mit den Dev-Tools ein Pixelratio von mindestens 3.0, damit der Hintergrund der Website zu Grün wird
- [ ] Schalte ein, dass alle Media-Queries angezeigt werden

Zeit: ~ 5 min

## Weiteres zum Thema

* [What is Mobile First Design?](https://medium.com/@Vincentxia77/what-is-mobile-first-design-why-its-important-how-to-make-it-7d3cf2e29d00)
* [Complete Guide to Responsive Images!](https://medium.com/@elad/a-complete-guide-for-responsive-images-b13db359c6c7)
* [Accessible, Simple, Responsive Tables](https://css-tricks.com/accessible-simple-responsive-tables/)
* [Responsive web design tricks and tips](https://webflow.com/blog/responsive-web-design-tricks-and-tips)