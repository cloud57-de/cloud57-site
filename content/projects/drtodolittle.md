---
title: "Dr Todo Little"
date: 2018-01-20T23:07:10+01:00
image: "img/drtodolittle.png"
external_link: ""
weight: 5
draft: false
---
[Dr ToDo Little](https://www.drtodolittle.de) is a simple ToDo-List-App implemented with AngularJS, Apache Camel, Apache Groovy and Camunda.

The application consists of several parts. The client part is
a Single Page Application (SPA) implemented with AngularJS. The app is responsive and thus adapts to the corresponding display device.

The use of the application is kept extremely simple. The goal is not to create an overloaded ToDo-List-App with as many functionalities as possible, but above all, it should be easy to use.

The server uses Spring Boot with Apache Camel. The REST interface is created with the CAMEL REST DSL. The transferred data is transformed by means of Camel processors, which are implemented in Groovy, and then forwarded to a Camunda BPM engine.

The persistence and administration of the ToDo list takes place in Camunda.

[Dr ToDo Little Website](https://www.drtodolittle.de)
