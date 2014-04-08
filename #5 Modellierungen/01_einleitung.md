# Modellierungen

Dieses Kapitel behandelt die visuelle und technische Umsetzung der Social Extranet Plattform, im weiteren Verlauf auch als *BAnet* bezeichnet. Ziel ist es, eine um Komponenten erweiterbare, flexible und auf mobile Geräte optimierte Applikation zu entwerfen. 

## Model View Controller

Mit AngularJS wird ein Framework eingesetzt, welches das Konzept des MVC Entwurfmusters unterstützt. Der Vorteil dabei ist, dass man mit diesem Muster eine sehr klare Trennung der verschiedenen technischen Bestandteile einer Applikation erreichen kann.

Views sind in AngularJS das, was der Benutzer der Applikation am Ende auf seinem Endgerät sehen soll. Eine solche View ist in der Regel eine einfache HTML-Datei, welche AngularJS-Expressions enthalten kann. AngularJS-Expressions sind eine von mehreren Möglichkeiten, um den Inhalt von Models auszugeben.

Controller stellen die Funktionen für eine View zur Verfügung. In einer View ist ganz genau definiert auf welchen Controller zugegriffen werden soll, weshalb auch nur die Funktionen des entsprechenden Controllers aufrufbar sind. In AngularJS hat man außerdem die Option, Teilen des HTML-Codes eigene Controller zuzuweisen.

Models speichern die Daten der Applikation. Die View und der Controller haben Zugriff auf Models und können die enthaltenen Werte manipulieren. Es ist üblich die Geschäftslogik, also das Verwalten, Validieren und Manipulieren der Daten, innerhalb einer eigenen Model-Datei auszulagern.

## Ordnerstrukturen und Entwicklungsumgebung

PhoneGap benötigt eine bestimmte Ordnerstruktur um den Build-Prozess für ein Betriebssystem durchführen zu können. Es empfiehlt sich daher ein PhoneGap Projekt mithilfe des Befehls `phonegap create ~/path/to/project` zu erstellen. Daraus resultierend erhält man die in ABBILDUNG gezeigte Ordnerstruktur, welche natürlich noch an die eigenen Bedürfnisse angepasst werden kann.

```
Phonegap Project
├── merges
├── platforms
├── plugins
└── www
    ├── config.xml
    ├── css
    ├── img
    ├── index.html
    ├── js
    ├── res
    └── spec
```

Die gesamte Applikationslogik wird unter dem Ordner www abgelegt. Zusätzlich befindet sich dort die Datei config.xml, eine Konfigurationsdatei in der beispielsweise Name und Versionsnummer der Applikation festgelegt wird, aber auch welche Berechtigungen auf Geräten mit Android benötigt oder welche Plugins eingebunden werden. PhoneGap stellt solche Plugins zur Verfügung und ermöglicht dadurch auf gerätespezifische Funktionen wie Kamera oder GPS zuzugreifen.

Zusätzlich zu dem PhoneGap Projektordner, wird ein weiterer getrennter Ordner zum aktiven Entwickeln der Applikation erstellt. Unter Zuhilfenahme der Tools Yeoman, Grunt und Bower wird das AngularJS Projekt verwaltet.  
Der Grund für den Einsatz dieser Tools ist, dass viele Prozesse während der Entwicklung an einer modernen Webapplikation automatisiert werden sollten, um  effizient daran arbeiten zu können.  
Als Beispiel soll hier der der Einsatz von Sass in Kombination mit Compass als CSS-Precompiler dienen. Wird eine Sass Datei geändert, kann Grunt automatisch den Compass Compiler ausführen und die CSS Datei damit aktualisieren. Ebenso ist es möglich den JavaScript Code mit einem sogenannten Linter zu validieren, der Warnungen ausgibt, sobald Fehler in der JavaScript Syntax gefunden wurden. 

Diese Prozesse sind vollständig auf die eigenen Bedürfnisse einer Anwendung konfigurierbar und bieten damit größtmögliche Flexibilität beim Entwickeln von Applikationen.

Die fertige Applikation wird mit `grunt serve` in einer optimierten Fassung mit minimiertem CSS, JavaScript und HTML generiert.

## JSON Schema

Das Schema der zu speichernden JSON Objekte ist zu jedem Zeitpunkt in der Entwicklung sehr flexibel, jedoch werden einige Schlüsselattribute vorgegeben um so z.B. eine Relation zwischen einem Beitrag und einem dazugehörenden Kommentar herzustellen.

```json
ppSyncServiceDocument = {
    type: string, // POST, COMMENT
    created: timestamp, // UNIX TIMESTAMP
    posting: string // id des Eltern-Elements
}
```










