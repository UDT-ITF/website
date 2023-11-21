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

## 1. Introduction
This is a DRAFT proposal for an Interoperable Text Framework specification for discussion.

### 1.1 Audience
This document is intended for developers building applications that either share 
textual resources or consume such resources for display, analytics or other 
purposes. The background use-cases for developing the APi are drawn from cultural 
heritage and research organisations but the use of the API is not restricted to these domains. 

## 2. Text API

### 2.1 Abstract
This section describes an API for the delivery of texts and text fragments via a
standard http request. The ITF Text API specifies a web service that returns textual
data in response to a standard http or https request. The URL can specify:

-  The version of the underlying source text to be accessed
-  The method used to specify a text fragment 
-  The text fragment to be returned
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
The ITF Text API URL for requesting an image MUST conform to the following format:

    http[s]://server/[prefix/]identifier/version/mode/fragment/quality[.format]

where \[\] delimits components which are optional.

The URI Template ([RFC6750](https://datatracker.ietf.org/doc/html/rfc6750)) form
is:

    http://{server}{/prefix}/{identifier}/{version}/{mode}/{quality}{.format}

For example:

    http://www.example.org/text-service/abcd1234/default/raw/0,247/tei.xml

The sections of the Image Request URL include:

| Syntax | Description |
| ------------------- | ------------ |
  |`http (or https)`   |                        Indicates the use of the http or https protocol in calling the service. |
  |`server` |  The host server on which the ITF-compliant image service resides. |
  |`prefix` | The path on the host server to the ITF-compliant image service. This prefix is optional, but may be useful when the host server supports multiple services. (note: The prefix MAY contain slashes or constructions that resemble service parameters.) |
  |`identifier` |                                A unique identifier of the requested source text, expressed as a string. This may be an ark, URN, filename, or other unique identifier but ideally SHOULD be a Persistent Identifier. Special characters MUST be URI encoded.
  |`version, mode, quality, format` |   Parameters defining the characteristics of the returned text fragment. These are described in detail below.|
{: .api-table}

See [Section 2.5 - Fragment Request Parameters](#parameters).

#### 2.2.2 Text Information Request URL Syntax

## 3. Another section
## 4. Changelog
