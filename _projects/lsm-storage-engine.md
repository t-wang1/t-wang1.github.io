---
layout: archive
title: 'LSM Storage Engine'
permalink: /projects/lsm-storage-engine
date: 2026-01-4
author_profile: true
---

A [LSM-based storage engine](https://github.com/t-wang1/mini-LSM) implemented in Rust

### Overview

Log structured merge trees are data structures that maintain key-value pairs.

The distinction between LSM trees and other key-value data structures becomes the most apparent when it comes to write operations. Rather than overwriting the disk space when you want to update a value, the operations are batched into SST files in a LSM tree and then written to disk. During compaction, the engine will merge these files to apply the updates. There are two main compaction algorithms: level and tiered compaction. In level compaction, we maintain the invariant that each level (besides L0) has sorted SSTables. This results in better read performance since there aren't any duplicate keys. In tiered compaction, keys can overlap anywhere within a level. While the read performance is slightly worse in comparison, this results in less CPU operations and disk bandwidth. 

An LSM storage engine consists of (1) a write ahead log (2) SSTs on the disk (3) memtables in memory.

To apply an update operation, we write the key-value pair to the WAL and the memtable. Once the memtable gets filled, it’s frozen before being flushed to the disk as SST files. To read, we’ll look through all the memtables before searching through the entire LSM tree.

### Questions 

<details>
    <summary> What’s the difference between RAM and disk? </summary>
    RAM is fast, temporary storage accessible by the CPU. There’s no persistence so data is lost when the power is off. In comparison, the disk is slower but has more permanent storage. 
</details>

<details>
    <summary> Why is batching and merging more efficient than in-place updates? </summary>
    Because the entire memtable gets flushed sequentially once filled, it’s much faster than the random I/O that would occur if you overwrote the disk space each time you wanted to update value.   
</details>

<details>
    <summary> What’s the difference between a read/write lock and a mutex? </summary>
    In a mutex, only one thread can access at a time. In a read/write lock, you can either have multiple readers or one writer.
</details>
