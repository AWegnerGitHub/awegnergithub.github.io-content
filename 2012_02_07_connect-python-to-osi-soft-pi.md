Title: Connect Python to OSI Soft PI
Date: 2012-2-07 12:45
Tags: technical
Category: Technical Solutions
Slug: connect-python-to-osi-soft-pi
Summary: How I connected Python to OSI Soft PI
Status: published
modified: April 16, 2012


OSI PI is a historian database. I had a task to connect a python application to this database. I asked a question on 
[Stack Overflow][1] about whether this was a simple problem to solve. After two weeks I still hadn't gotten a viable response,
so I had to build by own solution.
 
I did reach out to the vendor first for help. Their response back was not helpful. 

 > Looks like pyodbc is written against ODBC 3.x.  The OSI PI ODBC driver is using ODBC 2.0.  The python ODBC driver manager 
 > will convert most ODBC 3 calls on the fly to ODBC 2 ones. Anything added to 3, however, will obviously fail. You would 
 > need to find some way to make sure that your only using 2.0 compliant ODBC calls.  Currently their is not a PI ODBC 
 > driver that is compliant with ODBC 3.0.

So, it looks like the vendor doesn't support Python (odd, they are named "PI", but I digress). Additionally, the drivers provided 
by the company initially didn't work. 

The code below shows how I was able to finally connect python to OSI PI. It may not be the most elegant, but it 
functions for the purposes of my application. Initially I was attempting to connect using the [pyodbc][2] module. Unfortunately, 
OSI PI would return a message like this:

    pyodbc.Error: ('IM002', "[IM002] [OSI][PI ODBC][PI]PI-API Error <pilg_getdefserverinfo> 0 (0) (SQLDriverConnectW); [01000] [Microsoft][ODBC Driver Manager] The driver doesn't support the version of ODBC behavior that the application requested (see SQLSetEnvAttr). (0)")

They vendor mentioned that using OLEDB instead may prove more fruitful. Thus, the code below is how I got connected using 
the vendor provided OLDEB driver. The downside is that I also had to do this all through COM objects using [win32com][3]. 
I'm not knocking the module, because it is extremely useful and I've done some great things with it.

    from win32com.client import Dispatch
    
    oConn = Dispatch('ADODB.Connection')
    oRS = Dispatch('ADODB.RecordSet')
    
    oConn.ConnectionString = "Provider=PIOLEDB;Data Source=<server>;User ID=<username>;database=<database>;Password=<password>"
    oConn.Open()
    
    if oConn.State == 0:
        print "We've connected to the database."
        db_cmd = """SELECT tag FROM pipoint WHERE tag LIKE 'TAG0001%'"""
        oRS.ActiveConnection = oConn
        oRS.Open(db_cmd)
        while not oRS.EOF:
            #print oRS.Fields.Item("tag").Value   # Ability to print by a field name
            print oRS.Fields.Item(0).Value        # Ability to print by a field location
            oRS.MoveNext()
        oRS.Close()
        oRS = None
    else:
        print "Not connected"
    
    if oConn.State == 0:
        oConn.Close()
    oConn = None

I followed up on my Stack Overflow post about 2 months after posting my solution with the following note:

 > Just following up on this after using it for a couple months. This is still the only way I've found to do this with 
 > python, but it seems to be very slow when I need to run a large number of queries. I suspect it is because I have to 
 > open/close the database connection for each query, but OSI PI/ADODB complains if I do not. Performance has not reached a 
 > point where I am forced to rewrite this yet. If/when I do I will follow up again. In the meantime others using this 
 > solution should be aware that it is slow when running many queries.


 [1]: http://stackoverflow.com/questions/8898114/how-can-i-connect-python-2-6-to-osi-pi
 [2]: https://github.com/mkleehammer/pyodbc
 [3]: http://python.net/crew/skippy/win32/Downloads.html