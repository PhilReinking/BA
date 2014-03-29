## AngularJS

[AngularJS](http://www.angularjs.org) ist ein MVC (Model-View-Controller) JavaScript Framework zum Erstellen von interaktiven, dynamischen Single Page Applications.

Bis zum Jahr 2008 war JavaScript eine vergleichsweise langsame Skriptsprache. Durch ständiges Weiterentwickeln der JavaScript Engines, konnte das Ausführen von JavaScript Code in den letzten Jahren jedoch deutlich beschleunigt werden.[http://books.google.de/books?id=ED6ph4WEIoQC&lpg=PR7&ots=2Uwg0FZfiM&dq=javascript%20performance%20evolution&lr&hl=de&pg=PR13#v=onepage&q&f=false]
Durch diesen enormen Performance-Zuwachs in der JavaScript Technologie, konnte man in Erwägung ziehen JavaScript für sogenannte Rich Internet Applications zu verwenden.

Ein Problem das viele JavaScript Frameworks versuchen zu beheben, ist die Tatsache das HTML eine statische Auszeichnungssprache ist. HTML ist nicht dazu konzipiert worden dynamisch zu sein oder dem Nutzer reichhaltige Interaktionsmöglichkeiten zu bieten. Das moderne Web verlangt jedoch nach Lösungen, sowohl im Desktopbereich auch als im mobilen Sektor, Webapplikationen von Grund auf interaktiv zu gestalten.

Eine Methode um den Nutzer mit einer Webapplikation interagieren zu lassen, ist der herkömmliche Weg über einen Webserver, welcher Nutzereingaben verarbeitet und darauf in der Regel eine Antwort an den Nutzer in Form einer HTML Seite ausgibt. Hier entstehen jedoch schon einige Nachteile. Für jede Interaktion des Nutzers muss der Server mit einbezogen werden, was nicht nur Traffic verursacht und vor allem schlecht für mobile Geräte ist, sondern je nach Auslastung des Servers auch noch längere Ladezeiten verursachen kann.

Eine andere Methode zur Umsetzung interaktiver Webapplikationen ist die Manipulation von HTML und CSS über JavaScript. Als bekanntestes Beispiel ist hier jQuery zu nennen, eine JavaScript Bibliothek die unter anderem eine einfache API zur Manipulation des DOM (Document Object Model) bietet.  
Jedoch ist jQuery keine vollständige Lösung um interaktive Anwendungen zu erstellen, sondern bietet nur eine Schnittstelle zum DOM. AngularJS und vergleichbare Frameworks setzen hier an und bieten zum Teil sehr gut durchdachte Konzepte um Rich Internet Applications effizient zu erstellen.

Eine Fähigkeit von AngularJS ist das 2-way-data-binding, welches den Zugriff auf Models direkt aus der View heraus ermöglicht. Veränderungen am Model können dann durch das Framework erkannt werden und das DOM wird aktualisiert.[Mastering Web App Development with AngularJS, Seite 10ff]

```html
    <body ng-app ng-init="name = 'World'">
        Say hello to: <input type="text" ng-model="name">
        <h1>Hello, {{name}}!</h1>
    </body>
```

Das wirklich besondere an AngularJS ist die Fähigkeit HTML um eigene Tags anreichern zu können. Durch die Implementierung sogenannter Direktiven werden Funktionen nicht nur vom restlichen Quellcode abgekapselt und dadurch wiederverwendbar, sondern bekommen in der endgültigen View eine semantische Bedeutung und vereinfachen somit die Lesbarkeit für Entwickler. In Abbildung X.X wird die von AngularJS mitgelieferte Direktive ng-repeat verwendet um über ein Array zu iterieren und dessen Inhalte auszugeben.

```html
    <ul ng-controller="WorldCtrl">
        <li ng-repeat="country in countries">
            {{country.name}} has population of {{country.population}}
        </li>
        <hr>
        World's population: {{population}} millions
    </ul>
    
    <script>
        var WorldCtrl = function ($scope) {
            $scope.population = 7000;
            $scope.countries = [
                {name: 'France', population: 63.1},
                {name: 'United Kingdom', population: 61.8},
            ];
        };
    </script>
```
