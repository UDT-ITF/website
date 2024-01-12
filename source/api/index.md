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
  - name: Michael Hawkins
    ORCID: https://orcid.org/my-orcid?orcid=0000-0003-4306-7515
    institution: Cambridge Digital Humanities (University of Cambridge)
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
textual resources for display, computational analysis or any other purpose. 
The background use-cases for developing the API are drawn from cultural heritage and 
research organisations but the use of the API is not restricted to these domains. 

### 1.2 Terminology
The key words _MUST_, _MUST NOT_, _REQUIRED_, _SHALL_, _SHALL NOT_, _SHOULD_, _SHOULD NOT_, 
_RECOMMENDED_, and _OPTIONAL_ in this document are to be interpreted as described in
[RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

### 1.3 The ITF Text Model

Regardless of how underlying texts are stored and managed, ITF-compliant servers _MUST_ behave 
as if they were NFC normalised UTF-8 Unicode. Character counts and offsets _MUST_ be computed in 
terms of Unicode codepoints, and the texts returned as a result of invoking ITF APIs _MUST_ 
be in that form, unless the _OPTIONAL_ "raw" quality specifier is invoked.   

> DISCUSSION POINT: Is "raw" quality useful in the context of ITF? It may only make
> sense to allow it for full text retrieval.
> > MH: I assume that by 'full text retrieval' you are referring to the raw original underlying file and not the entire text returned by setting fragment to full.

*ITF Text Resource* 
: An abstract textual work, identified by a unique identifier on an ITF-compliant server. A Text Resource may contain one or more single texts or multiple versions of a work as it has evolved. ITF makes no assumptions about the type or level of document that a Text Resource contains.

*ITF Edition*
: An ITF Edition is created when a Text Resource is made available for access. If the Text Resource is subsequently updated, then it is considered a new Edition. In order to maintain the integrity of references and citations, ITF-compliant servers provide a way of accessing previous Editions of a Text Resource.

*ITF Version*
: Texts change over time through the actions of one or more contributors. Text Resources can represent this history by making multiple versions of a text available. Versions are identified by labels and, optionally, dates. When multiple contributors are active, there may be more than one version considered active on a particular date. 

*ITF Mode*
: It is useful to be able to specify or reference text fragments in terms of the structure of a document rather than just as character offsets from the start of a file. Modes describe the different ways that this can be achieved. Some modes reflect the physical structure of the source material (e.g. a volume, broken down by page, line and word) while others might reflect semantic structure (e.g. a novel, broken down by chapter, headers, paragraph, sentence and word). Such approaches are more human friendly, map readily to many analytical tools, and are also easier to map between versions of a text.

> Probably needs considerable expansion

### 1.4 Overview of the ITF Specification
The ITF specification is conceived to facilitate systematic referencing and reuse of 
textual resources in repositories in a manner that is both user- and machine-friendly.

The specifications defines two API forms: one to request a text fragment,
and a second to request technical information about the underlying source text.
Both forms convey the request's information in the path segment of the URL,
rather than as query parameters. This makes responses more easily cacheable, 
either by the server or standard web-caching infrastructure.

To allow for extension, this specification does not define the behaviour
of an implementing server when it receives requests that do not match the 
forms below. Similarly, it does not describe how a server manages or stores
textual resources, just how it must respond to ITF-compliant requests. 

## 2 Text Fragment API

### 2.1 Abstract
This section describes an API for the delivery of texts and text fragments via a
standard HTTP request. The ITF Text Fragment API specifies a web service that returns textual
data in response to a standard HTTP or HTTPS request. The URL can specify:

-  The version of the underlying source text to be accessed
-  The mode used to specify a text fragment 
-  The text fragment to be returned
-  Whether to include any enrichments, such as formatting 
-  The format in which to return the text data

### 2.2 Text Fragment Request URL Syntax
The ITF Text API URL for requesting a text fragment _MUST_ conform to the following format:

    http[s]://server/[prefix/]identifier/version/mode/fragment/quality[.format]

where \[\] delimits components which are optional.

The URI Template ([RFC6750](https://datatracker.ietf.org/doc/html/rfc6750)) form
is:

    http://{server}{/prefix}/{identifier}/{version}/{mode}/{fragment}/{quality}{.format}

For example:

    http://www.example.org/text-service/abcd1234/default/char/0,247/teilite.xml

> To avoid ambiguity, I think it's useful to make it clear that the files are presented using the minimalistic
> TEI Lite rather than TEI (in general). There will be considerable difference in many projects between their
> original TEI and the TEI Lite version.

The sections of the Text Fragment Request URL are:

| Syntax | Description |
| ------------------- | ------------ |
|`http (or https)`| Indicates the use of the HTTP or HTTPS protocol in calling the service. |
|`server`| The host server on which the ITF-compliant text service resides. |
|`prefix`| The path on the host server to the ITF-compliant text service. This prefix is optional, but may be useful when the host server supports multiple services. **Note:** The prefix may contain slashes or constructions that resemble service parameters. |
|`identifier`| A unique identifier of the requested text resource, expressed as a string. This may be an ark, URN, filename, or other unique identifier but ideally SHOULD be a Persistent Identifier. Special characters MUST be URI encoded. |
|`version, mode, fragment, quality, format`| Parameters defining the characteristics of the returned text fragment. These are described in detail in [Section 2.4 - Fragment Request Parameters](#24-text-fragment-request-parameters). |

> DISCUSSION POINT: Is this sufficient?

All parameters are required for compliant construction of an ITF Text Fragment API
URL. The sequence of parameters in the URL MUST be in the order described above. The 
order of the parameters reflects the order of operation a text service is expected
to use when processing a request. Thus, the desired version of a text is located, the relevant 
fragment is extracted using the mode specified, any formatting or tagging is applied,
and finally conversion to the desired format. This resulting text fragment is returned 
as the representation for the URL. 

### 2.3 Identifier
The API places no restrictions on the form of the identifiers that a server may use or 
support, although the identifier MUST be expressed as a string. Public facing servers
SHOULD use Persistent Identifiers and undertake to maintain the ITF endpoint and 
underlying resources.

> DISCUSSION POINT: How much do we want or need to go into persistence/preservation?
> Should a "friable" resource have to indicate this via the Info API? 
> > MH: I'd say that we should elaborate on persistence/preservation/friable as it would be useful 
> > for less technical audiences.

All special characters (e.g. ? or \#) MUST be URI encoded to avoid unpredictable
client behaviours. The URL syntax relies upon slash (/) separators so any slashes 
in the identifier MUST be URI encoded (aka. percent-encoded, replace / with %2F ). 
See discussion in [Section9 - URL Encoding and Decoding.](#url_encoding)

### 2.4 Version
An ITF text resource MAY contain multiple versions of a text which MUST be identified by a
label (a string) and MAY also have an associated date/dateTime. The version parameter defines 
which version of text a fragment is to be retrieved from. The special value "default" MUST 
be used with a resource that contains no versions.

| Form of Version Parameter| Description |
| ------------------- | ------------ |
|`default`| Specifies the only text in an unversioned resource. It MUST NOT be used with a versioned resource. |
|`l:label`| Specifies the version with the label "label". Labels MUST be URI encoded. |
|`d:yyyy-mm-dd`| Specifies the version current at a particular date in ISO 8601-1:2019 format. Negative (BCE) years are allowed. |
|`d:yyyy-mm-ddThh:mm:ss`| Specifies the version current at a particular dateTime in ISO 8601-1:2019 format. Negative (BCE) years are allowed. |

> DISCUSSION POINTS: Do we allow truncation to just yyyy-mm, for example? 
> Can support for times be optional?
> > MH: I think the question of optional times, optional days and, perhaps even months and days are all related.
> > The minimalist side of me wants to say that they are optional -- a project might simply have a 2023 release of the text 
> > that will be superseded by the 2024 release. The pragmatic side of me, however, leans towards the view that 
> > they MUST be dateTime values. Any other version state could be indicated with labels/version numbers, just as 
> > tags are used in Git.
> Do we need version numbers or are labels sufficient
> > MH: It depends on whether we can foresee a need for a text collection to use both labels and versions
> > to identify specific incarnations of the resource in conjunction. That is, l:main/v:1.0.0 - for the primary release version
> > and l:crowdsourced/v:1.0.0 - for a different version of the text (say with some crowdsourced additions that
> > the provider doesn't want to include within the main release but does want to make available in an explicit
> > crowdsourced release). If that specific scenario isn't to be supported, there's no reason that the label couldn't
> > be a version number. If we were to adopt the latter approach, should we call it a 'tag' just as Git does?

A server MUST return a 400 (bad request) code if 'default' is used with a versioned resource, or 
a date is specified for a resource with no version dates. 

> Is there a mechanism for retrieving the latest version of a versioned resource? If not, couldn't 'default' be used to
> retrieve it? Or, do I need more coffee?

### 2.5 Mode
The mode parameter specifies how a text fragment is specified in the subsequent fragment 
parameter by providing a system of "text coordinates". This provides the option for text 
fragments to be specified in terms of the higher level structural elements of a text, 
rather than just using a character offset from the start of the file. As well as being more 
friendly to human interpretation, mode allow fragment references to be more readily mapped between 
versions of a text, or to be updated as a text is edited. However, since modes reflect some aspects
of the structure of the underlying texts, most will only be applicable to particular types of 
Resources. Complex texts which have parts with different structures may not be fully 
addressable using just a single higher level mode. ITF defines a few basic modes but new modes 
can be defined using the predefined modes as an basis. 

The modes represent a generalised version of a text format. A [mode information request](253-mode-information-request) 
can be used to identify version-specific nomenclature (such as actual page numbering)
although this is primarily for information or display. The list of modes is extensible and
any custom modes can be discovered via the aforementioned mode information request.

| Form of Mode Parameter| Description |
| ------------------- | ------------ |
|`char`| The fragment will be specified in terms of characters and character offsets. |
|`token`| The fragment will be specified in terms of tokens (words in Western languages). | 
|`book`| The fragment will be specified in terms of the physical structure of a book. |

> DISCUSSION POINT: This is where it gets tricky! My preference would be to keep modes
> as simple as possible and discuss mapping to real world examples in the Implementation Notes.
> How many does it make sense to start with? Letter, Journal, Anthology, Play?  
> > MH: Char and token seem fine. Things get problematic after that. The physical features of the text are ultimately dependant on the physical nature of the artefact (books, scroll, funerary marker, clay tablets, woven strands of thread, etc). Using a book-based model is imposing a very particular type of cultural product onto texts that it doesn't apply to. Would 'surface' perhaps be a slightly better term? It certainly would avoid the problems mentioned in my next comment further down.

### 2.6 Fragment
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
> > MH: Everything apart from 'the first page of a book is page 1' seems fine. What do you mean by page? Page 1 in the book might actually be the 20th page. If we were to use 'surface' this would differentiate surface 1 from 'page 1' as it appears in the text.

#### 2.6.1 Char Mode Fragments

Char(acter) mode fragment specifiers identify a block of text in a text file by counting individual
characters (Unicode codepoints, as noted in [above](#13-the-itf-text-model). All counting _MUST_ be "1"-based.

| Form of Fragment Parameter| Description |
| ------------------- | ------------ |
|`x,y`| The fragment starts just before character number _x_, and extends until character number _y_ (inclusive). |
|`,y`| The fragment starts at the beginning of the text, and extends until character number _y_ (inclusive). Equivalent to `1,y`|
|`x+n`| The fragment starts just before character number _x_, and extends for _n_ characters. |
|`x`| Returns character number _x_. |

> DISCUSSION POINT: do we need to be able to specify zero-length fragments? E.g. for the purpose of being able later to specify via annotation 
> that a character string present in one version of a text is absent from that location in another version.

#### 2.6.1 Token Mode Fragments

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

#### 2.6.3 Book Mode Fragments

Book mode fragment specifiers identify a block of text in terms of a generalised physical book structure. Books are 
considered to be made up of pages, lines of text and characters. Book mode coordinates are thus triplets of the form _p;l;c_ 
corresponding to character number _c_ of line number _l_ on page number _p_. 

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

#### 2.6.4 Hierarchical Modes
The physical Book mode is an example of a hierarchical text mode which follows some relatively simple rules:

- A text location is expressed in a hierarchical mode as a series of positive integer numerical coordinates separated by semicolons
  - A numerical coordinate may be expressed as a dotted multiplet (e.g. 1.2.3) to represent a hierarchical structure in the document (e.g. the sections and subsections in a document such as this).
  - Coordinates _MUST_ end with a digit
  - Numerical coordinates always start counting with "1". How these sections are presented and labelled with respect to an actual document can be discovered by making an appropriate [Mode Information Request](253-mode-information-request)  
  - Coordinates are read from left-to-right with the leftmost corresponding to the largest scale structures in the source text (pages in the example above).   
- Coordinates may be truncated from the right hand side (removing the most fine-grained divisions first)
  - If a fragment starting point is truncated, missing values _MUST_ be assumed to be "1"
  - If a fragment end point is truncated, missing values _MUST_ be assumed to be their maximum valid value
  - A coordinate that comprises a dotted multiplet _MUST_ be truncated in its entirety (1.2.3 cannot be truncated to 1.2) 
- Length specifiers _MUST_ operate at the same granularity as the starting point
  - Length specifiers can flow over. For example, requesting more lines than are on a page is valid, provided subsequent pages have sufficient lines.   
- A single set of coordinates returns a single item at the same level of granularity as the coordinates 

IMPLEMENTATION NOTE: A mode has only one coordinate is non-hierarchical and behaves in a similar manner to _char_ or _token_ modes.       

#### 2.6.5 Custom Modes

### 2.7 Quality
The quality parameter allows an ITF server to include additional enrichment (e.g. formatting or 
tagging information) in the returned text, if this is available.

| Form of Quality Parameter | Description |
| ------------------- | ------------ |
|`raw`| The server will return the fragment in a form that most closely matches its underlying storage format. This is entirely server dependent and the following format parameter is ignored. |
|`compact`| The fragment will be returned as plaintext with sequences of whitespace characters reduced to a single space. | 
|`plaintext`| The fragment will be returned as plaintext. |
|`rich`| The fragment will be returned including any available enrichment. |

### 2.8 Format
The format parameter specifies the file format used to return the requested text fragment is returned.

| Form of Format Parameter | Description |
| ------------------- | ------------ |
|`txt`| The fragment will be returned as a UTF-8 text file. Not valid for "rich" quality fragments |
|`tei`| The fragment will be returned with any enrichment expressed using TEI XML tags. |
|`html`| The fragment will be returned with any enrichment expressed as HTML 5 tags. |
|`md`| The fragment will be returned with any enrichment expressed via Markdown formatting. |

> DISCUSSION POINT: This needs to be more nuanced since we are not necessarily returning complete TEI or HTML documents

## 3 Text Information API 

### 3.1 Abstract
This section describes an API to request technical information about text Resources and Versions via a
standard HTTP request. The ITF Text Information API specifies a web service that returns JSON
data in response to a standard HTTP or HTTPS request. The URL can specify:
- The Resource or Version that is the target of the request
- The type of information that is requested 

### 3.2 URL Syntax

The ITF API URL for requesting information about a text resource _MUST_ conform to
the following format:

    http[s]://server/[prefix/]identifier/[version/]info.json

where \[ \] delimits the optional prefix component.

The URI Template ([RFC6750](https://datatracker.ietf.org/doc/html/rfc6750)) form
is:

    http://{server}{/prefix}/{identifier}{/version}/{info}.json

For example:

    http://www.example.org/text-service/abcd1234/info.json

The sections of the Text Information Request URL include:

| Syntax | Description |
| ------------------- | ------------ |
|`http (or https)`| Indicates the use of the HTTP or HTTPS protocol in calling the service. |
|`server`| The host server on which the ITF-compliant text service resides. |
|`prefix`| The path on the host server to the ITF-compliant text service. This prefix is optional, but may be useful when the host server supports multiple services. **Note:** The prefix may contain slashes or constructions that resemble service parameters |
|`identifier`| A unique identifier of the requested text resource, expressed as a string. This may be an ark, URN, filename, or other unique identifier but ideally SHOULD be a Persistent Identifier. Special characters MUST be URI encoded. |
|`info`| Specifies the information being requested. These are described in detail below. |

> DISCUSSION POINT: Is this sufficient? Is json OK as the only return format? 

| Form of Info Parameter | Applies to | Description |
| ------------------- | ------------ | ------------ |
|`textinfo`| Text Resources and Versions | Provides basic information about a textual object, or a version. |
|`versions`| Text Resources | Provides information about all the versions held in a Text Resource. |
|`modes`| Text Resources | Provides information about the modes supported by a Text Resource, and defines any custom modes. |
|`modes`| Versions | Provides version specific information (bounds, nomenclature) about how modes relate to a specific version. |

### 3.3 Resource Textinfo Request

The response will return the following information for an ITF Resource.

| Resource Textinfo Element | Description|
| ------- | ---------------|
| `identifier` | The unique identifier of the Resource, expressed as a string. The identifier _MUST_ be supplied without URI encoding. |
| `date` | The date of the most recent edition of the Resource in ISO 8601 format. |
| `versioning` | Indicates of a Resource contain multiple versions, and, if so, how they are ordered. Possible values: _none, linear, date, graph_ |
| `modes` | Indicates which standard modes are supported. A list of one or more of _char, token, book, prose_. |
| `custom_modes` | _OPTIONAL_ List of custom modes that are supported. |
| `qualities` | Indicates which qualities are supported. A list of one or more international date format of _raw, compact, plaintext, rich_. |
| `formats` | Indicates which formats are supported. A list of one or more of _txt, tei, html, md_. |
| `DC-metadata` | _OPTIONAL_ URL of a DC metadata record |
| 'first_edition' | Date of the first edition of the Resource in ISO 8601 format. _MUST_ be the same as "date" if the Resource does not support editions.| 
| `editions` | _OPTIONAL_ If the server supports editions this section _MUST_ exist. The editions section can contains a list of dates of all the editions in ISO 8601 format. 

Example JSON response for a Resource.

```
{
  "identifier" : "1E34750D-38DB-4825-A38A-B60A345E591C",
  "versioning" : "date",
  "date" : "2023-11-24"
  "modes" : [ "char", "token", "book", "prose" ],
  "custom_modes" : [ "prose" ],
  "qualities" : ["compact", "plaintext"],
  "formats" : [ "txt" ],
  "first_edition" : "2022-05-07",
  "editions" : [
    "2022-05-07",
    "2023-01-06",
    "2023-11-24"
  ] 
}
```
### 3.4 Version TextInfo Request
`The response will return the following information for an ITF Version.

| Version Textinfo Element | Description |
| ------- | ---------------|
| `label` | The label used to identify the version. The label _MUST_ be unique within the containing Resource.|
| `date` | If a Resource has _date_ versioning, all versions _MUST_ have a unique date. Otherwise this is _OPTIONAL_. |
| `sequence` | If a Resource has _linear_ versioning, all versions _MUST_ have a unique sequence number. Otherwise this is _OPTIONAL_. |
| `succeeds` | Lists the label(s) of preceding versions if a Resource has _graph_ versioning. |
| `precedes` | Lists the label(s) of succeeding versions if a Resource has _graph_ versioning. |
| `modes` | Indicates which standard modes are supported. A list of one or more of _char, token, book, prose_. |
| `custom_modes` | _OPTIONAL_ List of custom modes that are supported. |
| `qualities` | Indicates which qualities are supported. A list of one or more of _raw, compact, plaintext, rich_. |
| `formats` | Indicates which formats are supported. A list of one or more of _txt, tei, html, md_. |
| `DC-metadata` | _OPTIONAL_ URL of a DC metadata record |

If a resource has _graph_ versioning, every version _MUST_ have a value  for at least one of the "succeeds", "precedes" keys.
If a "sequence" key exists, it _MUST_ contain a dotted numeric version number, for example: "1", "1.1", "1.1.12" which are 
collated in numerical order with priority to the right (_i.e_ 1.1.12 comes after 1.1.9 comes after 1.1). A "sequence" key _MUST NOT_ 
have a trailing dot.

IMPLEMENTATION NOTE: If a Resource does not have _date_ versioning, resources may still have a "date" key but it does
not have to be unique or ISO 8601 ordered. Versions with non-Gregorian or vague dating can be ordered 
by using _linear_ or _graph_ versioning. 

> DISCUSSION POINT: Do we need to return details of non-standard dating somewhere? Would support for ISO 8601 extensions for 
> vague dates/date intervals be useful, albeit at the expense of rather more complex ordering and validation?

Example JSON response for a _graph_ ordered version.
```
{
  "label" : "Second Version",
  "date" : "ca. 35 BCE",
  "succeeds" : "First Version",
  "precedes" : "Third Version",
  "modes" : [ "char", "token", "book" ],
  "qualities" : ["compact", "plaintext"],
  "formats" : [ "txt" ] 
} 
```
### 3.5 Versions Request
A versions request for a resource enumerates all the versions contained within a resource, and their relationships to each other. 
The "versioning" parameter determines how versions are ordered. For compactness, it does not return all the information about each
version but merely enumerates the version structure of the resource.

| Versions Element | Description|
| ------- | ---------------|
| `identifier` | The unique identifier of the Resource, expressed as a string. The identifier _MUST_ be supplied without URI encoding. |
| `date` | The date of the edition of the resource in ISO 8601 format |
| `versioning` | Indicates of a Resource contain multiple versions, and, if so, how they are ordered. Possible values: _none, linear, date, graph_ |
| `first_version` | Indicates the label of the earliest (or only) version of the text in the Resource. |
| `versions` | A keyed list of all the versions in this edition of this resource, if more the one version is present. This element _MUST NOT_ appear if there is only one text in the resource. Each version is described by a single version node in the list, keyed by the version label. |

DISCUSSION POINT: Is a very large number of versions a case we need to consider?

| Version Node Element | Description|
| ------- | ---------------|
| `date` | If a Resource has _date_ versioning, all versions _MUST_ have a unique date. Otherwise this is _MUST_ be omitted. |
| `sequence` | If a Resource has _linear_ versioning, all versions _MUST_ have a unique sequence number. Otherwise this _MUST_ be omitted. |
| `succeeds` | If a Resource has _graph_ versioning, lists the label(s) of preceding versions. Otherwise this _MUST_ be omitted.|
| `precedes` | If a Resource has _graph_ versioning, lists the label(s) of succeeding versions. Otherwise this _MUST_ be omitted.|

Example JSON response for _linear_ versioning.
```
{
  "identifier" : "1E34750D-38DB-4825-A38A-B60A345E591C",
  "date" : "2023-11-24"
  "versioning" : "linear",
  "first_version" : "First Version",
  "versions" : {
    "First Version" : {
      "sequence" : "1"
    },
    "Second Version" : {
      "sequence" : "3"
    },
    "Third Version" : {
      "sequence" : "3"
    }
  }
}
```
Example JSON response for _date_ versioning.
```
{
  "identifier" : "1E34750D-38DB-4825-A38A-B60A345E591C",
  "date" : "2023-11-24"
  "versioning" : "date",
  "first_version" : "First Version",
  "versions" : {
    "First Version" : {
      "date" : "2022-05-07"
    },
    "Second Version" : {
      "date" : "2023-01-06"
    },
    "Third Version" : {
      "date" : "2023-11-24"
    }
  }
}
```
Example JSON response for _graph_ versioning.
```
{
  "identifier" : "1E34750D-38DB-4825-A38A-B60A345E591C",
  "date" : "2023-11-24"
  "versioning" : "graph",
  "first_version" : "First Version",
  "versions" : {
    "First Version" : {
      "precedes" : "Second Version"
    },
    "Second Version" : {
      "succeeds" : "First Version",
      "precedes" : "Third Version"
    },
    "Third Version" : {
      "succeeds" : "Second Version",
    }
  }
}
```

### 3.6 Resource Modes Request
A Resource Modes Request provides information about the modes supported by a particular Resource. 
In particular, it describes, at a high level, any custom modes that that are used in the Resource. How a 
particular mode relates in detail to a particular version will require a [Version Mode Request](#254-version-mode-request). 

The response will return the following information.

| Resource Modes Element | Description|
| ------- | ---------------|
| `identifier` | The unique identifier of the Resource, expressed as a string. The identifier _MUST_ be supplied without URI encoding. |
| `date` | The date of the most recent edition of the Resource in ISO 8601 format. |
| `modes` | Indicates which standard modes are supported. A list of one or more of _char, token, book, prose_. |
| `custom_modes` | _OPTIONAL_ List of custom modes that are supported. |
| `custom_mode_definitions` | A keyed list of all the custom modes in this edition of this resource. This element _MUST NOT_ appear if there are no custom modes in the resource. Each custom mode is described by a set of custom mode elements detailed below. |

| Custom Mode Element | Description|
| ------- | ---------------|
| `description` | A description of what the mode represents |
| `base_mode` | Indicates the underlying mode used to define fragments in the custom mode, usually one of _char, token_. The most fine grained coordinate in the custom mode should be the same as the base mode. |
| `num_coordinates` | An integer that represents the number of coordinates used by the mode |
| `coordinates` | A list of each of the coordinates in the custom mode, keyed by the ordinal number of the coordinate counting from the left. Each coordinate is described by a set of coordinate elements detailed below. |

| Coordinate Element | Description|
| ------- | ---------------|
| `label` | A short label for the coordinate used for display purposes. Labels _MUST_ be unique within a mode. |
| `description` | An _OPTIONAL_ description of what the coordinate represents. |
| `levels` | Indicates the number of levels the coordinate allows for dotted multiplet values. If it is "1" then only a single numerical value is permitted. |

For example, a Resource containing a long form prose work might define a "prose" mode 
comprising sections, blocks, sentences and tokens as follows.
```
{
  "identifier" : "1E34750D-38DB-4825-A38A-B60A345E591C",
  "date" : "2023-11-24"
  "modes" : [ "char", "token", "book"],
  "custom_modes" : [ "prose" ],
  "custom_mode_definitions" : {
    "prose" : {
      "description" : "A simple semantic structure for a long form prose work",
      "base_mode" : "token",
      "num_coordinates" : "5",
      coordinates : {
        "1" : {
          "label" : "section",
          "description: : "Chapter-like elements (inc. forwards, appendices etc.)",
          "levels" : "1"
        },
        "2" : {
          "label" : "block",
          "description: : "Paragraph and sub-paragraph elmenets",
          "levels" : "2"
        },
        "3" : {
          "label" : "sentence",
          "description: : "Sentences, glossary items etc.",
          "levels" : "1"
        },
        "4" : {
          "label" : "token",
          "description: : "This is the same as the _base_mode_",
          "levels" : "1"
        }
      }
    }
  }
}
```
Coordinates for this mode are thus of the form _S;b;s;t_ corresponding to token _t_ of sentence _s_ of block _b_ of section _S_. Following 
the rules for hierarchical modes, we can define the following (non-exhaustive) list of fragments using this mode:

| Form of Prose Fragment Parameter | Description |
| ------------------- | ------------ |
|`S1,S2`| The fragment extends from the first character of section S1 until just after the last character of section S2. |
|`S1;b1;s1;t1,S2;b2;s2;t2`| The fragment starts from the start of token t1, of sentence s1, of block b1 of section S1 and ends after token2, of sentence s2 of block b2, of section S2. |
|`,S2`| The fragment extends from the beginning of the text, to just after the last character of section S2. |
|`,S2;b2;s2`| The fragment starts at the beginning of the text, and ends just after the last character of sentence s2, of block b2. of section S2. |
|`S1;b1+b2`| The fragment extends from the start of block b1 of section S1, and includes the following p2 blocks. |
|`S1;b1;s1+s2`| The fragment starts from the start of sentence s1, of block b1, of section S1, and includes the following s2 sentences. |
|`S1`| The fragment is the whole of section S1. |
|`S1;b1`| The fragment is the whole of block b1 of section S1.|

Custom mode definitions are limited in scope to a single resource, however, it is _RECOMMENDED_ that custom mode definitions
are designed to be reusable across a wide range of resources if possible.  

### 3.7 Version Modes Request
A Version Modes Request describes in detail how the mode coordinates map to the underlying text. This can be described at two levels of detail:
- Basic level: assigns labels to the higher levels of a mode to provide a navigational or structural map of the text
- Detailed level: describe all the levels of a mode in terms of _base_mode_ coordinates  
 



## 4 Accessing Previous Editions
An ITF text resource MAY change over time, allowing updates, corrections or the addition 
of new versions of a text. In ITF terminology, each change constitutes a new "edition" 
of the text resource. In order to resolve text references correctly, an ITF-compliant text
service that supports multiple editions MUST provide a mechanism for accessing these earlier 
editions.

> DISCUSSION POINT: The proposed mechanism for doing this would be to implement at least some of
> [RFC7089](https://datatracker.ietf.org/doc/html/rfc7089)

