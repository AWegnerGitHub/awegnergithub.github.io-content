Title: Fixing MYISAM Crashed Tables
Date: 2010-5-14 10:08
Tags: technical
Category: Technical Solutions
Slug: fixing-myisam-crashed-tables
Summary: How to fix MyISAM tables that are marked as crashed
Status: published

For various reasons, MyISAM tables are known to crash. When this happens, the following message will be displayed:

    INVALID SQL: 145 : Table '{something}' is marked as crashed and should be repaired

I've found this error occurs when MySQL is unexpectedly shut down - whether from a power failure to the entire server or
if MySQL itself has issues and you use the `kill` command to stop it. Unexpected shut downs, especially while these tables
are being used, do not make MyISAM tables happy.

To fix this, you need the ability to stop MySQL in a controlled manner, and you need to know where the database files
are stored.

    locate *.MYI

This will return where all the MYI files are stored. In this example, I am using

    /var/lib/mysql/mysql/

Go to the directory of the crashed table using the `cd` command. Next, stop MySQL. This is to ensure the tables are not
accessed while we perform our repair functions. If you don't perform this step, the repair may not succeed.

    service mysqld stop

Next we are going to perform two repair functions. The first one may take a while depending on the size of your tables.

    myisamchk -r --force --safe-recover *.MYI

The second repair step is used to ensure all table states are updated correctly and repair any minor indexing issues. It
is likely that this step is not needed after performing the previous step, but it should only take a few seconds now.

    myisamchk --force --fast --update-state *.MYI

Finally, restart MySQL

    service mysqld start

## Common Questions

### What does "Table is marked as crashed and should be repaired" mean?

It's a MySQL error (error 145) indicating a MyISAM table has become corrupted. It happens after MySQL shuts down unexpectedly, especially while the tables are in use. A server power failure does it, and so does using `kill` to stop MySQL.

### What causes MyISAM tables to crash?

Unexpected shutdowns while the tables are being used. That includes a power failure to the whole server, or MySQL itself having issues and being stopped with the `kill` command. Unexpected shutdowns while these tables are active do not make MyISAM happy.

### How do I repair a crashed MyISAM table?

Find where the table's files live with `locate *.MYI` and `cd` into that directory, then stop MySQL with `service mysqld stop` so nothing accesses the tables during the repair. Run `myisamchk -r --force --safe-recover *.MYI`, then `myisamchk --force --fast --update-state *.MYI`, and finally restart MySQL with `service mysqld start`.

### Why do I need to stop MySQL before repairing the tables?

Stopping MySQL ensures the tables aren't being accessed while the repair runs. If you skip this step and the tables are in use during the repair, the repair may not succeed.

### What's the difference between the two myisamchk commands?

The first, `myisamchk -r --force --safe-recover *.MYI`, performs the actual recovery and may take a while depending on table size. The second, `myisamchk --force --fast --update-state *.MYI`, makes sure the table states are updated and fixes any minor indexing issues; it usually isn't strictly necessary after the first step and only takes a few seconds.
