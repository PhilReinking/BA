http://blog.nahurst.com/visual-guide-to-nosql-systems
http://db-engines.com/de/system/CouchDB%3BMongoDB

## MongoDB http://docs.mongodb.org/manual/replication/
Written in C++
JSON Objects

Replikationsmechanismen:
Master-Slave Replikation

## CouchDB http://couchdb.apache.org/
Written in Erlang
Store your data with JSON documents. (--> JavaScript Objects)
RESTful HTTP/JSON API

Replikationsmechanismen:
Master-Master Replikation
Master-Slave Replikation

## PouchDB https://github.com/daleharvey/pouchdb/releases
PouchDB ist eine von CouchDB inspirierte JavaScript Datenbank, welche darauf ausgelegt ist im Browser zu laufen.

Release 1.1.0 am 02. Januar 2014
Release 2.0.0 am 01. März 2014

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
