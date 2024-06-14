---
layout: archive
title: "Designing Data-Intensive Applications"
permalink: /notes/ddia
date: 2024-1-25
---

These notes are _Designing Data-Intensive Applications_ by Martin Kleppmann

### Resources/Links
* [Textbook](https://github.com/ms2ag16/Books/blob/master/Designing%20Data-Intensive%20Applications%20-%20Martin%20Kleppmann.pdf)

### Reliable, Scalable, and Maintainable Applications

There are 3 main aspects of software systems that developers are concerned with: reliability, scalability, and maintainability. 

Having a reliable system stipulates that a program will continue to run correctly, even when it runs into faults. Kleppmann makes a distinction between faults and failures. Faults occur when a component of the system fails. Failures, however, occur when the entire system stops providing the required service. Developers are more concerned with preventing faults from becoming faults as opposed to preventing all faults. This should intuitively make sense because there would be a huge cost associated with preventing all faults, especially ones that don't have a high probability of occuring. 

Scalability deals with whether a system can be reliable with increased load. Vertical and horizontal scaling are both ways of coping with increased load. In vertical scaling, we move to a more powerful machine (ex. increasing CPU, RAM). In horizontal scaling, the load is distributed among multiple, smaller machines. 

There are three design principles associated with maintainability - operability, simplicity, and evolvability. Good operability makes routine tasks easy, allowing the operations team to focus their efforts on high-value activities. Abstraction removes _accidental complexity_. Evolvability makes it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases. 

### Data Models and Query Languages

Data models are built by layering one data model on top of another. For example, if we model the real world in terms of objects or data structures you can express them in terms of JSON or XML documents which can be further broken down into bytes which can yet be broken down into electric currents and so forth. 

The **relational model** stipulates that data is organized into relations, where each relation is an unordered collection of tuples. As computers became more powerful the relational model adapted alongside it, generalizing to a variety of use cases. With relational models, you only need to build a query optimizer once, and then all applications that use the database can benefit from it. **Polygot persistence** refers to how relational databases can be used in tandem with nonrelational datastores. **Impedance mismatch** refers to the layer of translation required between objects in application code and the database model of tables, rows, and columns. The **hierarchical model** represents data as a tree of records nested within records. 

**Schema-on-read** is used to describe when the structure of the data is implicit, and only interpreted when the data is read. **Schema-on-write** is used to describe when the schema is explicit and the database ensures all written data conforms to it. 

Declarative querying languages like SQL are typically more concise and are better suited to parallel execution. 

Graph-like data models consist of vertices and edges. They can store heterogenous data. 

### Storage and Retrieval

Storage engines are concerned with how databases handle storage and retrieval internally. The two families of storage engines are _log-structured_ and _page-oriented_ storage engines. 

Many databases internally use a _log_, which is an append-only data file. In implementation, this means that if there are several updates to a key, the previous versions aren't overwritten. Rather you would need to look at the last occurrence of a key to find the latest value. However, this has a time complexity of O(n) since you would have to sift through the entire database file in order to find the key. We can use an index as a signpost to workaround this problem. 

An index if an additional structure that is derived from the primary data and stores metadata. While utilizing indices can speed up read queries, it comes at the expense of slowing down writes. This is because the index also needs to be updated each time data is written. Because of this, we often don't index everything. 

The simplest possible indexing strategy is to keep an in-memory hash map where every key is mapped to the location at which the value can be found. Whenever you want to look up at a value, use the hash map, go to that location, and read the value. 

To avoid running out of disk space from continuously appending to file, we can break the log into segments and then writing to a new segment file once it reaches a certain capacity. We can then perform _compaction_, where we throw away duplicate keys, and keep only the most recent update for each key. We can also merge and compact segments in the background while reading and writing requests as normal. 

Each segment now has its own hash table. To find the value, we first check the most recent segment's hash map; if the key isn't present we check the second-most-recent segment, and so on. The merging process keeps the number of segments small, so lookups don't need to check many hash maps. 

Append-only designs are good for several reasons:
* appending and segment merging are sequential write operations, which are generally much faster than random writes
* concurrency and crash recovery are simpler if segment files are append-only or immutable
* merging old segments avoids the problem of data files getting fragmented over time

However, there are also some limitations:
* the hash table must fit in memory, so if you have a large number of keys you're out of luck
* range queries are not efficient
    * you cannot easily scan over all keys within a range; you'd have to look up each key individually in the hash map

_Sorted String Tables_ or _SSTables_ require that the sequence of key-value pairs are sorted by key and that each key only appears once. SSTables have several advantages over log segments:
* merging segments is simple and efficient
* in order to find a particular key in the file, you no longer need to keep an index of all the keys in memory
* compressing blocks of data are easier

With red-black trees or AVL trees, you can insert keys in any order and read them back in sorted order. To construct a SSTable, we add a write to an in-memory tree called a _memtable_. We write the memtable to disk as an SSTable file when it reaches a threshold. To serve a read request, try to find the key in the memtable, then in the most recent on-disk segment, and so forth. Run a merging and compaction process in the background to combine segment files and discard overwritten or deleted values. Storage engines that are based on this principle of merging and compacting sorted files are often called LSM storage engines. 

The most widely used indexing structure is the _B-tree_. They keep key-value pairs sorted by key. While log-structured indexes break the database down into variable-size segments, B-trees break the database down into fixed-size blocks or pages. Each page can be identified using an address or location, which allows a page to refer to another - similar to a pointer. In this way, we can construct a tree of pages. 

One page is designated as the root of the B-tree. The page contains several keys and references to child pages. The number of references to child pages in one page of the B-tree is called the _branching factor_. If you want to update the value for an existing key, you search for the leaf page containing the key, change the value, and write the page back to disk. If you want to add a new key, you need to find the page whose range encompasses the new key and add it to that page. This algorithm ensures that the tree remains balanced: a B-tree with n keys always has a depth of O(log n). The tree exists in memory with pointers to the disk. 

The basic underlying write operation of a B-tree is to modify the files in place. It's assumed the overwrite doesn't change of the location of the page. In order to make the database resilient to crashes, it's common for B-tree implementations to include a _write-ahead log_. This is an append-only file to which every B-tree modification must be written before it can be applied to the pages of the tree itself. 

As a rule of thumb, LSM-trees are typically faster for writes, whereas B-trees are thought ot be faster for reads. One write to the database resulting in multiple writes to the disk over the course of the database's lifetime is known as _write amplification_. LSM-trees are typically able to sustain high write because they sometimes have lower write amplification. They can also be compressed better, and thus produce smaller files on disk. A downside of log-structured storage is that the compaction process can sometimes interfere with the performance of ongoing reads and writes. 

When you query something, the key could be the actual row in question, or it could be a reference to the row stored elsewhere. In the latter case, the rows are stored in a _heap file_, and it stores data in no particular order. This is desirable because it avoids duplicating data when multiple secondary indexes are involved: each index just references a location in the hap file, and the actual data is kept in one place. In a _clustered index_, the indexed row in stored directly within another index. 

A _concatenated index_ is a type of multi-column index that combines several fields into one key by appending one column to another. 

Disks have two advantages compared to main memory: they're durable (their contents aren't lost if the power is turned off), and they have a lower cost per gigabyte.

### Replication

The difficulty in replication lies in handling _changes_ to replicated data. There are three popular algorithms for handling these scenarios:

1. Single-leader replication
2. Multi-leader replication
3. Leaderless replication

In leader-based replication, a replica is designated as the leader; when clients want to write to the database, they have to send their requests to the leader which writes the new data to its local storage and sends it to the other replicas (known as followers) via a _replication log_ or _change stream_. The followers then update its local copy of the data accordingly. 

Replication can happen either synchronously or asynchronously. In synchronous replication, the leader waits until the follower has confirmed that it's received the write before reporting success to the client. In asynchronous replication, the leader sends the message, but doesn't wait for a response from the follower. 
