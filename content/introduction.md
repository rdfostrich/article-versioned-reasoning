## Introduction
{:#introduction}

{:.todo}
rewrite to focus just on the querying part

RDF versioning —also known as RDF archiving— has been an active area of research
that looks into storage and querying techniques on different versions of Linked Datasets.
These versions don't necessarily have a temporal relationship,
as multiple different versions or _branches_ of versions could exist at the same time,
as opposed to [RDF streams](cite:cites streamreasoning).

One of the key benefits of the RDF is its ability to encode _semantics_ into the data.
This allows _reasoners_ to interpret this data, and use this encoded meaning to derive new knowledge.
Reasoning within static RDF data and temporal RDF streams are already well-established research domains.
Reasoning over RDF versions on the other hand is still an underexplored domain.

TODO: wrong: SemVersion versions both A-Box and T-Box?
On the one hand, the calculation of [semantic differences](cite:cites semversion) between versions has been investigated,
where the A-box is versioned, while the T-box remains the same.
On the other hand, [reasoning with multi-version ontologies](cite:cites more) was investigated
where the A-box remains the same, while the T-box is versioned.
No work has been done that combines the versioning of both the A-box and T-box.
Furthermore, the domain of combining reasoning and querying —as is done in [Ontology-Based Data Access techniques](cite:cites heymans2008ontology,poggi2008linking)— within the domain of RDF versioning remains unexplored.

In this work, we lay the foundations for semantic versioning
by introducing semantic versioned query atoms, where both the A-box and the T-box can change.
Furthermore, we present a prototypical implementation of a versioned RDF store
that offers basic rule-based reasoning capabilities at query-time.
This prototype demonstrates the benefits of semantic versioning,
such as finding all versions in which a certain fact can be inferred,
and storage space reduction by inferring facts instead of materializing them beforehand.

{:.todo}
article structure
