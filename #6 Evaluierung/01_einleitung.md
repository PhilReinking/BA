## Probleme während der Entwicklung

Die Entwicklung einer effizienten Methode zur Synchronisierung der Datenbank stellte eine große Herausforderung dar. Es galt zu beachten, dass die Applikation mit dem Fokus auf mobile Datennutzung ohne größere Einschränkungen benutzbar ist. Es ergab sich jedoch schnell das Problem, dass beim erstmaligen Starten der App eine lange Zeit benötigt wurde um alle Datensätze in die lokale Datenbank zu spiegeln.

Zur gleichen Zeit konnten die Entwickler der PouchDB große Fortschritte in der Implementierung ihrer Replikationsfunktionen aufweisen. Die in dieser Arbeit erlangten Erkenntnisse über die Performanceprobleme der Replikation, konnten im Austausch mit den PouchDB Entwicklern erheblich verbessert werden. So ist unter anderem in Version 2.0 der PouchDB, die Option zur Batch-Replikation hinzugefügt worden.

Ein anderes Problem enstand beim Synchronisieren der lokalen Daten auf den zentralen Server. Wenn der dauerhafte Replikationsprozess *PouchDB.replicat.to(remote)* gestartet wurde, mussten alle lokalen Daten mit denen des Servers abgeglichen werden, was zu einer erheblichen Belastung der Datenverbindung führte. Durch das gezielte Starten dieser Synchroniserung und die Einführung des Caches konnte jedoch auch dieses Problem behoben werden.

## Erweiterbarkeit und Weiterentwicklung

Durch das konsequente Einhalten einer modularen Programmstruktur, ist es gelungen, eine erweiterbare Social Extranet Plattform zu entwickeln. Die Pixelpark AG konnte erfolgreich eine um zusätzliche Komponenten angereicherte Applikation erstellen.

Der Einstieg zur Entwicklung eigener Komponenten, wird durch die 2 in *BAnet* enthaltenen Basiskomponenten, auch für fremde Entwickler, erleichtert.

## Cologne Carnival Experiment

Am 4. März 2014 fand in Kooperation mit einem Kölner Gymnasium und im Rahmen des FI-Content 2 Programms das *Cologne Carnival Experiment* statt. Dabei bekamen 15 Schüler eines Informatikkurses die Aufgabe einen Karnevalsumzug zu besuchen und über einen Prototypen der Social Extranet Applikation miteinander zu kommunizieren.

Die Schüler hatten insgesamt 3 Stunden Zeit und wurden jeweils mit einem Smartphone **Google Nexus 5, Android 4.4.2** ausgestattet.

Der verwendetete Prototyp der Social Extranet Applikation, hier *ppNet* genannt, enthielt neben der in dieser Arbeit erstellten Basiskomponenten, zusätzliche Funktionen. Alle Komponenten und Funktionen nutzten den Datenbankservice ppSyncService um die Daten im Netzwerk zu synchronisieren.

**Stand der Basis Applikation:**
- ppSyncService (Datenbank online/offline und Synchronisierung)
- Pinboard (Beiträge speichern und löschen)

**Stand der erweiterten Applikation durch Pixelpark:**
- Beiträge kommentieren und favorisieren
- Bilder hochladen (Base64 kodiert)
- Integration von Beiträgen in OpenStreetMap

Zum Ende des Experiments wurden die Schüler gebeten einen Fragebogen zu beantworten. Die Auswertung der Antworten erfolgte unmittelbar nach dem Experiment.

### Fragen und Ergebnis

Es konnten Noten von 1 (sehr gut) - 6 (ungenügend) vergeben werden.

*Wie beurteilst du Applikation?*
2.1

*Welche weiteren Komponenten könntest du dir für eine Social Extranet Plattform vorstellen?*
- integrierte Spiele
- Live Streams
- eine alternative zu YouTube

*Wie gut war der Karnevalsumzug für das Testen der App geeignet?*
2.8

*Zu welchen Events könntet ihr Euch vorstellen die App zu nutzen?*
- GamesCom
- Freizeitpark
- Sportevent
- Konzert

*Wie gut hat die App allgemein funktioniert?*
2.2

*Wie gut hat die App offline funtkioniert?*
2.1

*Wie beurteilst du das Interface der App?*
1.7

### Fazit

Die Applikation wurde von den Schülern sehr gut angenommen, obwohl noch einige technische Probleme auftraten. Insbesondere verursachte ein Fehler in dem Datenbankmodul ppSyncService, dass der Replikationsprozess beendet wurde und keine Daten mehr gesendet werden konnten. Durch neustarten der Applikation konnten die Schüler jedoch dieses Problem umgehen.


