## Semantic Versioned Query Atoms
{:#querying}

As discussed in [](#fundamentals), there exist five foundational query atoms for querying RDF archives.
In this section, we introduce semantic extensions of these query atoms,
similar to the structural diff to semantic diff extension introduced by Völkel et al.
More concretely, we will extend these five query atoms with parameters for _language versioning_,
instead of only _dataset versioning_.

### Semantic Closure

To remain in line with the definitions on RDF archiving as listed in [](#fundamentals),
we adapt the semantic closure definition by Völkel et al. as follows:
_The semantic closure s(A<sub>i</sub>, L<sub>j</sub>) of a version V<sub>i</sub> is the set of all triples that can be inferred from the triples in A<sub>i</sub> under the semantics of the RDF-based ontology language L<sub>j</sub>._

<span class="comment" data-author="MVS">Why do you redefine it and not extend it, like Versioned Semantic Closure (you do this for atoms)? Why is there no (alternative) _<sub>l</sub>_ here?</span>

In this definition, we consider the RDF-based ontology language l to represent an RDF archive as well,
for which we use the notation _L<sub>j</sub>_ to refer to the RDF version _j_ of the archive _L_.

### Query Atoms

In this section, we describe the semantic extensions of the five foundational query atoms for querying RDF archives.
The atoms that apply to multiple versions (VQ and CM) are subcategorized
to handle the different combinations of single and cross-version dataset and ontology versions.

Each extension is described formally, and an example of its usage is given.
All examples apply to the use case of a cat shelter
that makes use of an evolving ontology of cat species.

The semantic extension of the five versioned query atoms are defined as follows:

<span class="comment" data-author="MVS">Get rid of the nested list?</span>

1. **Semantic version materialization (S-VM)** retrieves data using a query _Q_
    targeted at a single version _A<sub>i</sub>_ in a single ontology version _L<sub>j</sub>_.<br />
    Formally: _S-VM(Q, A<sub>i</sub>, L<sub>j</sub>) = \[\[Q\]\]<sub>s(A<sub>i</sub>, L<sub>j</sub>)</sub>_<br />
    Example: _Which African wild cats were present in the shelter yesterday according to last year's classification?_
2. **Semantic delta materialization (S-DM)** retrieves query _Q_'s result change sets between
    two versions _A<sub>i</sub>_ and _A<sub>j</sub>_ respectively using ontology version _L<sub>k</sub>_ and _L<sub>l</sub>_.<br />
    Formally: _S-DM(Q, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>, L<sub>l</sub>) = (Ω<sup>+</sup>, Ω<sup>−</sup>)._
    With _Ω<sup>+</sup> = \[\[Q\]\]<sub>s(A<sub>i</sub>, L<sub>k</sub>)</sub> \ \[\[Q\]\]<sub>s(A<sub>j</sub>, L<sub>l</sub>)</sub>_
    and _Ω<sup>−</sup> = \[\[Q\]\]<sub>s(A<sub>j</sub>, L<sub>jl</sub>)</sub> \ \[\[Q\]\]<sub>s(A<sub>i</sub>, L<sub>k</sub>)</sub>_<br />
    Example: _Which cats that were present in the shelter since ten years ago became a different species over the last year?_
3. **Semantic version query (S-VQ)**
    1. **Semantic intermodal and interontological version query (MOS-VQ)** annotates query _Q_'s results with the versions
        of RDF archive _A_ and ontology _L_ in which they are valid.<br />
        Formally: _S-VQ(Q, A, L) = {(Ω, V) | V = {(A<sub>i</sub>, L<sub>j</sub>) | Ω=\[\[Q\]\]<sub>s(A<sub>i</sub>, L<sub>j</sub>)</sub>, i, j ∈ N} ∧ Ω ≠ ∅}_<br />
        Example: _At what points in time were there African wild cats in the schelter, and according to the classification of what time?_
    2. **Semantic intermodal version query (MS-VQ)** annotates query _Q_'s results with the versions
        of RDF archive _A_ in which they are valid according to ontology version _L<sub>j</sub>_.<br />
        Formally: _S-VQ(Q, A, L<sub>j</sub>) = {(Ω, V) | V = {A<sub>i</sub> | Ω=\[\[Q\]\]<sub>s(A<sub>i</sub>, L<sub>j</sub>)</sub>, j ∈ N} ∧ Ω ≠ ∅}_<br />
        Example: _At what points in time were there African wild cats in the schelter?_
    3. **Semantic interontological version query (OS-VQ)** annotates query _Q_'s results with the versions
        of ontology _L_ in which they are valid within dataset version _A<sub>i</sub>_.<br />
        Formally: _S-VQ(Q, A<sub>i</sub>, L) = {(Ω, V) | V = {L<sub>j</sub> | Ω=\[\[Q\]\]<sub>s(A<sub>i</sub>, L<sub>j</sub>)</sub>, i ∈ N} ∧ Ω ≠ ∅}_<br />
        Example: _At what points in time were the cats that are currently in the shelter ever African wild cats?_
4. **Semantic cross-version join (S-CV)** joins the results of two queries (_Q1_ and _Q2_) between versions _A<sub>i</sub>_ and _A<sub>j</sub>_ respectively using ontology version _L<sub>k</sub>_ and _L<sub>l</sub>_.<br />
    Formally: _S-CV(Q<sub>1</sub>, Q<sub>2</sub>, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>, L<sub>l</sub>) = S-VM(Q<sub>1</sub>, A<sub>i</sub>, L<sub>k</sub>) ⨝ S-VM(Q<sub>2</sub>, A<sub>j</sub>, L<sub>l</sub>)_<br />
    Example: _Which African wild cats were in the shelter yesterday (according to last year's classification) and the day before (according to the current classification)?_
5. **Semantic change materialization (S-CM)**
    1. **Semantic intermodal and interontological change materialization (MOS-CM)** returns a list of consecutive archive and ontology versions in which a given query _Q_ produces different results.<br />
        Formally: _S-CV(Q, A, L) = {(i, j, k, l) | i, j, k, l ∈ ℕ, i < j, k < l, S-DM(Q, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>, L<sub>l</sub>) = (Ω<sup>+</sup>, Ω<sup>−</sup>), Ω<sup>+</sup> ∪ Ω<sup>−</sup> ≠ ∅, ∄ a ∈ ℕ : i < a < j, ∄ b ∈ ℕ : k < b < l}_<br />
        Example: _At what times and in which classification did Bob become an African wild cat?_
    2. **Semantic intermodal change materialization (MS-CM)** returns a list of consecutive archive versions in which a given query _Q_ produces different results between two ontology versions.<br />
        Formally: _S-CV(Q, A, L<sub>k</sub>, L<sub>l</sub>) = {(i, j) | i, j ∈ ℕ, i < j, S-DM(Q, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>, L<sub>l</sub>) = (Ω<sup>+</sup>, Ω<sup>−</sup>), Ω<sup>+</sup> ∪ Ω<sup>−</sup> ≠ ∅, ∄ a ∈ ℕ : i < a < j, ∄ b ∈ ℕ : k < b < l}_<br />
        Example: _At what times did Bob become an African wild cat between last year's and today's classification?_
    3. **Semantic interontological change materialization (MS-CM)** returns a list of consecutive language versions in which a given query _Q_ produces different results between two dataset versions.<br />
        Formally: _S-CV(Q, A<sub>i</sub>, A<sub>j</sub>, L) = {(k, l) | k, l ∈ ℕ, k < l, S-DM(Q, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>, L<sub>l</sub>) = (Ω<sup>+</sup>, Ω<sup>−</sup>), Ω<sup>+</sup> ∪ Ω<sup>−</sup> ≠ ∅, ∄ a ∈ ℕ : i < a < j, ∄ b ∈ ℕ : k < b < l}_<br />
        Example: _In which classification versions did Bob become an African wild cat between yesterday and today?_

### Query Atom Derivations

Based on the semantic query atom extensions that were introduced in last section,
we can derive subtypes for the semantic delta materialisation and semantic cross-version join.
These subtypes can be used as simplified form of the foundational semantic query atoms.

#### Semantic delta materialisation

The definition of semantic delta materialization (S-DM) is similar to,
but more generic than the [semantic diff definition by Völkel et al](cite:cites semversion).
While the semantic diff only allows versioning on the dataset, our S-DM definition also enables versioning of the ontology.
Similarly, [Huang et al.](cite:cites more) introduce a diff that enables versioning of the ontology, but not on the dataset.
As such, our semantic delta materialization definition can be seen as a combination of both.
Furthermore, we can express these diff methods in terms of S-DM as follows:

* **Intermodal semantic delta materialisation (MS-DM)** is semantic delta materialization of different versions under the same ontology. This corresponds to the diff method of [Völkel et al.](cite:cites semversion).<br />
    Formally: _MS-DM(Q, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>) = S-DM(Q, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>, L<sub>k</sub>)_.
* **Interontological semantic delta materialisation (OS-DM)** is semantic delta materialization of the same version under different ontologies. This corresponds to the diff method of [Huang et al.](cite:cites more).<br />
    Formally: _OS-DM(Q, A<sub>i</sub>, L<sub>k</sub>, L<sub>l</sub>) = S-DM(Q, A<sub>i</sub>, A<sub>i</sub>, L<sub>k</sub>, L<sub>l</sub>)_.

#### Semantic cross-version join

Similarly, we can define the following derivations of the semantic cross-version join:

* **Intermodal semantic cross-version join (MS-CV)** is semantic cross-version join for different versions under the same ontology.
    Formally, _MS-CV(Q<sub>1</sub>, Q<sub>2</sub>, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>) = S-CV(Q<sub>1</sub>, Q<sub>2</sub>, A<sub>i</sub>, A<sub>j</sub>, L<sub>k</sub>, L<sub>k</sub>)_.
* **Interontological semantic cross-version join (OS-CV)** is semantic cross-version join of the same version under different ontologies.
    Formally, _OS-CV(Q<sub>1</sub>, Q<sub>2</sub>, A<sub>i</sub>, L<sub>k</sub>, L<sub>l</sub>) = S-CV(Q<sub>1</sub>, Q<sub>2</sub>, A<sub>i</sub>, A<sub>i</sub>, L<sub>k</sub>, L<sub>l</sub>)_.
