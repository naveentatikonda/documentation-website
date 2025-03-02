---
layout: default
title: Hyphenation decompounder
parent: Token filters
nav_order: 170
---

# Hyphenation decompounder token filter

The `hyphenation_decompounder` token filter is used to break down compound words into their constituent parts. This filter is particularly useful for languages like German, Dutch, and Swedish, in which compound words are common. The filter uses hyphenation patterns (typically defined in .xml files) to identify the possible locations within a compound word where it can be split into components. These components are then checked against a provided dictionary. If there is a match, those components are treated as valid tokens. For more information about hyphenation pattern files, see [FOP XML Hyphenation Patterns](https://offo.sourceforge.net/#FOP+XML+Hyphenation+Patterns).

## Parameters

The `hyphenation_decompounder` token filter can be configured with the following parameters.

Parameter | Required/Optional | Data type | Description
:--- | :--- | :--- | :--- 
`hyphenation_patterns_path` | Required | String | The path (relative to the `config` directory or absolute) to the hyphenation patterns file, which contains the language-specific rules for word splitting. The file is typically in XML format. Sample files can be downloaded from the [OFFO SourceForge project](https://sourceforge.net/projects/offo/).
`word_list` | Required if `word_list_path` is not set | Array of strings | A list of words used to validate the components generated by the hyphenation patterns.
`word_list_path` | Required if `word_list` is not set | String | The path (relative to the `config` directory or absolute) to a list of subwords.
`max_subword_size` | Optional | Integer | The maximum subword length. If the generated subword exceeds this length, it will not be added to the generated tokens. Default is `15`.
`min_subword_size` | Optional | Integer | The minimum subword length. If the generated subword is shorter than the specified length, it will not be added to the generated tokens. Default is `2`.
`min_word_size` | Optional | Integer | The minimum word character length. Word tokens shorter than this length are excluded from decomposition into subwords. Default is `5`.
`only_longest_match` | Optional | Boolean | Only includes the longest subword in the generated tokens. Default is `false`.

## Example

The following example request creates a new index named `test_index` and configures an analyzer with a `hyphenation_decompounder` filter:

```json
PUT /test_index
{
  "settings": {
    "analysis": {
      "filter": {
        "my_hyphenation_decompounder": {
          "type": "hyphenation_decompounder",
          "hyphenation_patterns_path": "analysis/hyphenation_patterns.xml",
          "word_list": ["notebook", "note", "book"],
          "min_subword_size": 3,
          "min_word_size": 5,
          "only_longest_match": false
        }
      },
      "analyzer": {
        "my_analyzer": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "my_hyphenation_decompounder"
          ]
        }
      }
    }
  }
}
```
{% include copy-curl.html %}

## Generated tokens

Use the following request to examine the tokens generated using the analyzer:

```json
POST /test_index/_analyze
{
  "analyzer": "my_analyzer",
  "text": "notebook"
}
```
{% include copy-curl.html %}

The response contains the generated tokens:

```json
{
  "tokens": [
    {
      "token": "notebook",
      "start_offset": 0,
      "end_offset": 8,
      "type": "<ALPHANUM>",
      "position": 0
    },
    {
      "token": "note",
      "start_offset": 0,
      "end_offset": 8,
      "type": "<ALPHANUM>",
      "position": 0
    },
    {
      "token": "book",
      "start_offset": 0,
      "end_offset": 8,
      "type": "<ALPHANUM>",
      "position": 0
    }
  ]
}
```
