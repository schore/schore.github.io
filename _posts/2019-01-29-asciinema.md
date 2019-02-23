---
layout: page
date: 2019-01-29 
title: Asciinema
excerpt_separator: <!--more-->
keywords: asciinema,gif,asciicast2gif
---
In diesem Post geht es um diese coolen Screencasts. Diese habe ich mit dem
Tool [Asciinema](https://asciinema.org/) und 
[asciicast2gif](https://github.com/asciinema/asciicast2gif) erzeugt.

![demo]({{site.baseurl}}/asset/demo.gif)

<!--more-->
## Installation

Das ganze ist absolut einfach zu installieren. Ich musste nur ein einziges Programm auf meinen
Laptop installieren, um dieses Tool zu nutzen und anschließen daraus ein Gif zu erzeugen. 
Darüber hinaus wird nur noch Docker benötigt.

```shell
sudo dnf install asciinema
```

## Aufnahme

Ein Aufnahme zu starten ist ebenfalls extrem einfach. Um das ganze dann Aufzunehmen und zu 
umwandeln braucht man noch folgende Kommandos.

```shell
asciinema rec test.cast
```

Die Anwendung ist ab diesen Zeitpunkt selbsterklärend. Einfach die zu aufzeichnenden Befehle eingeben
und mit <ctrl-d> die Aufnahme beenden. Das übersetzen erfolgt mit dem folgenden Kommando.

```shell
docker run --privileged --rm -v $PWD:/data asciinema/asciicast2gif -s 4 -S 1 -h20 -w80 test.cast asset/demo.gif
```

Beim ersten ausführen von asciicast2gif wird das benötige Image automatisch herunter geladen.
Dies ermöglicht ein sehr einfaches Setup.

## Fazit

Eine absolut einfache und auch runde Möglichkeit, die Konsolenausgaben zu visualisieren. Vielleicht
finde ich weitere Möglichkeiten dieses Tool zu nutzen, um diesen Block zu schreiben. Es ist auf jeden
Fall ein schönes Tool welches sicher Tech Blogs wie dieses aufpeppen kann.
