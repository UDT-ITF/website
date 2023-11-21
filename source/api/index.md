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

### 1.2 The ITF Text Model
An ITF text resource 

> Needs considerable expansion
> Explain /"editions/" of a resource - which are updates or changes
> Explain /"versions/" of a text - which may be a linear sequence, a hierarchy, or a graph
>> Versions MUST have a label, MAY have a date (for a linear sequence) and or a number
>> (for a linear sequence or hierarchy). MUST have follows/precedes predicates if a Graph.
> Explain access /"modes/" - coordinate systems for texts - by character (Unicode codepoint),
>> by physical structure (page, line, character), by semantic structure (chapter, paragraph,
>> sentence, word, character)...and others     

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

### 2.2 URL Syntax
The ITF Text API can be called in two forms: one to request a text fragment,
and a second to request technical information about the underlying source text.
Both forms convey the request\'s information in the path segment of the URL,
rather than as query parameters. This makes responses more easily able
to be cached, either at the server or by standard web-caching infrastructure. 

To allow for extension, this specification does not define the behaviour
of an implementing server when it receives requests that do not match
one of the two request syntaxes below.

#### 2.2.1 Text Fragment Request URL Syntax
The ITF Text API URL for requesting a text fragment MUST conform to the following format:

    http[s]://server/[prefix/]identifier/version/mode/fragment/quality[.format]

where \[\] delimits components which are optional.

The URI Template ([RFC6750](https://datatracker.ietf.org/doc/html/rfc6750)) form
is:

    http://{server}{/prefix}/{identifier}/{version}/{mode}/{fragment}/{quality}{.format}

For example:

    http://www.example.org/text-service/abcd1234/default/char/0,247/tei.xml

The sections of the Image Request URL include:

| Syntax | Description |
| ------------------- | ------------ |
|`http (or https)` | Indicates the use of the http or https protocol in calling the service. |
|`server` | The host server on which the ITF-compliant text service resides. |
|`prefix` | The path on the host server to the ITF-compliant text service. This prefix is optional, but may be useful when the host server supports multiple services. (note: The prefix MAY contain slashes or constructions that resemble service parameters.) |
|`identifier` | A unique identifier of the requested source text, expressed as a string. This may be an ark, URN, filename, or other unique identifier but ideally SHOULD be a Persistent Identifier. Special characters MUST be URI encoded. |
|`version, mode, fragment, quality, format` | Parameters defining the characteristics of the returned text fragment. These are described in detail below. |

> DISCUSSION POINT: Is this sufficient?

See [Section 2.4 - Fragment Request Parameters](#parameters).

#### 2.2.2 Text Information Request URL Syntax
The ITF Text API URL for requesting information about a text resource MUST conform to
the following format:

    http[s]://server/[prefix/]identifier[/version]/info.ext

where \[ \] delimits the optional prefix component, \"info\" is a
literal string, and ext is one of \"json\" or \"xml\".

The URI Template ([RFC6750](https://datatracker.ietf.org/doc/html/rfc6750)) form
is:

    http://{server}{/prefix}/{identifier}/[/version]info.{ext}

For example:

    http://www.example.org/text-service/abcd1234/info.json

#### 2.2.3 Accessing historical editions
An ITF text resource MAY change over time, allowing updates, corrections or the addition 
of new versions of a text. In ITF terminology, each change constitutes a new /"edition/" 
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

### 2.4 Text Fragment Request Parameters {#parameters)
All parameters described below are required for compliant construction of an ITF Text API
URL. The sequence of parameters in the URL MUST be in the order described below. The 
order of the parameters reflects the order of operation a text service is expected
to use to process a request. Thus, the desired version of a text is located, the relevant 
fragment is extracted using the mode specified, any formatting or tagging is applied,
and finally conversion to the desired format. This resulting text frgament is returned 
as the representation for the URL. 

#### 2.4.1 Version
An ITF text resource MAY contain multiple versions of a text which MUST be identified by a
label (a string) and MAY also have an associated date/time. The version parameter defines 
which version of text a fragment is to be retrieved from. The special value "default" MUST 
be used with a resource that contains no versions.

| Form of Version Parameter| Description |
| ------------------- | ------------ |
|`default` | Specifies the only text in an unversioned resource. MUST NOT be used with a versioned resource. |
|`vl:label` | Specifies the version with a label "label". Labels MUST be URI encoded. |
|`vd:yyyy-mm-dd`| Specifies the version current at a particular date in ISO 8601-1:2019 format. Negative (BCE) years are allowed. |
|`vd:yyyy-mm-ddThh:mm:ss`| Specifies the version current at a particular date/time in ISO 8601-1:2019 format. Negative (BCE) years are allowed. |

A server MUST return a 400 (bad request) code if 'default' is used with a versioned resource, or 
a date is specified for a resource with no version dates. 

#### 2.4.2 Mode

#### 2.4.3 Fragment
##### 2.4.3.1 Char Mode Fragments
##### 2.4.3.2 Book Mode Fragments
##### 2.4.3.3 Prose Mode Fragments 
