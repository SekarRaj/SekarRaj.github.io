---
title: "Database versioning"
date: 2026-11-30
draft: false
categories: ["database", "design"]
tags: ["versioned tables", "temporal tables"]
---

Auditability and State Machines

While implementing a recent feature, I encountered a requirement to maintain multiple versions of a domain record that is governed by a state machine. The complexity arose from the fact that a record update is not immediately final; instead, the updated version must progress through a series of state transitions before it becomes authoritative.

The workflow works as follows:
An updated record is created and begins navigating the state machine. Once it reaches a specific intermediate final substate, control passes to an external system, which triggers additional state transitions until the record reaches its terminal state.

The key constraint is visibility. Until the updated record reaches its terminal state, the user must continue to see the previous version of the record. When the terminal state is reached, the new version replaces the old one in the user view. During the transition period, both versions are visible, with a clear indication that an update is in progress. However, no further modifications are allowed while the update is pending.

This requirement maps directly to the problem of versioned tables. A robust solution to this class of problems is the use of temporal tables, which allow the system to track historical and current versions of records in a consistent and queryable manner.

Tech Stack

The implementation uses:
* Spring Boot 4+
* Gradle
* Java 25
* Microsoft SQL Server (with system-versioned temporal tables)

Other relational databases provide comparable capabilities for handling versioned data, though the exact features and syntax vary.

The application
The application is a simple patient information management system. The system captures cutomers information as given by the users or their digital devices and sent to an external system EHR/EMR. The the crux of the probelem is that we will have provide a interface for the user where they can input data points, review the information entered and at some point confirm that the information is correct and that it can be considered final. Subsequently the users can edit their information and go through the same flow again. 

The pain points 
We have to maintain every piece of information recorded by the users for auditability. The information is stores across multiple tables and different external API are invoked as provided by the external system providers. 

The external API 
1. Create Patient if not present 
2. Send patient preliminary information 
3. Send medication information

Datamodel 
* Patient - Profile Info 
* HealthScreening - Screening information 
* Medications - Set of medications taken by the user 
* HeathReadings - Current set of health readings 

Patterns considered 
* Transactional outbox with Async publisher 
* Versioned Write for auditability 
* State machines 

