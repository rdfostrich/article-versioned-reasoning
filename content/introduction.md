## Introduction
{:#introduction}

RDF versioning —also known as RDF archiving—
<span class="comment" data-author="MVS">Considering the next sentence, this is not correct. Archiving is mostly sequential in practice.</span>
has been an active area of research
that looks into storage and querying techniques for different versions of Linked Datasets.
These versions do not necessarily follow a purely linear temporal relation,
as multiple different versions or _branches_ of versions could exist at the same time,
as opposed to [RDF streams](cite:cites streamreasoning).
<span class="comment" data-author="RV">Very good point here. Should this be emphasized in abstract and/or title? <q>Versioned</q> made me think of linear versioning indeed.</span>

One of the key benefits of RDF is its ability to encode _semantics_ into the data through the use of ontologies.
This allows _reasoners_ to interpret this data, and use this encoded meaning to derive new knowledge.
<span class="comment" data-author="RV">So same remark as earlier: <q>transforming</q> knowledge might be better</span>
Reasoning within static RDF data and temporal RDF streams are already well-established research domains.
Reasoning over RDF versions on the other hand is still an underexplored domain.
<span class="comment" data-author="MVS">Check language above.</span>
Some preliminary work has already been done in the domain of reasoning over RDF versions.
In one work, the calculation of [semantic differences](cite:cites semversion) between versions has been investigated.
In another work, [reasoning with multi-version ontologies](cite:cites more) was investigated.
These works cover only very specific parts of the reasoning demands within RDF versioning.
<span class="comment" data-author="MVS">Check language above.</span>

For example, given a versioned dataset about the cat population inside a shelter,
it is currently impossible with the existing systems to retrieve the dataset version identifiers for which African wild cats were present,
<span class="comment" data-author="RV">Mmm, your example is a) very specific b) true or not true, depending on how the knowledge is encoded. Spend some more time introducing the example?</span>
according to the classification version, i.e., ontology, of a given institution.
Furthermore, the domain of combining reasoning and querying
—as is done in [Ontology-Based Data Access techniques](cite:cites heymans2008ontology,poggi2008linking)—
within the domain of RDF versioning remains unexplored.

In this work, we introduce a general formalization of semantic versioned querying,
i.e., reasoning within RDF versioning from the querying perspective.
<span class="comment" data-author="MVS">I like the main contribution, but it is not clearly stated (above sentence and paragraph) Are you formalizing cross-version reasoning (if yes, just say so)? Or what do you do exactly?</span>
For this, we extend the [five foundational versioned query types](cite:cites bear) that were introduced
to cover the retrieval demands within RDF versioning.
Furthermore, we present a prototypical implementation of a versioned RDF store
that offers basic rule-based reasoning capabilities at query-time.
This prototype demonstrates the benefits of semantic versioning,
such as finding all versions in which a certain fact can be inferred,
and storage space reduction by inferring facts instead of materializing them beforehand.
<span class="comment" data-author="MVS">Can't you start with the above benefits?</span>

The aim of this work is to provide a foundation
for the future research and development of semantic versioned querying within RDF stores.
This will lead to improvements inside domains that require the semantic analysis on Linked Datasets,
for example for analyzing [concept drift](cite:cites conceptdrift)
or [tracking diseases in biomedical datasets](cite:cites biomedical) over time.
<span class="comment" data-author="RV">^^this!</span>

This article is structured as follows:
In the next section, we discuss related work on semantic versioned querying.
In [](#fundamentals), we discuss the fundamental concepts on RDF versioning.
After that, in [](#querying), we introduce new foundational semantic versioned query atoms.
In [](#demo), we present a proof-of-concept implementation of these atoms with a preliminary evalution.
Finally, we conclude in [](#conclusions).
