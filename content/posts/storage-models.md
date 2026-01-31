---
title: "Storage models in database"
date: 2025-10-26T01:47:46-07:00
draft: false
categories: ["databases"]
tags: ["database-storage-models"]
---

### Database storage models 

Depending on the type of access method, the database storage model could be one of the following. 

* N-ary Storage Model (NSM) or Row storage 
* Decomposition Storage Model (DSM) or Column storage
* Hybrid Storage Model (PAX) - Mix of Row and Storage

### Row Storage

Based on block oriented storage

Slotted Page Array
Indexed Storage.      (Clustering)
LSM Trees 


### Column Store
Separate page per attribute 
Attributes and meta-data stored in separate arrays of physical length 

Identification of record has two options. 

Fixed length or embedded Ids (Not preferred)

### Partition Attributed Storage
Vertically partition attributes within pages


9698106549 Surya 
