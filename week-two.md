# Week two results


## Level 1

### How long did it take to index the 1.2M product data set? What docs/sec indexing rate did you see?
Indexing time: 4.79 minutes
Max indexing rate: 5.74k documents per seconds
Index size: 1.14 Gb

### Notice that the Index size rose (roughly doubled) while the content was being indexed, peaked, then ~ 5 minutes after indexing stopped, the index size dropped down substantially. Why did it drop back down?
Indexing time: 5.34 minutes
Max indexing rate: 5.17k documents per seconds
Index size: peak 2.85 Gb, then went to 2.05 and after ~5 minutes settled at 1.49 Gb

OpenSearch uses Lucene's append-only structures which first creates copies and subsequently merges the documents.


### Looking at the metrics dashboard, what queries/sec rate are you getting?
115 queries per second

### What resource(s) appear to be the constraining factor?
CPU is a constraining factor

## Level 2

| Time (minutes) | DPS | Query time | QPS | Size (GB)
--- | --- | --- | --- | --- | ---
1 CPU, 2GB RAM | 4.79 | 5.74 | ~15 | 115 | 1.14
2 CPU, 4GB RAM | 2.46 | 9.74 | ~9 | 189 | 1.18
4 CPU, 8GB RAM | 2.28 | 9.89 | ~7 | 224 | 1.49

### As you increased CPU and memory in your L2 tests, what seemed to be the constraining factor limiting indexing rate?
At least for my tests, it was still CPU 😅

## Level 3

### Using indexing and querying at the same time

| Time (minutes) | DPS | Query time | QPS | Size (GB)
--- | --- | --- | --- | --- | ---
4 CPU, 8GB RAM | 2.93 | 7.88 | ~9 | 225 | 1.24

