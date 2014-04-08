## Komponenten

Die Applikation *BAnet* wird in ihrer finalen Version 3 Komponenten enthalten, die verschiedene Aufgaben erfüllen und damit die Funktionalität der Synchronisierung demonstrieren.

Der Mechanismus zur Speicherung und Synchronisierung der Daten erfolgt in einem von der Applikation unabhängigem Modul. Durch das Abkapseln der Datenbanklogik als Modul wird es möglich, dieses auch ohne weiteres in anderen Projekten zu verwenden. Im weiteren Verlauf wird dieses Modul als *ppSync* bezeichnet.

In ABBILDUNG KLASSENDIAGRAMM wird das Zusammenspiel der einzelnen Schichten in *BAnet* deutlich.

## ppSync Service

AngularJS bietet neben den Controllern eine weitere Möglichkeit um die Logik für eine Applikation modular zu gliedern, die Services. Ein Service ist ein besonderes AngularJS Modul, das nach einer von drei Vorlagen implementiert werden kann, und über den AngularJS Dependency Injector für Controller oder andere Services verfügbar gemacht wird.

Dependency Injection (DI) ist eine sehr wichtige Funktion von AngularJS, denn durch den Einsatz von Services und DI erreicht man eine weitere Kapselung von wiederverwendbaren Code, der durch einfaches Deklarieren von Abhängigkeiten (Dependicies) in Controllern und Services, genau dort verfügbar gemacht wird, wo er auch gebraucht wird, ohne den globalen JavaScript Bereich zu belasten.

Für *ppSync* ist ein Service der optimalste Weg, die Datenbank- und Replikationslogik in *BAnet* zu implementieren. Zusätzlich wird der Umstand genutzt, das Services in AngularJS als Singleton erzeugt werden, also im globalen Bereich nur einmal existiert. Damit ist garantiert, dass jeder Controller mit dem gleichen Datenbank-Objekt interagieren kann und es nur einen Synchronisierungsprozess gibt.
