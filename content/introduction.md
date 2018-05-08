## Introduction
{:#introduction}

RDF versioning —also known as RDF archiving— has been an active area of research
that looks into storage and querying techniques for different versions of Linked Datasets.
These versions don't necessarily have a temporal relationship,
as multiple different versions or _branches_ of versions could exist at the same time,
as opposed to [RDF streams](cite:cites streamreasoning).

One of the key benefits of the RDF is its ability to encode _semantics_ into the data through the use of ontologies.
This allows _reasoners_ to interpret this data, and use this encoded meaning to derive new knowledge.
Reasoning within static RDF data and temporal RDF streams are already well-established research domains.
Reasoning over RDF versions on the other hand is still an underexplored domain.

Some preliminary work has already been done in the domain of reasoning over RDF versions.
In one work, the calculation of [semantic differences](cite:cites semversion) between versions has been investigated.
In another work, [reasoning with multi-version ontologies](cite:cites more) was investigated.
These works cover only very specific parts of the reasoning demands within RDF versioning.
For example, given a versioned dataset about the cat population inside a shelter,
it is currently impossible with the existing systems to retrieve the dataset version indentifiers for which African wild cats were present,
according to the classification version, i.e., ontology, of a given institution.
Furthermore, the domain of combining reasoning and querying
—as is done in [Ontology-Based Data Access techniques](cite:cites heymans2008ontology,poggi2008linking)—
within the domain of RDF versioning remains unexplored.

In this work, we introduce a general formalization of reasoning within RDF versioning from the querying perspective.
For this, we extend the [five foundational versioned query types](cite:cites bear) that were introduced
to cover the retrieval demands within RDF versioning.
Furthermore, we present a prototypical implementation of a versioned RDF store
that offers basic rule-based reasoning capabilities at query-time.
This prototype demonstrates the benefits of semantic versioning,
such as finding all versions in which a certain fact can be inferred,
and storage space reduction by inferring facts instead of materializing them beforehand.

{:.todo}
article structure
