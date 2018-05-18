## Conclusions
{:#conclusions}

In this work, we introduced the fundamental concepts on how to evaluate _semantic_ queries over versioned Linked Datasets.
For this, we extended existing _structural_ versioned query atoms by coupling them with _language versioning_ and _reasoning_.

Our wrapper-based prototypical implementation of these semantic extensions shows
that semantic querying, i.e., inference at runtime,
is able to significantly reduce storage requirements
at the cost of an increase in query time.
Together with that, it also brings the additional benefit of
being able to select the language version(s) for each query,
which would otherwise require dataset duplication when no semantic querying layer is present.

As this is merely a wrapper-based prototype,
inference is sub-optimal,
and a lot of room for improvement exist.
In future work, we foresee improvements regarding the runtime inference based on techniques from the world of OBDA.
Furthermore, some RDF stream reasoning techniques could potentially be generalized to work for versioned querying.
Native implementations of semantic versioning engines could also reduce the overhead of this wrapper-based approach.

The newly introduced foundational semantic versioned query atoms
forms a basis for future research and development of semantic versioned querying within RDF stores,
and will consequently enable enhanced analysis over multiple Linked Dataset versions.
