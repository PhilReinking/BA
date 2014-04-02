## PouchDB API

Zur Umsetzung der Daten-Synchronisierung wird die API der PouchDB verwendet. Die dazu benötigten Methoden werden im Folgenden Abschnitt erläutert.

```javascript
var db = new PouchDB('dbname');
var remote = 'http://couchdb.simple-url.com:5984/dbname';
```

Die Methode zum Erstellen einer Datenbank betrifft die Replikation zwar nicht direkt, jedoch dient es dem besseren Verständnis. Zur Initialisierung der Datenbank wird ein neues PouchDB-Objekt erstellt. Dabei ist es egal ob diese Datenbank schon besteht oder nicht. Beim erstmaligen Aufruf dieses Objekts wird in der IndexedDB die Struktur für die lokale Datenbank erstellt.

Das zurückgegebene PouchDB-Objekt wird in einer Variablen gespeichert und bietet danach Zugriff auf die API der PouchDB.

[EVTL GRAFIK console.log(db.prototype) einblenden zur Veranschaulichung der Methoden]

```javascript
db.post(object, [options]);
```

Um Dokumente in der lokalen Datenbank zu speichern, wird die Funktion *PouchDB.post* benutzt. Der erste Parameter enthält das zu speichernde Objekt im JSON-Format. Im zweiten Parameter kann ein Objekt mit Optionen übergeben werden. Da fast alle Methoden der PouchDB API asynchrone Funktionsaufrufe sind, werden *Promises* zur Abwicklung der Rückgabewerte verwendet.

Im Abbildung X wird ein Dokument gespeichert und der Rückgabewert in der Konsole ausgegeben.

```javascript
db.post({name: 'Hallo FH Zweibruecken'})
    .then(function(response){ // then wird aufgerufen wenn post fertig ist
        console.log(response);
    });
```

Die Funktion, die letztendlich die Synchronisierung durchführt ist *PouchDB.replicate*. Bei dieser Methode wird im ersten Parameter die URL der entfernten Datenbank übergeben. Der zweite Parameter kann ein Objekt mit Optionen enthalten. Die für diese Arbeit relevanten Optionen sollen im Folgenden erklärt werden.

```javascript
db.replicate.from(remote, [options]);
db.replicate.to(remote, [options]);
```

```javascript
db.replicate.from(remote, {
    filter: filterFunction,
    doc_ids: [],
    complete: completeFunction,
    continuous: false,
});
```

**filter:**  
Ruft eine Funktion auf um Dokumente selektiv zu replizieren. Hiermit kann erreicht werden, dass nur Daten geladen werden die einen Zeitstempel aufweisen der nicht älter als 24 Stunden ist. Alternativ kann aber auch nur nach Dokumenten gesucht werden die einen bestimmten Wert enthalten.

**doc_ids:**
Repliziert nur Dokumente mit den angebenen *ids*.

**complete**
Wenn der Replikationsprozess fertig ist, wird diese Funktion aufgerufen. 

**continuous (seit PouchDB 2.0 *live*)**
Diese Option legt fest ob ein Prozess nur einmalig durchlaufen wird und danach beendet wird oder ob der Prozess fortlaufen auf Änderungen wartet.









