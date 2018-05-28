## Related Work
{:#related-work}

Semantic versioned querying lies somewhere in between the domains of
semantic versioning, stream reasoning, and ontology-based data access.
In this section, we discuss the related work in these domains.

### Semantic Versioning

In the context of this paper, we consider semantic versioning to be
_the logical reasoning over a collection of dataset versions and ontologies_.
On the one hand, this concerns the [management of multiple versions of datasets](cite:cites bear,ostrich,diachronql,xrdf3x),
and on the other hand, the [reasoning over these versions](cite:cites semversion,more).

A lot of research has been done in the area of [ontology evolution](cite:cites surveyontologyevolutionpostcentric),
i.e., the maintenance of ontologies with respect to domain or requirement changes.
Some works focus on the [_management and maintenance_ of multiple ontology versions](cite:cites temporalowlversioning),
while other look more into [_applying_ different ontology versions on datasets](cite:cites semversion,more).
As we focus on _reasoning_ with multiple dataset and ontology versions in this work, we discuss the latter.

[SemVersion](cite:cites semversion) is a system that provides versioning for RDF ontologies.
The authors introduce the concept of a _semantic diff_
that takes the semantics of an ontology language into account
when calculating the difference between two dataset versions.
This concept will be explained in more detail in [](#fundamentals),
after which we generalize it in [](#querying) to enable versioning over both the dataset and the ontology.

[Huang et al.](cite:cites more) propose a reasoning framework over a versioned ontology,
which is based on a temporal logic approach.
They provide a prototypical implementation of their framework as the MORE system.
The difference with our approach is that we assume non-temporal versions,
where the relation between versions is not necessarily linear.

### Stream Processing

Within the domain of [RDF Stream Processing (RSP)](cite:cites rspql), _stream reasoning_ is defined as
[<q>the logical reasoning in real time on gigantic and inevitably noisy data streams in order to support the decision process of extremely large numbers of concurrent users</q>](cite:providesQuotationFor streamreasoningsofar).
RSP typically uses the concept of _windowing_ as a scalability measure to select subsets of data streams to perform reasoning over.
This windowing makes RSP similar to RDF versioning,
as not only a single dataset has to be taken into account,
but multiple different parts or versions of the dataset needs to be processed.
Next to this similarity, there are significant differences between the domains of streaming and versioning
which elicits a distinction between them.
For instance, stream elements are temporally identified and sorted,
while versions are not necessarily temporal, such as hash-based identifiers in version control systems (such as [Git](cite:cites git)).
Furthermore, streams typically have a high velocity,
while versions evolve at a lower rate.
For example, [DBpedia](cite:cites dbpedia) publishes a new version at a yearly frequency,
and the [RDF version of npm](cite:cites rdfnpm) is being generated every day.
Due to these significant differences regarding ordering and velocity,
we see the domains of streaming and versioning as distinct, but partially overlapping domains.

### Ontology-Based Data Access

[Ontology-Based Data Access (OBDA)](cite:cites mastro) is a technique that offers a semantic query interface
on top of a non-semantic datasource using semantic mappings that are applied at query-time.
Such mappings can for example be defined between RDF and SQL, using OWL ontologies.
These datasources are typically incomplete, on top of which semantic mappings can infer additional knowledge through reasoning.
In this work, we are mainly concerned with the inference aspect at query-time,
which can be referred to as [Ontology-Based Query Answering (OBQA)](cite:cites obqa).
The [SPARQL entailment regimes specification](cite:citeAsAuthority spec:sparqlentailment) defines several _entailment regimes_
that define how such inferences can be achieved at query-time.
At the time of writing, no systems exist yet that can offer OBQA on top of _versioned_ RDF datasets.
Those systems would require new querying capabilities specific to versioning,
which will be introduced in the next section.
