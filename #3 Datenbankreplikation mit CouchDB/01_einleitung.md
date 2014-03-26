Die Entscheidung für ein passendes Datenbanksystem ist für das Ergebnis dieser Arbeit ein wichtiger Faktor. Es gibt zahlreiche Hersteller und damit verbunden eine sehr große Anzahl von entsprechenden Lösungen. Im Folgenden werden die Anforderungen an die Datenbank definiert und die Auswahl für die in dieser Arbeit verwendeten Lösung begründet.

## Analyse der Anforderungen
Die Anforderungen an die Datenbank werden aus einem geplanten Testszenario hergeleitet, welches im Laufe dieser Arbeit an einer Schule mit ca 15 Testpersonen durchgeführt wird.

```
Die Applikation soll für maximal 15 Person ermöglichen, Statusmeldungen in Form von Text über das Netzwerk in Echtzeit auszutauschen. Das Experiment wird an einem Ort mit mangelhaftem Mobilfunk durchgeführt, sodass Verbindunsabbrüche zum Netzwerk auftreten können.
```

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
    2. Alle relevanten Daten aus dem Netzwerk werden zur Offline-Nutzung auf das Gerät gespeichert.
    3. Statusmeldungen werden angezeigt.
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
    Es wurden Statusmeldungen nur lokal gespeichert; Während der Offline-Nutzung wurden im Netzwerk neue Statusmeldungen versendet
Beschreibung:
    1. Das Teilnehmergerät verbindet sich mit dem Internet
    2. Zuvor nur lokal gespeicherte Statusmeldungen werden über das Netzwerk synchronisiert.
    3. Neue Statusmeldungen aus dem Netzwerk werden auf dem Gerät gespeichert und angezeigt.
Nachbedinung:
    Alle neuen Statusmeldungen wurden über das Netzwerk übertragen.
```
