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







