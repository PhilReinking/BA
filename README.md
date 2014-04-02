## Entwicklung einer Social Extranet Plattform als hybride Applikation für mobile Endgeräte mit Online und Offline Synchronisierung

### Motivation
Soziale Interaktionen mittels Facebook, Twitter und Co sind heutzutage Alltag. Jedoch ist absehbar, dass immer mehr Menschen kleinere und auf bestimmte Themen spezialisierte Plattformen den großen Netzwerken wie Facebook oder Google+ vorziehen werden. Auch in Unternehmen werden gerne solche Netzwerke, mangels Alternativen, für die interne Kommunikation benutzt.  
Um der Entstehung von kleinen themen- und aufgabenspezifischen Netzwerken eine Basis zu bieten, soll eine Plattform für ein soziales Extranet entwickelt werden.

### Technische Umsetzung
Das soziale Extranet soll als hybride Applikation für mobile Endgeräte programmiert werden. Dazu kommt das Framework PhoneGap von Adobe Systems zum Einsatz, welches die Ausführung von HTML5 und JavaScript als installierte Anwendung auf vielen mobilen Betriebssystemen ermöglicht (u.a. iOS, Android, Windows Phone uvm.). Die Entwicklung der Applikationslogik basiert unter Zuhilfenahme des Frameworks Angular.js vollständig auf JavaScript. Für die Datenspeicherung soll ein Konzept zum Tragen kommen, bei dem eine lokale, auch offline verfügbare Datenbank, gegen eine online verfügbare NoSQL-Datenbank abgeglichen wird.

### Zielsetzung
Das Besondere und Interessante an der Anwendung ist letztendlich die Tatsache das sie „online" sowie „offline“ funktionieren soll. In meiner Arbeit würde ich gerne auf die Mittel eingehen, wie eine solche Online/Offline Funktionalität umgesetzt werden kann. Folglich ist das Ziel dieser Arbeit, die bestmögliche Lösung zur reibungslosen Synchronisierung (wenn möglich in Echtzeit) zwischen lokalen und serverseitigen Datenbanken zu finden.

# Einleitung

## Motivation

Soziale Netzwerke haben sich in den letzten Jahren zu einem alltäglichen Instrument der Kommunikation und Informationsbeschaffung entwickelt. Menschen können mithilfe solcher Netzwerke sehr einfach miteinander in Kontakt treten, sich vernetzen und organisieren. 
Auch das Nachrichtenwesen befindet sich zur Zeit im Wandel, denn Informationen verbreiten sich auf Twitter oder Facebook meist schneller, als über klassische Medien wie TV, Radio oder Print. Die oftmals als "viral" bezeichnete Ausbreitung von Beiträgen ist ein Phänomen das man sehr häufig in sozialen Netzwerken beobachten kann und von dem viele Personen, Organisationen und Unternehmen zu profitieren versuchen, besonders wenn es um die Reichweite von Informationen und Kommunikation geht.

Ein soziales Netzwerk, in welchem sich Menschen miteinander vernetzen können, ob als Extranet einer Organisation oder als öffentliches Netzwerk, wird heutzutage von einer Vielzahl von Faktoren bestimmt und beeinflusst.

Ein vor allem entscheidener Faktor in einem Netzwerk ist die Orientierung und Ausrichtung an einem bestimmten Thema oder Themenbereich. Und immer häufiger entstehen auf solche sehr beschränkte Themenbereiche spezialisierte Plattformen, die unter Umständen zwar weniger Personengruppen ansprechen können, aber in der grundlegenden Funktionsweise das Ziel dieses Netzwerks besser unterstützen. (Beispiele: dribbble.com, stackoverflow.com, github.com).

Auch die mobile Internetnutzung muss als weiterer wichtiger Faktor genannt werden, dem immer größere Bedeutung zugesprochen wird. Facebook liefert beispielsweise interessante Statistiken dazu. So haben ca. 73% der monatlich aktiven Nutzer, das Netzwerk auch von einem mobilen Endgerät genutzt. Zusätzlich nutzten ca. 21% der Nutzer das Netzwerk ausschliesslich über ein mobiles Gerät. Ein Grund für die ausschliessliche mobile Nutzung kann das Fehlen der entsprechenden Infrastruktur für einen festen Internetanschluss sein, insbesondere in Entwicklungsländern.

    Facebook Nutzerzahlen (http://allfacebook.de/wp-content/uploads/2013/11/FB_Q313InvestorDeck.pdf)
    Q3 monthly active Users 1189M
    Q3 monthly active mobile Users 874M (73%)
    Q3 monthly active mobile-only Users 254M (21%)
 
Zusätzlich sind Sicherheit, Datenschutz und Privatsphäre gerade zur heutigen Zeit, nicht zuletzt wegen der NSA-Affäre und dem Whistleblower Edward Snowden, immer wieder Bestandteil von Diskussionen wenn es um die Übertragung von Kommunikation und Informationen über das Internet geht und ebenfalls ein Faktor für moderne soziale Netze. OpenSource Software gewinnt so natürlich immer mehr an Bedeutung, da offener Quellcode von jedem Menschen eingesehen werden kann und die Wahrscheinlichkeit reduziert, dass eine Applikation seine Nutzer aktiv ausspioniert.

## Aufgabenstellung im Detail

Die rasant fortschreitende Entwicklung im Bereich der Webtechnologien, welche auch immer stärker für die Verwendung auf mobilen Endgeräten wie Smartphones oder Tablets ausgelegt sind, ermöglicht webbasierten Applikationen eine größere Leistungsfähigkeit. Nicht nur durch schnellere Hardware, sondern auch durch bessere Webbrowser und deren JavaScript Interpreter hat ein Webentwickler heutzutage größere Chancen auf verschiedensten Endgeräten, ob Mobil oder Desktop, die gleichen technischen Vorraussetzungen vorzufinden und so Applikationen mit einer einheitlichen Codebasis zu schreiben, die auf allen Geräten ohne größere Unterschiede funktionieren.

Diese Arbeit widmet sich dem Ziel, unter Beachtung der zuvor genannten Faktoren, eine experimentelle Basisplattform für sogenannte Social Extranets zu entwickeln. Es soll untersucht werden ob die erarbeitete Lösung für einen Einsatz unter realen Bedingungen geeignet ist und welche Anforderungen erfüllt werden müssen um einen sinnvollen Einsatz der Plattform, auch im Hinblick auf die Erweiterbarkeit und Weiterentwicklung durch andere Entwickler, zu gewährleisten.

Im Kern entstehen so zwei Anforderungen die im Laufe dieser Arbeit bewältigt werden sollen. Die Applikation soll zum einen mit mobilen Geräten benutzbar sein und zum anderen auch noch ohne aktive Internetverbindung (eingeschränkt) funktionieren.

Als wichtigste Komponente für dieses Vorhaben, steht die Datenbank im Mittelpunkt. Im Allgemeinen hat man heutzutage meist noch sehr begrenzte Möglichkeiten um in einem Webbrowser große Mengen an Daten ohne den Einsatz externer Datenbanksoftware direkt im Browser zu speichern. Um die Applikation grundsätzlich unabhängig von externen Einflüssen und darüber hinaus auch offline funktionsfähig zu machen, muss die Datenbank also innerhalb der Laufzeitumgebung (PhoneGap/Webview) bereitgestellt werden.

Die Offline Funktionalität stellt eine weitere Herausforderung da, denn es muss dafür Sorge getragen werden, dass Änderungen welche auf einem Gerät ohne Internetverbindung getätigt wurden, auch auf andere Geräte übertragen werden können, sobald dieses Gerät wieder eine Internetverbindung hat und im Umkehrschluss müssen Änderungen anderer Geräte auch empfangen werden. Das Netzwerk muss also in der Lage sein Datenbanken in alle Richtungen zu replizieren.

## Umfeld

Diese Arbeit entstand im Innovation Lab der Pixelpark AG am Standort Köln. Das Innovation Lab ist eine Forschungsabteilung die aktiv mit neuen und teilweise experimentellen Technologien verschiedenste Projekte umsetzt. Als Teilnehmer des "Future Internet" Programms im Bereich "FIcontent2" der Europäischen Union übernimmt Pixelpark die Verantwortung zur Erstellung eines sogenannten "Social Network Enablers".

Das Social Network Enabler Projekt ist im Bereich der Smart City Services angesiedelt und steht als offene Plattform für Entwickler zur Verfügung, um darauf aufbauend eigene Ideen umzusetzen.

Die in dieser Arbeit erreichten Ergebnisse werden als Kernkomponente in den Social Network Enabler eingebunden.

## Planung
**Wie wurde vorgegangen um das zuvor beschriebene Ziel zu erreichen?**

# Theoretische Grundlagen

## NoSQL

NoSQL ist im allgemeinen ein Begriff für Datenbanksysteme, die nicht dem Prinzip von relationalen Datenbanken folgen. Daten werden nicht relational gespeichert und es wird auch kein SQL als Abfragesprache verwendet.[Getting Started with NoSQL: S.7]

In der Regel werden bei relationalen Datenbanken sogenannte Transaktionen verwendet, welche bei Zugriff auf einen Datensatz, diesen Datensatz für andere Transaktionen sperren und dabei dem ACID-Prinzip folgen. NoSQL setzt dagegen auf eine andere Vorgehensweise, welche auch als BASE bekannt ist und von Eric Brewer definiert wurde:[Getting Started with NoSQL: S.10]

- Basic availability: Jede Anfrage erhält garantiert eine Antwort
- Soft state: Daten können sich auch ohne spezifische Eingaben verändern
- Eventual consistency: Die Datenbank kann zeitweise inkonsistent sein, kehrt dann aber zu einem konsistenten Status zurück

In einem verteilten Datenbanksystem definiert Eric Brewer 3 Eigenschaften, welche für verschiedene Anwendungsfälle wichtig sein können: 'Consistency', 'Availability' und 'Partition tolerance'. Das sogenannte CAP-Theorem besagt, das lediglich nur zwei dieser drei Eigenschaften in einem Datenbanksystem garantiert werden können.[http://www.cs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf] Im Falle von NoSQL sind 'Availability' und 'Partition tolerance' die herrausstechenden Merkmale.

    Abbildung CAP Theorem(http://docs.couchdb.org/en/latest/intro/consistency.html#intro-consistency)

Letztendlich sind die Vorteile von NoSQL Datenbanken vor allem hohe Skalierbarkeit und Geschwindigkeit.[http://home.aubg.bg/students/ENL100/Cloud%20Computing/Research%20Paper/nosqldbs.pdf] Desweiteren ist auch die oft schemalose Repräsentation und die damit verbundene Flexibilität von Datensätzen ein Grund für Entwickler auf NoSQL Datenbanken zu setzen, da hier Entwicklungszeit eingespart wird.

## IndexedDB

Die IndexedDB ist eine HTML5 Spezifikation und befindet sich im Entwicklungsprozess des W3C in der 'Last Call Working Draft' Phase (http://www.w3.org/TR/IndexedDB/). Die Implementierung in den aktuellen Browsern ist relativ konsistent umgesetzt, d.h. es gibt keine merkbaren Unterschiede. Lediglich Apple mit Safari und iOS und Opera Mini bieten aktuell keine Unterstützung für die IndexedDB-Spezifikation.[http://caniuse.com/#feat=indexeddb]

IndexedDB macht es möglich große Datenmengen im Browser des Nutzers zu speichern. Die dort abgelegten Daten können mithilfe einer API durchsucht werden.

Im Gegensatz zu Cookies oder Webstorage besteht für die IndexedDB theoretisch kein Limit was die Größe des verfügbaren Datenspeichers betrifft, kann aber durch den Browser begrenzt sein. Jede Applikation die notwendigerweise große Datenmengen übertragen muss, kann davon profitieren diese Daten dauerhaft in dem Browser des Nutzers zu speichern.(https://developer.mozilla.org/en-US/docs/IndexedDB/Basic_Concepts_Behind_IndexedDB) Ebenso hat der Webstorage keinen Suchmechanismus, sondern nur die Möglichkeit über einen *Key* auf einen *Value*
zuzugreifen. Die IndexedDB kommt dem Begriff einer "Datenbank" also sehr viel näher als der einfache Webstorage.

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

## Phonegap

Phonegap ist ein OpenSource Framework um die Entwicklung von hybriden Applikationen für Smartphones mittels HTML5, CSS und JavaScript zu ermöglichen.

Eine erste wesentliche Fähigkeit von Phonegap ist das Bereitstellen installierbarer Applikationen für eine Vielzahl von Betriebssystemen, darunter iOS, Android, WindowsPhone, Blackberry, WebOS, Bada und Symbian. Dabei entsteht jede Applikation aus der gleichen Codebasis, bestehend aus HTML, CSS und JavaScript. Um auf allen Betriebssystemen denselben Code ausführen zu können, wird die Applikation in einer Webview ausgeführt.

Eine native Applikation auf dem Handy ist effektiver, wenn man auf gerätespezifische Funktionen, wie zum Beispiel die Kamera, zugreifen kann. Hier stellt Phonegap eine API zur Verfügung die mit JavaScript angesprochen werden kann.

Der Build-Prozess einer Applikation erfordert zusätzlich das SDK des entsprechenden Betriebssystems. Zum Beispiel muss für die Android Kompilierung das Android SDK installiert sein und für die Kompilierung zu einer iOS App wird xCode benötigt.

# Datenbankreplikation mit CouchDB

Die Entscheidung für ein passendes Datenbanksystem ist für das Ergebnis dieser Arbeit ein sehr wichtiger Faktor. Es gibt zahlreiche Hersteller und damit verbunden eine sehr große Anzahl von entsprechenden Lösungen. Im Folgenden werden die Anforderungen an die Datenbank definiert und die Auswahl für die in dieser Arbeit verwendeten Lösung begründet.

## Analyse der Anforderungen

Die Anforderungen an die Datenbank werden aus einem geplanten Testszenario hergeleitet, welches im Laufe dieser Arbeit an einer Schule mit ca. 15 Testpersonen durchgeführt wird:

*"Eine Smartphone Applikation soll für maximal 15 Person ermöglichen, Statusmeldungen in Form von Text über ein Netzwerk in Echtzeit auszutauschen. Das Experiment wird an einem Ort mit mangelhaftem Mobilfunk durchgeführt, sodass Verbindunsabbrüche zum Netzwerk auftreten können."*

Aus diesem Szenario ergeben sich folgende Einschränkungen:
- das Netzwerk hat maximal 15 Teilnehmer
- Teilnehmer können Statusmeldungen in Form von Text verschicken
- Statusmeldungen werden nicht editiert
- die Netzwerkverbindung kann unterbrochen werden

Die folgenden drei Anwendungsfälle verdeutlichen das gewünschte Verhalten der finalen Applikation:

```
Use Case: Initialer Start
Vorbedingungen: 
    App startet zum ersten Mal
Beschreibung:
    1. Es wird ein Ladesymbol angezeigt.
    2. Alle relevanten Daten aus dem Netzwerk werden zur Offline-Nutzung auf das Gerät geladen und gespeichert.
    3. Die Statusmeldungen werden chronologisch angezeigt.
```


```
Use Case: Statusmeldung absenden
Vorbedingungen: 
    App ist gestartet; Initialer Start durchgeführt
Beschreibung:
    1. Der Teilnehmer gibt seine Statusmeldung ein und drückt auf Senden.
    2. Der Teilnehmer ist online, die Statusmeldung wird lokal gespeichert und über das Netzwerk synchronisiert.
Alternativ:
    2b. Der Teilnehmer ist offline, die Statusmeldung wird nur lokal gespeichert.
Nachbedingung: 
    Die Statusmeldung ist garantiert auf dem Absendergerät gespeichert.
```


```
Use Case: Statusmeldungen synchronisieren
Vorbedingungen: 
    Statusmeldungen wurden nur lokal gespeichert; Während der Offline-Nutzung wurden im Netzwerk neue Statusmeldungen versendet
Beschreibung:
    1. Das Teilnehmergerät verbindet sich mit dem Internet
    2. Zuvor nur lokal gespeicherte Statusmeldungen werden über das Netzwerk synchronisiert.
    3. Neue Statusmeldungen aus dem Netzwerk werden auf dem Gerät gespeichert und angezeigt.
Nachbedinung:
    Alle neuen Statusmeldungen wurden über das Netzwerk übertragen.
```

## Übertragung der Anforderungen auf das Datenbanksystem

Auf Ebene der Datenbank lassen sich 3 funktionale Anforderungen identifizieren, die benötigt werden um das zuvor beschriebene Szenario erfolgreich bestehen zu können.

**A: Kompatibilität zur Laufzeitumgebung PhoneGap (Webview):**  
Diese Anforderung ergibt sich vorwiegend aus der Wahl von PhoneGap als Zielplattform. Die Datenbank muss mit JavaScript kompatibel sein und über eine entsprechende Schnittstelle verfügen die auch auch auf mobilen Geräten verwendet werden kann.

**B: Daten können offline gespeichert werden:**  
Die Datenbank kann Daten auch offline speichern und ist damit unabhängig von einer aktiven Internetverbindung.

**C: Synchronisierung zwischen beliebig vielen Geräten:**  
Um Statusmeldungen auf alle Geräte verteilen zu können muss die Datenbank in der Lage sein diese Daten zu synchronisieren.

## Auswahl der Datenbank

### Lokaler Datenspeicher in PhoneGap

Die wohl am schwersten zu bewältigende Anforderung ist durch die Wahl von PhoneGap als Zielplattform entstanden. Die Tatsache, dass die Anwendung in einer Webview laufen wird, verringert die Auswahlmöglichkeiten einer passenden Datenbank.

Für HTML5 sind bisher 3 Datenspeicher entwickelt worden:

- Webstorage kann als Ersatz für herkömmliche Cookies betrachet werden und funktioniert nach dem Key-Value-Prinzip, ist aber eher für kleine Datenmengen ausgelegt.
- WebSQL ist eine Sqlite-Implementation für den Browser, wird jedoch nicht weiterentwickelt und soll daher auch nicht näher betrachet werden.
- Die IndexedDB ist eine für größere Datenmengen ausgelegte Datenbankschnittstelle zum Speichern von komplex strukturierten Objekten, die durch *Keys* indexiert werden.

Objekte werden in der IndexedDB mit einem *Key* verknüpft. Es gibt keine Query-Language wie bei relationalen Datenbanken üblich, sondern es wird mithilfe eines Cursors über die *Keys* iteriert um Einträge zu finden. [IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB/Basic_Concepts_Behind_IndexedDB)

Die API wird über JavaScript angesprochen und Objekte werden in der JSON Notation gespeichert, sind somit also direkt in JavaScript verwendbar. Die IndexedDB wird auch zur Klasse der Dokumentenorientierten Datenbanken gezählt.

```javascript
    var request = indexedDB.open("library");

    request.onupgradeneeded = function() {
      // The database did not previously exist, so create object stores and indexes.
      var db = request.result;
      var store = db.createObjectStore("books", {keyPath: "isbn"});
      var titleIndex = store.createIndex("by_title", "title", {unique: true});
      var authorIndex = store.createIndex("by_author", "author");

      // Populate with initial data.
      store.put({title: "Quarry Memories", author: "Fred", isbn: 123456});
      store.put({title: "Water Buffaloes", author: "Fred", isbn: 234567});
      store.put({title: "Bedrock Nights", author: "Barney", isbn: 345678});
    };

    request.onsuccess = function() {
      db = request.result;
    };
```

### Synchronisierung

Die lokal gespeicherten Daten in der IndexedDB mit anderen Geräten im Netzwerk zu synchronisieren, ist eine weitere Anforderung die erfüllt werden muss. 

Datenbankreplikationen werden von den meisten modernen Datenbanken unterstützt. Es muss hier aber zwischen zwei Arten von Replikation unterschieden werden. Die meisten Datenbanken müssen früher oder später skaliert werden, um wachsende Schreib- und Lesezugriffe bewältigen zu können. Dafür wird überwiegend die Master-Slave Replikation eingesetzt. Eine im Slave-Modus angebundene Datenbank hat damit die Möglichkeit die Daten der Master-Datenbank zu lesen, besitzt jedoch keine Schreibrechte auf die Master-Datenbank.

Für diese Arbeit ist eine andere Art von Replikation relevant, da die Skalierung bei maximal 15 Teilnehmern keine Rolle spielt. Jedes Gerät im Netzwerk soll Daten auch lokal speichern und später zur zentralen Datenbank replizieren können. Alle Endgeräte besitzen somit eine Master-Datenbank und synchronisieren die Daten zum Server, auf dem sich ebenfalls eine Master-Datenbank befindet. Somit kann jede Datenbank unabhängig von den anderen im Netzwerk befindlichen Datenbanken funktionieren.

Datensätze liegen grundsätzlich als Objekte in der JSON-Notation vor, weshalb eine dazu kompatible Datenbank vorzuziehen ist.
MongoDB und CouchDB sind jeweils Dokumentenorientierte und mit JavaScript kompatible Datenbanken. In Abbildung x.x werden alle relevanten Spezifikationen der Datenbanken gegenübergestellt.

|               | MongoDB             | CouchDB       |
|---------------|---------------------|---------------|
| Format        | BSON                | JSON          |
| Versionierung | Nein                | Ja            |
| Map/Reduce    | JavaScript + andere | JavaScript    |
| Replikation   | Master-Slave        | Master-Master |
|               |                     | Master-Slave  |
| Protokoll     | TCP/IP              | HTTP/REST API |

http://weblogs.asp.net/britchie/archive/2010/08/17/document-databases-compared-mongodb-couchdb-and-ravendb.aspx

Die CouchDB übertrumpft im Anwendungsfall dieser Arbeit die MongoDB, da die Unterstützung für JSON-Objekte, Versionierung und Master-Master Replikation gegeben ist.

### Gemeinsame API für IndexedDB und CouchDB

Es gibt nicht viele Datenbanken die eine direkte Kompatibilität zur IndexedDB herstellen. Die PouchDB ist ein im Juni 2010 entstandenes OpenSource Projekt, dass es sich zur Aufgabe gemacht hat die funktionsweise der CouchDB direkt in den Browser zu übertragen. Das Projekt befindet sich fortlaufend in Entwicklung, und wurde am 2. Januar 2014 in der Version 1.1.0 veröffentlicht.

Mit der PouchDB ist es möglich die Replikation zwischen IndexedDB, welche von PouchDB als Speicher verwendet wird, und CouchDB durchzuführen.

### IndexedDB, PouchDB und CouchDB

Mit der IndexedDB kommt eine in modernen Browsern verfügbare Datenbank zum Einsatz, welche die Anforderungen A und B erfüllt. Zusätzlich wird die CouchDB in Kombination mit der PouchDB verwendet um Anforderung C, die Synchronisierung zwischen den Datenbanken, zu erfüllen.

## CouchDB

Es ist wichtig zu verstehen wie die CouchDB im Detail funktioniert, um einen effizienten Mechanismus zu erarbeiten, der die Daten im Netzwerk zwischen den einzelnen Geräten synchronisieren kann.

Daten werden grundsätzlich als JSON enkodierte Strings gespeichert. Die JSON-Notation ist eine in JavaScript verwendete Schreibweise um Objekte darzustellen. Jedoch wird diese Notation unabhängig von JavaScript, auch von anderen Programmiersprachen verwendet und so hat sich JSON, in Konkurrenz zu XML, zu einem sehr beliebten Format für die Übertragung von Daten innerhalb von Webapplikation entwickelt. Der Vorteil bei Verwendung von JSON als Format in der Applikation dieser Arbeit liegt auf der Hand, denn in AngularJS können die aus der CouchDB gelesenen Daten ohne größeren Aufwand, sprich Umwandlung oder Dekodierung, direkt verwertet werden.

Dokumente in der CouchDB folgen keinem festen Schema. Als Entwickler hat man die Freiheit jederzeit Objekte mit beliebiger Struktur und verschiedenen Datentypen zu speichern. Andere Dokumente werden dadurch nicht beeinflusst. Jedoch gibt es zwei Felder die jedem Dokument zugeordnet werden müssen. Zum einen muss das Feld *_id* einen innerhalb der Datenbank einzigartigen Wert haben und wird verwendet um das Dokument eindeutig zu identifizieren. Der zweite Wert ist das Feld *_rev*, welches die Revisionsnummer speichert. Revisionen enstehen genau dann, wenn ein Dokument verändert wird und in der Datenbank unter derselben *_id* gespeichert werden soll. So lassen sich Änderungen bis zum Erstellen von Dokumenten zurückverfolgen.

```json
{
    _id: '123',
    _rev: '1-123',
    city: 'Cologne'
}
```

Die Abfrage von Daten erfolgt über in der CouchDB erstellte Views und unterscheidet sich damit sehr stark von Datenbankabfragen in relationalen Datenbanksystemen. Jede View enthält eine *Map* Funktion, die auf jedes Dokument einzeln angewendet wird, um einen *key* zu erzeugen.

In der *Map* Funktion kann jeder Wert des aktuellen Dokumentes ausgelesen werden und dazu verwendet werden einen *key* für dieses Dokument zu generieren. Die CouchDB nutzt diese *keys* zur Sortierung in Spalten und macht es so möglich durch Angabe eines bestimmten Bereichs auch große Datenmengen effizient zu durchsuchen und das entsprechende Ergebnis auszugeben.

Das interessante daran ist, dass die *keys* nicht nur einzelne Werte, sondern auch Arrays sein können. Zueinander in Relation stehende Dokumente können damit in der View hintereinander indexiert werden.

```json
{
    _id: 'abc',
    city: 'Cologne'
},
{
    _id: 'def',
    city: 'Zweibruecken'
},
{
    _id: '123',
    place: {
        name: 'Pixelpark AG Cologne',
        related: 'abc'
    }
}
```

In diesem Beispiel wird gezeigt, wie mithilfe einer Map-Funktion *keys* erstellt werden, die der CouchDB eine sortierte Indexierung und Ausgabe ermöglichen.

```javascript
function(doc){
    if(doc.city){
        emit([doc._id, 0], doc.city);
    } else {
        emit([doc.place.related, 1], doc.place.name);
    }
}
```

```json
{
    key: ['abc', 0],
    value: Cologne
},
{
    key: ['abc', 1],
    value: 'Pixelpark AG Cologne'
},
{
    key: ['def', 0],
    value: 'Zweibruecken'
}
```

Die Replikation von Datenbanken innerhalb eines CouchDB Clusters erfolgt schrittweise. Das bedeutet dass die Datenbanken keine ständige Verbindung zueinander benötigen, wie es bei vielen relationalen Datenbanksystemen der Fall ist. Sobald eine Datenbank einen Replikationsprozess beendet hat ist sie auch eigenständig funktionsfähig.

Die Applikation entscheidet selbst, wann eine Replikation sinnvoll ist und kann diese durch die CouchDB API auslösen. Durch Angabe von Quelle und Ziel wird bestimmt in welche Richtung rpeliziert wird.

Natürlich kann es passieren dass Datenbanken während einer Replikation Dokumente beinhalten die zueinander in Konflikt stehen. In diesem Fall besitzt die CouchDB einen Algorithmus, der entscheidet welche der beiden Versionen als aktuell gespeichert werden soll. Das Verlierer-Dokument wird aber keinesfalls verworfen, sondern wird unter einer anderen Revision gespeichert. So ist es auch möglich Applikationen zu bauen, die Konflikte nachträglich überprüfen können und gegebenfalls selbst entscheiden welches Dokument behalten werden soll.

Um Dokumente auf allen Servern zu löschen, benutzt die CouchDB ein *deleted* Feld. Ein Dokument das gelöscht wurde kann keine neuen Revisionen mehr erhalten und wird ebenfalls bei einer Replikation übertragen, sodass auch in allen anderen Datenbanken dieses Dokument als gelöscht markiert wird.








