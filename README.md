COUCH-CLEANUP README
===

Usage
---
    $ couch-cleanup COUCHDB_ROOT_URL
COUCHDB_ROOT_URL should be a URL pointing to the root of the CouchDB server you 
want to perform cleanup actions upon, including admin credentials if your server
is not in *admin party* mode.

For example:

    $ couch-cleanup http://adminuser:pass@some.host:5984/

Why do I need this?
---
CouchDB databases and views use
[append-only data structures](http://wiki.apache.org/couchdb/Technical%20Overview#Document_Storage) 
on disk for reliability and MVCC. You'll likely want to compact your databases and
view indices, on occasion, to reclaim space from old revisions. In addition,
CouchDB doesn't delete old view index files, even after deleting the design document
that caused the view to be built.

This script will help you automate reclaiming this "wasted" disk space.

What specifically will this script do?
---
This script will compact each database, and remove unused view and CouchDB-Lucene
index files. The script also compacts the view indices of each design doc, and
optimizes any CouchDB-Lucene indexes it finds. Moreover, this script tries to wait
for each cleanup task to complete before beginning the next, so as not to bombard
the server with activity.

How to use this
---
Since the cleanup activities performed by this script can create a bunch of disk I/O,
I suggest launching this script during a period of minimal activity. For example, you
could schedule a cron job to execute this script in the middle of the night on the weekend.

Dependencies
---
Ruby and the JSON gem

Compatibility
---
This script is known to work with following versions of software:

* Ruby 1.8.7
* JSON gem 1.4.6
* CouchDB 1.0.1
