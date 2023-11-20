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
This is a DRAFT proposal for a Interopeable Text Framework specification for discussion.

## 2. Text API

### Abstract
This section describes an API for the delivery of texts and text fragments via a
standard http request. The ITF Text API specifies a web service that returns textual
data in response to a standard http or https request. The URL can specify:
-  The version of the underlying source text to be accessed
-  The method used to specify a text fragment 
-  The text fragment to be returned
-  The format in which to return the text data
A URL can also be constructed to request basic technical information about the source
text to support client applications. The ITF Text API is conceived to facilitate 
systematic referencing and reuse of textual resources in repositories. 

### 2.1 Audience
This document is intended for developers building applications that
either share textual resources or consume such resources for display, analytics or 
other purposes. This includes:

-   Developers building digital image repositories who would like to
    offer services for easily sharing digital images with other
    repositories, web applications, or other software that uses images
    from the web.
-   Developers building web applications or software that want to
    retrieve images from compliant repositories.
-   A specifically targeted audience are developers and managers of
    digital image repositories, web applications and image tools at
    cultural heritage institutions, like museums, libraries and
    archives.

## 3. Another section
## 4. Changelog
