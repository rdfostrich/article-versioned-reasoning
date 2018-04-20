## Fundamentals
{:#fundamentals}

### RDF Archiving

An [_RDF archive_](cite:cites bear) has been defined by Fernández et al. as follows:
_An RDF archive graph A is a set of version-annotated triples._
Where a _version-annotated triple_ _(s, p, o):\[i\]_ is _an RDF triple (s, p, o) with a label i ∈ N representing the version in which this triple holds._
The set of all [RDF triples](cite:cites spec:rdf) is defined as _(U ∪ B) × U × (U ∪ B ∪ L)_,
where _U_, _B_, and _L_, respectively represent the disjoint, infinite sets of URIs, blank nodes, and literals.
Finally,
_an RDF version of an RDF archive A at snapshot i is the RDF graph A(i) = {(s, p, o)|(s, p, o):\[i\] ∈ A}._
For the remainder of this article, we use the notation _Vi_ to refer to the RDF version _A(i)_.

### Versioned Query Atoms

To cover the retrieval demands in RDF archiving,
[five foundational query types were introduced](cite:cites bear),
which are referred to as _query atoms_.

These query atoms are based on
the [RDF data model](cite:cites spec:rdf) and [SPARQL query language](cite:cites spec:sparqllang).
In these models, a _triple pattern_ is defined as _(U ∪ V) × (U ∪ V) × (U ∪ L ∪ V)_, with _V_ being the infinite set of variables.
A set of triple patterns is called a _Basic Graph Pattern_, which forms the basis of a SPARQL query.
The evaluation of a SPARQL query _Q_ on an RDF graph _G_ containing RDF triples,
produces a bag of solution mappings _\[\[Q\]\]G_.

The five foundational query atoms introduced by Fernández et al. are the following:

1. **Version materialization (VM)** retrieves data using a query _Q_ targeted at a single version _Vi_.
Formally: _VM(Q, Vi) = \[\[Q\]\]Vi_.
Example: _Which books were present in the library yesterday?_
2. **Delta materialization (DM)** retrieves query _Q_'s result change sets between two versions _Vi_ and _Vj_.
Formally: _DM(Q, Vi, Vj)=(Ω<sup>+</sup>, Ω<sup>−</sup>). With Ω<sup>+</sup> = \[\[Q\]\]Vi \ \[\[Q\]\]Vj and Ω<sup>−</sup> = \[\[Q\]\]Vj \ \[\[Q\]\]Vi_.
Example: _Which books were returned or taken from the library between yesterday and now?_
3. **Version query (VQ)** annotates query _Q_'s results with the versions (of RDF archive A) in which they are valid.
Formally: _VQ(Q, A) = {(Ω, W) | W = {A(i) | Ω=\[\[Q\]\]A(i), i ∈ N} ∧ Ω ≠ ∅}_.
Example: _At what times was book X present in the library?_
4. **Cross-version join (CV)** joins the results of two queries (_Q1_ and _Q2_) between versions _Vi_ and _Vj_.
Formally: _VM(Q1, Vi) ⨝ VM(Q2, Vj)_.
Example: _What books were present in the library yesterday and today?_
5. **Change materialization (CM)** returns a list of versions in which a given query _Q_ produces
consecutively different results.
Formally: _{(i, j) | i,j ∈ ℕ, i < j, DM(Q, A(i), A(j)) = (Ω<sup>+</sup>, Ω<sup>−</sup>), Ω<sup>+</sup> ∪ Ω<sup>−</sup> ≠ ∅, ∄ k ∈ ℕ : i < k < j}_.
Example: _At what times was book X returned or taken from the library?_

### Semantic Diff

Völkel et al. introduce the concept of a [_semantic diff_](cite:cites semversion)
that takes the semantics of an ontology language into account when calculating the diff,
which is not the case for a _structural diff_.

As an example, consider the dataset with two versions from [](#semdiff-dataset-example).
The typical, structural diff just takes the difference between these two versions at triple level,
without taking into account the meaning of the triples.
The structural diff of this example can be found in [](#semdiff-dataset-example-structural).
A semantic diff on the other hand, takes into account the meaning of the data.
For this example, we know that cat is a subclass of animal.
Therefore, the removal of Bob being an animal does not actually take place, because it can still be inferred in version 1,
as shown in [](#semdiff-dataset-example-semantic).

<figure id="semdiff-dataset-example" class="listing">
````/code/semdiff-dataset-example.txt````
<figcaption markdown="block">
A simple example dataset with two versions about Bob the cat,
with a separate ontology language.
</figcaption>
</figure>

<figure id="semdiff-dataset-example-structural" class="listing">
````/code/semdiff-dataset-example-structural.txt````
<figcaption markdown="block">
The structural diff between the two versions in [](#semdiff-dataset-example).
</figcaption>
</figure>

<figure id="semdiff-dataset-example-semantic" class="listing">
````/code/semdiff-dataset-example-semantic.txt````
<figcaption markdown="block">
The semantic diff between the two versions in [](#semdiff-dataset-example).
</figcaption>
</figure>

Formally, a _semantic diff_ (_d<sub>l</sub>(A,B)_) of two sets of RDF triples (_A_ and _B_) is defined as _d<sub>l</sub>(A,B) = (+<sub>l</sub>(A,B),−<sub>l</sub>(A,B))_, with _+<sub>l</sub>(A, B) = s<sub>l</sub>(B) \ (s<sub>l</sub>(A) ∩ s<sub>l</sub>(B))_ and _−<sub>l</sub>(A, B) = s<sub>l</sub>(A) \ (s<sub>l</sub>(A) ∩ s<sub>l</sub>(B))_.
The _semantic closure_ (_s<sub>l</sub>(A)_) of a set of RDF triples _A_ is the set of all statements that can be concluded from the statements in _A_ under the semantics of the RDF-based ontology language _l_.
