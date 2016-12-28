

```
>>> try:
...     import sqlite3
... except:
...     from pysqlite2 import dbapi2 as sqlite3
...
>>> import json

>>> connection = sqlite3.connect("alive.db")
>>> cursor = connection.cursor()
>>> cursor.execute('CREATE TABLE names (name VARCHAR(20) PRIMARY KEY, state integer, ip VARCHAR(40), host_ip VARCHAR(40), uptime integer, downtime integer)')
<sqlite3.Cursor object at 0x10b0221f0>
>>> cursor.execute('INSERT INTO names VALUES ("worker_0", 1, "192.168.100.70", "10.10.10.213", 0, 0)')
<sqlite3.Cursor object at 0x10b0221f0>
>>> cursor.execute('INSERT INTO names VALUES ("worker_1", 1, "192.168.100.71", "10.10.10.213", 0, 0)')
<sqlite3.Cursor object at 0x10b0221f0>
>>> cursor.execute('INSERT INTO names VALUES ("worker_2", 0, "192.168.100.72", "10.10.10.213", 0, 0)')
<sqlite3.Cursor object at 0x10b0221f0>
>>> cursor.lastrowid
>>> connection.commit()
>>> cursor.execute('SELECT * FROM names')
<sqlite3.Cursor object at 0x10b0221f0>
>>> print cursor.fetchall()
[(u'worker_0', 1, u'192.168.100.70', u'10.10.10.213'), (u'worker_1', 1, u'192.168.100.71', u'10.10.10.213'), (u'worker_2', 0, u'192.168.100.72', u'10.10.10.213')]


>>> cursor.execute('SELECT * FROM names')
<sqlite3.Cursor object at 0x10b0221f0>
>>> for row in cursor:
...     print "%20s %d %30s %30s" %(row[0], row[1], row[2], row[3])
...
         worker_0 1                 192.168.100.70                 10.10.10.213
         worker_1 1                 192.168.100.71                 10.10.10.213
         worker_2 0                 192.168.100.72                 10.10.10.213


>>> cursor.execute('SELECT * FROM names WHERE state is 1')
<sqlite3.Cursor object at 0x10b0221f0>
>>> for row in cursor:
...     print "%20s %d %30s %30s" %(row[0], row[1], row[2], row[3])
...
         worker_0 1                 192.168.100.70                 10.10.10.213
         worker_1 1                 192.168.100.71                 10.10.10.213
>>> cursor.close()
>>> connection.close()


worker_2의 상태를 1로 변경

>>> cursor.execute('UPDATE vm_list SET state=1 where name == "worker_2"');
<sqlite3.Cursor object at 0x10507f1f0>

>>> cursor.execute('SELECT * FROM vm_list WHERE state == 1')
<sqlite3.Cursor object at 0x10507f1f0>
>>> print "%16s     state %16s %16s %10s %10s" %("name", "ip", "host", "worker_time", "down_time")
            name     state               ip             host    worker_time  down_time
>>> print "-"*82
----------------------------------------------------------------------------------
>>> for row in cursor:
...     print "%16s   %7s %16s %16s %10u %10u" %(row[0], "active" if row[1] == 1 else "disable", row[2], row[3], row[4], row[5])
...
     worker_0    active   192.168.100.70   10.10.10.213          0          0
     worker_1    active   192.168.100.71   10.10.10.213          0          0
     worker_2    active   192.168.100.72   10.10.10.213          0          0
     
     
     
Change uptime of worker_2

>>> cursor.execute('UPDATE vm_list SET uptime=100 where name == "worker_2"');
<sqlite3.Cursor object at 0x10507f1f0>
>>> cursor.execute('SELECT * FROM vm_list WHERE state == 1')
<sqlite3.Cursor object at 0x10507f1f0>
>>> for row in cursor:
...     print "%16s   %7s %16s %16s %10u %10u" %(row[0], "active" if row[1] == 1 else "disable", row[2], row[3], row[4], row[5])
...
     worker_0    active   192.168.100.70   10.10.10.213          0          0
     worker_1    active   192.168.100.71   10.10.10.213          0          0
     worker_2    active   192.168.100.72   10.10.10.213        100          0

increase uptime of worker_2

>>> cursor.execute('UPDATE vm_list SET uptime=uptime+100 where name == "worker_2"');
<sqlite3.Cursor object at 0x10507f1f0>

>>> cursor.execute('SELECT * FROM vm_list WHERE state == 1')
<sqlite3.Cursor object at 0x10507f1f0>
>>> for row in cursor:
...     print "%16s   %7s %16s %16s %10u %10u" %(row[0], "active" if row[1] == 1 else "disable", row[2], row[3], row[4], row[5])
...
     worker_0    active   192.168.100.70   10.10.10.213          0          0
     worker_1    active   192.168.100.71   10.10.10.213          0          0
     worker_2    active   192.168.100.72   10.10.10.213        200          0
     
     
>>> cursor.execute('SELECT * FROM vm_list WHERE state == 1')
<sqlite3.Cursor object at 0x10507f1f0>
>>> r = [dict((cursor.description[i][0], value) for i, value in enumerate(row)) for row in cursor.fetchall()]

>>> json_output = json.dumps(r)
>>> print json_output
[{"uptime": 0, "name": "worker_0", "ip": "192.168.100.70", "host_ip": "10.10.10.213", "state": 1, "downtime": 0}, {"uptime": 0, "name": "worker_1", "ip": "192.168.100.71", "host_ip": "10.10.10.213", "state": 1, "downtime": 0}, {"uptime": 200, "name": "worker_2", "ip": "192.168.100.72", "host_ip": "10.10.10.213", "state": 1, "downtime": 0}]
```
