# ppnet

[Projekt auf Github](https://github.com/pixelpark/ppnet)

## CouchDB

*CouchDB is a database that completely embraces the web. Store your data with JSON documents. Access your documents with your web browser, via HTTP. Query, combine, and transform your documents with JavaScript. CouchDB works well with modern web and mobile apps. You can even serve web apps directly out of CouchDB. And you can distribute your data, or your apps, efficiently using CouchDB’s incremental replication. CouchDB supports master-master setups with automatic conflict detection.*

*CouchDB comes with a suite of features, such as on-the-fly document transformation and real-time change notifications, that makes web app development a breeze. It even comes with an easy to use web administration console. You guessed it, served up directly out of CouchDB! We care a lot about distributed scaling. CouchDB is highly available and partition tolerant, but is also eventually consistent. And we care a lot about your data. CouchDB has a fault-tolerant storage engine that puts the safety of your data first.*

"Datenbank, welche auf dem Server läuft."

## PouchDB

*PouchDB is an Open Source JavaScript Database inspired by Apache CouchDB that is designed to run well within the browser.*

*PouchDB was created to help web developers build applications that work equally as well offline as they do online. It enables applications to store data locally while offline, and synchronise it with CouchDB and compatible servers when the application is back online, keeping the user's data in sync no matter where they next login.*

"Datenbank, welche auf dem lokalen Gerät läuft."

## Probleme bei der Replikation

Bei der initialen Datenbank-Replikation von *remote* zu *local* wird die komplette Datenbank in einzelnen Requests geladen.

Gelöschte Elemente und alte Revisionen werden ebenfalls geladen, was dazu führt das die Ladezeit ansteigt.

Alte Revisionen können durch *Compaction* gelöscht werden.  
Konfigurationsbeispiel für CouchDB:

`_default = [{db_fragmentation, "70%"}, {view_fragmentation, "60%"}, {from, "23:00"}, {to, "04:00"}]`

Beim Erstellen der PouchDB kann die Option `{auto_compaction: true}` gesetzt werden. Diese Funktion ist aber sehr CPU-lastig und verlangsamt die App während die *Compaction* durchgeführt wird.

### Ablauf der Replikation

Zur Zeit werden 2 Datenbank-Funktionen aufgerufen um den dauerhaften Replikationsprozess zu starten.

```
var sync = function(){
	var opts = {
		continuous: true
	};
	$scope.db.replicate.from(remoteCouch, opts);
	$scope.db.replicate.to(remoteCouch, opts);
};
```
Der größte Nachteil ist vor allem die erste Replikatioen von der serverseitigen CouchDB in die lokale PouchDB.

Ein weiterer Nachteil ist dass beide Funktionen parallell ausgeführt werden. `db.replicate.to` wird jedoch nur benötigt wenn lokale Änderungen gemacht wurden. (Könnte nur dann aufgerufen werden, wenn ein `db.put` oder `db.post` ausgeführt wird).

### Optimierter Ablauf der Replikation

Bei Programmstart wird die Synchronisierung der serverseitigen DB zur lokalen DB mit einem Filter gestartet. (z.B. nur Posts der letzten 24 Stunden)

Wenn die Synchronisierung abgeschlossen ist kann damit begonnen Änderungen der lokalen Datenbank zum Server zu replizieren.

Ist auch diese Replikation abgeschlossen, wird wieder die Funktion `db.replicate.from` mit der Option `{continuous: true}` gestartet, welche dann dauerhaft auf Änderungen in der Datenbank achtet.

Die Funktion `db.replicate.to` sollte nur dann ausgelöst werden, wenn auch wirklich Änderungen in der lokalen Datenbank stattgefunden haben.
