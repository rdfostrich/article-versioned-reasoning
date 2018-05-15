## Proof of Concept
{:#demo}

In order to provide a baseline of the proposed semantic versioned querying atoms,
we provide a prototypical implementation of a subset of the semantic versioned query atoms that were introduced in [](#querying).
In this section, we describe this system, followed by an evaluation description, and a presentation of the results.

### System

Our prototype is implemented as an additional semantic layer on top of [OSTRICH](cite:cites ostrich),
which is a versioned RDF triple store that offers syntactical versioning.
As OSTRICH only supports VM, DM and VQ triple pattern queries at the time of writing,
we limit our implementation to the semantic extension of these atoms,
namely, S-VM, S-DM and S-VQ triple pattern queries.

As can be seen in [](#architecture), our semantic layer internally uses two OSTRICH stores,
one for the versioned dataset, and one for the versioned language.
Using a simple backwards rule-based reasoner,
this semantic layer infers additional triples for each S-VM, S-DM or S-VQ query.
It does this through the following steps:

1. Select applicable rules for the given triple pattern.
2. Query the dataset for the given triple pattern and version options.
3. Query the language for the given triple pattern and version options.
4. Loop until set of inferred triples stops growing.
    1. Determine rules that can infer new triples
    2. Perform backwards reasoning with these rules by querying the dataset for the given triple pattern and version options.
    3. Add newly inferred triples to set of triples

<figure id="architecture">
<img src="img/architecture.svg" alt="[architecture]" class="fig-architecture">
<figcaption markdown="block">
Architecture overview of our semantic versioned querying layer on top of OSTRICH.
The required parameters for the three triple pattern-based queries are indicated,
with _v<sup>d</sup>_ indicating the version of the dataset,
and _v<sup>l</sup>_ indicating the version of the language.
</figcaption>
</figure>

The source code of this prototype can be found on [GitHub](https://github.com/rdfostrich/poc-semantic-versioning){:.mandatory}
and is available under the MIT license.

### Evaluation

In order to evaluate the performance of our semantic layer,
we executed several queries with inferencing of `rdfs:subClassOf` relationships
within the BEAR-B-daily dataset from the [BEAR benchmark](cite:cites bear).

To achieve this, we created a derived version of this BEAR-B-daily dataset
where we removed all `rdf:type` relationships from instances to classes that can be inferred
through `rdfs:subClassOf` relationships for each instance.
The (44) triples identifying the subclass relationships were stored in a single version inside the language store.
As the BEAR-B-daily dataset does not provide any versioning of the language,
our evaluation excludes versioning of the language.

As OSTRICH only supports VM, DM and VQ triple pattern queries,
we only evaluate their respective semantic extension,
always using the single language version.
For S-VM, we query the last version ,
for S-DM, we query between the first and last version,
and for S-VQ, we do an intermodal query using the single language version.

### Results

The original BEAR-B-daily dataset contains 48,914 unique triples in 88 dataset versions,
while the derived dataset contains 31,761 triples, which is a reduction of 35,07%.

[](#results-s-vm), [](#results-s-dm) and [](#results-s-vq) respectively contain the evaluation results
for the S-VM, S-DM and S-VQ queries.
The table columns indicate the following:

* _Query_: The subject of the triple pattern that is queried.
* _Original_: Execution time of the query against the original BEAR-B-daily dataset.
* _Reduced_: Execution of the query against the derived BEAR-B-daily dataset, without semantic layer.
* _Inferred_: Execution of the query against the derived BEAR-B-daily dataset, with semantic layer.
* _Inference queries_: The number of queries against the OSTRICH store that were performed by the semantic layer.
* _Inferred normalized _: _Inferred_ execution time divided by the number of _inference queries_.

The results show that the backwards reasoner within our prototype
requires almost eight queries to the OSTRICH stores on average for this dataset.
The queries to the OSTRICH stores clearly form the main bottleneck.

<figure id="results-s-vm" class="table" markdown="1">

| **Query** | **Original** | **Reduced** | **Inferred** | **Inference queries** | **Inferred normalized** |
| --------- | ------------ | ----------- | ------------ | --------------------- | ----------------------- |
| dbr:Palazzo_Parisio_(Valletta) | 0.77 | 0.42 | 4.38 | 10 | 0.44 |
| dbr:Singaporean_general_election,_2015 | 0.80 | 0.31 | 5.52 | 10 | 0.55 |
| dbr:What_Do_You_Mean? | 0.95 | 0.25 | 4.66 | 9 | 0.52 |
| dbr:Dancing_with_the_Stars_(U.S._season_21) | 0.56 | 0.37 | 1.70 | 6 | 0.28 |
| dbr:Doctor_Who_(series_9) | 0.32 | 0.23 | 1.52 | 6 | 0.25 |
| dbr:My_Little_Pony... | 0.58 | 0.21 | 2.58 | 7 | 0.37 |
| dbr:2015 | 0.61 | 0.35 | 2.11 | 6 | 0.35 |
| _Average_ | _0.65_ | _0.31_ | _3.21_ | _7.71_ | _0.39_ |

<figcaption markdown="block">
Execution times in milliseconds for S-VM queries against the last dataset version,
using the first language version for an 7 S?? triple patterns.
</figcaption>
</figure>

<figure id="results-s-dm" class="table" markdown="1">

| **Query** | **Original** | **Reduced** | **Inferred** | **Inference queries** | **Inferred normalized** |
| --------- | ------------ | ----------- | ------------ | --------------------- | ----------------------- |
| dbr:Palazzo_Parisio_(Valletta) | 0.51 | 0.24 | 3.98 | 10 | 0.40 |
| dbr:Singaporean_general_election,_2015 | 0.49 | 0.24 | 3.46 | 10 | 0.35 |
| dbr:What_Do_You_Mean? | 0.47 | 0.20 | 3.09 | 9 | 0.34 |
| dbr:Dancing_with_the_Stars_(U.S._season_21) | 0.35 | 0.20 | 1.97 | 6 | 0.33 |
| dbr:Doctor_Who_(series_9) | 0.21 | 0.12 | 1.28 | 6 | 0.21 |
| dbr:My_Little_Pony... | 0.18 | 0.15 | 3.60 | 7 | 0.51 |
| dbr:2015 | 0.46 | 0.15 | 1.89 | 6 | 0.32 |
| _Average_ | _0.38_ | _0.19_ | _2.75_ | _7.71_ | _0.35_ |

<figcaption markdown="block">
Execution times in milliseconds for S-DM queries between the first and last dataset versions,
both using the first language version for 7 S?? triple patterns.
</figcaption>
</figure>

<figure id="results-s-vq" class="table" markdown="1">

| **Query** | **Original** | **Reduced** | **Inferred** | **Inference queries** | **Inferred normalized** |
| --------- | ------------ | ----------- | ------------ | --------------------- | ----------------------- |
| dbr:Palazzo_Parisio_(Valletta) | 0.76 | 0.50 | 2.91 | 10 | 0.29 |
| dbr:Singaporean_general_election,_2015 | 0.51 | 0.45 | 2.60 | 10 | 0.26 |
| dbr:What_Do_You_Mean? | 0.57 | 0.18 | 1.30 | 9 | 0.14 |
| dbr:Dancing_with_the_Stars_(U.S._season_21) | 0.45 | 0.20 | 1.99 | 6 | 0.33 |
| dbr:Doctor_Who_(series_9) | 0.31 | 0.22 | 2.34 | 6 | 0.39 |
| dbr:My_Little_Pony... | 0.17 | 0.12 | 0.93 | 7 | 0.13 |
| dbr:2015 | 0.27 | 0.10 | 1.00 | 6 | 0.17 |
| _Average_ | _0.43_ | _0.25_ | _1.87_ | _7.71_ | _0.25_ |

<figcaption markdown="block">
Execution times in milliseconds for S-VQ queries for 7 S?? triple patterns.
</figcaption>
</figure>
