# dictionary_example
Dictionaries are a first class citizen in GeoDeepDive. Gathered from external
sources, they contain terms that are used to cull and subset articles and
sentences. Additionally, they may be used as an advanced form of NER tagging.

This repository contains an example of a way to define a github-based
dictionary.  This has the benefit of being 1. publically accessible to the
GeoDeepDive infrastructure and 2. version controlled.

Two files are required at minimum to define the dictionary:

## config.yaml
The configuration for the dictionary. Attributes must include:

**name (string)** : Name of the dictionary
**user (string)** : Your preferred email. We will send an email when indexing
is complete.
**knowledge_domain (string)** : General domain of the list of terms provided.
For example, a list of taxa may have a domain of *biology*. A dictionary of
elementary particle types may have a domain of *physics* or *particle physics*.
**case_sensitive (bool)** : Whether or not case matters for the terms. If
case_sensitive is true, then capitalization of words matters.
**word_stemming (bool) **: Whether or not words should be stemmed before
matching is considered. If true, word stemming is applied and the effect is
that the matching is looser. For example, "cretaceous" and "cretacallis" both
have word stemmings of "cretac". As a general rule, if you want to find proper
nouns as terms, word stemming should be false. If you want matching to include
variants of a word (pluralizations, verbified nouns, etc), word_stemming should
be true.
**base_classification (string)** : General category or classification of terms contained in dictionary

## dictionary.csv

A list of terms in the dictionary, one per line.  If multiple words are
provided, the first word is treated as the key term, and the others are treated
as supplementary terms. This is useful for creating hierarchical/synonym/filter
structures. 

For example, an entry of:

```
Homo sapiens,Mammalia,Primates,Hominidae
```
Will create a dictionary entry of "Homo sapiens", index all documents and
mentions of "Home sapiens" in the GeoDeepDive corpus, and track whether
mentions of one of [Mammalia, Primates, Hominidae] are also be mentioned within
a document to be considered a match.

# Instructions

To create your own dictionary of indexed terms within the GeoDeepDive infrastructure:

    1. Fork this repository into your own repository.
    2. Update the config.yaml file with the attributes prescribed above.
    3. Update the dictionary.csv file with your list of terms. The first word
       of the line will the primary term matched. The remaining words are
       treated as supplementary terms, and the article is flagged if *any* of
       these additional terms are found. **NOTE** the indexing on the primary
       term will still occur -- the supplemental terms just provide an optional
       level of filtering.
    4. Once your changes are complete, send a pull request. This will trigger
       the GeoDeepDive infrastructure to add your dictionary and begin the
       indexing process.
    5. You will get an email when the dictionary is fully indexed.
