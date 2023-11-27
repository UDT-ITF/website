---
title: "UDT-ITF API 0.1"
title_override: "UDT-ITF API 0.1"
id: text-api
layout: spec
tags: [specifications, text-api]
major: 0
minor: 1
patch: 0
pre: beta # change to "final" when stable
cssversion: 0
redirect_from:
  - /api/0.1.0/index.html
editors:
  - name: Neil Jefferies
    ORCID: https://orcid.org/0000-0003-3311-3741
    institution: Bodleian Libraries (University of Oxford)
  - name: Nilo Pedrazzini
    ORCID: https://orcid.org/0000-0003-3757-2961
    institution: Digital Scholarship @ Oxford (University of Oxford) & The Alan Turing Institute
  - name: Name Surname
    ORCID: https://orcid.org/xxxx-xxxx-xxxx-xxxx
    institution: Dept (Institute)
---

## Status of this Document
{:.no_toc}
__This Version:__ {{ page.major }}.{{ page.minor }}.{{ page.patch }}{% if page.pre != 'final' %}-{{ page.pre }}{% endif %}

__Previous Version:__ 
<!-- when you have a new version, move old version into folder in index.md file, e.g. /api/0.1.0/index.md and point to it like this: -->
<!-- __Previous Version:__ [0.1.0](/api/0.1.0/) -->

**Editors**

{% include api/editors.md editors=page.editors %}

<!-- {% include copyright.md %} -->

----

## 1 Introduction
This is a DRAFT proposal for an Interoperable Text Framework (ITF) specification for
discussion.

### 1.1 Audience
This document is intended for developers building applications that either share 
textual resources or consume such resources for display, analytics or other purposes. 
The background use-cases for developing the API are drawn from cultural heritage and 
research organisations but the use of the API is not restricted to these domains. 

### 1.2 Terminology
The key words _MUST_, _MUST NOT_, _REQUIRED_, _SHALL_, _SHALL NOT_, _SHOULD_, _SHOULD NOT_, 
_RECOMMENDED_, and _OPTIONAL_ in this document are to be interpreted as described in
[RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

### 1.3 The ITF Text Model

Regardless of how underlying texts are stored and managed, ITF-compliant servers _MUST_ behave 
as if they were NFC normalised UTF-8 Unicode. Character counts and offsets _MUST_ computed in 
terms of Unicode codepoints, and the texts returned as a result of invoking ITF API's _MUST_ 
be in that form, unless the _OPTIONAL_ "raw" quality specifier is invoked.   

> DISCUSSION POINT: Is "raw" quality useful in the context of ITF? It may only make
> sense to allow it for full text retreival. 

*ITF Text Resource* 
: An abstract textual work, identified by a unique identifier on an ITF-complaint server. An Text Resource may contain just a single text or multiple versions of a work as it has evolved. ITF makes no assumptions about the type or level of document that a Text Resource contains.

*ITF Edition*
: An ITF Edition is created when an Text Resource is made available for access. If the Text Resource is subsequently updated, then it is considered a new Edition. In order to maintain the integrity of references and citations, ITF-compliant servers provide a way of accessing previous Editions of a Text Resource.

*ITF Version*
: Texts evolve over time through the actions of one or more contributors. Text Resources can represent this history by making multiple versions of a text available. Versions are identified by labels and, optionally, dates. When multiple contributors are active, there may be more than one Version considered active on a particular date. 

*ITF Mode*
: It is useful to be able to specify or reference text fragments in terms of the structure of a document rather than just as character offsets from the start of a file. Modes describe the different ways that this can be achieved. Some modes reflect the physical structure of the source material (e.g. a volume, broken down by page, line and word) while others might reflect semantic structure (e.g. a novel, broken down by chapter, paragraph, sentence and word). Such approaches are more human friendly, map readily to many analytical tools, and are also easier to map between versions of a text.     

> Probably needs considerable expansion   

## 2 Text API

### 2.1 Abstract
This section describes an API for the delivery of texts and text fragments via a
standard http request. The ITF Text API specifies a web service that returns textual
data in response to a standard http or https request. The URL can specify:

-  The version of the underlying source text to be accessed
-  The mode used to specify a text fragment 
-  The text fragment to be returned
-  Whether to include any enrichments, such as formatting 
-  The format in which to return the text data

A URL can also be constructed to request basic technical information about the source
text to support client applications. The ITF Text API is conceived to facilitate 
systematic referencing and reuse of textual resources in repositories in a manner
that is both user- and machine-friendly. 

This specification does not describe how a server manages or stores textual resources,
just how it must respond to ITF-compliant requests. 

### 2.2 URL Syntax
The ITF Text API can be called in two forms: one to request a text fragment,
and a second to request technical information about the underlying source text.
Both forms convey the request's information in the path segment of the URL,
rather than as query parameters. This makes responses more easily able
to be cached, either at the server or by standard web-caching infrastructure. 

To allow for extension, this specification does not define the behaviour
of an implementing server when it receives requests that do not match
one of the two request syntaxes below.

#### 2.2.1 Text Fragment Request URL Syntax
The ITF Text API URL for requesting a text fragment _MUST_ conform to the following format:

    http[s]://server/[prefix/]identifier/version/mode/fragment/quality[.format]

where \[\] delimits components which are optional.

The URI Template ([RFC6750](https://datatracker.ietf.org/doc/html/rfc6750)) form
is:

    http://{server}{/prefix}/{identifier}/{version}/{mode}/{fragment}/{quality}{.format}

For example:

    http://www.example.org/text-service/abcd1234/default/char/0,247/tei.xml

The sections of the Text Fragment Request URL include:

| Syntax | Description |
| ------------------- | ------------ |
|`http (or https)`| Indicates the use of the http or https protocol in calling the service. |
|`server`| The host server on which the ITF-compliant text service resides. |
|`prefix`| The path on the host server to the ITF-compliant text service. This prefix is <br>optional, but may be useful when the host server supports multiple services. (note: The prefix MAY contain slashes or constructions that resemble service parameters.) |
|`identifier`| A unique identifier of the requested text resource, expressed as a string. This may be an ark, URN, filename, or other unique identifier but ideally SHOULD be a Persistent Identifier. Special characters MUST be URI encoded. |
|`version, mode, fragment, quality, format`| Parameters defining the characteristics of the returned text fragment. These are described in detail in [Section 2.4 - Fragment Request Parameters](#24-text-fragment-request-parameters). |

> DISCUSSION POINT: Is this sufficient?

#### 2.2.2 Resource Information Request URL Syntax
The ITF Text API URL for requesting information about a text resource _MUST_ conform to
the following format:

    http[s]://server/[prefix/]identifier/[version/]info.format

where \[ \] delimits the optional prefix component.

The URI Template ([RFC6750](https://datatracker.ietf.org/doc/html/rfc6750)) form
is:

    http://{server}{/prefix}/{identifier}{/version}/{info}.{format}

For example:

    http://www.example.org/text-service/abcd1234/info.json

The sections of the Text Information Request URL include:

| Syntax | Description |
| ------------------- | ------------ |
|`http (or https)`| Indicates the use of the http or https protocol in calling the service. |
|`server`| The host server on which the ITF-compliant text service resides. |
|`prefix`| The path on the host server to the ITF-compliant text service. This prefix is optional, but may be useful when the host server supports multiple services. (note: The prefix MAY contain slashes or constructions that resemble service parameters.) |
|`identifier`| A unique identifier of the requested text resource, expressed as a string. This may be an ark, URN, filename, or other unique identifier but ideally SHOULD be a Persistent Identifier. Special characters MUST be URI encoded. |
|`info`| Specifies the information being requested. These are described in detail below. |
|`format`| Specifies the format of the returned information, which can be "json" or "xml". |

> DISCUSSION POINT: Is this sufficient? Do we need xml at all here, or is json sufficient? 

#### 2.2.3 Accessing historical editions
An ITF text resource MAY change over time, allowing updates, corrections or the addition 
of new versions of a text. In ITF terminology, each change constitutes a new "edition" 
of the text resource. In order to resolve text references correctly, an ITF-compliant text
service that supports multiple editions MUST provide a mechanism for accessing these earlier 
editions.

> DISCUSSION POINT: The proposed mechanism for doing this would be to implement at least some of
> [RFC7089](https://datatracker.ietf.org/doc/html/rfc7089)

### 2.3 Identifier
The API places no restrictions on the form of the identifiers that a server may use or 
support, although the identifier MUST be expressed as a string. Public facing servers
SHOULD use Persistent Identifiers and undertake to maintain the ITF endpoint and 
underlying resources.

> DISCUSSION POINT: How much do we want or need to go into persistence/preservation?
> Should a "friable" resource have to indicate this via the Info API? 

All special characters (e.g. ? or \#) MUST be URI encoded to avoid unpredictable
client behaviors. The URL syntax relies upon slash (/) separators so any slashes 
in the identifier MUST be URI encoded (aka. percent-encoded, replace / with %2F ). 
See discussion in [Section9 - URL Encoding and Decoding.](#url_encoding)

### 2.4 Text Fragment Request Parameters
All parameters described below are required for compliant construction of an ITF Text API
URL. The sequence of parameters in the URL MUST be in the order described below. The 
order of the parameters reflects the order of operation a text service is expected
to use to process a request. Thus, the desired version of a text is located, the relevant 
fragment is extracted using the mode specified, any formatting or tagging is applied,
and finally conversion to the desired format. This resulting text fragment is returned 
as the representation for the URL. 

#### 2.4.1 Version
An ITF text resource MAY contain multiple versions of a text which MUST be identified by a
label (a string) and MAY also have an associated date/time. The version parameter defines 
which version of text a fragment is to be retrieved from. The special value "default" MUST 
be used with a resource that contains no versions.

| Form of Version Parameter| Description |
| ------------------- | ------------ |
|`default`| Specifies the only text in an unversioned resource. MUST NOT be used with a versioned resource. |
|`l:label`| Specifies the version with a label "label". Labels MUST be URI encoded. |
|`d:yyyy-mm-dd`| Specifies the version current at a particular date in ISO 8601-1:2019 format. Negative (BCE) years are allowed. |
|`d:yyyy-mm-ddThh:mm:ss`| Specifies the version current at a particular date/time in ISO 8601-1:2019 format. Negative (BCE) years are allowed. |

> DISCUSSION POINTS: Do we allow truncation to just yyyy-mm, for example? 
> Can support for times be optional?
> Do we need version numbers or are labels sufficient

A server MUST return a 400 (bad request) code if 'default' is used with a versioned resource, or 
a date is specified for a resource with no version dates. 

#### 2.4.2 Mode
The mode parameter specifies how a text fragment is specified in the subsequent fragment 
parameter. This enables text fragments to be specified in terms of the higher level 
structural elements of a text, rather than just using a character offset from the start 
of the file. As well as being more friendly to human interpretation, this enables fragment 
references to be more readily mapped between versions of a text, or to be updated 
as a text is edited.  ITF defines a few common modes but new modes can be defined
using the predefined modes as an illustration.

The modes represent a generalised version of a text format. A [mode information request](253-mode-information-request) 
can be used to identify version-specific nomenclature (such as designating pages recto-verso)
although this is purely for information for display. The list of modes is extensible and
any custom modes can be discovered via the aforementioned mode information request.

| Form of Mode Parameter| Description |
| ------------------- | ------------ |
|`char`| The fragment will be specified in terms of characters and character offsets. |
|`token`| The fragment will be specified in terms of tokens (words inn Western languages). | 
|`book`| The fragment will be specified in terms of the physical structure of a book. |
|`prose`| The fragment will be specifies in terms of the semantic structure of a prose work. |

> DISCUSSION POINT: This is where it gets tricky! My preference would be to keep modes
> as simple as possible and discuss mapping to real world examples in the Implementation Notes.
> How many does it make sense to start with? Letter, Journal, Anthology, Play?  

#### 2.4.3 Fragment
The fragment parameter specifies the text fragment to be returned as a result of invoking the 
Text Fragment API. The format of the fragment parameter depends on the mode selected except for
the special value "full" which returns the entirety of the text of the Version specified, 
irrespective of the value of the mode parameter.

Fragment specifiers operate on the plaintext of the version being accessed. Thus, regardless 
of the format of the underlying source data, the server _MUST_ ignore any tagging and formatting
when computing fragments. When counting characters, and sequence of contiguous whitespace 
characters (codepoints of [Unicode class "Space Separator"](https://unicodeplus.com/category/Zs)) 
_MUST_ be counted as a single character.   

In ITF all counting _MUST_ be "1"-based, so the first page of a book is page 1. 

> DISCUSSION POINT: Are these reasonable restrictions/simplifications?

##### 2.4.3.1 Char Mode Fragments

Char(acter) mode fragment specifiers identify a block of text in a text file by counting individual
characters (Unicode codepoints, as noted in [above](#13-the-itf-text-model). 

| Form of Fragment Parameter| Description |
| ------------------- | ------------ |
|`x,y`| The fragment starts just before character number _x_, and extends until character number _y_ (inclusive). |
|`,y`| The fragment starts at the beginning of the text, and extends until character number _y_ (inclusive). |
|`x+n`| The fragment starts just before character number _x_, and extends for _n_ characters. |
|`x`| Returns character number _x_. |

> DISCUSSION POINT: do we need to be able to specify zero-length fragments? E.g. for the purpose of being able later to specify via annotation 
> that a character string present in one version of a text is absent from that location in another version.

##### 2.4.3.1 Token Mode Fragments

Token mode fragment specifiers identify a block of text in a text file by counting tokens, usually 
delimited by whitespace characters (spaces, tabs etc.). Fragments specified in this manner can only 
include complete tokens.

| Form of Fragment Parameter| Description |
| ------------------- | ------------ |
|`x,y`| The fragment starts just before token number _x_, and extends until token number _y_ (inclusive). |
|`,y`| The fragment starts at the beginning of the text, and extends until token number _y_ (inclusive). |
|`x+n`| The fragment starts just before token number _x_, and extends for _n_ tokens. |
|`x`| Returns token number _x_. |

> DISCUSSION POINT: _OPTIONAL_: If the parameters are enclosed in square brackets (\[\]) then the fragment will be extended to include any relevant 
> leading and/or trailing punctuation according to the language rules relevant to the version.    

##### 2.4.3.3 Book Mode Fragments

Book mode fragment specifiers identify a block of text in terms of a generalised physical book structure. Books are 
considered to be made up of pages, lines of text and characters. Book mode coordinates are thus triplets of the form _p;l;c_ 
corresponding to character number _c_ of line number _l_ on page number _p_. 

This is an example of a hierarchical text mode which follows some relatively simple rules, which are expanded in detail the table below:
- Coordinates may be truncated from the right hand side (removing the most fine-grained divisions first)
  - If a fragment starting point is truncated, missing values _MUST_ be assumed to be "1"
  - If a fragment end point is truncated, missing values _MUST_ be assumed to be their maximum valid value
- Length specifiers _MUST_ operate at the same granularity as the starting point
  - Length specifiers can flow over. For example, requesting more lines than are on a page is valid, provided subsequent pages have sufficient lines.   
- A single set of coordinates returns a single item at the same level of granularity as the coordinates 

| Form of Fragment Parameter| Description |
| ------------------- | ------------ |
|`p1,p2`| The fragment extends from the first character on page p1 until just after the last character on page p2. |
|`p1;l1,p2;l2`| The fragment extends from the start of line l1, on page p1 until the end of line l2, on page p2. |
|`p1;l1;c1,p2;l2;c2`| The fragment starts from character c1, line l1, on page p1 and ends just after character c2, on line l2, on page p2. |
|`,p2`| The fragment extends from the beginning of the text, to just after the last character on page p2. |
|`,p2;l2`| The fragment extends from the beginning of the text, to the ends of line l2, on page p2. |
|`,p2;l2;c2`| The fragment starts at the beginning of the text, and ends just after character c2, on line l2, on page p2. |
|`p1+p2`| The fragment extends from the first character on page p1 until just after the last character on page p1+p2. |
|`p1;l1+l2`| The fragment extends from the start of line l1, and includes the following l2 lines (regardless of pagination). |
|`p1;l1;c1+c2`| The fragment starts from character c1, line l1, on page p1 and includes the following c2 characters. |
|`p1`| The fragment is the whole of the page p1. |
|`p1;l1`| The fragment is the whole of line l1 on page p1. |
|`p1;l1;c1`| The fragment is character c1 of line l1 on page p1. |

##### 2.4.3.4 Prose Mode Fragments 

Prose mode is another hierarchical mode that identifies a block of text in terms of a 
generalised semantic prose work structure. Prose works are considered to be made up of sections, 
paragraphs, sentences, words (tokens) and characters. Prose mode coordinates are thus of the form 
_S;p;s;w;c_ corresponding to character number _c_ of word _w_ of sentence _s_ of paragraph _p_ of 
section _S_. As a hierarchical mode, it follows the same basic rules so the table below just shows a
few examples.

| Form of Fragment Parameter| Description |
| ------------------- | ------------ |
|`S1,S2`| The fragment extends from the first character of section S1 until just after the last character of section S2. |
|`S1;p1;s1;w1;c1;,S2;p2;s2;w2;c2`| The fragment starts from character c1, word w1, of sentence s1, of paragraph p1 of section S1 and ends just after character c2, of word w2, of sentence s2 of paragraph p2, of section S2. |
|`,S2`| The fragment extends from the beginning of the text, to just after the last character of section S2. |
|`,S2;p2;s2`| The fragment starts at the beginning of the text, and ends just after the last character of sentence s2, of paragraph p2. of section S2. |
|`S1;p1+p2`| The fragment extends from the start of paragraph p1 of section S1, and includes the following p2 paragraphs. |
|`S1;p1;s1+s2`| The fragment starts from the start of sentence s1, of paragraph p1, of section S1, and includes the following s2 sentences. |
|`S1`| The fragment is the whole of section S1. |
|`S1;p1`| The fragment is the whole of paragraph p1 of section S1.|

In the simplest case, sections can be considered to equate broadly to chapters. However, in practice, documents often contain additional 
elements such as forewards, appendices, chapter summaries etc. These are accommodated by allowing sections to be numbered hierarchically.
Thus, a more complex document might have the following sections.

| Section Number | Description |
| ---------- | ---------- |
|`1`| Place holder for elements that come before the main text |
|`1.1`| Title Page |
|`1.2`| Contents List |
|`1.3`| Foreward |
|`2`| Place holder for elements comprising the main text |
|`2.1`| Chapter 1 |
|`2.2`| Chapter 2 |
|`2.N`| Chapter N |
|`3`| Place holder for elements come after the main text |
|`3.1`| Appendix 1 |
|`3.2`| Appendix 2 |
|`3.3`| Index |

For display purposes, how these sections are presented and labelled can be discovered by making
an appropriate [Mode Information Request](253-mode-information-request) 

#### 2.4.4 Quality

The quality parameter specifies . This allows an ITF server to 
include additional enrichment (e.g. formatting or tagging information), if this is available.

| Form of Quality Parameter | Description |
| ------------------- | ------------ |
|`raw`| The server will return the fragment in a form that most closely matches it's underlying storage format. This is entirely server dependent and the following format parameter is ignored. |
|`compact`| The fragment will be returned as plaintext with sequences of whitespace characters reduced to a single space. | 
|`plaintext`| The fragment will be returned as plaintext. |
|`rich`| The fragment will be returned including any available enrichment. |

#### 2.4.5 Format

The quality parameter specifies the requested text fragment is returned. This allows an ITF server to 
include additional enrichment (e.g. formatting or tagging information), if this is available.

| Form of Quality Parameter | Description |
| ------------------- | ------------ |
|`txt`| The fragment will be returned as a UTF-8 text file. Not valid for "rich" quality fragments |
|`tei`| The fragment will be returned with any enrichment expressed using TEI XML tags. |
|`html`| The fragment will be returned with any enrichment expressed as HTML 5 tags. |
|`md`| The fragment will be returned with any enrichment expressed via Markdown formatting. |

> DISCUSSION POINT: This needs to be more nuanced since we are not necessarily returning complete TEI or HTML documents

### 2.5 Resource Information Request

The ITF Text API URL for requesting information about a text resource _MUST_ conform to
the following format:

    http[s]://server/[prefix/]identifier/[version/]info.format

| Form of Info Parameter | Applies to | Description |
| ------------------- | ------------ | ------------ |
|`textinfo`| Text Resources and Versions | Provides basic information about a textual object, or a version. |
|`versions`| Text Resources | Provides information about all the versions held in a Text Resource. |
|`modes`| Text Resources | Provides information about the modes supported by a Text Resource, and defines any custom modes. |
|`modes`| Versions | Provides version specific information (bounds, nomenclature) about how modes relate to a specific version. |

### 2.5.1 Text Information Request

The response will return the following information for an ITF Resource.

| Element | Description|
| ------- | ---------------|
| `identifier` | The unique identifier of the Resource, expressed as a string. The identifier _MUST_ be supplied without URI encoding. |
| `date` | The date of the most recent edition of the resource in ISO 8601 format |
| `versioning` | Indicates of a Resource contain multiple versions. Possible values: _none, linear, date, graph_ |
| `modes` | Indicates which standard modes are supported. A list of one or more of _char, token, book, prose_. |
| `custom_modes` | _OPTIONAL_ List of custom modes that are supported. |
| `qualities` | Indicates which qualities are supported. A list of one or more of _raw, compact, plaintext, rich_. |
| `formats` | Indicates which formats are supported. A list of one or more of _txt, tei, html, md_. |
| `DC-metadata` | _OPTIONAL_ URL of a DC metadata record |
| `editions` | _OPTIONAL_ If the server supports editions they should be detailed here. |
| `  first_ed` | Date of the first edition of the Resource |
| `  ed_list` | _OPTIONAL_ List of the dates of all the editions of the resource. If there are many editions this may be omitted. |

> DISCUSSION POINT: DC is rich enough metadata to be useful but not too onerous. Thoughts?

Example JSON response for a Resource.

```
    {
      "identifier" : "1E34750D-38DB-4825-A38A-B60A345E591C",
      "versioning" : "date",
      "date" : "2023-11-24"
      "modes" : [ "char", "token", "book", "prose" ],
      "qualities" : ["compact", "plaintext"],
      "formats" : [ "txt" ],
      "editions" : {
        "first_ed" : "2022-05-07",
        "ed_list" : [
          "2022-05-07",
          "2023-01-06",
          "2023-11-24"
        ]
      } 
    }
```

`The response will return the following information for an ITF Version.

| Element | Description |
| ------- | ---------------|
| `label` | The label used to identify the version. The label _MUST_ be unique within the containing Resource. Labels are used to sequence _linear_ versions|
| `date` | If a Resource has _date_ based versioning, all versions _MUST_ have a unique date. Otherwise this is _OPTIONAL_. |
| `succeeds` | Lists the label(s) of preceeding versions if a Resource has _graph_ based versioning. |
| `preceeds` | Lists the label(s) of succeeding versions if a Resource has _graph_ based versioning. |
| `modes` | Indicates which standard modes are supported. A list of one or more of _char, token, book, prose_. |
| `custom_modes` | _OPTIONAL_ List of custom modes that are supported. |
| `qualities` | Indicates which qualities are supported. A list of one or more of _raw, compact, plaintext, rich_. |
| `formats` | Indicates which formats are supported. A list of one or more of _txt, tei, html, md_. |
| `DC-metadata` | _OPTIONAL_ URL of a DC metadata record |

> DISCUSSION POINT: DC is rich enough metadata to be useful but not too onerous. Thoughts?

Example JSON response for a Resource.

```
{
  "identifier" : "1E34750D-38DB-4825-A38A-B60A345E591C",
  "versioning" : "date",
  "modes" : [ "char", "token", "book", "prose" ],
  "qualities" : ["compact", "plaintext"],
  "formats" : [ "txt" ] 
}
```

### 2.5.2 Version Information Request

ITF Versions 


### 2.5.3 Resource Mode Information Request

### 2.5.3 Version Mode Information Request
