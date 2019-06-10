# 5 Splitting the Monolith

## 1 It’s All About Seams
(Michael Feathers) Seam : a portion of the code that can be treated in isolation and worked on without impacting the rest of the codebase.

We find and identify seams that can become service boundaries.

What makes seams : bounded contexts, namespaces, packages in Java, …

## 2 Breaking Apart MusicCorp
Identify contexts and Create packages representing the contexts
with refactoring features in modern IDEs, test, …

as analyzing dependencies (dependencies should be aligned with the organizations and the bounded contexts)

bit by bit and day by day rather than a big-bang approach. With tracking tools.

## 3 The Reasons to Split the Monolith
Chipping away (an incremental approach) is strongly advised.

### 3.1 Pace of Change
Expecting a load of changes coming up in a certain part? -> Then 

### 3.2 Team Structure
Team and Code and Full ownership

### 3.3 Security
Individual service <- additional protections

### 3.4 Technology
Easy to apply and test better/alternative technology

## 4 Tangled Dependencies
We want to pull out the seam that is least depended on.

## 5 Database
Often, the database is the root of the evil.

## 6 Getting to Grips with the Problems
Take a look at the code : It’s clear if we use a repository later, map objects, data structures, and even mapping files per bounded contexts.

Problems : tables such as a foreign key(database-level constraints), a table shared between multiple bounded contexts, …

## 7 Breaking Foreign Key Relationships
Table sharing or Foreign key -> API calls between services
(Performance concern? It might be trivial.)

Now : Database-level constraints -> Service-level constraints, which means we need our own consistency check across services. Someone(?) is in charge of this.

## 8 Example: Shared Static Data
Utility-level static data, such as country codes.
1. Copying tables for each package.
2. (Sensible) Shared static data as code. (property/configuration file, e numerations, …)
3. (Extreme) Push this static data into each service.

(I don’t agree with the author’s opinion. Personally, I prefer using the database. Because it’s easy to modify, keep consistency, and stay aware of this for developers. But, it’s still a trade-off especially about how often it changes.)

## 9 Shared Data
Domain concept : isn’t modeled in the code, and is in fact implicitly modeled in the database.

-> becomes a new service.

## 10 Shared Table
Split the table like a table normalization.

## 11 Refactoring Database
### 11.1 Staging the Break
Incremental splitting
1. Split out a single schema but keep the service together.
2. Split out the application code into separate microservices.
Database calls may be increased, it might be necessary to join in memory, …

## 12 Transactional Boundaries
We might lose the transaction safety on several schemas.

### 12.1 Try Again Later
(In some cases) We could queue up the part of an operation in a queue or logfile, and TRY AGAIN LATER.

Eventual Consistency
- Rather than using a transactional boundary to ensure that the system is in a consistent state when the transaction completes, we accept that the system will get itself into a consistent state at some point in the future.

### 12.2 Abort the Entire Operation
Compensating Transaction : reverses some commits.

Backend process : cleaning up inconsistency, retrying failed compensating transactions…

We hope it will be simple, but can be super complex.

### 12.3 Distributed Transaction
A distributed transaction using a transaction manager.
(related techs : JTA, X/Open XA, …)

The most common algorithm is two-phase commit, especially for short-lived transactions. It just tries to catch most failure cases. And this coordination process means locks (-> difficult to scale the system, causes resource contention,…).

### 12.4 So What to Do?
Ask : Is really this operation transactional? It’s preferred to have eventual consistency and local transactions.

Try really hard : Step back from a technical view and create a concrete concept to represent the transaction itself. eg. the idea “in-processing order”

## 13 Reporting

## 14 The Reporting Database
Easy. For performance, a read replica can be used.
But.. (skip)

## 15 Data Retrieval via Service Calls
It works on small amount of data, but breaks down with larger volumes of data. We cannot track changes by keeping a local copy of this data in the reporting system.

APIs are not for reporting. Caching data to speed up of the data retrieval doesn’t work well. long-tail data -> cache miss.

Batch APIs : API for many elements. or API for requesting a resource(such as a file export)

Still network load exists.

## 16 Data Pumps
Directly access the the database of service and pumps it into a reporting database.

This is an exception! Less downsides. Only one purpose.

The same team should manages and build the data pump and the service. And the team should always deploy the data pump as an artifact with the service.

A materialized view to create a aggregated view. Less coupling. Performance concerns.

Still complex.

### 16.1 Alternative Destinations
On one project I was involved with, we used a series of data pumps to populate JSON files in AWS S3, effectively using S3 to masquerade as a giant data mart!

## 17 Event Data Pump
Event publisher(service), event(Create/update/delete/…), event feed, event subscriber(reporting mapper)

Less coupling and propagating immediately.

Downsides : many broadcast, might not be less scalable than a direct data pump.

## 18 Backup Data Pump
Netflix uses it.

Backing up the SSTable files of Cassandra in S3, then using them for reporting with Hadoop.

## 19 Toward Real Time
We are moving more and more toward generic eventing systems capable of routing our data to multiple different place depending on need.

## 20 Cost of Change
Mistakes are inevitable. How best to mitigate the costs of those mistakes.

High cost changes mean high risk. Try to make mistakes where the impact will be lowest.

## 21 Understanding Root Causes
It’s ok to grow a service to the point that it need to be split.

The key is knowing it needs to be split before the split becomes too expensive.

A number of ways to keep the cost down : libraries, lightweight service frameworks, self-service provisioning virtual machines, PaaS, …

## 22 Summary
Incremental approach (which really works)

finding seams > emerging service boundaries > decomposing system
