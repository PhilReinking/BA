# Modellierungen

Dieses Kapitel behandelt die visuelle und technische Umsetzung der Social Extranet Plattform, im weiteren Verlauf auch als *BAnet* bezeichnet. Ziel ist es, eine um Komponenten erweiterbare, flexible und auf mobile Geräte optimierte Applikation zu entwerfen. 

## Model View Controller

Mit AngularJS wird ein Framework eingesetzt, welches das Konzept des MVC Entwurfmusters unterstützt. Der Vorteil dabei ist, dass man mit diesem Muster eine sehr klare Trennung der verschiedenen technischen Bestandteile einer Applikation erreichen kann.

Views sind in AngularJS das, was der Benutzer der Applikation am Ende auf seinem Endgerät sehen soll. Eine solche View ist in der Regel eine einfache HTML-Datei, welche AngularJS-Expressions enthalten kann. AngularJS-Expressions sind eine von mehreren Möglichkeiten, um den Inhalt von Models auszugeben.

Controller stellen die Funktionen für eine View zur Verfügung. In einer View ist ganz genau definiert auf welchen Controller zugegriffen werden soll, weshalb auch nur die Funktionen des entsprechenden Controllers aufrufbar sind. In AngularJS hat man außerdem die Option, bestimmten Teilen des HTML-Codes eigene Controller zuzuweisen.

Models speichern die Daten der Applikation. Die View und der Controller haben Zugriff auf Models und können die enthaltenen Werte manipulieren. Es ist üblich die Geschäftslogik, also das Verwalten, Validieren und Manipulieren der Daten, innerhalb einer eigenen Model-Datei auszulagern.

## Komponenten

Die Applikation *BAnet* wird in ihrer finalen Version 3 Komponenten besitzen, die verschiedene Aufgaben erfüllen und damit die Funktionalität der Synchronisierung demonstrieren.

Der Mechanismus zur Speicherung und Synchronisierung der Daten erfolgt in einem von der Applikation unabhängigem Modul. Durch das Abkapseln der Datenbanklogik als Modul wird es möglich, dieses auch ohne weiteres in anderen Projekten zu verwenden. Im weiteren Verlauf wird dieses Modul als *ppSync* bezeichnet.

In ABBILDUNG KLASSENDIAGRAMM wird das Zusammenspiel der einzelnen Schichten in *BAnet* deutlich.

## ppSync

AngularJS bietet neben den Controllern eine weitere Möglichkeit um die Logik für eine Applikation modular zu gliedern, die Services. Ein Service ist ein besonderes AngularJS Modul, das nach einer von drei Vorlagen implementiert werden kann, und über den AngularJS Dependency Injector für Controller oder andere Services verfügbar gemacht wird.

Dependency Injection (DI) ist eine sehr wichtige Funktion von AngularJS, denn durch den Einsatz von Services und DI erreicht man eine weitere Kapselung von wiederverwendbaren Code, der durch einfaches Deklarieren von Abhängigkeiten (Dependicies) in Controllern und Services, genau dort verfügbar gemacht wird, wo er auch gebraucht wird, ohne den globalen JavaScript Bereich zu belasten.

Für *ppSync* ist ein Service der optimalste Weg, die Datenbank- und Replikationslogik in *BAnet* zu implementieren. Zusätzlich wird der Umstand genutzt, das Services in AngularJS als Singleton erzeugt werden, also im globalen Bereich nur einmal existiert. Damit ist garantiert, dass jeder Controller mit dem gleichen Datenbank-Objekt interagieren kann und es nur einen Synchronisierungsprozess gibt.

## Dateistrukturen und Entwicklungsumgebung

AngularJS und PhoneGap 

## Grafischer Entwurf
Die App setzt sich aus 3 wesentlichen Bereichen zusammen: 

- Die Navigation im Kopfbereich
- Der Hauptbereich für Inhalte der aktiven Komponente
- Der Fußbereich für optionale Inhalte

Es ist sinnvoll, diese 3 Bereiche vertikal anzuordnen um so jedem Bereich die Möglichkeit zu geben sich auf die volle verfügbare Breite des Anzeigerätes zu erstrecken.

[GRAFIK Mockup01]

Der Kopfbereich beinhaltet die Navigation und optional ein Logo der Applikation. Hier soll jeder Komponente ein Link zugeordnet werden, welcher bei Bedarf auch ein weiteres Dropdown Menü öffnen kann.

Der Hauptbereich ist ausschliesslich für die Inhalte der Komponente reserviert. Das Layout wird von der Komponente bestimmt und kann sich dementprechend von anderen Seiten unterscheiden. Für diese Arbeit setzt jede Komponente auf eine ebenfalls vertikale Anordnung der Elemente, um so die maximale verfügbare Breite auszunutzen.

Der Fußbereich ist ein optionaler Bereich, der beispielsweise mit ergänzenden Inhalten gefüllt werden kann.

## Klassendiagramm

Mit AngularJS lassen sich Projekte sehr gut 

## JSON-Objekt Schema

