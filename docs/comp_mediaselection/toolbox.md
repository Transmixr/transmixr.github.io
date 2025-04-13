---
title: Newsroom Toolbox
layout: home
parent: Components - Media Understanding & Selection
---

# Newsroom Toolbox

# **webLyzard API Specification** **Application Programming Interface** **05 February 2025**

**Table of Contents**

[Overview](#overview)

[Search API](#search-api)

[/search](#/search)

[Endpoint](#endpoint)

[General query structure](#general-query-structure)

[Query Types](#query-types)

[Bool query](#bool-query)

[Phrase query](#phrase-query)

[Regexp query](#regexp-query)

[Term query](#term-query)

[Wildcard query](#wildcard-query)

[Range query](#range-query)

[Similar to query](#similar-to-query)

[Entity query](#entity-query)

[Feature query](#feature-query)

[Topdocs query](#topdocs-query)

[Metric query](#metric-query)

[Boosting queries](#boosting-queries)

[Examples](#examples)

[/keyentities](#/keyentities)

[/entities](#/entities)

[Visualization API](#visualization-api)

[Appendix A: Authentication | Authorization](#appendix-a:-authentication-|-authorization)

[Obtaining a new token](#obtaining-a-new-token)

[Calling API methods using the obtained token](#calling-api-methods-using-the-obtained-token)


# **Overview** {#overview}


The **webLyzard API** specification outlines how to leverage the various technologies and services behind the *webLyzard* platform for third-party applications. The API specification contains two separate parts:

1. The Search API returns relevant documents sets based on a search query.  
2. The Visualization API renders individual visualizations and allows to embed them as widgets within third-party Web applications.

The webLyzard APIs are stateless RESTful APIs accessed via HTTPS. They typically use JSON as input and output formatting. All API requests need to be authenticated using JSON Web Tokens. Tokens are time-limited and repository-restricted. How to get access tokens is documented in [Appendix A, Authentication](#bookmark=kix.5azsxyugnl4).

This document should be considered beta status. While the overall set of features is stable, specific parts of the specification might still change in line with specific use case requirements.


# **Search API**

The WLT Search API allows to retrieve documents as well as keyword and entity aggregations from the WLT document repository.

The following endpoints are available in the WLT Search API:

* [/search](https://api.weblyzard.com/doc/ui/#/Search_API/post_search) Search a document repository. The filter object supports the same syntax as the query object, but does not affect search result ranking. Try to use filters as much as possible to speed up query times (e.g. date and sentiment should always be put into a filter, as it makes no sense to rank based on these) and only use queries for fulltext queries that should affect the order of documents.  
* [/keyentities](https://api.weblyzard.com/doc/ui/#/Search_API/post_keyentities) Return aggregated **key** entities for a given query. The filter object supports the same syntax as the query object, but does not affect search result ranking. Try to use filters as much as possible to speed up query times.

Clients need to send Content-Type:application/json and Accept:application/json headers.

## **/search** 

### Endpoint 

The endpoint for the search API is https://api.weblyzard.com/1.0/search

### General query structure 

A search query conforms to the following JSON document:

```json
 {   "sources" : \[\],   "fields" : \[\],   "query" : {},   "filter" : {},   "count" : 10,   "offset" : 0,   "ranking" : {},   "beginDate" : "",   "endDate" : "" }
```

| Search |  |
| :---- | :---- |
| Request Fields |  |
| source | A list of strings, specifying the set of sources the query should be run against. Currently, there is only EN content supported, via source [`api.weblyzard.com/news_en`](http://api.weblyzard.com/news_en). If other languages are required, please contact the WLT admin (available are NL, FR, ES, DE, IT, PT, more can be set up). |
| fields | A list of strings, specifying which fields should be returned for each document. Possible values: document.contentid the internal content id of the document document.date the publishing date of the document document.title the title of the document document.url the original URL of the document document.summary a summary of the full text content of the document document.fulltext the full text content of the document (if available) document.snippet a short snippet of content around the best query match in the document document.quote a longer snippet of content around the best query match in the document document.sourceid an internal identifier for the source of the document document.keywords a list of significant keywords extracted from the document document.annotations a list of all annotations extracted from the document document.features additional project-specific features extracted from or annotated on the document document.sentiment positive/negative sentiment of the document document.domain Web domain of the document document.metric a list of user-specific rating values annotated on the document |
| query | Each query object can contain one of the following types of query: bool a boolean query supported query types: bool title search for text matches in the title supported query types: phrase, regexp, term, similarto text search for text matches in the full text supported query types: phrase, regexp, term, similarto date limit search results to a date range supported query types: range sentiment limit search results to a sentiment range supported query types: range url limit search results to matching URLs supported query types: regexp, term, wildcard, similarto contentid limit search results to existing documents (identified by given internal contentid) supported query types: range, similarto entity limit search results to documents containing the given entity feature limit search results to documents containing the given feature supported query types: term, exists topdocs limit search results to best matching documents matching the given sub-query (useful for aggregation queries) metric limit search results to documents annotated with given metric |
| filter | The filter object supports the same syntax as the query object, but does not affect search result ranking. Try to use filters as much as possible to speed up query times (e.g. date and sentiment should always be put into a filter, as it makes no sense to rank based on these) and only use queries for fulltext queries that should affect the order of documents. |
| count | The number of matches to return. |
| offset | Offset for the documents to return. |
| ranking | Allows to influence the order of search results in the response. |
| boost | A list of boosting queries to influence search result order. |
| beginDate | Only return documents published at or after the given date |
| endDate | Only return documents published at or before the given date |

### Query Types

#### Bool query

A boolean query combines any other queries into a single, aggregated query:

| "bool" : {   "must" : \[\],   "should" : \[\],   "must\_not: \[\],   "minimum\_should\_match: 0 } |
| :---- |

**must** \- each of the sub-queries must match.  
**should** \- any of the sub-queries should match.  
**must\_not** \- none of the sub-queries must match.  
**minimum\_should\_match** \- number of optional (**should**) clauses that must match.

#### Phrase query 

A phrase query tries to match the given string as a phrase in the field:

| "text" : {   "phrase" : "climate change } |
| :---- |

#### Regexp query

A regexp query tries to match the given string as a regular expression in the field:

| "title" : {   "regexp" : "climate( |-)change } |
| :---- |

**Allowed regular expression operators:**  
() grouping  
| alternatives  
? optional parts

### Term query 

Match the given string to the full content of the field (text has to be an exact match on the full field):

| "title" : {   "term" : "Media Watch on Climate Change" } |
| :---- |

#### Wildcard query 

Match the given string to the full content of the field, supporting wildcards:

| "url" : {   "wildcard" : "www.google.com/\*" } |
| :---- |

where \* matches any number of characters and ? matches a single character.

#### Range query 

A query that supports matching values in a range:

| "sentiment" : {   "lt" : 0,   "gt" : \-0.5 } |
| :---- |

| "date" : {   "gte" : "2022-01-01",   "lte" : "2022-12-31" } |
| :---- |

**eq** \- value equals the given value  
**lt** \- value is less than the given value  
**gt** \- value is greater than the given value  
**lte** \- value is less than or equal to the given value  
**gte** \- value is greater than or equal to the given value

**Supported date formats:**  
yyyy-MM-dd  
yyyyMMdd  
dd-MM-yyyy  
ddMMyyyy

### Similar to query 

A query that allows to search for documents similar to the given text or URL:

| "similarto" : {   "value" : "",   "options" : {     "maxdocuments": 1,     "maxqueryterms": 10,     "minmatching": {       "terms": 1,       "percent": 30     },     "termfrequency": {       "mintermfrequency": 10,       "mindocfrequency": 10,       "maxdocfrequency": 100     },     "wordlength": {       "min": 0,       "max": 100     },     "stopwords": \[\]   } } |
| :---- |

**value** \- the input value for which to search for similar content  
**options** \- specify options to influence the behavior of the query  
  **maxdocuments** \- maximum number of documents to consider as “similar”  
  **maxqueryterms** \- maximum number of significant terms to extract from the input text  
  **minmatching** \- control matching behavior of terms extracted from the input text  
    **terms** \- at least this number of extracted terms need to match  
    **percent** \- at least this percentage of extracted terms need to match  
**termfrequency** \- control selection of terms used in the similar to query  
  **mintermfrequency** \- a term has to appear at least this often in the input text to be considered for the query  
  **mindocfrequency** \- a term has to appear in at least this many documents in the queried source(s) to be considered for the query  
  **maxdocfrequency** \- a term has to appear in at most this many documents in the queried source(s) to be considered for the query  
**wordlength** \- control selection of terms used in the similar to query  
  **min** \- minimum word length of terms in the input text to be considered query terms  
  **max** \- maximum word length of terms in the input text to be considered query terms

#### Entity query 

Search for documents containing the given extracted entity:

| "entity" : {   "key" : "",   "type" : "",   "name" : "" } |
| :---- |

**key** \- exact URI of the entity  
**type** \- type of entity (e.g. geo, org, person)  
**name** \- name of the entity

#### Feature query
Search for documents containing the given feature:

| "feature" : {   "name-of-feature" : {     "exists" : true|false,     "term" : ""   } } |
| :---- |

**name-of-feature** \- name of the feature to query on (project specific)  
**exists** \- check if a feature of that name is annotated on the document  
**term** \- query on exact value of the annotated feature

#### Topdocs query

Limit results to the top documents matching the given sub query (useful for aggregations):

| "topdocs" : {   "query" : {},   "options" : {     "maxdocuments" : 10,   } } |
| :---- |

**query** \- any supported query can be used as a sub-query  
**options** \- additional options to control query behavior  
  **maxdocuments** \- maximum number of documents to return

#### Metric query 

Search for documents containing the given annotated metric:

| "metric" : {   "not\_exists|exists" : {     "user" : "",     "name" : "",     "value" : ""   },   "eq|lt|gt|lte|gte" : {     "user" : "",     "name" : "",     "value" : ""   } } |
| :---- |

**exists** \- a metric matching the given specification must exist on the document  
**not\_exists** \- a metric matching the given specification must not exist on the document  
**eq** \- the value of the metric has to be equal to the given value  
**lt** \- the value of the metric has to be less than the given value  
**gt** \- the value of the metric has to be greater than the given value  
**lte** \- the value of the metric has to be less than or equal to the given value  
**gte** \- the value of the metric has to be greater than or equal to the given value  
**user** \- identifier of the user for which the metric is annotated  
**name** \- name of the metric to query  
**value** \- value of the metric to query

#### Boosting queries {#boosting-queries}

Query ranking can be influenced by setting boosting queries, where each boosting query has the following attributes:

* **query** ranking weights for documents matching the query are changed  
* **mode** score replacement mode (supported values: replace, sum, multiply)  
* **boost** weight that influences the score.

**Response**

| {  "hits": \[\],  "total": 0 } |
| :---- |

hits \- a list of documents  
total \- the total number of documents matching the query

Note: if only document counts need to be retrieved, put all queries inside the filter attribute  
and set count to 0 for best performance.

### Examples

| curl \\   \--request POST \\   \--url https://api.weblyzard.com/1.0/search \\   \--header 'Authorization: Bearer xxx' \\   \--header 'Content-Type: application/json' \\   \--data '{     "sources" : \["api.weblyzard.com/news\_en"\],     "fields" : \["document.title", "document.url"\],     "query" : {       "text" : {         "phrase" : "climate change"       }     },     "beginDate" : "2024-01-01",     "endDate" : "2024-01-31",     "count" : 100   }' {   "result": {     "total": 9723,     "hits": \[       {         "title": "Climate change denial on Facebook, YouTube, Twitter and TikTok is ‘as bad as ever'",         "url": "https://www.usatoday.com/story/tech/2024/01/21/climate-change-misinformation-facebook-youtube-twitter/6594691001/"       },       …     \]   } }  |
| :---- |

**Example 1: Search for 100 documents matching "climate change" as a phrase in the full text in January 2024 in English News Media, returning title and URL**

| curl \\   \--request POST \\   \--url https://api.weblyzard.com/1.0/search \\   \--header 'Authorization: Bearer xxx' \\   \--header 'Content-Type: application/json' \\   \--data '{     "sources" : \["api.weblyzard.com/news\_en"\],     "fields" : \["document.title", "document.url", "document.sentiment"\],     "query" : {       "bool" : {         "must" : \[{           "text" : {             "phrase" : "climate change"           }         }, {           "entity" : {             "key" : "http://sws.geonames.org/2077456/"           }         }\]       }     },     "filter" : {       "sentiment" : {         "lt" : 0       }     },     "beginDate" : "2024-01-01",     "endDate" : "2024-01-31",     "count" : 100   }' |
| :---- |

| {   "result": {     "total": 415,     "hits": \[       {         "title": "Environmental disasters are fuelling migration — here's why international law must recognize climate refugees",         "url": "https://theconversation.com/environmental-disasters-are-fuelling-migration-heres-why-international-law-must-recognize-climate-refugees-173714",         "sentiment": \-1.0       },       …     \]   } } |
| :---- |

**Example 2: Search for 100 negative documents matching "climate change" as a phrase in the full text and containing “Australia” as an annotated entity in January 2024 in English News Media, returning title, URL and document sentiment.**

| curl \\   \--request POST \\   \--url https://api.weblyzard.com/1.0/search \\   \--header 'Authorization: Bearer xxx' \\   \--header 'Content-Type: application/json' \\   \--data '{     "sources" : \["api.weblyzard.com/news\_en"\],     "fields" : \["document.title", "document.url", "document.sentiment"\],     "query" : {       "text" : {         "phrase" : "climate change"       }     },     "filter" : {       "sentiment" : {         "lt" : 0       }     },     "beginDate" : "2024-01-01",     "endDate" : "2024-01-31",     "count" : 100,     "ranking" : {       "boost" : \[{         "query" : {           "url" : {             "wildcard" : "\*.cnn.com/\*"           }         },         "mode" : "multiply",         "boost" : 10       }\]     }   }' |
| :---- |

| {   "result": {     "total": 4598,     "hits": \[       {         "title": "Climate change is coming for our coffee",         "url": "https://edition.cnn.com/2024/01/26/business/coffee-climate-change/index.html",         "sentiment": \-0.48450157046318054       },       …     \]   } } |
| :---- |

**Example 3: Search for 100 negative documents matching "climate change" as a phrase in the full text in January 2024 in English News Media, ranking matches on cnn.com first, returning title, URL and document sentiment.**

## **/keyentities** 

Return aggregated **key** entities for a given search query. Key Entities are terms and named entities that are marked to be **keywords**. The filter object supports the same syntax as the query object, but does not affect search result ranking. Try to use filters as much as possible to speed up query times.  
The response contains both "sentiment" (average aggregated sentiment) and "count" (total number of annotations) for a search query.

| curl \\   \--request POST \\   \--url https://api.weblyzard.com/1.0/keyentities \\   \--header 'Authorization: Bearer xxx' \\   \--header 'Content-Type: application/json' \\   \--data '{     "sources": \["api.weblyzard.com/news\_en"\],     "fields": \["keyword.key", "keyword.name", "keyword.sentiment", "keyword.count"\],     "filter": {       "text":{         "phrase":"climate change"       }     },     "beginDate":"2024-01-01",     "endDate":"2024-01-31",     "count": 10   }' |
| :---- |

| {   "result": {     "keyEntities": \[       {         "key": "http://weblyzard.com/skb/keyword/en/noun/climate\_change",         "name": {           "de": "klimawandel",           "en": "climate change",           "fr": "changement climatique",           "es": "climate change"         },         "sentiment": \-0.048035378672140146,         "count": 1581       },       {         "key": "http://weblyzard.com/skb/keyword/en/noun/davos",         "name": {           "de": "davos",           "en": "davos",           "fr": "davos",           "es": "davos"         },         "sentiment": 0.023966228648235922,         "count": 285       },       {         "key": "http://weblyzard.com/skb/keyword/en/noun/economic\_forum",         "name": {           "de": "wirtschaftsforum",           "en": "economic forum",           "fr": "forum économique",           "es": "foro económico"         },         "sentiment": 0.03826945910648424,         "count": 245       },       ...     \]   } } |
| :---- |

**Example 4: Search for the top 10 key entities within documents matching "climate change" as a phrase in the full text in January 2024 in English News Media, ranking matches on cnn.com first, returning entity key, their translated labels and entity sentiment.**

## **/entities** 
Return aggregated entities for a given search query. In contrast to the /keyentities endpoint, /entities only considers named entity matches (e.g. GeoEntity, PersonEntity, OrganizationEntity, but not KeywordEntities). The filter object supports the same syntax as the query object, but does not affect search result ranking. Try to use filters as much as possible to speed up query times.

| curl \--request POST \\   \--url https://api.weblyzard.com/1.0/entities \\   \--header 'Authorization: Bearer xxx   \--header 'Content-Type: application/json' \\   \--data '{   "sources": \[ 	"api.weblyzard.com/news\_en"     \],   "fields": \[ 	"keyword.key", 	"keyword.name", 	"keyword.sentiment", 	"keyword.count"   \],   "filter": { 	"text": {   	"phrase": "climate change" 	}   },   "entityTypes": \["geocapital"\],   "num\_keywords": 10 }' |
| :---- |

| { 	"result": {     	"keyEntities": \[         	{             	"key": "http://sws.geonames.org/184745/",             	"name": {                 	"de": "Nairobi",                 	"en": "Nairobi",                 	"fr": "Nairobi",                 	"es": "Nairobi"             	},             	"sentiment": \-0.12584571216919627,             	"count": 129         	},         	{             	"key": "http://sws.geonames.org/2643743/",             	"name": {                 	"de": "London",                 	"en": "London",                 	"fr": "Londres",                 	"es": "Londres"             	},             	"sentiment": \-0.025695290529366695,             	"count": 99         	}, …     	\] 	} } |
| :---- |

**Example 4: Search for the top 10 geo capital named entities within documents matching "climate change" as a phrase in the full text in January 2024 in English News Media, returning the respective fields as requested.**

# **Visualization API** 

The webLyzard dashboard offers a feature-rich and customizable solution for visual analytics, semantic search and Web intelligence applications. To support use cases that require a more granular approach, the webLyzard Visualization API enables the integration of distinct portal components into third-party Web applications as embeddable widgets (2D).

Version 1 uses \<iframe\> tags to embed these components. While this approach ensures ease of use and widespread compatibility across platforms, it also comes with shortcomings if a deeper integration is desired. This will be addressed by additional features in future versions of the API, complementing (but not replacing) the \<iframe\> approach.

| \<iframe\> |  |
| :---- | :---- |
| Attribute FieldsThe iframe should be provided with all necessary attributes, width and height could be preset to the desired dimensions, similar to the approach of e.g. YouTube. |  |
| width | int width in pixels |
| height | int height in pixels |
| frameborder | int should always be \`0\` |
| seamless | boolean \`true\` if you want to inherit styles from the parent window and open links in the parent window |
| sandbox | string not required, but if present a value of \`'allow-same-origin allow-scripts'\` is required |
| src | string see next section “URL Schema” |

| URL Schema |  |
| :---- | :---- |
| /embed/:token/:view | token:stringFor authorization view:string \`:view\` determines the desired visualization to be shown in the iframe. One of \`clustermap\`, \`linechart\`, \`storygraph\`, \`tagcloud\`, \`keywordgraph\`, or \`geomap\` |

| URL Schema Additional optional parameters allow the configuration of the embedded visualization. |  |
| :---- | :---- |
| search=:search | search:stringoptionally this supports a search string, which will be treated as individual search to support multiple lines in the trend chart |
| date=:start-date,:end-date | start-date:stringend-date:stringExample: date=2023-01-01,2023-05-28 |
| topics=:topics | topics:string a comma separated list of unique numerical IDs, representing predefined topic definitions |
| source=:source | source:stringComma-separated list of sources (news etc.) |
| language=:language | language:string The language of the sources in ISO 3166-1 alpha-2 format |

| Examples |
| :---- |
| \<\!-- The tag cloud using the default search term \--\>\<iframe width='400' height='600' src='/embed/xkVUVgdpF9QPbzr2BWaT8syTaofarU6K/tagcloud' frameborder='0'\> |
| \<\!-- The geo map using a custom search term \--\>\<iframe width='400' height='600' src='/embed/xkVUVgdpF9QPbzr2BWaT8syTaofarU6K/geomap/search=fracking' frameborder='0'\> |


# **Appendix A: Authentication | Authorization** 

Authentication and authorization is handled using JSON Web Tokens (JWT). Until tokens are issued using the global webLyzard login server, new tokens can be obtained using the /token API endpoint using Basic Authentication.   
*A token is valid for 8 hours, after which the token will be rejected by the API and a new token must be generated*.

## **Obtaining a new token**

To obtain a new token, do a GET request to the /token endpoint:

| $ curl \-u \<user\>:\<pass\> https://api.weblyzard.com/1.0/token |
| :---- |

The server responds with the issued token for the user:

| xxx |
| :---- |

For TransMIXR, the user credentials can be requested from webLyzard.

## **Calling API methods using the obtained token** {#calling-api-methods-using-the-obtained-token}

All API calls must be authenticated using a valid token (see above). Pass the token using the “Authorization: Bearer” request header:

| $ curl \-i \-H "Authorization: Bearer xxx" https://api.weblyzard.com/1.0/... |
| :---- |
