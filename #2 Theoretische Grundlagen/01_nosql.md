NoSQL ist im allgemeinen ein Begriff für Datenbanksysteme, die nicht dem Prinzip von relationalen Datenbanken folgen. Daten werden nicht relational gespeichert und es wird auch kein SQL als Abfragesprache verwendet.[Getting Started with NoSQL: S.7]

In der Regel werden bei relationalen Datenbanken sogenannte Transaktionen verwendet, welche bei Zugriff auf einen Datensatz, diesen Datensatz für andere Transaktionen sperren und dabei dem ACID-Prinzip folgen. NoSQL setzt dagegen auf eine andere Vorgehensweise, welche auch als BASE bekannt ist und von Eric Brewer definiert wurde:[Getting Started with NoSQL: S.10]

- Basic availability: Jede Anfrage erhält garantiert eine Antwort
- Soft state: Daten können sich auch ohne spezifische Eingaben verändern
- Eventual consistency: Die Datenbank kann zeitweise inkonsistent sein, kehrt dann aber zu einem konsistenten Status zurück

In einem verteilten Datenbanksystem definiert Eric Brewer 3 Eigenschaften, welche für verschiedene Anwendungsfälle wichtig sein können: 'Consistency', 'Availability' und 'Partition tolerance'. Das sogenannte CAP-Theorem besagt, das lediglich nur zwei dieser drei Eigenschaften in einem Datenbanksystem garantiert werden können.[http://www.cs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf] Im Falle von NoSQL sind 'Availability' und 'Partition tolerance' die herrausstechenden Merkmale.

    Abbildung CAP Theorem(http://docs.couchdb.org/en/latest/intro/consistency.html#intro-consistency)

Letztendlich sind die Vorteile von NoSQL Datenbanken vor allem hohe Skalierbarkeit und Geschwindigkeit.[http://home.aubg.bg/students/ENL100/Cloud%20Computing/Research%20Paper/nosqldbs.pdf] Desweiteren ist auch die oft schemalose Repräsentation und die damit verbundene Flexibilität von Datensätzen ein Grund für Entwickler auf NoSQL Datenbanken zu setzen, da hier Entwicklungszeit eingespart wird.

# Annotations

(http://highscalability.com/blog/2013/5/1/myth-eric-brewer-on-why-banks-are-base-not-acid-availability.html)
