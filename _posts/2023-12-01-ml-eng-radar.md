# The modern Open Source Data Stack Overview

The modern stack is cons titude by many tools and vendors and at times can be complex to navigate that space.

### Open Operational Databases

Relational Databases : Your relational Postgresql, Mysql.

Distributed Databases: Your Cap Theorem bound databases - Cassandra, MongoDB , etc.

Specialized Databases : Metric Databases, Graph Databases.

### Open Data Lakes

A combination of Cloud Storage and MPP engines . Best for crunching large amounts of data, 
inexpensive data format.

### Open Data Warehouses

Harder to find open , for some reason they are mostly propertaly.

Are generally a combination of Data Lakes and Open MPPs .  Good examples are Trino/Presto and open similarities.


### The Lake House Architecture
Warehouse on Data Lake and App, contains data management . Unified storage for BI/DS/ML - source of truth for 
analytics and data.

### Open Stream Processing

* Process data as it arrives
* * Incremental to avoid recomputing
* * Out of order and late-arriving data
 
Ideal for low latency access to data.

Pub-sub, event queuing.


Open Real-time/Interactive Engines
Starock - contains already proven data models.

===== A Blue Print for Open Data Architecture by Vinoth Chandar =====

https://events.solutionmonday.com/e/open-source-data-summit/portal/stage/309686



![Screenshot 2023-11-16 at 22 09 06 (2)](https://github.com/nlauchande/nlauchande.github.io/assets/646979/01776af6-afe9-47e5-8afd-43f5b1215273)







