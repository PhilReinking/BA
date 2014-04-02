# Mechanik der Daten-Synchronisierung

Die Applikation benötigt eine Mechanik, welche den Replikationsprozess zwischen der lokalen Datenbank und der CouchDB auslösen kann. Dazu ist es zum einen wichtig die mitgelieferten Methoden der PouchDB zu kennen, zum anderen muss aber auch darauf geachtet werden, dass der erdachte Mechanismus performant ist.

Es ist ebenfalls von Vorteil den Mechanismus so unabhängig wie möglich von der restlichen Applikationslogik zu gestalten. Im Detail bedeutet dies, dass die gesamte Logik zum Schreiben, Lesen und Synchronisieren von Daten als Modul für AngularJS programmiert wird.

Die erarbeitete Strategie trennt den Replikationsprozess in 2 Komponenten, das replizieren von der lokalen Datenbank zum Server und umgekehrt. Abbildung X  zeigt das zugehörige Aktivitätsdiagramm für diese beiden Komponenten.

[FLOWCHART REPLIKATION MECHANIK DOWN]
{REPLICATE.FROM(24HOURS)}
    onComplete: {REPLICATE.FROM(continuous)}

Die Synchronisierung vom Server zur lokalen Datenbank beinhaltet zwei Schritte und geht davon aus das eine aktive Verbindung zum Netzwerk besteht (ONLINE). Apps die zum ersten Mal gestartet werden haben natürlich noch keine lokale Kopie der Datenbank des Netzwerks. Um den Nutzer aber nicht zu lange auf aktuelle Datensätze warten zu lassen, denn diese würden normalerweise als letztes geladen werden, wird der initiale Replikationsprozess (bei App-Start) mit einem Filter versehen, welcher nur Dokumente vom Server synchronisiert, die in den letzten 24 Stunden gespeichert wurden. Die Zeitspanne wurde für das geplante Szenario auf 24 Stunden festgelegt, kann für andere Anwendungfälle jedoch unpassend sein und sollte dann dementsprechend geändert werden.

Nachdem die initiale Sychronisierung abgeschlossen ist, wird ein weiterer Replikationsprozess gestartet. Diesmal ohne Filter, jedoch als andauernder Prozess, welcher jede Änderung auf dem Server sofort registriert und die entsprechenden Daten auf die lokale Datenbank überträgt.

Durch dieses Vorgehen werden 2 Ziele erreicht, zum einen ein performanter Start für die Applikation, denn es werden zuerst die aktuellsten Daten geladen und zum anderen eine dauerhafte Synchronisierung aller Änderungen auf dem Server.

[FLOWCHART REPLIKATION MECHANIK UP]
{postDocument}
    online?: {REPLICATE.TO(id)}
    offline?: {saveToCache}
        eventOnline: {syncCache} -> {REPLICATE.TO(cache.ids)}

Abbildung X zeigt das Aktivitätsdiagramm zum Replizieren der Daten auf den Server.

Die zweite Komponente ist für das Hochladen der Daten auf den Server verantwortlich. Hier macht es wenig Sinn einen dauerhaften Replikationsprozess zu starten, weil neue Daten auf dem lokalen Gerät nur dann entstehen wenn der Nutzer aktiv einen Beitrag hinzufügt. Entgegen dem Vorgehen beim Herunterladen aktueller Daten, wird das Hochladen gezielt beim Speichern neuer Daten ausgelöst.

Nach diesem Vorgehen entsteht für die Offline-Funktionalität ein Hindernis, denn es gibt keine Methode die pauschal alle Daten in der lokalen Datenbank zum Server hochlädt. Neue Beiträge, die gespeichert werden während das Gerät *Offline* ist, würden so nicht zum Server repliziert werden. Die Lösung hier ist beim Speichern eines neuen Datensatzes direkt zwischen *Online* und *Offline* zu unterscheiden.

Ist das Gerät *Online*, so kann sofort die Methode zum Replizieren auf den Server ausgelöst werden.

Ist das Gerät jedoch *Offline*, wird eine Referenz auf das Dokument zwischengespeichert. Dieser Cache-Mechanismus erlaubt es so alle Änderungen nachträglich zum Server zu replizieren, nämlich genau dann wenn das Gerät wieder eine Netzwerkverbindung erlangt.

Der Nutzen dieses Vorgehens ist eine effiktivere Verwendung der Netzwerkapazitäten des Endgerätes. Durch den gezielten Einsatz der Replikation nur auf geänderte bzw neue Datensätze wird vermieden das alle anderen gespeicherten Daten in der lokalen Datenbank nochmals mit denen der Datenbank auf dem Server abgeglichen werden muss.







