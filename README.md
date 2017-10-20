# DCS Mini Project

## Introduction

In this project, you will implement a Simple Distributed Filesystem using the RMI library you created in project 1. Files will be hosted remotely on one or more storage servers. Separately, a single naming server will index the files, indicating which one is stored where. When a client wishes to access a file, it first contacts the naming server to obtain a stub for the storage server hosting it. After that, it communicates directly with the storage server to complete the operation.

When completed, your filesystem will support file reading, writing, creation, deletion, and size queries. It will also support certain directory operations — listing, creation and deletion. It will be possible to lock files, and commonly accessed files will be replicated on multiple storage servers.

## Checkpoint

This project involves a large amount of work, and many features must be implemented before it is complete. To help you with this, we suggest an optional checkpoint, for which you should implement all file and directory operations except locking and replication. The starter code provides you with two sets of conformance tests. The default set includes tests only up to the optional checkpoint: none of these tests depend on locking or replication. Once you are ready to proceed on to the full version of the filesystem, simply replace the default `conformance/` subdirectory with the `conformance/` subdirectory distributed inside `conformance-final.zip` in the starter code archive. The checkpoint is not part of your grade, nor is there a deadline for it. It’s simply a convenient intermediate goal for you to work towards.

## Makefile Targets

To compile all Java files, execute
```
make
```

To run all test cases, run
```
make tests
```

To package class files into a single, monolithic distribution of the filesystem, run
```
make jar
```

This produces a file called dfs.jar, which can be used to start naming and storage servers, and to run utilities. To package source files into an archive, run
```
make archive
```

To generate documentation, execute
```
make docs
```

The documentation can then be viewed at javadoc/index.html. The suggested reading order for the packages is rmi, then common, naming, storage, client, apps. Alternatively, complete documentation of all classes and members can be generated using
```
make docs-all
```
and then viewed at javadoc-all/index.html.

To clean the build directories, execute
```
make clean
```

## Tests

* Various tests can be run by executing:
```
java conformance.ConformanceTests
java -cp ./:./unit unit.UnitTests
java test.SelfTest
```

Conformance tests check the public interfaces of the classes in the various packages for conformance to the written specifications. The tests are thorough but not exhaustive. Conformance tests are grouped by the packages they test. For example, conformance tests for the RMI library, which is in the package rmi, are grouped in the package conformance.rmi. Conformance tests are used for grading. You have been provided with a large number of conformance tests to help you find problems with your code. However, there may be additional tests used by the staff during grading. Testing thoroughly is your responsibility.

Unit tests can be written to check package-private classes. Unit tests are in the same package as the class they are testing: a unit test for a class in the package rmi would also be in the package rmi (whereas a conformance test would be in the different package conformance.rmi). Unit tests, are, however, kept in a different directory in the source tree. The Java classpath is altered when running unit tests to put the unit tests logically in the same package as the code they are testing.

The class test.SelfTest runs some basic self-tests on the testing library.

## Applications

The naming and storage servers can be started as follows:
```
java -jar dfs.jar naming
java -jar dfs.jar storage (local-address) (naming-server) (local-path)
```

The naming server is fairly self-explanatory. The arguments for starting the storage server are:
```
local-address: the externally-visible hostname or IP address of the machine
               on which the server is running. This is necessary because the
               externally-visible name of the local machine cannot always be
               easily determined due to routers, firewalls, etc.
naming-server: the hostname or IP address of the naming server with which
               the storage server is to register.
local-path:    the local directory in which the storage server is to locate
               the files it is to serve. Be careful with this directory -
               the storage server may choose to delete some of the files in
               it.
```