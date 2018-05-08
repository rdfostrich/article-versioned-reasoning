## Related Work
{:#related-work}

In this section, we introduce the related work on semantic versioned querying.
In the following, we elaborate on semantic versioning in general, stream reasoning, and ontology-based data access.

### Semantic Versioning

[SemVersion](cite:cites semversion) is a system that provides versioning for RDF ontologies.
They distinguish between structural and semantic versioning.
Whereas structural versioning simply takes into account the changes on triple-level,
i.e., which triples have been added and which ones have been removed,
semantic versioning also takes into account the meaning of these triples using ontologies.
The authors introduce the concept of a _semantic diff_
that takes the semantics of an ontology language into account when calculating the diff,
as opposed to a traditional _structural diff_.
This concept will be explained in more detail in [](#fundamentals).

[Huang et al.](cite:cites more) propose a reasoning framework over a versioned ontology,
which is based on a temporal logic approach.
They provide a prototypical implementation of their framework as the MORE system.

### Stream Processing

Within the domain of [RDF Stream Processing (RSP)](cite:cites rspql), stream reasoning is defined as
["the logical reasoning in real time on gigantic and inevitably noisy data streams in order to support the decision process of extremely large numbers of concurrent users"](cite:providesQuotationFor streamreasoningsofar).
RSP typically uses the concept of _windowing_ as a scalability measure to select subsets of data streams to perform reasoning over.
This windowing makes RSP similar to RDF versioning,
as not only a single dataset has to be taken into account,
but multiple different parts or versions of the dataset needs to be processed.
Next to this similarity, there are significant differences between the domains of streaming and versioning
which elicits a distinction between them.
In the following, we discuss five characteristics that are typically attributed to RSP,
and explain how they differ to RDF versioning.

#### Temporal identification

The most important characteristic of stream processing is its temporality,
i.e., stream elements are identified by timestamps or time intervals.

Within the domain of versioning, versions are not necessarily temporal.
For example, version control systems (such as [Git](cite:cites git))
identify versions using _hashes_, as timestamps are merely metadata,
and multiple versions can exist at exactly the same time.

#### Temporal relation

This temporal identification within stream processing leads to an ordering in time,
using which stream elements form a single linear sequence.

Versions do however not necessarily form a single linear ordered sequence.
Using the concept of _branching_, multiple parallel sequences of versions can exist,
that have a common ancestor somewhere in the chain.
Within versioning, the order of version only becomes relevant in cases when you want to calculate the difference between two version.
In those cases, the order can be defined by the user, or automatically using a temporal annotation if present.

#### Velocity

Velocity is a characteristic that is typically attributed to RDF Stream Processing.
Use cases include data streams that move very fast, in the order of milliseconds,
such as telecom call records or financial market analysis.
This requires applications that handle such quickly evolving data, and can react without too much delay.

Within the domain of versioning, velocity is less important.
New versions of RDF datasets are typically published at a relatively low frequency.
[DBpedia](cite:cites dbpedia) for example publishes a new version at a yearly frequency.
The [RDF version of npm](cite:cites rdfnpm) on the other hand is being generated every day.

#### Real-time

Stream processors typically work under some real-time constraints,
where an answer must be provided within a certain maximum amount of delay.
That is why stream processors typically work over small time windows,
so that the amount of data that needs to be processed is smaller, and can happen more quickly.
A fast response is in some cases more important than a completely correct response,
this is for example the case in autonomous vehicles where reaction time must be quick when an collision must be avoided.

Within RDF versioning, real-time is less important,
mostly because the velocity is much lower,
which gives processors much more time to come up with an answer over the complete dataset.

#### Storage

Due to the velocity characteristic, streams are typically not being stored,
because the latest results are typically the most important within stream processing.
Storing these highly volatile streams would require a massive and continuously increasing storage space.

As the velocity for versioning is typically much lower,
historical versions can be stored.
For this, specific [RDF archiving solutions](cite:cites archiving) are being investigated and implemented.

#### Continuous

Coupled with the velocity characteristic, stream processing typically uses _continuous_ queries.
Because the latest results are typically the most important, and delay should be minimized.
That is why continuous queries are registered to a processing engine once,
after which the engine continuously evaluates the query and streams back the results.

Continuous querying is possible within the versioning domain,
but due to its lower velocity, this is less important,
as the overhead of query registration and continuous processing might outweigh
the cost of repeatedly sending single-time queries at a low frequency.
Furthermore, as historical dataset versions are typically stored,
access to these different versions, or the union over all of them is requires as well.

### Ontology-Based Data Access

[Ontology-Based Data Access (OBDA)](cite:cites mastro) is a technique that offers a semantic query interface
on top of a non-semantic datasource using semantic mappings that are applied at query-time.
Such mappings can for example be defined between RDF and SQL, using OWL ontologies.
These datasources are typically incomplete, whereas the semantic mappings can infer additional knowledge through reasoning.

In this work, we are mainly concerned with the inference aspect at query-time,
which can be referred to as [Ontology-Based Query Answering (OBQA)](cite:cites obqa).
At the time of writing, no systems exist yet that can offer OBQA on top of versioned RDF datasets,
which is the gap we aim to fill with this work.
