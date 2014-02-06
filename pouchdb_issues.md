PouchDB Issue:

[https://github.com/daleharvey/pouchdb/issues/1240](https://github.com/daleharvey/pouchdb/issues/1240)

Replication checkpoint error #1240

First Solution from @ig3

[https://github.com/ig3/pouchdb/commit/ccedd13598d20d8869589bb498c2cc6d9a93439b](https://github.com/ig3/pouchdb/commit/ccedd13598d20d8869589bb498c2cc6d9a93439b)



## Explanation from ig3

This change adds a test for replication checksum errors following an
interruption of or error in replication and corrects the tested behaviour.

This is a long note because this is a significant change: more so than I
had anticipated when I started, and it should, therefore, not be accepted
without careful consideration. I have tried to document some of the
motivations and benefits of the re-implementation.

The fundamental fault was that the internal last_seq was updated in
replicate's onChange, as soon as the change notification is received and
before the document is replicated. After a subsequent write of any document,
the checkpoint is updated to the value in last_seq. Because of the order of
processing the changes, the checkpoint is typically updated before the
corresponding documents are replicated. If replication does not complete (e.g.
cancelled, source database goes unavailable, etc.) then the document(s) may
never be written. Subsequent replications will see the incorrect checkpoint and
will not replicate the failed document(s), leaving the databases permanently
inconsistent.

The problem was exacerbated by unbounded RequestManager queue length, with
a tendency to long queues and processing all or the vast majority of changes
before writing documents to the target database. In ad-hoc test cases, there
were hundreds to thousands of documents queued to be read and/or written in
the process queue, leaving long windows during which a failure or interruption
would cause an inconsistent result. Ad-hoc testing by closing the browser tab
before replication completed often resulted in only a few hundred documents of
a database of several thousand documents being replicated, but the checkpoint
indicating they had all be replicated.

The problem was further exacerbated by lack of error detection in replicate,
when reading and writing documents.

The fundamental requirements to correct the problem is to update the checkpoint
only after the relevant documents have been written to the target database.

While a 'simple' change to add state so that the checkpoint could be set
correctly after a document is written might have sufficed, there were other
issues which motivated a more fundamental revision:

Long latency from reading to writing documents. The queueing system tended to
read large numbers of documents before writing any. From start of a replication
to a new database, there tended to be a long lag before any documents were
written. A user frequently refreshing the page might result in no updates to
the database but intense processor and network activity, almost all reads in
preparation for writes that were never done.

Unbounded size of bulk writes. The number of documents read before the next
bulkWrite was not bounded and depended on the timing of async operations and
the order in which requests were queued. If the replication was interrupted
for any reason, all that processing was wasted.

There were several issues related to replication outstanding which were more
or less related to these issues: 1087, 1102, 942, 793 and 380 were noticed
and briefly considered.

The interleaving and overlap of processing of unrelated changes made optimal
handling of errors difficult. The simple handling is to abort all incomplete
work, but this might cause work on hundreds of changes to be aborted due to
a problem with a change much later in the change sequence. Otherwise, it
would be necessary to filter all subsequent processing based on the sequence
number at which the error occurred.

Adding multiple process queues or prioritization of requests in the queue
might have mitigated some of the issues, but this is a non-trivial change.

The solution I have implemented is to re-implement the queue, to batch changes
(with a default of one change per batch - the current norm) and to process
batches to completion in change sequence order.

This minimizes latency from reading document meta data and contents to
completion of processing of the related change and update of the checkpoint.

Checkpoint update is strictly after successful writing of the documents to
the target database.

Checkpoint is updated if there are no missing revisions for a batch
of changes. This addresses issues #1102 and 1087, at least in part.

Replication is aborted if errors are passed to the callbacks for get and
bulkWrite. This addresses issue #942, at least in part.

The complete callback is called with an error and result.ok is set to false
if replication is aborted on any error. This addresses issues #793 and #380,
at least in part.

With replicate now more strict and aborting on errors, an error in changes
for LevelDB (at least) was disclosed: the changes feed calls the complete
callback before the (continuous) change is cancelled and then calls the
onChange callback again if there are subsequent changes. In test Test basic
continuous pull replication, this results in the sequence: onChange,
onChange, onChange, complete, onChange. The new implementation of
replicate ignores the last call to onChange because it comes after
the call to complete. It might be better to ignore the call to complete
on a continuous replication but this seems inappropriate, at least for
an initial implementation. A separate issue (#1267) has been raised
for this and a change to utils.processChanges to fix it is included here.