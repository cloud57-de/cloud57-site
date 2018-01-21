---
title: "Dr Todo Little"
date: 2018-01-20T23:07:10+01:00
image: "img/drtodolittle.png"
external_link: ""
weight: 5
draft: false
---
[Dr ToDo Little](https://www.drtodolittle.de) ist eine einfache ToDo List implementiert mit AngularJS, Apache Camel, Apache Groovy und Camunda.

Die Applikation besteht aus mehreren Teilen. Der Client ist
eine Single Page Application (SPA) implementiert mit AngularJS. Die App ist responsive und passt sich somit dem entsprechenden Anzeigegerät an.

Die Benutzung der Applikation ist extrem einfach gehalten. Ziel ist es keine überladene ToDo Liste mit möglichst vielen Funktionalitäten zu erstellen, sondern sie soll in aller erster Linie einfach zu bedienen sein.

Auf dem Server wird Spring Boot mit Apache Camel verwendet. Die REST Schnittstelle ist mit der CAMEL REST DSL erstellt. Die übergeben Daten werden mit Hilfe von Camel Prozessoren, die in Groovy implementiert sind, transfomiert und dann an eine Camunda BPM Engine weitergeleitet.

Die Persistierung und die Verwaltung der ToDo Liste erfolgt in Camunda.

[Dr ToDo Little Website](https://www.drtodolittle.de)
