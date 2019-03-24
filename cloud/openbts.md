# OpenBTS

http://openbts.org

## zeromq 사용

> Starting with release 4, OpenBTS requires zeromq (zmq).  This can be installed by running:
> $ sudo ./NodeManager/install_libzmq.sh

### install_libzmq.sh  
> grep apt-get NodeManager/install_libzmq.sh
> sudo apt-get install -y software-properties-common python-software-properties
> sudo apt-get update
> sudo apt-get install -y libzmq3-dev libzmq3 python-zmq

## database에 대한 언급 

> Standrd paths:
> /OpenBTS -- Binary installation and authorization keys.
> /etc/OpenBTS -- Configuration databases.
> /var/run/ -- Real-time reporting databases.  

DB는 sqlite 사용 

> sqlite3 /etc/OpenBTS/OpenBTS.db



```
NodeManager/JSONDB/JSONDB.cpp
/var/run/NeighborTable.db
/var/run/ChannelTable.db
/var/run/TMSITable.db
/var/run/TransactionTable.db
/var/run/PowerScannerResults.db
```



## CLOC
```
http://github.com/AlDanial/cloc has numerous examples and more information.
Chae-yongs-MacBook-Pro:openbts cychong$ cloc *
     589 text files.
     564 unique files.
      99 files ignored.

github.com/AlDanial/cloc v 1.72  T=4.49 s (110.8 files/s, 33343.2 lines/s)
-----------------------------------------------------------------------------------
Language                         files          blank        comment           code
-----------------------------------------------------------------------------------
C++                                166          11353          16112          57167
C/C++ Header                       156           8112          11926          19886
Verilog-SystemVerilog              107           1250           3666          12418
C                                    3            208            249           1382
Bourne Shell                        17            284            473           1322
make                                23            215            529            751
m4                                   6             77             24            702
awk                                  1             57             17            543
SQL                                  1              2              7            238
Pascal                               8             24              0            230
Python                               3             33             22            136
MATLAB                               2             16             23             59
Markdown                             1              9              0             43
Bourne Again Shell                   3              3              0             11
-----------------------------------------------------------------------------------
SUM:                               497          21643          33048          94888
-----------------------------------------------------------------------------------
```

