## Zusammenfassung

*Was wurde gemacht?*
In dieser Arbeit wurde mit Hilfe aktuellster Webtechnologien, eine für mobile Geräte funktionsfähige Plattform für soziale Extranets geschaffen. Es wurde gezeigt dass die Echtzeitsynchronisierung auch unter echten Bedingungen funktioniert. Die Nutzung von PhoneGap ermöglicht es zukünftigen, auf *BAnet* basierenden Projekten, gerätespezifische Funktionen von Smartphones zu benutzen.

*Was ist die besondere Leistung dieser Arbeit?*
Betrachet man die für den Betrieb des Social Extranets notwendige Serverinfrastruktur, so stellt man fest, dass diese minimal ist. Die einzige benötigte Komponente ist ein CouchDB Server. In der heutigen, von Debatten über Datenschutz und Privatsphäre geprägten Zeit, kann dieser Umstand nur als Vorteil angesehen werden. Da die Applikationslogik dezentral auf dem Smartphone oder in einem Browser ausgeführt wird und damit nicht erst über einen zentralen Server gesendet werden muss, kann man von einem für jedermann offenen und daher auch von jedermann veränderbarem System sprechen.

*Warum PhoneGap?*

Das World Wide Web hat schon viele technologische Evolutionen durchlaufen. Eine davon war der Schritt zum dynamischen Web 2.0, dank dem viele Software-Unternehmen den Erfolg fanden. 
Und bis heute hat sich das Web stetig in eine cloudbasierte Infrastruktur von Dienstleistungen und Applikationen weiterentwickelt.

Die nächste Evolution des Internets könnte schon kurz bevorstehen und den Einfluss mobiler Webseiten betreffen. Eine native Applikation hat im Hinblick auf die Leistungsfähigkeit zwar immer noch einen großen Vorsprung, jedoch nimmt dieser immer weiter ab. Durch den Einsatz von Frameworks wie PhoneGap wird auch der Vorteil des Zugriffs auf Kameras, GPS und Beschleunigungssensor ausgeglichen.

Webbasierte, und damit im Browser ausführbare Applikationen haben gegenüber nativen Apps darüber hinaus immer den Vorteil, nicht gezwungenermaßen installiert werden zu müssen. So ist es für ein angenehmes Nutzererlebnis positiv zu bewerten, wenn man in der Lage ist mit einem einfachen Link auf eine Anwendung zu verweisen, die sich direkt und ohne weiteres zutun im Browser öffnen kann.

*Die Zukunft von BAnet?*
Das während dieser Arbeit entstandende Konzept für eine Social Extranet Plattform weist ein großes Potenzial auf. Man kann sich ohne weiteres Vorstellen einfache Multiplayer-Spiele oder auch einfach nur interaktive Führungen durch Museen als Komponenten zu integrieren. Der Fantasie sind hier kaum Grenzen gesetzt. Der Quellcode des *ppSyncService* und *BAnet* ist frei verfügbar und kann damit von Entwicklern benutzt werden, um eigene Netzwerke zu erstellen.

Ein Problem das noch in Zukunft beseitigt werden muss, ist der Umgang mit Daten auf dem lokalen Gerät um eine Skalierbarkeit auch für größere Nutzergruppen zu ermöglichen. In dieser Arbeit lag der Fokus auf einer kleinen Anzahl von Nutzern, bei der die Größe der Datenbank innerhalb einer nur kurzen Zeit auch nicht sehr groß wurde und daher auch außen vor gelassen wurde. Bei einem größeren Netzwerk und über einen längeren Zeitraum fallen natürlich auch größere Datenmengen an, womit es auch keinen Sinn machen kann, auf jedem Gerät eine vollständige Kopie der Datenbank des Netzwerks zu halten. Hier muss ein Weg gefunden werden, eine Untermenge der Datenbank lokal speichern zu können.




