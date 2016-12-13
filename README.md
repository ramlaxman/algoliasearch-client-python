# Algolia Search API Client for Python

[Algolia Search](https://www.algolia.com) is a hosted full-text, numerical, and faceted search engine capable of delivering realtime results from the first keystroke.

We implemented an asynchronous version of the client that may suit your need if
you are using a framework such as `aiohttp` in your backend. This version can
be found [here](https://github.com/algolia/algoliasearch-client-python-async).

Our Python client lets you easily use the [Algolia Search API](https://www.algolia.com/doc/rest) from your backend. It wraps the [Algolia Search REST API](https://www.algolia.com/doc/rest).
[![Build Status](https://travis-ci.org/algolia/algoliasearch-client-python.svg?branch=master)](https://travis-ci.org/algolia/algoliasearch-client-python) [![PyPI version](https://badge.fury.io/py/algoliasearch.svg?branch=master)](http://badge.fury.io/py/algoliasearch) [![Coverage Status](https://coveralls.io/repos/algolia/algoliasearch-client-python/badge.svg?branch=master)](https://coveralls.io/r/algolia/algoliasearch-client-python)


# Table of Contents


**Getting Started**

1. [Install](#install)
1. [Init index - `init_index`](#init-index)
1. [Quick Start](#quick-start)

**Search**

1. [Search in an index - `search`](#search-in-an-index)
1. [Search Response Format](#search-response-format)
1. [Search Parameters](#search-parameters)
1. [Search in indices - `multiple_queries`](#search-in-indices)
1. [Get Objects - `get_objects`](#get-objects)

**Indexing**

1. [Add Objects - `add_objects`](#add-objects)
1. [Update objects - `save_objects`](#update-objects)
1. [Partial update objects - `partial_update_objects`](#partial-update-objects)
1. [Delete objects - `delete_objects`](#delete-objects)
1. [Delete by query - `delete_by_query`](#delete-by-query)
1. [Wait for operations - `wait_task`](#wait-for-operations)

**Settings**

1. [Get settings - `get_settings`](#get-settings)
1. [Set settings - ``](#set-settings)
1. [Index settings parameters](#index-settings-parameters)

**Parameters**

1. [Overview](#overview)
1. [Search](#search)
1. [Attributes](#attributes)
1. [Ranking](#ranking)
1. [Filtering / Faceting](#filtering--faceting)
1. [Highlighting / Snippeting](#highlighting--snippeting)
1. [Pagination](#pagination)
1. [Typos](#typos)
1. [Geo-Search](#geo-search)
1. [Query Strategy](#query-strategy)
1. [Advanced](#advanced)

**Manage Indices**

1. [Create an index](#create-an-index)
1. [List indices - `list_indexes`](#list-indices)
1. [Delete index - `delete_index`](#delete-index)
1. [Clear index - `clear_index`](#clear-index)
1. [Copy index - `copy_index`](#copy-index)
1. [Move index - `move_index`](#move-index)

**Api keys**

1. [Overview](#overview)
1. [Generate key - `generate_secured_api_key`](#generate-key)

**Synonyms**

1. [Save synonym - `save_synonym`](#save-synonym)
1. [Batch synonyms - `batch_synonyms`](#batch-synonyms)
1. [Editing Synonyms](#editing-synonyms)
1. [Delete synonym - `delete_synonym`](#delete-synonym)
1. [Clear all synonyms - `clear_synonyms`](#clear-all-synonyms)
1. [Get synonym - `get_synonym`](#get-synonym)
1. [Search synonyms - `search_synonyms`](#search-synonyms)

**Advanced**

1. [Custom batch - `batch`](#custom-batch)
1. [Backup / Export an index - `browse`](#backup--export-an-index)
1. [List api keys - `list_api_keys`](#list-api-keys)
1. [Add user key - `add_user_key`](#add-user-key)
1. [Update user key - `update_user_key`](#update-user-key)
1. [Delete user key - `delete_user_key`](#delete-user-key)
1. [Get key permissions - `get_user_key_acl`](#get-key-permissions)
1. [Get logs - `get_logs`](#get-logs)

**Troubleshooting**

1. [Unreachable hosts](#unreachable-hosts)


# Guides & Tutorials

Check our [online guides](https://www.algolia.com/doc):

* [Data Formatting](https://www.algolia.com/doc/indexing/formatting-your-data)
* [Import and Synchronize data](https://www.algolia.com/doc/indexing/import-synchronize-data/php)
* [Autocomplete](https://www.algolia.com/doc/search/auto-complete)
* [Instant search page](https://www.algolia.com/doc/search/instant-search)
* [Filtering and Faceting](https://www.algolia.com/doc/search/filtering-faceting)
* [Sorting](https://www.algolia.com/doc/relevance/sorting)
* [Ranking Formula](https://www.algolia.com/doc/relevance/ranking)
* [Typo-Tolerance](https://www.algolia.com/doc/relevance/typo-tolerance)
* [Geo-Search](https://www.algolia.com/doc/geo-search/geo-search-overview)
* [Security](https://www.algolia.com/doc/security/best-security-practices)
* [API-Keys](https://www.algolia.com/doc/security/api-keys)
* [REST API](https://www.algolia.com/doc/rest)


# Getting Started




<section>

## Install

Install AlgoliaSearch using pip:

```bash
pip install --upgrade algoliasearch
```




</section>

<section>


## Init index - `init_index` 

To initialize the client, you need your **Application ID** and **API Key**. You can find both of them on [your Algolia account](https://www.algolia.com/api-keys).

```python
from algoliasearch import algoliasearch

client = algoliasearch.Client("YourApplicationID", 'YourAPIKey')
index = client.init_index("contact")
```

If you use this API Client with Google AppEngine (Thanks [@apassant](https://github.com/apassant)), it will use `urlfetch` instead of using the `request` module. Please be aware of [urlfetch's limits](https://cloud.google.com/appengine/docs/python/urlfetch/), and note that SSL certificates will not be verified for calls to domains other than algolia.net due to the lack of SNI support in `urlfetch`. To run unit tests on the AppEngine stub, please define an `APPENGINE_RUNTIME` enviroment variable.




</section>



<section>

## Quick Start

In 30 seconds, this quick start tutorial will show you how to index and search objects.

Without any prior configuration, you can start indexing [500 contacts](https://github.com/algolia/algoliasearch-client-csharp/blob/master/contacts.json) in the ```contacts``` index using the following code:
```python
index = client.init_index("contact")
batch = json.load(open('contacts.json'))
index.add_objects(batch)
```


You can now search for contacts using firstname, lastname, company, etc. (even with typos):

```python
# search by firstname
print index.search("jimmie")
# search a firstname with typo
print index.search("jimie")
# search for a company
print index.search("california paint")
# search for a firstname & company
print index.search("jimmie paint")
```


Settings can be customized to tune the search behavior. For example, you can add a custom sort by number of followers to the already great built-in relevance:

```python
index.set_settings({"customRanking": ["desc(followers)"]})
```


You can also configure the list of attributes you want to index by order of importance (first = most important):

```python
index.set_settings({"searchableAttributes": ["lastname", "firstname", "company",
                                         "email", "city", "address"]})
```


Since the engine is designed to suggest results as you type, you'll generally search by prefix. In this case the order of attributes is very important to decide which hit is the best:

```python
print index.search("or")
print index.search("jim")
```


**Note:** If you are building a web application, you may be more interested in using our [JavaScript client](https://github.com/algolia/algoliasearch-client-js) to perform queries.


It brings two benefits:
  * Your users get a better response time by not going through your servers
  * It will offload unnecessary tasks from your servers

```html
<script src="https://cdn.jsdelivr.net/algoliasearch/3/algoliasearch.min.js"></script>
<script>
var client = algoliasearch('ApplicationID', 'apiKey');
var index = client.initIndex('indexName');

// perform query "jim"
index.search('jim', searchCallback);

// the last optional argument can be used to add search parameters
index.search(
  'jim', {
    hitsPerPage: 5,
    facets: '*',
    maxValuesPerFacet: 10
  },
  searchCallback
);

function searchCallback(err, content) {
  if (err) {
    console.error(err);
    return;
  }

  console.log(content);
}
</script>
```

</section>


# Search



<section>


## Search in an index - `search` 


**Notes:** If you are building a web application, you may be more interested in using our [JavaScript client](https://github.com/algolia/algoliasearch-client-js) to perform queries. It brings two benefits:
  * Your users get a better response time by not going through your servers
  * It will offload unnecessary tasks from your servers.


To perform a search, you only need to initialize the index and perform a call to the search function.

The search query allows only to retrieve 1000 hits. If you need to retrieve more than 1000 hits (e.g. for SEO), you can use [Backup / Retrieve all index content](#backup--export-an-index).

```python
index = client.init_index("contacts")
res = index.search("query string")
res = index.search("query string", { "attributesToRetrieve": "firstname,lastname", "hitsPerPage": 20})
```



</section>



<section>

## Search Response Format

### Sample

The server response will look like:

```json
{
  "hits": [
    {
      "firstname": "Jimmie",
      "lastname": "Barninger",
      "objectID": "433",
      "_highlightResult": {
        "firstname": {
          "value": "<em>Jimmie</em>",
          "matchLevel": "partial"
        },
        "lastname": {
          "value": "Barninger",
          "matchLevel": "none"
        },
        "company": {
          "value": "California <em>Paint</em> & Wlpaper Str",
          "matchLevel": "partial"
        }
      }
    }
  ],
  "page": 0,
  "nbHits": 1,
  "nbPages": 1,
  "hitsPerPage": 20,
  "processingTimeMS": 1,
  "query": "jimmie paint",
  "params": "query=jimmie+paint&attributesToRetrieve=firstname,lastname&hitsPerPage=50"
}
```

### Fields

- `hits` (array): The hits returned by the search, sorted according to the ranking formula.

    Hits are made of the JSON objects that you stored in the index; therefore, they are mostly schema-less. However, Algolia does enrich them with a few additional fields:

    - `_highlightResult` (object, optional): Highlighted attributes. *Note: Only returned when [attributesToHighlight](#attributestohighlight) is non-empty.*

        - `${attribute_name}` (object): Highlighting for one attribute.

            - `value` (string): Markup text with occurrences highlighted. The tags used for highlighting are specified via [highlightPreTag](#highlightpretag) and [highlightPostTag](#highlightposttag).

            - `matchLevel` (string, enum) = {`none` | `partial` | `full`}: Indicates how well the attribute matched the search query.

            - `matchedWords` (array): List of words *from the query* that matched the object.

            - `fullyHighlighted` (boolean): Whether the entire attribute value is highlighted.

    - `_snippetResult` (object, optional): Snippeted attributes. *Note: Only returned when [attributesToSnippet](#attributestosnippet) is non-empty.*

        - `${attribute_name}` (object): Snippeting for the corresponding attribute.

            - `value` (string): Markup text with occurrences highlighted and optional ellipsis indicators. The tags used for highlighting are specified via [highlightPreTag](#highlightpretag) and [highlightPostTag](#highlightposttag). The text used to indicate ellipsis is specified via [snippetEllipsisText](#snippetellipsistext).

            - `matchLevel` (string, enum) = {`none` | `partial` | `full`}: Indicates how well the attribute matched the search query.

    - `_rankingInfo` (object, optional): Ranking information. *Note: Only returned when [getRankingInfo](#getrankinginfo) is `true`.*

        - `nbTypos` (integer): Number of typos encountered when matching the record. Corresponds to the `typos` ranking criterion in the ranking formula.

        - `firstMatchedWord` (integer): Position of the most important matched attribute in the attributes to index list. Corresponds to the `attribute` ranking criterion in the ranking formula.

        - `proximityDistance` (integer): When the query contains more than one word, the sum of the distances between matched words. Corresponds to the `proximity` criterion in the ranking formula.

        - `userScore` (integer): Custom ranking for the object, expressed as a single numerical value. Conceptually, it's what the position of the object would be in the list of all objects sorted by custom ranking. Corresponds to the `custom` criterion in the ranking formula.

        - `geoDistance` (integer): Distance between the geo location in the search query and the best matching geo location in the record, divided by the geo precision.

        - `geoPrecision` (integer): Precision used when computed the geo distance, in meters. All distances will be floored to a multiple of this precision.

        - `nbExactWords` (integer): Number of exactly matched words. If `alternativeAsExact` is set, it may include plurals and/or synonyms.

        - `words` (integer): Number of matched words, including prefixes and typos.

        - `filters` (integer): *This field is reserved for advanced usage.* It will be zero in most cases.

    - `_distinctSeqID` (integer): *Note: Only returned when [distinct](#distinct) is non-zero.* When two consecutive results have the same value for the attribute used for "distinct", this field is used to distinguish between them.

- `nbHits` (integer): Number of hits that the search query matched.

- `page` (integer): Index of the current page (zero-based). See the [page](#page) search parameter. *Note: Not returned if you use `offset`/`length` for pagination.*

- `hitsPerPage` (integer): Maximum number of hits returned per page. See the [hitsPerPage](#hitsperpage) search parameter. *Note: Not returned if you use `offset`/`length` for pagination.*

- `nbPages` (integer): Number of pages corresponding to the number of hits. Basically, `ceil(nbHits / hitsPerPage)`. *Note: Not returned if you use `offset`/`length` for pagination.*

- `processingTimeMS` (integer): Time that the server took to process the request, in milliseconds. *Note: This does not include network time.*

- `query` (string): An echo of the query text. See the [query](#query) search parameter.

- `queryAfterRemoval` (string, optional): *Note: Only returned when [removeWordsIfNoResults](#removewordsifnoresults) is set to `lastWords` or `firstWords`.* A markup text indicating which parts of the original query have been removed in order to retrieve a non-empty result set. The removed parts are surrounded by `<em>` tags.

- `params` (string, URL-encoded): An echo of all search parameters.

- `message` (string, optional): Used to return warnings about the query.

- `aroundLatLng` (string, optional): *Note: Only returned when [aroundLatLngViaIP](#aroundlatlngviaip) is set.* The computed geo location. **Warning: for legacy reasons, this parameter is a string and not an object.** Format: `${lat},${lng}`, where the latitude and longitude are expressed as decimal floating point numbers.

- `automaticRadius` (integer, optional): *Note: Only returned for geo queries without an explicitly specified radius (see `aroundRadius`).* The automatically computed radius. **Warning: for legacy reasons, this parameter is a string and not an integer.**

When [getRankingInfo](#getrankinginfo) is set to `true`, the following additional fields are returned:

- `serverUsed` (string): Actual host name of the server that processed the request. (Our DNS supports automatic failover and load balancing, so this may differ from the host name used in the request.)

- `parsedQuery` (string): The query string that will be searched, after normalization. Normalization includes removing stop words (if [removeStopWords](#removestopwords) is enabled), and transforming portions of the query string into phrase queries (see [advancedSyntax](#advancedsyntax)).

- `timeoutCounts` (boolean): Whether a timeout was hit when computing the facet counts. When `true`, the counts will be interpolated (i.e. approximate). See also `exhaustiveFacetsCount`.

- `timeoutHits` (boolean): Whether a timeout was hit when retrieving the hits. When true, some results may be missing.

... and ranking information is also added to each of the hits (see above).

When [facets](#facets) is non-empty, the following additional fields are returned:

- `facets` (object): Maps each facet name to the corresponding facet counts:

    - `${facet_name}` (object): Facet counts for the corresponding facet name:

        - `${facet_value}` (integer): Count for this facet value.

- `facets_stats` (object, optional): *Note: Only returned when at least one of the returned facets contains numerical values.* Statistics for numerical facets:

    - `${facet_name}` (object): The statistics for a given facet:

        - `min` (integer | float): The minimum value in the result set.

        - `max` (integer | float): The maximum value in the result set.

        - `avg` (integer | float): The average facet value in the result set.

        - `sum` (integer | float): The sum of all values in the result set.

- `exhaustiveFacetsCount` (boolean): Whether the counts are exhaustive (`true`) or approximate (`false`). *Note: When using [distinct](#distinct), the facet counts cannot be exhaustive.*

</section>

<section>

## Search Parameters

Here is the list of parameters you can use with the search method (`search` [scope](#scope)):
Parameters that can also be used in a setSettings also have the `indexing` [scope](#scope)


**Search**

- [query](#query) `search`

**Attributes**

- [attributesToRetrieve](#attributestoretrieve) `settings`, `search`
- [restrictSearchableAttributes](#restrictsearchableattributes) `search`

**Filtering / Faceting**

- [filters](#filters) `search`
- [facets](#facets) `search`
- [maxValuesPerFacet](#maxvaluesperfacet) `settings`, `search`

**Highlighting / Snippeting**

- [attributesToHighlight](#attributestohighlight) `settings`, `search`
- [attributesToSnippet](#attributestosnippet) `settings`, `search`
- [highlightPreTag](#highlightpretag) `settings`, `search`
- [highlightPostTag](#highlightposttag) `settings`, `search`
- [snippetEllipsisText](#snippetellipsistext) `settings`, `search`
- [restrictHighlightAndSnippetArrays](#restricthighlightandsnippetarrays) `settings`, `search`

**Pagination**

- [page](#page) `search`
- [hitsPerPage](#hitsperpage) `settings`, `search`
- [offset](#offset) `search`
- [length](#length) `search`

**Typos**

- [minWordSizefor1Typo](#minwordsizefor1typo) `settings`, `search`
- [minWordSizefor2Typos](#minwordsizefor2typos) `settings`, `search`
- [typoTolerance](#typotolerance) `settings`, `search`
- [allowTyposOnNumericTokens](#allowtyposonnumerictokens) `settings`, `search`
- [ignorePlurals](#ignoreplurals) `settings`, `search`
- [disableTypoToleranceOnAttributes](#disabletypotoleranceonattributes) `settings`, `search`

**Geo-Search**

- [aroundLatLng](#aroundlatlng) `search`
- [aroundLatLngViaIP](#aroundlatlngviaip) `search`
- [aroundRadius](#aroundradius) `search`
- [aroundPrecision](#aroundprecision) `search`
- [minimumAroundRadius](#minimumaroundradius) `search`
- [insideBoundingBox](#insideboundingbox) `search`
- [insidePolygon](#insidepolygon) `search`

**Query Strategy**

- [removeWordsIfNoResults](#removewordsifnoresults) `settings`, `search`
- [advancedSyntax](#advancedsyntax) `settings`, `search`
- [optionalWords](#optionalwords) `settings`, `search`
- [removeStopWords](#removestopwords) `settings`, `search`
- [exactOnSingleWordQuery](#exactonsinglewordquery) `settings`, `search`
- [alternativesAsExact](#alternativesasexact) `setting`, `search`

**Advanced**

- [synonyms](#synonyms) `search`
- [replaceSynonymsInHighlight](#replacesynonymsinhighlight) `settings`, `search`
- [minProximity](#minproximity) `settings`, `search`
- [responseFields](#responsefields) `settings`, `search`
- [distinct](#distinct) `settings`, `search`
- [getRankingInfo](#getrankinginfo) `search`
- [numericFilters (deprecated)](#numericfilters-deprecated) `search`
- [facetFilters (deprecated)](#facetfilters-deprecated) `search`
- [analytics](#analytics) `search`
- [analyticsTags](#analyticstags) `search`

</section>

<section>


## Search in indices - `multiple_queries` 

You can send multiple queries with a single API call using a batch of queries:

```python
# perform 3 queries in a single API call:
# - 1st query targets index `categories`
# - 2nd and 3rd queries target index `products`
results = self.client.multiple_queries([{"indexName" : "categories", "query" : myQueryString, "hitsPerPage": 3}
  , {"indexName" : "categories", "query" : myQueryString, "hitsPerPage": 3, "filters": "_tags:promotion"}
  , {"indexName" : "categories", "query" : myQueryString, "hitsPerPage": 10}])

print results["results"]
```


You can specify a `strategy` parameter to optimize your multiple queries:

- `none`: Execute the sequence of queries until the end.
- `stopIfEnoughMatches`: Execute the sequence of queries until the number of hits is reached by the sum of hits.

### Response

The resulting JSON contains the following fields:

- `results` (array): The results for each request, in the order they were submitted. The contents are the same as in [Search in an index](/doc/api-client/python/search#search-in-an-index).

    Each result also includes the following additional fields:

    - `index` (string): The name of the targeted index.

    - `processed` (boolean, optional): *Note: Only returned when `strategy` is `stopIfEnoughmatches`.* Whether the query was processed.


</section>



<section>


## Get Objects - `get_objects` 

You can easily retrieve an object using its `objectID` and optionally specify a comma separated list of attributes you want:

```python
# Retrieves all attributes
index.get_object("myID")
# Retrieves firstname and lastname attributes
res = index.get_object("myID", "firstname,lastname")
# Retrieves only the firstname attribute
res = index.get_object("myID", "firstname")
```


You can also retrieve a set of objects:

```python
res = index.get_objects(["myID1", "myID2"])
```



</section>





# Indexing



<section>


## Add Objects - `add_objects` 

Each entry in an index has a unique identifier called `objectID`. There are two ways to add an entry to the index:

 1. Supplying your own `objectID`.
 2. Using automatic `objectID` assignment. You will be able to access it in the answer.

You don't need to explicitly create an index, it will be automatically created the first time you add an object.
Objects are schema less so you don't need any configuration to start indexing. If you wish to configure things, the settings section provides details about advanced settings.

Example with automatic `objectID` assignments:

```python
res = index.add_objects([{"firstname": "Jimmie",
                         "lastname": "Barninger"},
                        {"firstname": "Warren",
                         "lastname": "Speach"}])
```


Example with manual `objectID` assignments:

```python
res = index.add_objects([{"objectID": "1",
                         "firstname": "Jimmie",
                         "lastname": "Barninger"},
                        {"objectID": "2",
                         "firstname": "Warren",
                         "lastname": "Speach"}])
```


To add a single object, use the `[Add Objects](/doc/api-client/python/indexing#add-objects)` method:

```python
res = index.add_object({"firstname": "Jimmie",
                       "lastname": "Barninger"}, "myID")
print "ObjectID=%s" % res["objectID"]
```



</section>


<section>


## Update objects - `save_objects` 

You have three options when updating an existing object:

 1. Replace all its attributes.
 2. Replace only some attributes.
 3. Apply an operation to some attributes.

Example on how to replace all attributes existing objects:

```python
res = index.save_objects([{"firstname": "Jimmie",
                          "lastname": "Barninger",
                           "objectID": "myID1"},
                          {"firstname": "Warren",
                          "lastname": "Speach",
                           "objectID": "myID2"}])
```


To update a single object, you can use the following method:

```python
index.save_object({"firstname": "Jimmie",
                  "lastname": "Barninger",
                  "city": "New York",
                  "objectID": "myID"})
```



</section>


<section>


## Partial update objects - `partial_update_objects` 

You have many ways to update an object's attributes:

 1. Set the attribute value
 2. Add a string or number element to an array
 3. Remove an element from an array
 4. Add a string or number element to an array if it doesn't exist
 5. Increment an attribute
 6. Decrement an attribute

Example to update only the city attribute of an existing object:

```python
index.partial_update_object({"city": "San Francisco",
                           "objectID": "myID"})
```


Example to add a tag:

```python
index.partial_update_object({"_tags": { "value": "MyTag", "_operation": "Add"},
                           "objectID": "myID"})
```


Example to remove a tag:

```python
index.partial_update_object({"_tags": { "value": "MyTag", "_operation": "Remove"},
                           "objectID": "myID"})
```


Example to add a tag if it doesn't exist:

```python
index.partial_update_object({"_tags": { "value": "MyTag", "_operation": "AddUnique"},
                           "objectID": "myID"})
```


Example to increment a numeric value:

```python
index.partial_update_object({"price": { "value": 42, "_operation": "Increment"},
                           "objectID": "myID"})
```


Note: Here we are incrementing the value by `42`. To increment just by one, put
`value:1`.

Example to decrement a numeric value:

```python
index.partial_update_object({"price": { "value": 42, "_operation": "Decrement"},
                           "objectID": "myID"})
```


Note: Here we are decrementing the value by `42`. To decrement just by one, put
`value:1`.

To partial update multiple objects using one API call, you can use the `[Partial update objects](/doc/api-client/python/indexing#partial-update-objects)` method:

```python
res = index.partial_update_objects([{"firstname": "Jimmie",
                                   "objectID": "myID1"},
                                  {"firstname": "Warren",
                                   "objectID": "myID2"}])
```



</section>


<section>


## Delete objects - `delete_objects` 

You can delete objects using their `objectID`:

```python
res = index.delete_objects(["myID1", "myID2"])
```


To delete a single object, you can use the `[Delete objects](/doc/api-client/python/indexing#delete-objects)` method:

```python
index.delete_object("myID")
```



</section>


<section>


## Delete by query - `delete_by_query` 

You can delete all objects matching a single query with the following code. Internally, the API client performs the query, deletes all matching hits, and waits until the deletions have been applied.


Take your precautions when using this method. Calling it with an empty query will result in cleaning the index of all its records.

```python
params = {}
index.delete_by_query("John", params)
```



</section>


<section>


## Wait for operations - `wait_task` 

All write operations in Algolia are asynchronous by design.

It means that when you add or update an object to your index, our servers will
reply to your request with a `taskID` as soon as they understood the write
operation.

The actual insert and indexing will be done after replying to your code.

You can wait for a task to complete using the `waitTask` method on the `taskID` returned by a write operation.

For example, to wait for indexing of a new object:

```python
res = index.add_object({"firstname": "Jimmie",
                       "lastname": "Barninger"})
index.wait_task(res["taskID"])
```


If you want to ensure multiple objects have been indexed, you only need to check
the biggest `taskID`.


</section>



# Settings



<section>


## Get settings - `get_settings` 

You can retrieve settings:

```python
settings = index.get_settings()
print settings
```



</section>


<section>


## Set settings - `` 

```python
index.set_settings({"customRanking": ["desc(followers)"]})
```


**Warning**

Performance wise, it's better to do a `` before pushing the data

### Replica settings

You can forward all settings updates to the replicas of an index by using the `forwardToReplicas` option:

```python
index.set_settings({"customRanking": ["desc(followers)"]}, True)
```





</section>



<section>

## Index settings parameters

Here is the list of parameters you can use with the set settings method (`settings` [scope](#scope)).


Parameters that can be overridden at search time also have the `search` [scope](#scope).


**Attributes**

- [searchableAttributes](#searchableattributes) `settings`
- [attributesForFaceting](#attributesforfaceting) `settings`
- [unretrievableAttributes](#unretrievableattributes) `settings`
- [attributesToRetrieve](#attributestoretrieve) `settings`, `search`

**Ranking**

- [ranking](#ranking) `settings`
- [customRanking](#customranking) `settings`
- [replicas](#replicas) `settings`

**Filtering / Faceting**

- [maxValuesPerFacet](#maxvaluesperfacet) `settings`, `search`

**Highlighting / Snippeting**

- [attributesToHighlight](#attributestohighlight) `settings`, `search`
- [attributesToSnippet](#attributestosnippet) `settings`, `search`
- [highlightPreTag](#highlightpretag) `settings`, `search`
- [highlightPostTag](#highlightposttag) `settings`, `search`
- [snippetEllipsisText](#snippetellipsistext) `settings`, `search`
- [restrictHighlightAndSnippetArrays](#restricthighlightandsnippetarrays) `settings`, `search`

**Pagination**

- [hitsPerPage](#hitsperpage) `settings`, `search`
- [paginationLimitedTo](#paginationlimitedto) `settings`

**Typos**

- [minWordSizefor1Typo](#minwordsizefor1typo) `settings`, `search`
- [minWordSizefor2Typos](#minwordsizefor2typos) `settings`, `search`
- [typoTolerance](#typotolerance) `settings`, `search`
- [allowTyposOnNumericTokens](#allowtyposonnumerictokens) `settings`, `search`
- [ignorePlurals](#ignoreplurals) `settings`, `search`
- [disableTypoToleranceOnAttributes](#disabletypotoleranceonattributes) `settings`, `search`
- [separatorsToIndex](#separatorstoindex) `settings`

**Query Strategy**

- [queryType](#querytype) `settings`
- [removeWordsIfNoResults](#removewordsifnoresults) `settings`, `search`
- [advancedSyntax](#advancedsyntax) `settings`, `search`
- [optionalWords](#optionalwords) `settings`, `search`
- [removeStopWords](#removestopwords) `settings`, `search`
- [disableExactOnAttributes](#disableexactonattributes) `settings`
- [exactOnSingleWordQuery](#exactonsinglewordquery) `settings`, `search`

**Advanced**

- [attributeForDistinct](#attributefordistinct) `settings`
- [replaceSynonymsInHighlight](#replacesynonymsinhighlight) `settings`, `search`
- [placeholders](#placeholders) `settings`
- [altCorrections](#altcorrections) `settings`
- [minProximity](#minproximity) `settings`, `search`
- [responseFields](#responsefields) `settings`, `search`
- [distinct](#distinct) `settings`, `search`
- [numericAttributesForFiltering](#numericattributesforfiltering) `settings`
- [allowCompressionOfIntegerArray](#allowcompressionofintegerarray) `settings`

# Parameters



<section>

## Overview

### Scope

Each parameter in this page has a scope. Depending on the scope, you can use the parameter within the `setSettings`
and/or the `search` method

They are three scopes:

- `settings`: The setting can only be used in the `setSettings` method
- `search`: The setting can only be used in the `search` method
- `settings` `search`: The setting can be used in the `setSettings` method and be override in the`search` method


### Parameters List


**Search**

- [query](#query) `search`

**Attributes**

- [searchableAttributes](#searchableattributes) `settings`
- [attributesForFaceting](#attributesforfaceting) `settings`
- [unretrievableAttributes](#unretrievableattributes) `settings`
- [attributesToRetrieve](#attributestoretrieve) `settings`, `search`
- [restrictSearchableAttributes](#restrictsearchableattributes) `search`

**Ranking**

- [ranking](#ranking) `settings`
- [customRanking](#customranking) `settings`
- [replicas](#replicas) `settings`

**Filtering / Faceting**

- [filters](#filters) `search`
- [facets](#facets) `search`
- [maxValuesPerFacet](#maxvaluesperfacet) `settings`, `search`

**Highlighting / Snippeting**

- [attributesToHighlight](#attributestohighlight) `settings`, `search`
- [attributesToSnippet](#attributestosnippet) `settings`, `search`
- [highlightPreTag](#highlightpretag) `settings`, `search`
- [highlightPostTag](#highlightposttag) `settings`, `search`
- [snippetEllipsisText](#snippetellipsistext) `settings`, `search`
- [restrictHighlightAndSnippetArrays](#restricthighlightandsnippetarrays) `settings`, `search`

**Pagination**

- [page](#page) `search`
- [hitsPerPage](#hitsperpage) `settings`, `search`
- [offset](#offset) `search`
- [length](#length) `search`
- [paginationLimitedTo](#paginationlimitedto) `settings`

**Typos**

- [minWordSizefor1Typo](#minwordsizefor1typo) `settings`, `search`
- [minWordSizefor2Typos](#minwordsizefor2typos) `settings`, `search`
- [typoTolerance](#typotolerance) `settings`, `search`
- [allowTyposOnNumericTokens](#allowtyposonnumerictokens) `settings`, `search`
- [ignorePlurals](#ignoreplurals) `settings`, `search`
- [disableTypoToleranceOnAttributes](#disabletypotoleranceonattributes) `settings`, `search`
- [separatorsToIndex](#separatorstoindex) `settings`

**Geo-Search**

- [aroundLatLng](#aroundlatlng) `search`
- [aroundLatLngViaIP](#aroundlatlngviaip) `search`
- [aroundRadius](#aroundradius) `search`
- [aroundPrecision](#aroundprecision) `search`
- [minimumAroundRadius](#minimumaroundradius) `search`
- [insideBoundingBox](#insideboundingbox) `search`
- [insidePolygon](#insidepolygon) `search`

**Query Strategy**

- [queryType](#querytype) `settings`
- [removeWordsIfNoResults](#removewordsifnoresults) `settings`, `search`
- [advancedSyntax](#advancedsyntax) `settings`, `search`
- [optionalWords](#optionalwords) `settings`, `search`
- [removeStopWords](#removestopwords) `settings`, `search`
- [disablePrefixOnAttributes](#disableprefixonattributes) `seetings`
- [disableExactOnAttributes](#disableexactonattributes) `settings`
- [exactOnSingleWordQuery](#exactonsinglewordquery) `settings`, `search`
- [alternativesAsExact](#alternativesasexact) `setting`, `search`

**Advanced**

- [attributeForDistinct](#attributefordistinct) `settings`
- [synonyms](#synonyms) `search`
- [replaceSynonymsInHighlight](#replacesynonymsinhighlight) `settings`, `search`
- [placeholders](#placeholders) `settings`
- [altCorrections](#altcorrections) `settings`
- [minProximity](#minproximity) `settings`, `search`
- [responseFields](#responsefields) `settings`, `search`
- [distinct](#distinct) `settings`, `search`
- [getRankingInfo](#getrankinginfo) `search`
- [numericAttributesForFiltering](#numericattributesforfiltering) `settings`
- [allowCompressionOfIntegerArray](#allowcompressionofintegerarray) `settings`
- [numericFilters (deprecated)](#numericfilters-deprecated) `search`
- [facetFilters (deprecated)](#facetfilters-deprecated) `search`
- [analytics](#analytics) `search`
- [analyticsTags](#analyticstags) `search`

</section>


<section>

## Search






<div class='api-client-parameter'>


### query


- scope: `search`
- type: `string`
- default: ""


The instant search query string, used to set the string you want to search in your index.
If no query parameter is set, the textual search will match with all the objects.


</div>

</section>

<section>

## Attributes






<div class='api-client-parameter'>


### searchableAttributes


- scope: `settings`
- type: `array of strings`
- default: *
- formerly known as: `attributesToIndex`

The list of attributes you want index (i.e. to make searchable).

If set to null, all textual and numerical attributes of your objects are indexed.
Make sure you updated this setting to get optimal results.

This parameter has two important uses:

1. **Limit the attributes to index.** For example, if you store the URL of a picture, you want to store it and be able to retrieve it, but you probably don't want to search in the URL.

2. **Control part of the ranking.** The contents of the `searchableAttributes` parameter impacts ranking in two complementary ways:

    First, the order in which attributes are listed defines their ranking priority: matches in attributes at the beginning of the list will be considered more important than matches in attributes further down the list. To assign the same priority to several attributes, pass them within the same string, separated by commas. For example, by specifying `["title,"alternative_title", "text"]`, `title` and `alternative_title` will have the same priority, but a higher priority than `text`.

    Then, within the same attribute, matches near the beginning of the text will be considered more important than matches near the end. You can disable this behavior by wrapping your attribute name inside an `unordered()` modifier. For example, `["title", "unordered(text)"]` will consider all positions inside the `text` attribute as equal, but positions inside the `title` attribute will still matter.

To get a full description of how the ranking works, you can have a look at our [Ranking guide](https://www.algolia.com/doc/guides/relevance/ranking).



</div>




<div class='api-client-parameter'>


### attributesForFaceting


- scope: `settings`
- type: `array of strings`



The list of attributes you want to use for faceting.
All strings within these attributes will be extracted and added as facets.
If set to `null`, no attribute is used for faceting.


</div>




<div class='api-client-parameter'>


### unretrievableAttributes


- scope: `settings`
- type: `array of strings`



The list of attributes that cannot be retrieved at query time.
This feature allows you to have attributes that are used for indexing
and/or ranking but cannot be retrieved.


</div>




<div class='api-client-parameter'>


### attributesToRetrieve


- scope: `settings` `search`
- type: `array of strings`
- default: *


A string that contains the list of attributes you want to retrieve in order to minimize the size of the JSON answer.

Attributes are separated with a comma (for example `"name,address"`).
You can also use a string array encoding (for example `["name","address"]` ).
By default, all attributes are retrieved.
You can also use `*` to retrieve all values when an **attributesToRetrieve** setting is specified for your index.

**Note:** `objectID` is always retrieved, even when not specified.


</div>




<div class='api-client-parameter'>


### restrictSearchableAttributes


- scope: `search`
- type: `array of strings`
- default: searchableAttributes


List of attributes you want to use for textual search (must be a subset of the `searchableAttributes` index setting).
Attributes are separated with a comma such as `"name,address"`.
You can also use JSON string array encoding such as `encodeURIComponent("[\"name\",\"address\"]")`.
By default, all attributes specified in the `searchableAttributes` settings are used to search.


</div>

</section>

<section>

## Ranking






<div class='api-client-parameter'>


### ranking


- scope: `settings`
- type: `array of strings`
- default: ['typo', 'geo', 'words', 'filters', 'proximity', 'attribute', 'exact', 'custom']



Controls the way results are sorted.

We have nine available criterion:

* `typo`: Sort according to number of typos.
* `geo`: Sort according to decreasing distance when performing a geo location based search.
* `words`: Sort according to the number of query words matched by decreasing order. This parameter is useful when you use the `optionalWords` query parameter to have results with the most matched words first.
* `proximity`: Sort according to the proximity of the query words in hits.
* `attribute`: Sort according to the order of attributes defined by searchableAttributes.
* `exact`:
  * If the user query contains one word: sort objects having an attribute that is exactly the query word before others. For example, if you search for the TV show "V", you want to find it with the "V" query and avoid getting all popular TV shows starting by the letter V before it.
  * If the user query contains multiple words: sort according to the number of words that matched exactly (not as a prefix).
* `custom`: Sort according to a user defined formula set in the `customRanking` attribute.
* `asc(attributeName)`: Sort according to a numeric attribute using ascending order. `attributeName` can be the name of any numeric attribute in your records (integer, double or boolean).
* `desc(attributeName)`: Sort according to a numeric attribute using descending order. `attributeName` can be the name of any numeric attribute in your records (integer, double or boolean).

To get a full description of how the Ranking works, you can have a look at our [Ranking guide](https://www.algolia.com/doc/guides/relevance/ranking).



</div>




<div class='api-client-parameter'>


### customRanking


- scope: `settings`
- type: `array of strings`
- default: []



Lets you specify part of the ranking.

The syntax of this condition is an array of strings containing attributes
prefixed by the asc (ascending order) or desc (descending order) operator.

For example, `"customRanking" => ["desc(population)", "asc(name)"]`.

To get a full description of how the Custom Ranking works,
you can have a look at our [Ranking guide](https://www.algolia.com/doc/guides/relevance/ranking).


</div>




<div class='api-client-parameter'>


### replicas


- scope: `settings`
- type: `array of strings`
- default: []
- formerly known as: `slaves`


The list of indices on which you want to replicate all write operations.

In order to get response times in milliseconds, we pre-compute part of the ranking during indexing.

If you want to use different ranking configurations depending of the use case,
you need to create one index per ranking configuration.

This option enables you to perform write operations only on this index and automatically
update replica indices with the same operations.


</div>

</section>

<section>

## Filtering / Faceting






<div class='api-client-parameter'>


### filters


- scope: `search`
- type: `string`
- default: ""



Filter the query with numeric, facet or/and tag filters.

The syntax is a SQL like syntax, you can use the OR and AND keywords.
The syntax for the underlying numeric, facet and tag filters is the same than in the other filters:

`available=1 AND (category:Book OR NOT category:Ebook) AND _tags:public`
`date: 1441745506 TO 1441755506 AND inStock > 0 AND author:"John Doe"`

If no attribute name is specified,
the filter applies to `_tags`.

For example: `public OR user_42` will translate to `_tags:public OR _tags:user_42`.


</div>




<div class='api-client-parameter'>


### facets


- scope: `search`
- type: `string`
- default: ""



You can use [facets](#facets) to retrieve only a part of your attributes declared in
**[attributesForFaceting](#attributesforfaceting)** attributes.
It will not filter your results, if you want to filter results you should use [filters](#filters).

For each of the declared attributes, you'll be able to retrieve a list of the most relevant facet values,
and their associated count for the current query.

**Example**

If you have defined in your **[attributesForFaceting](#attributesforfaceting)**:

```
["category", "author", "nb_views", "nb_downloads"]
```

... but, for the current search, you want to retrieve facet values only for `category` and `author`, then you can specify:

```
["category", "author"]
```

**Warnings**

- When using [facets](#facets) in a search query, only attributes that have been added in **attributesForFaceting** index setting can be used in this parameter.
You can also use `*` to perform faceting on all attributes specified in `attributesForFaceting`.
If the number of results is important, the count can be approximate,
the attribute `exhaustiveFacetsCount` in the response is true when the count is exact.


</div>




<div class='api-client-parameter'>


### maxValuesPerFacet


- scope: `settings` `search`
- type: `integer`
- default: 100



Limit the number of facet values returned for each facet.

For example, `maxValuesPerFacet=10` will retrieve a maximum of 10 values per facet.

**Warnings**
- The engine has a hard limit on the `maxValuesPerFacet` of `1000`. Any value above that will be interpreted by the engine as being `1000`.


</div>

</section>

<section>

## Highlighting / Snippeting






<div class='api-client-parameter'>


### attributesToHighlight


- scope: `settings` `search`
- type: `array of string`




Default list of attributes to highlight.
If set to null, all indexed attributes are highlighted.

A string that contains the list of attributes you want to highlight according to the query.
Attributes are separated by commas.
You can also use a string array encoding (for example `["name","address"]`).
If an attribute has no match for the query, the raw value is returned.
By default, all indexed attributes are highlighted (as long as they are strings).
You can use `*` if you want to highlight all attributes.

A matchLevel is returned for each highlighted attribute and can contain:

* `full`: If all the query terms were found in the attribute.
* `partial`: If only some of the query terms were found.
* `none`: If none of the query terms were found.


</div>




<div class='api-client-parameter'>


### attributesToSnippet


- scope: `settings` `search`
- type: `array of strings`




Default list of attributes to snippet alongside the number of words to return (syntax is `attributeName:nbWords`).
If set to null, no snippet is computed.


</div>




<div class='api-client-parameter'>


### highlightPreTag


- scope: `settings` `search`
- type: `string`
- default: <em>



Specify the string that is inserted before the highlighted parts in the query result (defaults to `<em>`).




</div>




<div class='api-client-parameter'>


### highlightPostTag


- scope: `settings` `search`
- type: `string`
- default: </em>



Specify the string that is inserted after the highlighted parts in the query result (defaults to `</em>`).


</div>




<div class='api-client-parameter'>


### snippetEllipsisText


- scope: `settings` `search`
- type: `string`
- default: ...



String used as an ellipsis indicator when a snippet is truncated.

Defaults to an empty string for all accounts created before 10/2/2016, and to `…` (U+2026) for accounts created after that date.



</div>




<div class='api-client-parameter'>


### restrictHighlightAndSnippetArrays


- scope: `settings` `search`
- type: `boolean`
- default: false



If set to true, restrict arrays in highlights and snippets to items that matched the query at least partially else return all array items in highlights and snippets.


</div>

</section>

<section>

## Pagination






<div class='api-client-parameter'>


### page


- scope: `search`
- type: `integer`
- default: 0



Pagination parameter used to select the page to retrieve.

**Warning:** Page is zero based. Thus, to retrieve the 10th page, you need to set `page=9`.



</div>




<div class='api-client-parameter'>


### hitsPerPage


- scope: `settings` `search`
- type: `integer`
- default: 20



Pagination parameter used to select the number of hits per page.


</div>




<div class='api-client-parameter'>


### offset


- scope: `search`
- type: `integer`




Offset of the first hit to return (zero-based).

**Warning:** In most cases, `page`/`hitsPerPage` is the recommended method for pagination; `offset`/`length` is reserved for advanced use.



</div>




<div class='api-client-parameter'>


### length


- scope: `search`
- type: `integer`




Offset of the first hit to return (zero-based).

**Warning:** In most cases, `page`/`hitsPerPage` is the recommended method for pagination; `offset`/`length` is reserved for advanced use.



</div>




<div class='api-client-parameter'>


### paginationLimitedTo


- scope: `settings`
- type: `integer`
- default: 1000



Allows to control the maximum number of hits accessible via pagination. By default, this parameter is limited to 1000 to guarantee good performance.

**Warning:** We recommend to keep the default value to guarantee excellent performance.
Increasing this limit will have a direct impact on the performance of search.
A big value will also make it very easy for anyone to download all your dataset.



</div>

</section>

<section>

## Typos






<div class='api-client-parameter'>


### minWordSizefor1Typo


- scope: `settings` `search`
- type: `integer`
- default: 4



The minimum number of characters needed to accept one typo.


</div>




<div class='api-client-parameter'>


### minWordSizefor2Typos


- scope: `settings` `search`
- type: `integer`
- default: 8



The minimum number of characters needed to accept two typos.


</div>




<div class='api-client-parameter'>


### typoTolerance


- scope: `settings` `search`
- type: `boolean`
- default: true



This option allows you to control the number of typos allowed in the result set:

* `true`: The typo tolerance is enabled and all matching hits are retrieved (default behavior).
* `false`: The typo tolerance is disabled. All results with typos will be hidden.
* `min`: Only keep results with the minimum number of typos. For example, if one result matches without typos, then all results with typos will be hidden.
* `strict`: Hits matching with 2 typos are not retrieved if there are some matching without typos.


</div>




<div class='api-client-parameter'>


### allowTyposOnNumericTokens


- scope: `settings` `search`
- type: `boolean`
- default: true



If set to false, disables typo tolerance on numeric tokens (numbers).


</div>




<div class='api-client-parameter'>


### ignorePlurals


- scope: `settings` `search`
- type: `boolean` `array of strings`
- default: true


Consider singular and plurals forms a match without typo. For example, car and
cars, or foot and feet will be considered equivalent. This parameter can be:

- a **boolean**: enable or disable plurals for all 59 supported languages.
- a **list of language ISO codes** for which plurals should be enabled.

This option is set to `false` by default.

Afrikaans=`af`, Arabic=`ar`, Azeri=`az`, Bulgarian=`bg`, Catalan=`ca`,
Czech=`cs`, Welsh=`cy`, Danis=`da`, German=`de`, English=`en`,
Esperanto=`eo`, Spanish=`es`, Estonian=`et`, Basque=`eu`, Finnish=`fi`,
Faroese=`fo`, French=`fr`, Galician=`gl`, Hebrew=`he`, Hindi=`hi`,
Hungarian=`hu`, Armenian=`hy`, Indonesian=`id`, Icelandic=`is`, Italian=`it`,
Japanese=`ja`, Georgian=`ka`, Kazakh=`kk`, Korean=`ko`, Kyrgyz=`ky`,
Lithuanian=`lt`, Maori=`mi`, Mongolian=`mn`, Marathi=`mr`, Malay=`ms`,
Maltese=`mt`, Norwegian=`nb`, Dutch=`nl`, Northern Sotho=`ns`, Polish=`pl`,
Pashto=`ps`, Portuguese=`pt`, Quechua=`qu`, Romanian=`ro`, Russian=`ru`,
Slovak=`sk`, Albanian=`sq`, Swedish=`sv`, Swahili=`sw`, Tamil=`ta`,
Telugu=`te`, Tagalog=`tl`, Tswana=`tn`, Turkish=`tr`, Tatar=`tt`,


</div>




<div class='api-client-parameter'>


### disableTypoToleranceOnAttributes


- scope: `settings` `search`
- type: `string`
- default: ""



List of attributes on which you want to disable typo tolerance
(must be a subset of the `searchableAttributes` index setting).

Attributes are separated with a comma such as `"name,address"`.
You can also use JSON string array encoding such as `encodeURIComponent("[\"name\",\"address\"]")`.


</div>




<div class='api-client-parameter'>


### separatorsToIndex


- scope: `settings`
- type: `string`
- default: ""



Specify the separators (punctuation characters) to index.

By default, separators are not indexed.

**Example:** Use `+#` to be able to search for "Google+" or "C#".


</div>

</section>

<section>

## Geo-Search

Geo search requires that you provide at least one geo location in each record at indexing time, under the `_geoloc` attribute. Each location must be an object with two numeric `lat` and `lng` attributes. You may specify either one location:

```
{
  "_geoloc": {
    "lat": 48.853409,
    "lng": 2.348800
  }
}
```

... or an array of locations:

```
{
  "_geoloc": [
    {
      "lat": 48.853409,
      "lng": 2.348800
    },
    {
      "lat": 48.547456,
      "lng": 2.972075
    }
  ]
}
```





<div class='api-client-parameter'>


### aroundLatLng


- scope: `search`
- type: `string`
- default: ""





Search for entries around a given location (specified as two floats separated by a comma).

For example, `aroundLatLng=47.316669,5.016670`.

- By default the maximum distance is automatically guessed based on the density of the area
  but you can specify it manually in meters with the **aroundRadius** parameter.
  The precision for ranking can be set with **aroundPrecision** parameter.
- If you set aroundPrecision=100, the distances will be considered by ranges of 100m.
- For example all distances 0 and 100m will be considered as identical for the "geo" ranking parameter.




</div>




<div class='api-client-parameter'>


### aroundLatLngViaIP


- scope: `search`
- type: `boolean`
- default: false





Search for entries around a given latitude/longitude automatically computed from user IP address.

To enable it, use `aroundLatLngViaIP=true`.

You can specify the maximum distance in meters with the `aroundRadius` parameter
and the precision for ranking with `aroundPrecision`.

For example:
- if you set aroundPrecision=100,
two objects that are in the range 0-99m
will be considered as identical in the ranking for the "geo" ranking parameter (same for 100-199, 200-299, ... ranges).




</div>




<div class='api-client-parameter'>


### aroundRadius


- scope: `search`
- type: `integer` `string`




Control the radius associated with a geo search. Defined in meters.

If not set, the radius is computed automatically using the density of the area.
You can retrieve the computed radius in the `automaticRadius` attribute of the response.
You can also specify a minimum value for the automatic radius by using the `minimumAroundRadius` query parameter.

You can specify `aroundRadius=all` if you want to compute the geo distance without filtering in a geo area;
this option will be faster than specifying a big integer value.


</div>




<div class='api-client-parameter'>


### aroundPrecision


- scope: `search`
- type: `integer`




Control the precision of a geo search.
Defined in meters.

For example, if you set `aroundPrecision=100`, two objects that are in the range 0-99m will be considered as
identical in the ranking for the `geo` ranking parameter (same for 100-199, 200-299, … ranges).


</div>




<div class='api-client-parameter'>


### minimumAroundRadius


- scope: `search`
- type: `integer`




Define the minimum radius used for a geo search when `aroundRadius` is not set.
The radius is computed automatically using the density of the area.
You can retrieve the computed radius in the `automaticRadius` attribute of the answer.


</div>




<div class='api-client-parameter'>


### insideBoundingBox


- scope: `search`
- type: `string`




Search entries inside a given area defined by the two extreme points of a rectangle
(defined by 4 floats: p1Lat,p1Lng,p2Lat,p2Lng).

For example:

- `insideBoundingBox=47.3165,4.9665,47.3424,5.0201`


You can use several bounding boxes (OR) by passing more than 4 values.
For example: instead of having 4 values you can pass 8 to search inside the UNION of two bounding boxes.


</div>




<div class='api-client-parameter'>


### insidePolygon


- scope: `search`
- type: `string`
- default: ""



Search entries inside a given area defined by a set of points
  (defined by a minimum of 6 floats: p1Lat,p1Lng,p2Lat,p2Lng,p3Lat,p3Long).

  For example:
  
  - `InsidePolygon=47.3165,4.9665,47.3424,5.0201,47.32,4.98`
  


</div>

</section>

<section>

## Query Strategy






<div class='api-client-parameter'>


### queryType


- scope: `settings`
- type: `string`
- default: prefixLast



Selects how the query words are interpreted. It can be one of the following values:
* `prefixAll`:
All query words are interpreted as prefixes. This option is not recommended.
* `prefixLast`:
Only the last word is interpreted as a prefix (default behavior).
* `prefixNone`:
No query word is interpreted as a prefix. This option is not recommended.


</div>




<div class='api-client-parameter'>


### removeWordsIfNoResults


- scope: `settings` `search`
- type: `string`
- default: none



This option is used to select a strategy in order to avoid having an empty result page.
There are four different options:

- `lastWords`:
When a query does not return any results, the last word will be added as optional.
The process is repeated with n-1 word, n-2 word, ... until there are results.
- `firstWords`:
When a query does not return any results, the first word will be added as optional.
The process is repeated with second word, third word, ... until there are results.
- `allOptional`:
When a query does not return any results, a second trial will be made with all words as optional.
This is equivalent to transforming the AND operand between query terms to an OR operand.
- `none`:
No specific processing is done when a query does not return any results (default behavior).


</div>




<div class='api-client-parameter'>


### advancedSyntax


- scope: `settings` `search`
- type: `boolean`
- default: false



Enables the advanced query syntax.

This syntax allow to do two things:

* **Phrase query**: A phrase query defines a particular sequence of terms. A phrase query is built by Algolia's query parser for words surrounded by `"`.
  For example, `"search engine"` will retrieve records having `search` next to `engine` only. Typo tolerance is _disabled_ on phrase queries.
* **Prohibit operator**: The prohibit operator excludes records that contain the term after the `-` symbol.
For example, `search -engine` will retrieve records containing `search` but not `engine`.


</div>




<div class='api-client-parameter'>


### optionalWords


- scope: `settings` `search`
- type: `array of strings`
- default: []



A string that contains the comma separated list of words that should be considered as optional when found in the query.


</div>




<div class='api-client-parameter'>


### removeStopWords


- scope: `settings` `search`
- type: `boolean` `array of strings`
- default: false



Remove stop words from the query **before** executing it. It can be:

- a **boolean**: enable or disable stop words for all 41 supported languages; or
- a **list of language ISO codes** (as a comma-separated string) for which stop words should be enabled.

In most use-cases, **we don’t recommend enabling this option**.

List of 41 supported languages with their associated iso code: Arabic=`ar`, Armenian=`hy`, Basque=`eu`, Bengali=`bn`, Brazilian=`pt-br`, Bulgarian=`bg`, Catalan=`ca`, Chinese=`zh`, Czech=`cs`, Danish=`da`, Dutch=`nl`, English=`en`, Finnish=`fi`, French=`fr`, Galician=`gl`, German=`de`, Greek=`el`, Hindi=`hi`, Hungarian=`hu`, Indonesian=`id`, Irish=`ga`, Italian=`it`, Japanese=`ja`, Korean=`ko`, Kurdish=`ku`, Latvian=`lv`, Lithuanian=`lt`, Marathi=`mr`, Norwegian=`no`, Persian (Farsi)=`fa`, Polish=`pl`, Portugese=`pt`, Romanian=`ro`, Russian=`ru`, Slovak=`sk`, Spanish=`es`, Swedish=`sv`, Thai=`th`, Turkish=`tr`, Ukranian=`uk`, Urdu=`ur`.

Stop words removal is applied on query words that are not interpreted as a prefix. The behavior depends of the `queryType` parameter:

* `queryType=prefixLast` means the last query word is a prefix and it won’t be considered for stop words removal
* `queryType=prefixNone` means no query word are prefix, stop words removal will be applied on all query words
* `queryType=prefixAll` means all query terms are prefix, stop words won’t be removed

This parameter is useful when you have a query in natural language like “what is a record?”.
In this case, before executing the query, we will remove “what”, “is” and “a” in order to just search for “record”.
This removal will remove false positive because of stop words, especially when combined with optional words.
For most use cases, it is better to not use this feature as people search by keywords on search engines.




</div>




<div class='api-client-parameter'>


### disablePrefixOnAttributes


- scope: `seetings`
- type: `array of strings`
- default: []



List of attributes on which you want to disable prefix matching
(must be a subset of the `searchableAttributes` index setting).

This setting is useful on attributes that contain string that should not be matched as a prefix
(for example a product SKU).


</div>




<div class='api-client-parameter'>


### disableExactOnAttributes


- scope: `settings`
- type: `search`
- default: []



List of attributes on which you want to disable the computation of `exact` criteria
(must be a subset of the `searchableAttributes` index setting).


</div>




<div class='api-client-parameter'>


### exactOnSingleWordQuery


- scope: `settings` `search`
- type: `string`
- default: attribute



This parameter control how the `exact` ranking criterion is computed when the query contains one word. There are three different values:

* `none`: no exact on single word query
* `word`: exact set to 1 if the query word is found in the record. The query word needs to have at least 3 chars and not be part of our stop words dictionary
* `attribute` (default): exact set to 1 if there is an attribute containing a string equals to the query


</div>




<div class='api-client-parameter'>


### alternativesAsExact


- scope: `setting` `search`
- type: `string`
- default: ['ignorePlurals', 'singleWordSynonym']



Specify the list of approximation that should be considered as an exact match in the ranking formula:

* `ignorePlurals`: alternative words added by the ignorePlurals feature
* `singleWordSynonym`: single-word synonym (For example "NY" = "NYC")
* `multiWordsSynonym`: multiple-words synonym (For example "NY" = "New York")


</div>

</section>

<section>

## Advanced






<div class='api-client-parameter'>


### attributeForDistinct


- scope: `settings`
- type: `string`




The name of the attribute used for the `Distinct` feature.

This feature is similar to the SQL "distinct" keyword.
When enabled in queries with the `distinct=1` parameter,
all hits containing a duplicate value for this attribute are removed from the results.

For example, if the chosen attribute is `show_name` and several hits have the same value for `show_name`,
then only the first one is kept and the others are removed from the results.

To get a full understanding of how `Distinct` works,
you can have a look at our [guide on distinct](https://www.algolia.com/doc/search/distinct).


</div>




<div class='api-client-parameter'>


### synonyms


- scope: `search`
- type: `boolean`
- default: true



If set to `false`, the search will not use the synonyms defined for the targeted index.


</div>




<div class='api-client-parameter'>


### replaceSynonymsInHighlight


- scope: `settings` `search`
- type: `boolean`
- default: true



If set to `false`, words matched via synonym expansion will not be replaced by the matched synonym in the highlighted result.


</div>




<div class='api-client-parameter'>


### placeholders


- scope: `settings`
- type: `hash of array of words`
- default: ""



This is an advanced use-case to define a token substitutable by a list of words
without having the original token searchable.

It is defined by a hash associating placeholders to lists of substitutable words.

For example, `"placeholders": { "<streetnumber>": ["1", "2", "3", ..., "9999"]}`
would allow it to be able to match all street numbers. We use the `< >` tag syntax
to define placeholders in an attribute.

For example:

* Push a record with the placeholder:
`{ "name" : "Apple Store", "address" : "&lt;streetnumber&gt; Opera street, Paris" }`.
* Configure the placeholder in your index settings:
`"placeholders": { "<streetnumber>" : ["1", "2", "3", "4", "5", ... ], ... }`.


</div>




<div class='api-client-parameter'>


### altCorrections


- scope: `settings`
- type: `array of objects`
- default: []



Specify alternative corrections that you want to consider.

Each alternative correction is described by an object containing three attributes:

* `word` (string): The word to correct.
* `correction` (string): The corrected word.
* `nbTypos` (integer): The number of typos (1 or 2) that will be considered for the ranking algorithm (1 typo is better than 2 typos).

For example:

```
"altCorrections": [
  { "word" : "foot", "correction": "feet", "nbTypos": 1 },
  { "word": "feet", "correction": "foot", "nbTypos": 1 }
]
```


</div>




<div class='api-client-parameter'>


### minProximity


- scope: `settings` `search`
- type: `integer`
- default: 1



Configure the precision of the `proximity` ranking criterion.
By default, the minimum (and best) proximity value distance between 2 matching words is 1.
Setting it to 2 (or 3) would allow 1 (or 2) words to be found between the matching words without degrading the proximity ranking value.

Considering the query *“javascript framework”*, if you set `minProximity=2`, the records *“JavaScript framework”* and *“JavaScript charting framework”*
will get the same proximity score, even if the second contains a word between the two matching words.

**Note:** the maximum `minProximity` that can be set is 7. Any higher value will disable the `proximity` criterion from the ranking formula.



</div>




<div class='api-client-parameter'>


### responseFields


- scope: `settings` `search`
- type: `array of strings`
- default: ["*"]



Choose which fields the response will contain. Applies to search and browse queries.

By default, all fields are returned. If this parameter is specified, only the fields explicitly listed will be returned, unless `*` is used, in which case all fields are returned. Specifying an empty list or unknown field names is an error.

This parameter is mainly intended to limit the response size. For example, for complex queries, echoing of request parameters in the response's `params` field can be undesirable.

Some fields cannot be filtered out:

- warning `message`
- `cursor` in browse queries
- fields triggered explicitly via [getRankingInfo](#getrankinginfo)


</div>




<div class='api-client-parameter'>


### distinct


- scope: `settings` `search`
- type: `integer`
- default: 0



If set to 1,
enables the distinct feature, disabled by default, if the `attributeForDistinct` index setting is set.

This feature is similar to the SQL "distinct" keyword.
When enabled in a query with the `distinct=1` parameter,
all hits containing a duplicate value for the attributeForDistinct attribute are removed from results.

For example, if the chosen attribute is `show_name` and several hits have the same value for `show_name`,
then only the best one is kept and the others are removed.

To get a full understanding of how `Distinct` works,
you can have a look at our [guide on distinct](https://www.algolia.com/doc/search/distinct).


</div>




<div class='api-client-parameter'>


### getRankingInfo


- scope: `search`
- type: `boolean`
- default: false



If set to 1,
the result hits will contain ranking information in the **_rankingInfo** attribute.


</div>




<div class='api-client-parameter'>


### numericAttributesForFiltering


- scope: `settings`
- type: `array of strings`
- default: []



All numerical attributes are automatically indexed as numerical filters
(allowing filtering operations like `<` and `<=`).
If you don't need filtering on some of your numerical attributes,
you can specify this list to speed up the indexing.

If you only need to filter on a numeric value with the `=` operator,
you can speed up the indexing by specifying the attribute with `equalOnly(AttributeName)`.
The other operators will be disabled.


</div>




<div class='api-client-parameter'>


### allowCompressionOfIntegerArray


- scope: `settings`
- type: `boolean`
- default: false



Allows compression of big integer arrays.

In data-intensive use-cases,
we recommended enabling this feature and then storing the list of user IDs or rights as an integer array.
When enabled, the integer array is reordered to reach a better compression ratio.


</div>




<div class='api-client-parameter'>


### numericFilters (deprecated)


- scope: `search`
- type: `array of strings`
- default: []



*This parameter is deprecated. Please use [filters](#filters) instead.*

A string that contains the comma separated list of numeric filters you want to apply.
The filter syntax is `attributeName` followed by `operand` followed by `value`.
Supported operands are `<`, `<=`, `=`, `>` and `>=`.

You can easily perform range queries via the `:` operator.
This is equivalent to combining a `>=` and `<=` operand.

For example, `numericFilters=price:10 to 1000`.

You can also mix OR and AND operators.
The OR operator is defined with a parenthesis syntax.

For example, `(code=1 AND (price:[0-100] OR price:[1000-2000]))`
translates to `encodeURIComponent("code=1,(price:0 to 100,price:1000 to 2000)")`.

You can also use a string array encoding (for example `numericFilters: ["price>100","price<1000"]`).


</div>




<div class='api-client-parameter'>


### facetFilters (deprecated)


- scope: `search`
- type: `string`
- default: ""



*This parameter is deprecated. Please use [filters](#filters) instead.*

Filter the query with a list of facets. Facets are separated by commas and is encoded as `attributeName:value`.
To OR facets, you must add parentheses.

For example: `facetFilters=(category:Book,category:Movie),author:John%20Doe`.

You can also use a string array encoding.

For example, `[["category:Book","category:Movie"],"author:John%20Doe"]`.


</div>




<div class='api-client-parameter'>


### analytics


- scope: `search`
- type: `boolean`
- default: true



If set to false, this query will not be taken into account in the analytics feature.


</div>




<div class='api-client-parameter'>


### analyticsTags


- scope: `search`
- type: `array of strings`




If set, tag your query with the specified identifiers. Tags can then be used in the Analytics to analyze a subset of searches only.


</div>

</section>

# Manage Indices



<section>

## Create an index

To create an index, you need to perform any indexing operation like:
- set settings
- add object

</section>

<section>


## List indices - `list_indexes` 

You can list all your indices along with their associated information (number of entries, disk size, etc.) with the `list_indexes` method:

```python
print client.list_indexes()
```



</section>




<section>


## Delete index - `delete_index` 

You can delete an index using its name:

```python
client.delete_index("contacts")
```



</section>


<section>


## Clear index - `clear_index` 

You can delete the index contents without removing settings and index specific API keys by using the `clearIndex` command:

```python
index.clear_index()
```



</section>


<section>


## Copy index - `copy_index` 

You can copy an existing index using the `copy` command.

**Warning**: The copy command will overwrite the destination index.

```python
# Copy MyIndex in MyIndexCopy
print client.copy_index("MyIndex", "MyIndexCopy")
```



</section>


<section>


## Move index - `move_index` 

In some cases, you may want to totally reindex all your data. In order to keep your existing service
running while re-importing your data we recommend the usage of a temporary index plus an atomical
move using the `move_index` method.

```python
# Rename MyNewIndex in MyIndex (and overwrite it)
print client.move_index("MyNewIndex", "MyIndex")
```


**Note**:

The move_index method will overwrite the destination index, and delete the temporary index.

**Warning**

The move_index operation will override all settings of the destination,
There is one exception for the [replicas](#replicas) parameter which is not impacted.

For example, if you want to fully update your index `MyIndex` every night, we recommend the following process:

 1. Get settings and synonyms from the old index using [Get settings](/doc/api-client/python/settings#get-settings)
  and [Get synonym](/doc/api-client/python/synonyms#get-synonym).
 1. Apply settings and synonyms to the temporary index `MyTmpIndex`, (this will create the `MyTmpIndex` index)
  using [Set settings](/doc/api-client/python/settings#set-settings) and [Batch synonyms](/doc/api-client/python/synonyms#batch-synonyms)
  (make sure to remove the [replicas](#replicas) parameter from the settings if it exists).
 1. Import your records into a new index using [Add Objects](/doc/api-client/python/indexing#add-objects).
 1. Atomically replace the index `MyIndex` with the content and settings of the index `MyTmpIndex`
 using the [Move index](/doc/api-client/python/manage-indices#move-index) method.
 This will automatically override the old index without any downtime on the search.
 1. You'll end up with only one index called `MyIndex`, that contains the records and settings pushed to `MyTmpIndex`
 and the replica-indices that were initially attached to `MyIndex` will be in sync with the new data.


</section>




# Api keys




<section>

## Overview

When creating your Algolia Account, you'll notice there are 3 different API Keys:

- **Admin API Key** - it provides full control of all your indices.
*The admin API key should always be kept secure;
do NOT give it to anybody; do NOT use it from outside your back-end as it will
allow the person who has it to query/change/delete data*

- **Search-Only API Key** - It allows you to search on every indices.

- **Monitoring API Key** - It allows you to access the [Monitoring API](https://www.algolia.com/doc/rest-api/monitoring)

### Other types of API keys

The *Admin API Key* and *Search-Only API Key* both have really large scope and sometimes you want to give a key to
someone that have restricted permissions, can it be an index, a rate limit, a validity limit, ...

To address those use-cases we have two different type of keys:

- **Secured API Keys**

When you need to restrict the scope of the *Search Key*, we recommend to use *Secured API Key*.
You can generate them on the fly (without any call to the API)
from the *Search Only API Key* or any search *User Key* using the [Generate key](/doc/api-client/python/api-keys#generate-key) method

- **User API Keys**

If *Secured API Keys* does not meet your requirements, you can make use of *User keys*.
Managing and especially creating those keys requires a call to the API.

We have several methods to manage them:

- [Add user key](/doc/api-client/python/advanced#add-user-key)
- [Update user key](/doc/api-client/python/advanced#update-user-key)
- [Delete user key](/doc/api-client/python/advanced#delete-user-key)
- [List api keys](/doc/api-client/python/advanced#list-api-keys)
- [Get key permissions](/doc/api-client/python/advanced#get-key-permissions)

</section>

<section>


## Generate key - `generate_secured_api_key` 


When you need to restrict the scope of the *Search Key*, we recommend to use *Secured API Key*.
You can generate a *Secured API Key* from the *Search Only API Key* or any search *User API Key*

There is a few things to know about *Secured API Keys*
- They always need to be generated **on your backend** using one of our API Client
- You can generate them on the fly (without any call to the API)
- They will not appear on the dashboard as they are generated without any call to the API
- The key you use to generate it **needs to become private** and you should not use it in your frontend.
- The generated secured API key **will inherit any restriction from the search key it has been generated from**

You can then use the key in your frontend code

```js
var client = algoliasearch('YourApplicationID', 'YourPublicAPIKey');

var index = client.initIndex('indexName')

index.search('something', function(err, content) {
  if (err) {
    console.error(err);
    return;
  }

  console.log(content);
});
```

#### Filters

Every filter set in the API key will always be applied. On top of that [filters](#filters) can be applied
in the query parameters.

```python
# generate a public API key for user 42. Here, records are tagged with:
#  - 'user_XXXX' if they are visible by user XXXX
public_key = client.generate_secured_api_key('YourSearchOnlyApiKey', {'filters': '_tags:user_42'})
```


**Warning**:

If you set filters in the key `groups:admin`, and `groups:press OR groups:visitors` in the query parameters,
this will be equivalent to `groups:admin AND (groups:press OR groups:visitors)`

##### Having one API Key per User

One of the usage of secured API keys, is to have allow users to see only part of an index, when this index
contains the data of all users.
In that case, you can tag all records with their associated `user_id` in order to add a `user_id=42` filter when
generating the *Secured API Key* to retrieve only what a user is tagged in.

**Warning**
If you're generating *Secured API Keys* using the [JavaScript client](http://github.com/algolia/algoliasearch-client-js) in your frontend,
it will result in a security breach since the user is able to modify the filters you've set
by modifying the code from the browser.

#### Valid Until

You can set a Unix timestamp used to define the expiration date of the API key

```python
# generate a public API key that is valid for 1 hour:
valid_until = Time.now.to_i + 3600
public_key = client.generate_secured_api_key('YourSearchOnlyApiKey', {'validUntil': valid_until})
```


#### Index Restriction

You can restrict the key to a list of index names allowed for the secured API key

```python
# generate a public API key that is restricted to 'index1' and 'index2':
public_key = client.generate_secured_api_key('YourSearchOnlyApiKey', {'restrictIndices': 'index1,index2'})
```


#### Rate Limiting

If you want to rate limit a secured API Key, the API key you generate the secured api key from need to be rate-limited.
You can do that either via the dashboard or via the API using the
[Add user key](/doc/api-client/python/advanced#add-user-key) or [Update user key](/doc/api-client/python/advanced#update-user-key) method

##### User Rate Limiting

By default the rate limits will only use the `IP`.

This can be an issue when several of your end users are using the same IP.
To avoid that, you can set a `userToken` query parameter when generating the key.

When set, a unique user will be identified by his `IP + user_token` instead of only by his `IP`.

This allows you to restrict a single user to performing a maximum of `N` API calls per hour,
even if he shares his `IP` with another user.

```python
# generate a public API key for user 42. Here, records are tagged with:
#  - 'user_XXXX' if they are visible by user XXXX
public_key = client.generate_secured_api_key('YourSearchOnlyApiKey', {'filters': '_tags:user_42', 'userToken': 'user_42'})
```


#### Network restriction

For more protection against API key leaking and reuse you can restrict the key to be valid only from specific IPv4 networks

```python
# generate a public API key that is restricted to '192.168.1.0/24':
public_key = client.generate_secured_api_key('YourSearchOnlyApiKey', {'restrictSources': '192.168.1.0/24'})
```



</section>


# Synonyms




<section>


## Save synonym - `save_synonym` 

This method saves a single synonym record into the index.

In this example, we specify true to forward the creation to replica indices.
By default the behavior is to save only on the specified index.

```python
index.save_synonym({
  'objectID': 'a-unique-identifier',
  'type': 'synonym',
  'synonyms': ['car', 'vehicle', 'auto']
}, 'a-unique-identifier', True)
```



</section>


<section>


## Batch synonyms - `batch_synonyms` 

Use the batch method to create a large number of synonyms at once,
forward them to replica indices if desired,
and optionally replace all existing synonyms
on the index with the content of the batch using the replaceExistingSynonyms parameter.

You should always use replaceExistingSynonyms to atomically replace all synonyms
on a production index. This is the only way to ensure the index always
has a full list of synonyms to use during the indexing of the new list.

```python
# Batch synonyms, with replica forwarding and atomic replacement of existing synonyms
index.batch_synonyms([{
  'objectID': 'a-unique-identifier',
  'type': 'synonym',
  'synonyms': ['car', 'vehicle', 'auto']
}, {
  'objectID': 'another-unique-identifier',
  'type': 'synonym',
  'synonyms': ['street', 'st']
}], True, True)
```



</section>



<section>

## Editing Synonyms

Updating the value of a specific synonym record is the same as creating one.
Make sure you specify the same objectID used to create the record and the synonyms
will be updated.
When updating multiple synonyms in a batch call (but not all synonyms),
make sure you set replaceExistingSynonyms to false (or leave it out,
false is the default value).
Otherwise, the entire synonym list will be replaced only partially with the records
in the batch update.

</section>

<section>


## Delete synonym - `delete_synonym` 

Use the normal index delete method to delete synonyms,
specifying the objectID of the synonym record you want to delete.
Forward the deletion to replica indices by setting the forwardToReplicas parameter to true.

```python
# Delete and forward to replicas
index.delete_synonym('a-unique-identifier', True)
```



</section>


<section>


## Clear all synonyms - `clear_synonyms` 

This is a convenience method to delete all synonyms at once.
It should not be used on a production index to then push a new list of synonyms:
there would be a short period of time during which the index would have no synonyms
at all.

To atomically replace all synonyms of an index,
use the batch method with the replaceExistingSynonyms parameter set to true.

```python
# Clear synonyms and forward to replicas
index.clear_synonyms(True)
```



</section>


<section>


## Get synonym - `get_synonym` 

Search for synonym records by their objectID or by the text they contain.
Both methods are covered here.

```python
synonym = index.get_synonym('a-unique-identifier')
```



</section>


<section>


## Search synonyms - `search_synonyms` 

Search for synonym records similar to how you’d search normally.

Accepted search parameters:
- query: the actual search query to find synonyms. Use an empty query to browse all the synonyms of an index.
- type: restrict the search to a specific type of synonym. Use an empty string to search all types (default behavior). Multiple types can be specified using a comma-separated list or an array.
- page: the page to fetch when browsing through several pages of results. This value is zero-based.
hitsPerPage: the number of synonyms to return for each call. The default value is 100.

```python
# Searching for "street" in synonyms and one-way synonyms; fetch the second page with 10 hits per page
results = index.search_synonyms('street', ['synonym', 'oneWaySynonym'], 1, 10)
```



</section>


# Advanced



<section>


## Custom batch - `batch` 

You may want to perform multiple operations with one API call to reduce latency.



If you have one index per user, you may want to perform a batch operations across several indices.
We expose a method to perform this type of batch:

```python
res = index.batch([
	{"action": "addObject", "indexName": "index1", {"firstname": "Jimmie", "lastname": "Barninger"}},
	{"action": "addObject", "indexName": "index2", {"firstname": "Warren", "lastname": "Speach"}}])
```


The attribute **action** can have these values:

- addObject
- updateObject
- partialUpdateObject
- partialUpdateObjectNoCreate
- deleteObject


</section>


<section>


## Backup / Export an index - `browse` 

The `search` method cannot return more than 1,000 results. If you need to
retrieve all the content of your index (for backup, SEO purposes or for running
a script on it), you should use the `browse` method instead. This method lets
you retrieve objects beyond the 1,000 limit.

This method is optimized for speed. To make it fast, distinct, typo-tolerance,
word proximity, geo distance and number of matched words are disabled. Results
are still returned ranked by attributes and custom ranking.



#### Response Format

##### Sample

```json
{
  "hits": [
    {
      "firstname": "Jimmie",
      "lastname": "Barninger",
      "objectID": "433"
    }
  ],
  "processingTimeMS": 7,
  "query": "",
  "params": "filters=level%3D20",
  "cursor": "ARJmaWx0ZXJzPWxldmVsJTNEMjABARoGODA4OTIzvwgAgICAgICAgICAAQ=="
}
```

##### Fields

- `cursor` (string, optional): A cursor to retrieve the next chunk of data. If absent, it means that the end of the index has been reached.

- `query` (string): Query text used to filter the results.

- `params` (string, URL-encoded): Search parameters used to filter the results.

- `processingTimeMS` (integer): Time that the server took to process the request, in milliseconds. *Note: This does not include network time.*

The following fields are provided for convenience purposes, and **only when the browse is not filtered**:

- `nbHits` (integer): Number of objects in the index.

- `page` (integer): Index of the current page (zero-based).

- `hitsPerPage` (integer): Maximum number of hits returned per page.

- `nbPages` (integer): Number of pages corresponding to the number of hits. Basically, `ceil(nbHits / hitsPerPage)`.


#### Example

```python
# Iterate with a filter over the index
res = self.index.browse_all({"query": "test", "filters": "i<42"})
for hit in res
	# Do something

# Retrieve the next cursor from the browse method
res = self.index.browse_from({"query": "test", "filters": "i<42"}, None)
print res["cursor"]
```





</section>




<section>


## List api keys - `list_api_keys` 

To list existing keys, you can use:

```python
# Lists global API Keys
client.list_user_keys()
# Lists API Keys that can access only to this index
index.list_user_keys()
```


Each key is defined by a set of permissions that specify the authorized actions. The different permissions are:

* **search**: Allowed to search.
* **browse**: Allowed to retrieve all index contents via the browse API.
* **addObject**: Allowed to add/update an object in the index.
* **deleteObject**: Allowed to delete an existing object.
* **deleteIndex**: Allowed to delete index content.
* **settings**: allows to get index settings.
* **editSettings**: Allowed to change index settings.
* **analytics**: Allowed to retrieve analytics through the analytics API.
* **listIndexes**: Allowed to list all accessible indexes.


</section>


<section>


## Add user key - `add_user_key` 

To create API keys:

```python
# Creates a new global API key that can only perform search actions
res = client.add_user_key(["search"])
print res["key"]
# Creates a new API key that can only perform search action on this index
res = index.add_user_key(["search"])
print res["key"]
```


You can also create an API Key with advanced settings:

##### validity

Add a validity period. The key will be valid for a specific period of time (in seconds).

##### maxQueriesPerIPPerHour

Specify the maximum number of API calls allowed from an IP address per hour. Each time an API call is performed with this key, a check is performed. If the IP at the source of the call did more than this number of calls in the last hour, a 403 code is returned. Defaults to 0 (no rate limit). This parameter can be used to protect you from attempts at retrieving your entire index contents by massively querying the index.



##### maxHitsPerQuery

Specify the maximum number of hits this API key can retrieve in one call. Defaults to 0 (unlimited). This parameter can be used to protect you from attempts at retrieving your entire index contents by massively querying the index.

##### indexes

Specify the list of targeted indices. You can target all indices starting with a prefix or ending with a suffix using the '\*' character. For example, "dev\_\*" matches all indices starting with "dev\_" and "\*\_dev" matches all indices ending with "\_dev". Defaults to all indices if empty or blank.

##### referers

Specify the list of referers. You can target all referers starting with a prefix, ending with a suffix using the '\*' character. For example, "https://algolia.com/\*" matches all referers starting with "https://algolia.com/" and "\*.algolia.com" matches all referers ending with ".algolia.com". If you want to allow the domain algolia.com you can use "\*algolia.com/\*". Defaults to all referers if empty or blank.

##### queryParameters

Specify the list of query parameters. You can force the query parameters for a query using the url string format (param1=X&param2=Y...).

##### description

Specify a description to describe where the key is used.

```python
# Creates a new index specific API key valid for 300 seconds, with a rate limit of 100 calls per hour per IP and a maximum of 20 hits

params = {
  'validity': 300,
  'maxQueriesPerIPPerHour': 100,
  'maxHitsPerQuery': 20,
  'indexes': ['dev_*'],
  'referers': ['algolia.com/*'],
  'queryParameters': 'typoTolerance=strict&ignorePlurals=false',
  'description': 'Limited search only API key for algolia.com'
}

res = client.add_user_key(params)
print res["key"]
```



</section>


<section>


## Update user key - `update_user_key` 

To update the permissions of an existing key:

```python
# Update an existing global API key that is valid for 300 seconds
res = client.update_user_key("myAPIKey", ["search"], 300)
print res["key"]
# Update an existing index specific API key valid for 300 seconds, with a rate limit of 100 calls per hour per IP and a maximum of 20 hits
res = index.update_user_key("myAPIKey", ["search"], 300, 100, 20)
print res["key"]
```


To get the permissions of a given key:

```python
# Gets the rights of a global key
print client.get_user_key_acl("f420238212c54dcfad07ea0aa6d5c45f")
# Gets the rights of an index specific key
print index.get_user_key_acl("71671c38001bf3ac857bc82052485107")
```



</section>


<section>


## Delete user key - `delete_user_key` 

To delete an existing key:

```python
# Deletes a global key
print client.delete_user_key("f420238212c54dcfad07ea0aa6d5c45f")
# Deletes an index specific key
print index.delete_user_key("71671c38001bf3ac857bc82052485107")
```



</section>


<section>


## Get key permissions - `get_user_key_acl` 

To get the permissions of a given key:

```python
# Gets the rights of a global key
print client.get_user_key_acl("f420238212c54dcfad07ea0aa6d5c45f")
# Gets the rights of an index specific key
print index.get_user_key_acl("71671c38001bf3ac857bc82052485107")
```



</section>


<section>


## Get logs - `get_logs` 

You can retrieve the latest logs via this API. Each log entry contains:

* Timestamp in ISO-8601 format
* Client IP
* Request Headers (API Key is obfuscated)
* Request URL
* Request method
* Request body
* Answer HTTP code
* Answer body
* SHA1 ID of entry

You can retrieve the logs of your last 1,000 API calls and browse them using the offset/length parameters:

#### offset

Specify the first entry to retrieve (0-based, 0 is the most recent log entry). Defaults to 0.

#### length

Specify the maximum number of entries to retrieve starting at the offset. Defaults to 10. Maximum allowed value: 1,000.

#### onlyErrors

Retrieve only logs with an HTTP code different than 200 or 201. (deprecated)

#### type

Specify the type of logs to retrieve:

* `query`: Retrieve only the queries.
* `build`: Retrieve only the build operations.
* `error`: Retrieve only the errors (same as `onlyErrors` parameters).

```python
# Get last 10 log entries
print client.get_logs()
# Get last 100 log entries
print client.get_logs(0, 100)
```



</section>




<section>

### REST API

We've developed API clients for the most common programming languages and platforms.
These clients are advanced wrappers on top of our REST API itself and have been made
in order to help you integrating the service within your apps:
for both indexing and search.

Everything that can be done using the REST API can be done using those clients.

The REST API lets your interact directly with Algolia platforms from anything that can send an HTTP request
[Go to the REST API doc](https://algolia.com/doc/rest)

</section>
# Troubleshooting




<section>

## Unreachable hosts

If you are seeing an error for `Unreachable hosts` when pushing data to Algolia, this is caused by `PyOpenSSL` and older versions of Python. You can see [more information here](https://github.com/algolia/algoliasearch-client-python/issues/30).

To fix, either upgrade your Python version or [your urllib3](https://github.com/algolia/algoliasearch-client-python/issues/30#issuecomment-151933567).

</section>

