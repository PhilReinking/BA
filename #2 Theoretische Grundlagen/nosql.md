# Final Text

NoSQL ist im allgemeinen ein Begriff für Datenbanksysteme, die nicht dem Prinzip von relationalen Datenbanken folgen. Daten werden nicht relational gespeichert und es wird auch kein SQL als Abfragesprache verwendet.[Getting Started with NoSQL: S.7]

Der Bedarf an solchen Datenbankmodellen entstand Ende der Neunziger Jahre, als man an die Leistungsgrenze der bis dahin eingesetzten relationalen Systeme stieß. Ein stetiger Anstieg von Schreib- und Lesezugriffe, führte dazu, dass Datenbankoperationen nicht mehr effizient ausgeführt werden konnten. In der Regel werden bei relationalen Datenbanken sogenannte Transaktionen verwendet, welche bei Zugriff auf einen Datensatz, diesen Datensatz für andere Transaktionen sperren.

Der Vorteil von NoSQL Datenbanken ist vor allem die hohe Skalierbarkeit und Geschwindigkeit.[http://home.aubg.bg/students/ENL100/Cloud%20Computing/Research%20Paper/nosqldbs.pdf] 

In einem verteilten Datenbanksystem definiert Eric Brewer 3 Eigenschaften, welche für verschiedene Anwendungsfälle wichtig sein können: 'Consistency', 'Availability' und 'Partition tolerance'. Das CAP-Theorem besagt, das lediglich nur zwei dieser drei Eigenschaften in einem Datenbanksystem garantiert werden können.[http://www.cs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf] Im Falle von NoSQL sind 'Availability' und 'Partition tolerance' die herrausstechenden Merkmale.

    Abbildung CAP Theorem

# Annotations

Als Gegenstück zum ACID-Prinzip in relationalen Datenbanken, nutzen die meisten NoSQL-Systeme das BASE-Prinzip (Basically Available, Soft State, Eventual consistency).

nosql ist im allgemeinen ein begriff für datenbankensysteme, welche nicht dem relationalen datenbankschema folgen. Die Daten werden nicht relational gespeichert und es wird auch kein SQL als Abfragesprache verwendet.

Viel wichtiger als die ACID Transaktionen in RDBMS Applikationen ist bei solchen NoSQL-Systemen die Skalier- und Verfügbarkeit.

RDBMS: Fokus auf ACID (Atomicity, Consistency, Isolation, Durability)
NoSQL: Fokus auf Skalier- und Verfügbarkeit

Einführung von BASE: (Eric Brewer)

http://www.infoq.com/presentations/NoSQL-History

Basic availability
Soft state
Eventual consistency

[Getting Started With NoSQL]
NoSQL Features:
Schemaless data representation (schema flexibility)
Development time
Speed
Plan ahead for scalability

CAP Theorem (http://docs.couchdb.org/en/latest/intro/consistency.html#intro-consistency)
Consistency, Availability, Partition tolerance

#### "eventual consistency"

--> If availability is a priority, we can let clients write data to one node of the database without waiting for other nodes to come into agreement. If the database knows how to take care of reconciling these operations between nodes, we achieve a sort of “eventual consistency” in exchange for high availability. This is a surprisingly applicable trade-off for many applications.


(http://highscalability.com/blog/2013/5/1/myth-eric-brewer-on-why-banks-are-base-not-acid-availability.html)
Why? 1) Availability correlates with revenue and consistency generally does not. 2) Historically there was never an idea of perfect communication so everything was partitioned.

Da es in verteilten Systemen nicht immer sinnvoll ist, alle Replika konsistent zu halten, gibt es hier auch sogenannte schwache Konsistenz (englisch weak consistency), d. h. es werden keinerlei Konsistenzgarantien abgegeben, und die sogenannte eventual consistency, die besagt, dass ein Datensatz irgendwann konsistent sein wird, sofern nur eine hinreichend lange Zeit ohne Schreibvorgänge und Fehler vorausgesetzt werden kann.(http://de.wikipedia.org/wiki/Konsistenz_(Datenspeicherung))



##CouchDB (http://docs.couchdb.org/en/latest/intro/consistency.html#intro-consistency)
1.3.6. Incremental Replication
CouchDB’s operations take place within the context of a single document. As CouchDB achieves eventual consistency between multiple databases by using incremental replication you no longer have to worry about your database servers being able to stay in constant communication. Incremental replication is a process where document changes are periodically copied between servers. We are able to build what’s known as a shared nothing cluster of databases where each node is independent and self-sufficient, leaving no single point of contention across the system.

Need to scale out your CouchDB database cluster? Just throw in another server.

As illustrated in Figure 4. Incremental replication between CouchDB nodes, with CouchDB’s incremental replication, you can synchronize your data between any two databases however you like and whenever you like. After replication, each database is able to work independently.

You could use this feature to synchronize database servers within a cluster or between data centers using a job scheduler such as cron, or you could use it to synchronize data with your laptop for offline work as you travel. Each database can be used in the usual fashion, and changes between databases can be synchronized later in both directions.

Incremental replication between CouchDB nodes
Figure 4. Incremental replication between CouchDB nodes

What happens when you change the same document in two different databases and want to synchronize these with each other? CouchDB’s replication system comes with automatic conflict detection and resolution. When CouchDB detects that a document has been changed in both databases, it flags this document as being in conflict, much like they would be in a regular version control system.

This isn’t as troublesome as it might first sound. When two versions of a document conflict during replication, the winning version is saved as the most recent version in the document’s history. Instead of throwing the losing version away, as you might expect, CouchDB saves this as a previous version in the document’s history, so that you can access it if you need to. This happens automatically and consistently, so both databases will make exactly the same choice.

It is up to you to handle conflicts in a way that makes sense for your application. You can leave the chosen document versions in place, revert to the older version, or try to merge the two versions and save the result.