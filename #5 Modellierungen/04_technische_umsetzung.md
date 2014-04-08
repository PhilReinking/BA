## Technische Umsetzung ppSyncService

### AngularJS Modul Aufbau
Der Aufbau des ppSyncService Moduls als Factory ist in ABBILDUNG zu sehen. Der Rückgabewert dieses Services ist ein Objekt mit Funktionen, die benötigt werden um mit der Datenbank zu interagieren, also Daten zu lesen und zu schreiben. Funktionen und Variablen die nicht von ausserhalb dieses Moduls zugänglich sein sollen, können innerhalb der anonymen Funktion der Factory erstellt werden.

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
    filter: syncFilter,
    batch_size: 1000,
    complete: continuousSync
  });
};
syncFromRemote();
```

### monitorNetwork (private)

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
```


```javascript
var cache = new Cache();
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

```javascript
fetchChanges: function() {
  var deferred = $q.defer();

  db.changes({
    continuous: true,
    since: 'latest',
    include_docs: true,
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

```javascript
syncCache: function() {
  syncCache();
}
```

### debug (public)

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

```javascript
reset: function() {
  PouchDB.destroy(dbname);
  db = new PouchDB(dbname);
}
```


## Technische Umsetzung Dashboard

```javascript

```

```html

```

## Technische Umsetzung Pinboard

```javascript

```

```html

```

## Technische Umsetzung Channel

```javascript

```

```html

```
