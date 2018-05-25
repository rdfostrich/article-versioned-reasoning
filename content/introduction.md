## Introduction
{:#introduction}

RDF versioning has been an active area of research
that looks into storage and querying techniques for different versions of Linked Datasets.
These versions do not necessarily follow a purely linear temporal relation,
as multiple different versions or _branches_ of versions could exist at the same time,
as opposed to [RDF streams](cite:cites streamreasoning).

One of the key benefits of RDF is its ability to encode _semantics_ into the data through the use of ontologies.
This allows _reasoners_ to interpret this data, and use this encoded meaning to transform and infer knowledge.
Reasoning within static RDF data and temporal RDF streams are already well-established research domains.
Reasoning over RDF versions is however still an underexplored domain.
Some preliminary work has already been done in the domain of reasoning over RDF versions.
In one work, the calculation of [semantic differences](cite:cites semversion) between versions has been investigated.
In another work, [reasoning with multi-version ontologies](cite:cites more) was investigated.
These works only cover very specific parts of the reasoning demands within RDF versioning.

For instance, given a versioned dataset,
it is currently impossible with the existing systems to efficiently find all versions in which a certain fact can be inferred.
Furthermore, the domain of combining reasoning and querying
—as is done in [Ontology-Based Data Access techniques](cite:cites heymans2008ontology,poggi2008linking)—
within the domain of RDF versioning remains unexplored.

In this work, we introduce a general formalization of semantic versioned querying,
i.e., reasoning within RDF versioning from the querying perspective,
For this, we extend the [five foundational versioned query types](cite:cites bear) that were introduced
to cover the retrieval demands within RDF versioning.
We formalize concepts such as reasoning within a single version, version differences, and different versions.
Furthermore, we present a prototypical implementation of a versioned RDF store
that offers basic rule-based reasoning capabilities at query-time.
This prototype demonstrates the benefits of semantic versioning,
such as finding all versions in which a certain fact can be inferred,
and storage space reduction by inferring facts instead of materializing them beforehand.

The aim of this work is to provide a foundation
for the future research and development of semantic versioned querying within RDF stores.
This will lead to improvements inside domains that require the semantic analysis on Linked Datasets,
for example for analyzing [concept drift](cite:cites conceptdrift)
or [tracking diseases in biomedical datasets](cite:cites biomedical) over time.

This article is structured as follows:
In the next section, we discuss related work on semantic versioned querying.
In [](#fundamentals), we discuss the fundamental concepts on RDF versioning.
After that, in [](#querying), we introduce new foundational semantic versioned query atoms.
In [](#demo), we present a proof-of-concept implementation of these atoms with a preliminary evalution.
Finally, we conclude in [](#conclusions).
