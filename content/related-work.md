## Related Work
{:#related-work}

Semantic versioned querying lies somewhere in between the domains of
semantic versioning, stream reasoning, and ontology-based data access.
In this section, we discuss the related work in these domains.

### Semantic Versioning

[SemVersion](cite:cites semversion) is a system that provides versioning for RDF ontologies.
They distinguish between _structural_ and _semantic_ versioning.
Whereas structural versioning simply takes into account the changes on the triple level,
i.e., which triples have been added and which ones have been removed,
semantic versioning also takes into account the _meaning_ of these triples using ontologies.
The authors introduce the concept of a _semantic diff_
that takes the semantics of an ontology language into account when calculating the diff,
as opposed to a traditional _structural diff_.
This concept will be explained in more detail in [](#fundamentals).
<span class="comment" data-author="RV">Delta of our work?</span>
<span class="comment" data-author="MVS">The text above can also be more concise. And what about: [https://www.emeraldinsight.com/doi/abs/10.1108/17440080910983556](https://www.emeraldinsight.com/doi/abs/10.1108/17440080910983556), [https://www.worldscientific.com/doi/abs/10.1142/S0218194012500040](https://www.worldscientific.com/doi/abs/10.1142/S0218194012500040), [https://onlinelibrary.wiley.com/doi/full/10.1111/exsy.12157](https://onlinelibrary.wiley.com/doi/full/10.1111/exsy.12157), [http://ceur-ws.org/Vol-1376/LDQ2015_paper_06.pdf](http://ceur-ws.org/Vol-1376/LDQ2015_paper_06.pdf), [https://www.tandfonline.com/doi/abs/10.1080/12460125.2017.1252230](https://www.tandfonline.com/doi/abs/10.1080/12460125.2017.1252230). Any relation to RDF change detection and Ontology versioning?</span>

[Huang et al.](cite:cites more) propose a reasoning framework over a versioned ontology,
which is based on a temporal logic approach.
They provide a prototypical implementation of their framework as the MORE system.
<span class="comment" data-author="RV">Diff with our work?</span>
<span class="comment" data-author="MVS">This ref is from 2008 and has 88 citations. Sure none of them is related?</span>

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
Due to these significant differences,
<span class="comment" data-author="RV">which?</span>
we see the domains of streaming and versioning as distinct, but partially overlapping domains.

### Ontology-Based Data Access

[Ontology-Based Data Access (OBDA)](cite:cites mastro) is a technique that offers a semantic query interface
on top of a non-semantic datasource using semantic mappings that are applied at query-time.
Such mappings can for example be defined between RDF and SQL, using OWL ontologies.
These datasources are typically incomplete, whereas the semantic mappings can infer additional knowledge through reasoning.
<span class="comment" data-author="RV">Is the previous sentence entirely correct? Is the connection between incompleteness and inferences correctly expressed with the <q>whereas</q> conjunctive?</span>
In this work, we are mainly concerned with the inference aspect at query-time,
which can be referred to as [Ontology-Based Query Answering (OBQA)](cite:cites obqa).
At the time of writing, no systems exist yet that can offer OBQA on top of _versioned_ RDF datasets,
which is the gap we aim to fill with this work.
<span class="comment" data-author="RV">maybe a small insight into what OBQA is in the context of versioning, and why it is different? (or a pointer to a next paragraph that explains this)</span>
<span class="comment" data-author="MVS">Isn't SPARQL entailment relevant as well?</span>