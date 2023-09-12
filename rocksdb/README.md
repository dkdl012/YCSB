<!--
Copyright (c) 2012 - 2018 YCSB contributors. All rights reserved.

Licensed under the Apache License, Version 2.0 (the "License"); you
may not use this file except in compliance with the License. You
may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
implied. See the License for the specific language governing
permissions and limitations under the License. See accompanying
LICENSE file.
-->

## Quick Start

This section describes how to run YCSB on RocksDB running locally (within the same JVM).
NOTE: RocksDB is an embedded database and so articles like [How to run in parallel](https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload-in-Parallel) are not applicable here.

### 1. Set Up YCSB

Clone the YCSB git repository and compile:

    git clone https://github.com/brianfrankcooper/YCSB.git
    cd YCSB
    mvn -pl site.ycsb:rocksdb-binding -am clean package

### 2. Run YCSB

Now you are ready to run! First, load the data:

    ./bin/ycsb load rocksdb -s -P workloads/workloada -p rocksdb.dir=/tmp/ycsb-rocksdb-data

Then, run the workload:

    ./bin/ycsb run rocksdb -s -P workloads/workloada -p rocksdb.dir=/tmp/ycsb-rocksdb-data

## RocksDB Configuration Parameters

* ```rocksdb.dir``` - (required) A path to a folder to hold the RocksDB data files.
    * EX. ```/tmp/ycsb-rocksdb-data```
* ```rocksdb.optionsfile``` - A path to a [RocksDB options file](https://github.com/facebook/rocksdb/wiki/RocksDB-Options-File).
    * EX. ```ycsb-rocksdb-options.ini```

## Note on RocksDB Options

If `rocksdb.optionsfile` is given, YCSB will apply all [RocksDB options](https://github.com/facebook/rocksdb/wiki/Setup-Options-and-Basic-Tuning) exactly as specified in the options file.
Otherwise, YCSB will try to set reasonable defaults.

**[IMPORTANT]** After compiling, please note to refer to YCSB/rocksdb/target/test-classes/testcase.ini. Its format is slightly different from the original rocksdb option file.
```[CFOptions "usertable"]``` and ```[TableOptions/BlockBasedTable "usertable"]``` should be added on the rocksdb option file (formatted .ini). The original rocksdb option file has only four section: ```[Version]```, ```[DBOptions]```, ````[CFOptions "default"]``, and ```[TableOptions/BlockBasedTable "default"]```.

If do not, this error will be thrown.

    site.ycsb.DBException: org.rocksdb.RocksDBException: You have to open all column families. Column families not opened: usertable
        at site.ycsb.db.rocksdb.RocksDBClient.init(RocksDBClient.java:80)
        at site.ycsb.DBWrapper.init(DBWrapper.java:90)
        at site.ycsb.ClientThread.run(ClientThread.java:91)
        at java.lang.Thread.run(Thread.java:748)
    Caused by: org.rocksdb.RocksDBException: You have to open all column families. Column families not opened: usertable
        at org.rocksdb.RocksDB.open(Native Method)
        at org.rocksdb.RocksDB.open(RocksDB.java:290)
        at site.ycsb.db.rocksdb.RocksDBClient.initRocksDBWithOptionsFile(RocksDBClient.java:108)
        at site.ycsb.db.rocksdb.RocksDBClient.init(RocksDBClient.java:75)
        ... 3 more
    site.ycsb.DBException: org.rocksdb.RocksDBException: You have to open all column families. Column families not opened: usertable
        at site.ycsb.db.rocksdb.RocksDBClient.init(RocksDBClient.java:80)
        at site.ycsb.DBWrapper.init(DBWrapper.java:90)
        at site.ycsb.ClientThread.run(ClientThread.java:91)
        at java.lang.Thread.run(Thread.java:748)
    Caused by: org.rocksdb.RocksDBException: You have to open all column families. Column families not opened: usertable
        at org.rocksdb.RocksDB.open(Native Method)
        at org.rocksdb.RocksDB.open(RocksDB.java:290)
        at site.ycsb.db.rocksdb.RocksDBClient.initRocksDBWithOptionsFile(RocksDBClient.java:108)
        at site.ycsb.db.rocksdb.RocksDBClient.init(RocksDBClient.java:75)
        ... 3 more
