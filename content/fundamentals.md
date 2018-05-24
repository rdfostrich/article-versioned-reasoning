## Fundamentals
{:#fundamentals}

### RDF Archiving

An [_RDF archive_](cite:cites bear) has been defined by Fernández et al. as follows:

> An RDF archive graph A is a set of version-annotated triples,
> where a _version-annotated triple_ _(s, p, o):\[i\]_ is an RDF triple (s, p, o) with a label i ∈ N representing the version in which this triple holds.
> The set of all [RDF triples](cite:cites spec:rdf) is defined as _(U ∪ B) × U × (U ∪ B ∪ L)_,
> where <var>U</var>, <var>B</var>, and <var>L</var>, respectively represent the disjoint, infinite sets of URIs, blank nodes, and literals.
> Finally,
> an RDF version of an RDF archive <var>A</var> at snapshot <var>i</var> is the RDF graph _A(i) = {(s, p, o)|(s, p, o):\[i\] ∈ A}._

For the remainder of this article, we use the shorthand notation _A<sub>i</sub>_ to refer to the RDF version _A(i)_.

<span class="comment" data-author="RV">We need some thought on styling formulas. I'd surround variables (within text) with &lt;var&gt;; the main problem is the italicized ∪ which resembles U too closely.</span>
<span class="comment" data-author="MVS">I think you can get rid of 3.1 and merge it in 3.2</span>

### Versioned Query Atoms

To cover the retrieval demands in RDF archiving,
[five foundational query types were introduced](cite:cites bear),
which are referred to as _query atoms_.
These query atoms are based on
the [RDF data model](cite:cites spec:rdf) and [SPARQL query language](cite:cites spec:sparqllang).
In these models, a _triple pattern_ is defined as _(U ∪ V) × (U ∪ V) × (U ∪ L ∪ V)_, with _V_ being the infinite set of variables.
A set of triple patterns is called a _Basic Graph Pattern_, which forms the basis of a SPARQL query.
The evaluation of a SPARQL query _Q_ on an RDF graph _G_ containing RDF triples,
produces a bag of solution mappings _\[\[Q\]\]<sub>G</sub>_.

The five foundational query atoms introduced by Fernández et al. are the following:

1. **Version materialization (VM)** retrieves data using a query _Q_ targeted at a single version _A<sub>i</sub>_.<br />
Formally: _VM(Q, A<sub>i</sub>) = \[\[Q\]\]<sub>A<sub>i</sub></sub>_.<br />
Example: _Which books were present in the library yesterday?_
2. **Delta materialization (DM)** retrieves query _Q_'s result change sets between two versions _A<sub>i</sub>_ and _A<sub>j</sub>_.<br />
Formally: _DM(Q, A<sub>i</sub>, A<sub>j</sub>)=(Ω<sup>+</sup>, Ω<sup>−</sup>). With Ω<sup>+</sup> = \[\[Q\]\]<sub>A<sub>i</sub></sub> \ \[\[Q\]\]<sub>A<sub>j</sub></sub> and Ω<sup>−</sup> = \[\[Q\]\]<sub>A<sub>j</sub></sub> \ \[\[Q\]\]<sub>A<sub>i</sub></sub>_.<br />
Example: _Which books were returned or taken from the library between yesterday and now?_
3. **Version query (VQ)** annotates query _Q_'s results with the versions (of RDF archive A) in which they are valid.<br />
Formally: _VQ(Q, A) = {(Ω, W) | W = {A(i) | Ω=\[\[Q\]\]<sub>A(i)</sub>, i ∈ N} ∧ Ω ≠ ∅}_.<br />
Example: _At what times was book X present in the library?_
4. **Cross-version join (CV)** joins the results of two queries (_Q1_ and _Q2_) between versions _A<sub>i</sub>_ and _A<sub>j</sub>_.<br />
Formally: _VM(Q1, A<sub>i</sub>) ⨝ VM(Q2, A<sub>j</sub>)_.<br />
Example: _What books were present in the library yesterday and today?_
5. **Change materialization (CM)** returns a list of versions in which a given query _Q_ produces
consecutively different results.<br />
Formally: _{(i, j) | i,j ∈ ℕ, i < j, DM(Q, A(i), A(j)) = (Ω<sup>+</sup>, Ω<sup>−</sup>), Ω<sup>+</sup> ∪ Ω<sup>−</sup> ≠ ∅, ∄ k ∈ ℕ : i < k < j}_.<br />
Example: _At what times was book X returned or taken from the library?_

<span class="comment" data-author="RV">Is this the moment to refer to our own previous work? (not strictly necessary)</span>

### Semantic Diff

Völkel et al. introduce the concept of a [_semantic diff_](cite:cites semversion)
that takes the semantics of an ontology language into account when calculating the diff,
which is not the case for a regular _structural diff_.
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

The _semantic closure_ _s<sub>l</sub>(A)_ of a set of RDF triples _A_
is the set of all statements that can be concluded from the statements in _A_ under the semantics of the RDF-based ontology language _l_.
A _semantic diff_ _d<sub>l</sub>(A,B)_ of two sets of RDF triples (_A_ and _B_) is then formally defined as _d<sub>l</sub>(A,B) = (+<sub>l</sub>(A,B),−<sub>l</sub>(A,B))_, with _+<sub>l</sub>(A, B) = s<sub>l</sub>(B) \ (s<sub>l</sub>(A) ∩ s<sub>l</sub>(B))_ and _−<sub>l</sub>(A, B) = s<sub>l</sub>(A) \ (s<sub>l</sub>(A) ∩ s<sub>l</sub>(B))_.
<span class="comment" data-author="RV">Your def or someone else's? Mention in either case.</span>
