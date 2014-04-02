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

Die lokal gespeicherten Daten in der IndexedDB mit anderen Geräten im Netzwerk zu synchronisieren, ist eine weitere Anforderung, die erfüllt werden muss. 

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

Es gibt nicht viele Datenbanken, die eine direkte Kompatibilität zur IndexedDB herstellen. Die PouchDB ist ein im Juni 2010 entstandenes OpenSource Projekt, dass es sich zur Aufgabe gemacht hat, die Funktionsweise der CouchDB direkt in den Browser zu übertragen. Das Projekt befindet sich fortlaufend in Entwicklung, und wurde am 2. Januar 2014 in der Version 1.1.0 veröffentlicht.

Mit der PouchDB ist es möglich, die Replikation zwischen IndexedDB, welche von PouchDB als Speicher verwendet wird, und CouchDB durchzuführen.


### IndexedDB, PouchDB und CouchDB

Mit der IndexedDB kommt eine in modernen Browsern verfügbare Datenbank zum Einsatz, welche die Anforderungen A und B erfüllt. Zusätzlich wird die CouchDB in Kombination mit der PouchDB verwendet, um Anforderung C, die Synchronisierung zwischen den Datenbanken, zu erfüllen.
