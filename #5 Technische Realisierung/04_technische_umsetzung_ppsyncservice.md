## Technische Umsetzung ppSyncService

### AngularJS Modul Aufbau

Der grundlegende Aufbau des ppSyncService Moduls als Factory ist in ABBILDUNG zu sehen. Der Rückgabewert dieses Services ist ein Objekt mit Funktionen, die benötigt werden um mit der Datenbank zu interagieren. Funktionen und Variablen die nicht ausserhalb dieses Moduls zugänglich sein sollen, werden innerhalb der anonymen Funktion der Factory erstellt werden.

```javascript
var ppSync = angular.module('ppSync', ['ng']);
ppSync.factory('ppSyncService', function($q, $window) {
    // Code zur Initialisierung der Klasse
    // oder private Funktionen
    
    return {
        // Objekt mit den Datenbank Funktionen
    }
});
```

### Initialisierung (private)

Hier erfolgt die Initialsierung der von ppSyncService benötigten Variablen. Die Variable *dbname* enthält den Namen der Datenbank, welche zur Verwendung mit der PouchDB auf dem lokalen Gerät verwendet werden soll. Die Serveradresse eines CouchDB Servers befindet sich in der Variablen *remote*. Der lokale Datenbankname wird an die Serveradresse angehängt um die Datenbankbezeichnung auf Server und lokalen Gerät konsistent zu halten.  
Das wohl wichtigste Objekt des Services, wird in der Variablen *db* gespeichert. Hier wird ein PouchDB Objekt mit dem vorher definierten Datenbanknamen erstellt. Die Option Option *auto_compation: true* sorgt dafür das PouchDB die lokale Datenbank automatisch säubert.

```javascript
var dbname = 'banet_default';
// var remote = 'http://127.0.0.1:5984/' + dbname;
var remote = 'http://philreinking.de:5984/' + dbname;

/* Create the PouchDB object */
var db = new PouchDB(dbname, {
  auto_compaction: true
});
```

### syncFromRemote (private)

Die Funktion syncFromRemote repliziert die Daten des Servers in die lokale Datenbank. Dazu werden innerhalb von syncFromRemote zwei weitere Funktionen deklariert, um den Prozess wie in der Planung durchführen zu können.  

Dabei ist *syncFilter* die Filterfunktion, welche in den Optionen der initialen Replikation übergeben wird, um nur die Beiträge der letzten 24 Stunden zu laden.  
In *syncFilter* wird ein Unix-Timestamp *timeBarrier* erstellt, der genau 24 Stunden alt ist. Danach wird überprüft ob der Timestamp des Dokuments größer ist, als der zuvor Berechnete Wert und gibt in diesem Fall *true* zurück, was den Replikationsprozess dazu veranlasst das Dokument zu replizieren.

Die *continuousSync* Funktion wird in der Option *complete* übergeben und gestartet wenn *PouchDB.replicate* beendet ist. In *continuousSync* findet ebenfalls ein Aufruf von *PouchDB.replicate* statt, jedoch ohne die zuvor verwendeten Optionen filter und complete, aber mit *continuous: true*.

```javascript
var syncFromRemote = function() {
  // Fiterfunktion 24 Stunden
  var syncFilter = function(doc) {
    var timeBarrier = Date.now() - (86400 * 1000);
    return doc.created > timeBarrier ? true : false;
  };
  // Funktion zur dauerhaften Synchronisierung
  var continuousSync = function() {
    setTimeout(function() {
      PouchDB.replicate(remote, dbname, {
        continuous: true
      });
    }, 1000);
  };
  // Initiale Replikation mit Filter
  PouchDB.replicate(remote, dbname, {
    continuous: false,
    filter: syncFilter
    complete: continuousSync
  });
};
syncFromRemote();
```

### monitorNetwork (private)

Um festzustellen wann die Applikation Online oder Offline ist, wird für ppSyncService eine Variable erstellt die den aktuellen Status beinhaltet und über die Funktion monitorNetwork aktualisiert wird.

Es werden innerhalb dieser Funktion EventListener für die Events *offline* und *online* registriert, die beim Eintreten dieser Events die Variable network mit dem entsprechenden Status versehen.

Beim Event *online* werden die Funktionen syncCache und syncFromRemote aufgerufen um den Replikationsprozess neuzustarten, welche ohne aktive Verbindung zuvor beendet wurde.

```javascript
var network = 'online';
var monitorNetwork = function() {
  // Prüfe den Verbindungsstatus zum Netzwerk
  if ('onLine' in navigator) {
    $window.addEventListener('offline', function() {
      network = 'offline';
    });
    $window.addEventListener('online', function() {
      network = 'online';
      syncCache(); // Push Cache
      syncFromRemote(); // Restart Replication
    });
  }
};
monitorNetwork();
```

### Cache (private)

Der Cache ist als Objekt mit eigenen Methoden angelegt, um die Verwaltung des Caches so einfach wie möglich zu machen.

Das Attribut docs ist ein Array, in welches die Ids der Dokumente abgelegt werden können, die zu einem späteren Zeitpunkt repliziert werden sollen.

Die Methode addDoc erwartet eine Id als Parameter und speichert diese in das Array. Um den Cache dauerhaft zu speichern, wird dieses Array im LocalStorage abgelegt.

Mit getDocs wird das Array aus dem LocalStorage zurückgegeben und reset löscht den Cache.

```javascript
function Cache() {
  this.docs = [];
}
Cache.prototype = {
  addDoc: function(docId) {
    if (JSON.parse(localStorage.getItem('cache')) !== null) {
      this.docs = JSON.parse(localStorage.getItem('cache'));
    }
    this.docs.push(docId);
    localStorage.setItem('cache', JSON.stringify(this.docs));
  },
  getDocs: function(){
    return JSON.parse(localStorage.getItem('cache'));
  },
  reset: function() {
    this.docs = [];
    localStorage.setItem('cache', JSON.stringify(this.docs));
  }
};
var cache = new Cache();
```

Desweiteren gibt es noch die Funktion syncCache, welche einen Replikationsprozess startet und nur die Dokumente zum Server synchronisiert, die sich im Cache befinden. Am Ende dieses Prozesses wird überprüft ob alle Dokumente hochgeladen wurden und der Cache geleert.

```javascript
var syncCache = function() {
  db.replicate.to(remote, {
    doc_ids: cache.getDocs(),
    complete: function(err, response) {
      if (response.docs_read === cache.getDocs().length) {
        cache.reset();
      }
    }
  });
};
```

### fetchChanges (public)

syncFromRemote repliziert dauerhaft neue Daten vom Server in die lokale Datenbank. Um diese neuen Daten zu verwerten, wird die Funktion fetchChanges zur Verfügung gestellt.

Über *PouchDB.changes()* wird ein Prozess gestartet der bei jeder neuen Änderung in der lokalen Datenbank die *onChange* Funktion auslöst.

Der Rückgabewert von fetchChanges ist ein mithilfe des AngularJS Services $q erstellter Promise. Über *deferred.notify(change)* wird die registrierte Änderung über den Promise an den Funktionsaufruf weitergeleitet und kann dort weiterverarbeitet werden.

```javascript
fetchChanges: function() {
  var deferred = $q.defer();

  db.changes({
    continuous: true,
    since: 'latest', // Nur neue Änderungen
    include_docs: true, // Die Inhalte der Dokumente einbinden
    complete: function(err, changes) {
      deferred.resolve(changes);
      deferred.reject(err);
    },
    onChange: function(change) {
      deferred.notify(change);
    }
  });

  return deferred.promise;
}
```

### postDocument (public)

Mit postDocument kann ein Dokument in der Datenbank gespeichert werden. Die Funktion erwartet ein JSON-Objekt als Parameter.

Mit *PouchDB.post(obj)* wird das Dokument zunächst in die lokale Datenbank gespeichert. In der Callback Funktion *then* wird dann der aktuelle Status der Netzwerkverbindung überprüft.
Im Falle, dass das Gerät online ist, wird das neu gespeicherte Dokument zum Server repliziert. Ist das Gerät jedoch offline, muss die Id des neuen Dokuments über *cache.addDoc()* in den Cache gespeichert werden.

Der Rückgabewert von postDocument ist ein Promise welche bei Erfolg die id des neuen Dokuments enthält.

```javascript
postDocument: function(obj) {
  var deferred = $q.defer();

  db.post(obj).then(function(response) {
    if (network === 'online') {
      db.replicate.to(remote, {
        doc_ids: [response.id],
        complete: function() {
          deferred.resolve(response.id);
        }
      });
    } else {
      deferred.resolve(response.id);
      cache.addDoc(response.id);
    }
  }).catch (function(error) {
    deferred.reject(error);
  });

  return deferred.promise;
}
```

### deleteDocument (public)

Mit deleteDocument kann ein Dokument aus der Datenbank gelöscht werden. Damit diese Löschung auch auf anderen Geräten registriert wird, kann das Dokument nur als *deleted* markiert werden, um dann auf den Server synchronisiert zu werden.

Als Parameter wird das zu löschende Dokument mit *id* und *rev* erwartet und optional ein boolescher Wert ob die Änderung im Cache gespeichert oder direkt synchronisiert werden soll.

Das Löschen wird mit *PouchDB.remove()* durchgeführt. Wie beim Erstellen eines Dokuments erfolgt in der Callback Funktion eine Abfrage, ob die Änderungen direkt auf den Server repliziert werden kann, oder aber ob das Dokument im Cache gespeichert werden muss.

```javascript
deleteDocument: function(doc, push) {
  var deferred = $q.defer();

  push = typeof push !== 'undefined' ? push : true;

  db.remove(doc, {}, function(err, response) {
    if (push) {
      db.replicate.to(remote, {
        doc_ids: [response.id],
        complete: function() {
          deferred.resolve(response.id);
        }
      });
    } else {
      cache.addDoc(response.id);
      deferred.resolve(response.id);
    }
  });

  return deferred.promise;
}
```

### getDocument (public)

Mit dieser Funktion wird ein einzelnes Dokument aus der Datenbank geladen. Als einziger Parameter muss die entsprechende Id des Dokuments übergeben werden.

Der Rückgabewert ist ein Promise mit dem vollständigen Dokument als Inhalt.

```javascript
getDocument: function(docId) {
  var deferred = $q.defer();

  db.get(docId).then(function(doc) {
    deferred.resolve(doc);
  });

  return deferred.promise;
}
```

### getDocuments (public)

Um alle Dokumente aus der Datenbank zu laden, wird die Funktion getDocuments benötigt. Als Parameter kann ein Array mit den Dokumententypen übergeben werden, die ausgelesen werden sollen. Die Reihenfolge der Werte in dem Array bestimmt auch die Reihenfolge in welcher die Dokumente mithilfe der Map-Funktion angeordnet werden.

*documentTypes = ['POST', 'COMMENT']* würde zur Folge haben das Dokumente vom Typ *POST* einen key in der Form [doc.id, 0] und Dokumente vom Typ 'COMMENT' einen key in der Form [posting.id, 1] haben.

Für das Auslesen wird *PouchDB.query()* verwendet. Die Optionen in *queryOptions* bewirken das die Dokumente mit Inhalten und in absteigender Reihenfolge geladen werden.

Der Rückgabewert ist ein Promise mit einem Array der ausgelesen Dokumente.

```javascript
getDocuments: function(documentTypes) {
  var deferred = $q.defer();

  documentTypes = typeof documentTypes !== 'undefined' ? documentTypes : 'uncategorized';

  var queryOptions = {
    descending: true,
    include_docs: true,
  };

  db.query(function(doc, emit) {
    if (documentTypes === 'uncategorized') {
      emit([doc, 0], doc.created);
    } else {
      // compare each type in the array with the current queried doc
      for (var i = 0; i < documentTypes.length; i++) {
        if (doc.type === documentTypes[i]) {
          // The POST type is kind of special because it relates to no other document
          if (doc.type === 'POST') {
            emit([doc._id, i], doc.created);
          } else {
            emit([doc.posting, i], doc.created);
          }
          break;
        }
      }
    }
  }, queryOptions, function(error, response) {
    deferred.resolve(response.rows);
  });

  return deferred.promise;
}
```

### getChannelStream (public)

Die Funktion getChannelStream ist eine alternative Implementierung zu getDocuments. 

Zum einen werden nur Dokumente geladen, die dem Wert im Parameter *channel* entsprechen. Zusätzlich ist es über numberOfPosts möglich, die Anzahl der Dokumente zu begrenzen und mit end kann ein key bestimmt werden ab dem Dokumente geladen werden. Damit ist es möglich eine Paginierung zu implementieren.

In den queryOptions befinden sich die zwei zusätzlichen Optionen zur Eingrenzung der Dokumente.

```javascript
getChannelStream: function(channel, numberOfPosts, end) {
  var deferred = $q.defer();

  numberOfPosts = typeof numberOfPosts !== 'undefined' ? numberOfPosts : 50;
  end = typeof end !== 'undefined' ? end : [Date.now(), 0];
  channel = typeof channel !== 'undefined' ? channel : 'uncategorized';

  var queryOptions = {
    descending: true,
    limit: numberOfPosts,
    include_docs: true,
    endkey: end
  };

  db.query(function(doc, emit) {
    if (doc.channel === channel) {
      emit([doc.created, 0], doc.created);
    }
  }, queryOptions, function(error, response) {
    deferred.resolve(response.rows);
  });

  return deferred.promise;
}
```

### syncCache (public)

Mit syncCache kann der Mechanismus zum Synchronisieren des Caches manuell ausgelöst werden.

```javascript
syncCache: function() {
  syncCache();
}
```

### debug (public)

Die Funktion debug gibt mithilfe von *PouchDB.info()* aktuelle Informationen über die lokale Datenbank zurück.

```javascript
debug: function() {
  var deferred = $q.defer();

  db.info().then(function(response) {
    deferred.resolve(response);
  }).catch (function(error) {
    console.log(error);
  });

  return deferred.promise;
}
```

### reset (public)

Zum Löschen der lokalen Datenbank kann die Funktion reset aufgerufen werden. Die Datenbank wird zerstört und ein neues PouchDB Objekt erstellt.

```javascript
reset: function() {
  PouchDB.destroy(dbname);
  db = new PouchDB(dbname);
}
```
