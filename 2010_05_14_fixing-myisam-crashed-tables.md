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
    