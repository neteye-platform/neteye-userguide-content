Elasticsearch Backup and Restore
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Elasticsearch provides snapshot functionality which is great for backups
because they can be restored relatively quickly.

The main features of Elasticsearch snapshots are:
 * They are incremental
 * They can store either individual indices or an entire cluster
 * They can be stored in a remote repository such as a shared file system

The destination for snapshots must be a shared file system mounted on
each Elasticsearch node.

For further details see the `Official Elasticsearch snapshot
documentation
<https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshot-restore.html>`__.
