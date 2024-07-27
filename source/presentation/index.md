---
title: "UDT-ITF PRESENTATION API 0.1"
title_override: "UDT-ITF PRESENTATION API 0.1"
id: text-api
layout: spec
tags: [specifications, presentation-api]
major: 0
minor: 1
patch: 0
pre: beta # change to "final" when stable
cssversion: 0
redirect_from:
  - /presentation/0.1.0/index.html
editors:
  - name: Neil Jefferies
    ORCID: https://orcid.org/0000-0003-3311-3741
    institution: Bodleian Libraries (University of Oxford)
  - name: Michael Hawkins
    ORCID: https://orcid.org/0000-0003-4306-7515
    institution: Cambridge Digital Humanities (University of Cambridge) & Digital Innovation and Development (Cambridge University Library)
  - name: Robert Ralley
    ORCID: https://orcid.org/0000-0002-9769-754X
    institution: Cambridge Digital Humanities (University of Cambridge)
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

##  1. Introduction

This is a DRAFT proposal for an Interoperable Text Framework (ITF) presentation API specification for discussion. Its base text was derived from the [IIIF Presentation API](https://github.com/IIIF/api).

### 1.1 Audience

This document is intended for developers building applications that share 
textual resources for display, computational analysis or any other purpose. 
The background use-cases for developing the API are drawn from cultural heritage and research organisations but the use of the API is not restricted to these domains. 

### 1.2 Terminology

The key words _MUST_, _MUST NOT_, _REQUIRED_, _SHALL_, _SHALL NOT_, _SHOULD_, _SHOULD NOT_, _RECOMMENDED_, and _OPTIONAL_ in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

##  2. Resource Type Overview

ITF’s objectives require a model in which one can characterize the compound object (via the _Manifest_ resource) and the individual views of the object (_TextFrame_ resources). A compound object may also have sections; for example, a book may have chapters of several pages, or a play might be divided into acts and scenes (_Range_ resources) and there may be groups of such objects (_Collection_ resources). These resource types, along with their properties, make up the ITF Presentation API.

This section provides an overview of the resource types (or classes) that are used in the specification. They are each presented in more detail in [Section 5][prezi30-resource-structure].

### 2.1. Defined Types

This specification defines the following resource types:

##### Collection
{: #overview-collection}

An ordered list of Manifests, and/or further Collections. Collections allow Manifests and child Collections to be grouped in a hierarchical structure for presentation, which can be for generating navigation, showing dynamic results from a search, or providing fixed sets of related resources for any other purpose.

##### Manifest
{: #overview-manifest}

A description of the structure and properties of the compound object. It carries information needed for the client to present the content to the user, such as a title and other descriptive information about the object or the intellectual work that it conveys. Each Manifest usually describes how to present a single ‘text’. While this text may, of course, represent an entire document, such as a book, manuscript, or any portion thereof.

##### TextFrame
{: #overview-textframe}

A virtual container that represents a particular view of the object and has content resources associated with it or with parts of it. The TextFrame provides a frame of reference for the layout of the content, both spatially and temporally. The name of a TextFrame is derived from classic letterpress typesetting where individual letter blocks are arranged within a frame before being inked and pressed on physical sheets. The TextFrame functions analogously to IIIF’s Canvas, providing the container onto which other content is placed.

##### Range
{: #overview-range}

An ordered list of TextFrames, and/or further Ranges. Ranges allow TextFrames to be grouped together in some way. It could be for content-based reasons, such as might be described in a table of contents or the set of scenes in a play. Or, it could be for reasons due to the physical features of the original item, such as page gatherings in a manuscript or printed book. Obviously, texts can be grouped together for any number of reasons that have nothing to due with the physicality of the object.

### 2.2. Additional Types

This specification makes use of types defined in the [Web Annotation Data Model][org-w3c-webanno] specification, in particular the following:

##### Annotation Page
{: #overview-annotationpage}

An ordered list of Annotations that is typically associated with a TextFrames but may be referenced from other types of resource. Annotation Pages collect and order lists of Annotations that provide commentary about a resource or content that is part of a TextFrames.

##### Annotation
{: #overview-annotation}

Annotations associate content resources with TextFrames. The same mechanism is used for all types of content, such as comments, tags, textual notes/enrichments, other textual resources, images, audios or video. This provides a single, unified method for aligning information, and provides a standards-based framework for distinguishing parts of resources and parts of TextFrames. As Annotations can be added later, it promotes a distributed system in which publishers can align their content with the descriptions created by others. Annotation-related functionality may also rely on further classes such as SpecificResource, Choice or Selectors.

##### Content 
{: #overview-content}

Web resources such as text, images, audio or video that are associated with a TextFrames via an Annotation, or provide a representation of any resource.

##### Annotation Collection
{: #overview-annotationcollection}

An ordered list of Annotation Pages. Annotation Collections allow higher level groupings of Annotations to be recorded. For example, all the English translation Annotations of a medieval French document could be kept separate from the transcription or an edition in modern French, or the director's commentary on a film can be separated from the script.

##  3. Resource Properties

Most properties defined in this specification may be associated with any of the resource types described above, and may have more than one value. Properties relate to the resource with which they are associated. For example, the `label` property on a Manifest is the human-readable label of the Manifest; the `label` property on a TextFrame is the human-readable label for that TextFrame.

The requirements for which properties each class contains are summarized in [Appendix A][prezi30-appendixa]. 

This section also defines processing requirements for clients for each of the combinations of class and property. Any properties that a client does not understand or are encountered in an undocumented context  _MUST_ be ignored. See the [Linked Data Context and Extensions][prezi30-ldce] section for more information about extensions.

 These requirements are for general purpose client implementations that are intended to be used to render the entire resource to the user, and not necessarily for consuming applications with specialized use or individual component implementations that might be used to construct a client. The inclusion of these requirements gives publishers a baseline expectation for how they can expect implementations advertised as compliant with this specification to behave when processing their content.

###  3.1. Descriptive Properties

These properties describe or represent the resource they are associated with, and are typically rendered to the user.

##### label

A human-readable label, name or title. The `label` property is intended to be displayed as a short, textual surrogate for the resource if a human needs to make a distinction between it and other similar resources. The `label` property can be internationalized, and each language can have multiple values. This pattern is described in more detail in the [languages][prezi30-languages] section.

The value of the property _MUST_ be a JSON object, as described in the [languages][prezi30-languages] section.

 * A Collection _MUST_ have the `label` property with at least one entry.<br/>
   Clients _MUST_ render `label` on a Collection.
 * A Manifest _MUST_ have the `label` property with at least one entry.<br/>
   Clients _MUST_ render `label` on a Manifest.
 * A TextFrame _SHOULD_ have the `label` property with at least one entry.<br/>
   Clients _MUST_ render `label` on a TextFrame, and _SHOULD_ generate a `label` for TextFrames that do not have them.
 * A content resource _MAY_ have the `label` property with at least one entry. If there is a Choice of content resource for the same TextFrame, then they _SHOULD_ each have at least a `label` property with at least one entry.<br/>
   Clients _MAY_ render `label` on content resources, and _SHOULD_ render them when part of a Choice.
 * A Range _SHOULD_ have the `label` property with at least one entry. <br/>
   Clients _MUST_ render `label` on a Range.
 * An Annotation Collection _SHOULD_ have the `label` property with at least one entry.<br/>
   Clients _SHOULD_ render `label` on an Annotation Collection.
 * Other types of resource _MAY_ have the `label` property with at least one entry.<br/>
   Clients _MAY_ render `label` on other types of resource.

``` json-doc
{
    "label": {
        "en": ["Example Title of Text"]
    }
}
```

##### metadata

An ordered list of descriptions to be displayed to the user when they interact with the resource, given as pairs of human-readable `label` and `value` entries. The content of these entries is intended for presentation only; descriptive semantics _SHOULD NOT_ be inferred. An entry might be used to convey information about the creation of the object, a physical description, ownership information, or other purposes.

The value of the `metadata` property _MUST_ be an array of JSON objects, where each item in the array has both `label` and `value` properties. The values of both `label` and `value` _MUST_ be JSON objects, as described in the [languages][prezi30-languages] section.

 * A Collection _SHOULD_ have the `metadata` property with at least one item. <br/>
   Clients _MUST_ render `metadata` on a Collection.
 * A Manifest _SHOULD_ have the `metadata` property with at least one item.<br/>
   Clients _MUST_ render `metadata` on a Manifest.
 * A TextFrame _MAY_ have the `metadata` property with at least one item.<br/>
   Clients _SHOULD_ render `metadata` on a TextFrame.
 * Other types of resource _MAY_ have the `metadata` property with at least one item.<br/>
   Clients _MAY_ render `metadata` on other types of resource.

Clients _SHOULD_ display the entries in the order provided. Clients _SHOULD_ expect to encounter long texts in the `value` property, and render them appropriately.

``` json-doc
{
    "metadata": [
        {
            "label": {
                "en": ["Author"]
            },
            "value": {
                "en": ["Mr Gerence James [Marks]"]
            }
        }
    ]
}
```

##### summary

A short textual summary intended to be conveyed to the user. This could be used as a brief description for item-level search results, small-screen environments, or as an alternative user interface when the `metadata` property is not currently rendered. The `summary` property follows the same pattern as the `label` property described above.

The value of the property _MUST_ be a JSON object, as described in the [languages][prezi30-languages] section.

 * A Collection _SHOULD_ have the `summary` property with at least one entry.<br/>
   Clients _SHOULD_ render `summary` on a Collection.
 * A Manifest _SHOULD_ have the `summary` property with at least one entry.<br/>
   Clients _SHOULD_ render `summary` on a Manifest.
 * A TextFrame _MAY_ have the `summary` property with at least one entry.<br/>
   Clients _SHOULD_ render `summary` on a TextFrame.
 * Other types of resource _MAY_ have the `summary` property with at least one entry.<br/>
   Clients _MAY_ render `summary` on other types of resource.

``` json-doc
{
    "summary": {
        "en": ["A case from 19 March 1596 at 09:30 concerning Roger George, recorded by Dr Simon Forman in MS Ashmole 234, f. 3v (upper left part of page)."]
    }
}
```

##### requiredStatement

Text that _MUST_ be shown when the resource is displayed or used. For example, the `requiredStatement` property could be used to present copyright or ownership statements, an acknowledgement of the owning and/or publishing institution, or any other text that the publishing organization deems critical to display to the user. Given the wide variation of potential client user interfaces, it will not always be possible to display this statement to the user in the client's initial state. If initially hidden, clients _MUST_ make the method of revealing it as obvious as possible.

The value of the property _MUST_ be a JSON object, that has the `label` and `value` properties, just like a `metadata` property entry. The values of both `label` and `value` _MUST_ be JSON objects, as described in the [languages][prezi30-languages] section.

 * Any resource type _MAY_ have the `requiredStatement` property.<br/>
   Clients _MUST_ render `requiredStatement` on every resource type.

``` json-doc
{
    "requiredStatement": {
        "label": {
            "en": ["Attribution"]
        },
        "value": {
            "en": ["Lauren Kassell, Michael Hawkins, Robert Ralley, John Young, Joanne Edge, Janet Yvonne Martin-Portugues, and Natalie Kaoukji (eds.), ‘CASE9026’, The casebooks of Simon Forman and Richard Napier, 1596–1634: a digital edition, https://casebooks.lib.cam.ac.uk/case/CASE9026, accessed 18 July 2024."]
        }
    }
}
```

##### rights

A string that identifies a license or rights statement that applies to the content of the resource, such as the JSON of a Manifest or the text in the TextFrame. The value _MUST_ be drawn from the set of [Creative Commons][org-cc-licenses] license URIs, the [RightsStatements.org][org-rs-terms] rights statement URIs, or those added via the [extension][prezi30-ldce] mechanism. The inclusion of this property is informative.

If displaying rights information directly to the user is the desired interaction, or a publisher-defined label is needed, it is _RECOMMENDED_ to include the information using the `requiredStatement` property or in the `metadata` property.

The value _MUST_ be a string. If the value is drawn from Creative Commons or RightsStatements.org, then the string _MUST_ be a URI defined by that specification.

 * Any resource type _MAY_ have the `rights` property.<br/>
   Clients _MAY_ render `rights` on any resource type.

``` json-doc
{ "rights": "https://creativecommons.org/licenses/by/4.0/" }
```

__Machine actionable URIs and links for users__<br/>
The machine actionable URIs for both Creative Commons licenses and RightsStatements.org right statements are `HTTP(S)` URIs. Human-readable descriptions are available from URIs to either sites. 

##### provider

An organization or person that contributed to providing the content of the resource. Clients can then display this information to the user to acknowledge the provider's contributions. This differs from the `requiredStatement` property, in that the data is structured, offering the clients richer information about the people and organizations to use in different interfaces.

The organization or person is represented as an `Agent` resource.

* Agents _MUST_ have the `id` property, and its value _MUST_ be a string. The string _MUST_ be a URI that identifies the agent.
* Agents _MUST_ have the `type` property, and its value _MUST_ be the string "Agent".
* Agents _MUST_ have the `label` property, and its value _MUST_ be a JSON object as described in the [languages][prezi30-languages] section.
* Agents _SHOULD_ have the `homepage` property, and its value _MUST_ be an array of JSON objects as described in the [homepage][prezi30-homepage] section.
* Agents _SHOULD_ have the `logo` property, and its value _MUST_ be an array of JSON objects as described in the [logo][prezi30-logo] section.
* Agents _MAY_ have the `seeAlso` property, and its value _MUST_ be an array of JSON object as described in the [seeAlso][prezi30-seealso] section.

The value _MUST_ be an array of JSON objects, where each item in the array conforms to the structure of an Agent, as described above.

 * A Collection _SHOULD_ have the `provider` property with at least one item. <br/>
   Clients _MUST_ render `provider` on a Collection.
 * A Manifest _SHOULD_ have the `provider` property with at least one item. <br/>
   Clients _MUST_ render `provider` on a Manifest.
 * Other types of resource _MAY_ have the `provider` property with at least one item. <br/>
   Clients _SHOULD_ render `provider` on other types of resource.

``` json-doc
{
  "provider": [
    {
      "id": "https://example.org/about",
      "type": "Agent",
      "label": { "en": [ "Example Organization" ] },
      "homepage": [
        {
          "id": "https://example.org/",
          "type": "Text",
          "label": { "en": [ "Example Organization Homepage" ] },
          "format": "text/html"
        }
      ],
      "logo": [
        {
          "id": "https://example.org/images/logo.png",
          "type": "Image",
          "format": "image/png",
          "height": 100,
          "width": 120
        }
      ],
      "seeAlso": [
        {
          "id": "https://data.example.org/about/us.jsonld",
          "type": "Dataset",
          "format": "application/ld+json",
          "profile": "https://schema.org/"
        }
      ]
    }
  ]
}
```

##### navDate

A date that clients may use for navigation purposes when presenting the resource to the user in a date-based user interface, such as a calendar or timeline. More descriptive date ranges, intended for display directly to the user, *SHOULD* be included in the `metadata` property for human consumption.

The value _MUST_ be an [XSD dateTime literal][org-w3c-xsd-datetime]. The value _MUST_ have a timezone, and _SHOULD_ be given in UTC with the `Z` timezone indicator, but _MAY_ instead be given as an offset of the form `+hh:mm`.

The use of an XSD dateTime literal poses challenges for historical materials or those dated according to different calendars. Prior to 1752 England used the Julian Calendar. Dates on the Julian calendar could be off by almost a fortnight from the Gregorian. To further complicate things, Lady Day (25 March) and not 1 Jan was considered the start of the New Year. Finally, there’s the fact that timezones are a late nineteenth-century invention.

At present, it’s recommended that these difficulties are glossed over in `navDate` since it is intended to provide a value to sort items chronologically. These difficulties are usually dealt with by domain experts when creating the textual resource and should be recorded using an appropriate textual form of the date in the `metadata` property. For example, documents written in England after 1 January and before Lady Day prior to 1752 were generally written with split years: ’19 January 1601/2’. In most instances, the most pragmatic solution would be to use the Julian date for the navDate: `1602-01-19:00:00:00Z’. If a collection of texts involved materials dated in multiple calendars, such as a collection of 17th-century letters to and from the continent and England, it would likely be worthwhile to convert the Julian dates into Gregorian so that the letters appear in the correct order. 

 * A Collection _MAY_ have the `navDate` property.<br/>
   Clients _MAY_ render `navDate` on a Collection.
 * A Manifest _MAY_ have the `navDate` property.<br/>
   Clients _MAY_ render `navDate` on a Manifest.
 * A Range _MAY_ have the `navDate` property.<br/>
   Clients _MAY_ render `navDate` on a Range.
 * A TextFrame _MAY_ have the `navDate` property.<br/>
   Clients _MAY_ render `navDate` on a TextFrame.
 * Other types of resource _MUST NOT_ have the `navDate` property.<br/>
   Clients _SHOULD_ ignore `navDate` on other types of resource.

``` json-doc
{ "navDate": "1602-01-19T00:00:00Z" }
```

###  3.2. Technical Properties

These properties describe technical features of the resources, and are typically processed by the client to understand how to render the resource.

##### id

The URI that identifies the resource. If the resource is only available embedded within another resource (see the [terminology section][prezi30-terminology] for an explanation of "embedded"), such as a Range within a Manifest, then the URI _MAY_ be the URI of the embedding resource with a unique fragment on the end. TextFrames, however, _MUST_ have their own URI formatted according to the [ITF Text Fragment API](https://textframe.io/api/).

The value _MUST_ be a string referring to the HTTP(S) URI for resources defined in this specification. If the resource is retrievable via HTTP(S), then the URI _MUST_ be the URI at which it is published. External resources, such as profiles, _MAY_ have non-HTTP(S) URIs defined by other communities.

The existence of an HTTP(S) URI in the `id` property does not mean that the URI will always be dereferencable. If the resource with the `id` property is [embedded][prezi30-terminology], it _MAY_ also be dereferenceable. If the resource is referenced (again, see the [terminology section][prezi30-terminology] for an explanation of "referenced"), it _MUST_ be dereferenceable. The [definitions of the Resources][prezi30-resource-structure] give further guidance.

 * All resource types _MUST_ have the `id` property.<br/>
   Clients _MAY_ render `id` on any resource type, and _SHOULD_ render `id` on Collections, Manifests and TextFrames.

``` json-doc
{ "id": "https://example.org/itf/1/manifest" }
```

##### type

The type or class of the resource. For classes defined for this specification, the value of `type` will be described in the sections describing each individual class below.

For content resources, the value of `type` is drawn from other specifications. Recommendations for common content types such as text, image, or audio are given in the table below.

The JSON objects that appear in the value of the `service` property will have many different classes, and can be used to distinguish the type of service, with specific properties defined in a [registered context document][prezi30-ldce].

The value _MUST_ be a string.

 * All resource types _MUST_ have the `type` property.<br/>
   Clients _MUST_ process, and _MAY_ render, `type` on any resource type.

| Class         | Description                                                                                                         |
| ------------- |---------------------------------------------------------------------------------------------------------------------|
| `Dataset`     | Data not intended to be rendered to humans directly                                                                 |
| `Image`       | Two dimensional visual resources primarily intended to be seen, such as might be rendered with an &lt;img> HTML tag |
| `Model`       | A three (or more) dimensional model intended to be interacted with by humans                                        |
| `Sound`       | Auditory resources primarily intended to be heard, such as might be rendered with an &lt;audio> HTML tag            |
| `Text`        | Resources primarily intended to be read                                                                             |
| `Video`       | Moving images, with or without accompanying audio, such as might be rendered with a &lt;video> HTML tag             |

``` json-doc
{ "type": "Dataset" }
```

##### format

The specific media type (often called a MIME type) for a content resource, for example `text/html`. This is important for distinguishing different formats of the same overall type of resource, such as distinguishing text in XML from html or plain text.

The value _MUST_ be a string, and it _SHOULD_ be the value of the `Content-Type` header returned when the resource is dereferenced.

 * A content resource _SHOULD_ have the `format` property.<br/>
   Clients _MAY_ render the `format` of any content resource.
 * Other types of resource _MUST NOT_ have the `format` property.<br/>
   Clients _SHOULD_ ignore `format` on other types of resource.

``` json-doc
{ "format": "application/xml" }
```

##### language

The language or languages used in the content of this external resource. This property is already available from the Web Annotation model for content resources that are the body or target of an Annotation, however it _MAY_ also be used for resources [referenced][prezi30-terminology] from `homepage`, `rendering`, and `partOf`.

The value _MUST_ be an array of strings. Each item in the array _MUST_ be a valid language code, as described in the [languages section][prezi30-languages].

 * An external resource _SHOULD_ have the `language` property with at least one item.<br/>
   Clients _SHOULD_ process the `language` of external resources.
 * Other types of resource _MUST NOT_ have the `language` property.<br/>
   Clients _SHOULD_ ignore `language` on other types of resource.

``` json-doc
{ "language": [ "en" ] }
```

##### profile

A schema or named set of functionality available from the resource. The profile can further clarify the `type` and/or `format` of an external resource or service, allowing clients to customize their handling of the resource that has the `profile` property.

The value _MUST_ be a string, either taken from the [profiles registry][registry-profiles] or a URI.

* Resources [referenced][prezi30-terminology] by the `seeAlso` or `service` properties _SHOULD_ have the `profile` property.<br/>
  Clients _SHOULD_ process the `profile` of a service or external resource.
* Other types of resource _MAY_ have the `profile` property.<br/>
  Clients _MAY_ process the `profile` of other types of resource.

``` json-doc
{ "profile": "https://casebooks.lib.cam.ac.uk/about-us/editorial-and-tagging-guidelines" }
```

##### duration

The duration of the external content resource or TextFrame, given in seconds.

The value _MUST_ be a positive floating point number.

 * A TextFrame _MAY_ have the `duration` property.<br/>
   Clients _MUST_ process `duration` on a TextFrame.
 * Content resources _SHOULD_ have the `duration` property, if appropriate to the resource type.<br/>
   Clients _SHOULD_ process `duration` on content resources.
 * Other types of resource _MUST NOT_ have a `duration`.<br/>
   Clients _SHOULD_ ignore `duration` on other types of resource.

``` json-doc
{ "duration": 125.0 }
```

##### viewingDirection

The direction in which a set of TextFrames _SHOULD_ be displayed to the user. It refers to the order in which multiple TextFrames are arranged and _not_ to the order of the text that they might contain. This specification defines four direction values in the table below. Others may be defined externally [as an extension][prezi30-ldce].

The value _MUST_ be a string.

 * A Collection _MAY_ have the `viewingDirection` property.<br/>
   Clients _SHOULD_ process `viewingDirection` on a Collection.
 * A Manifest _MAY_ have the `viewingDirection` property.<br/>
   Clients _SHOULD_ process `viewingDirection` on a Manifest.
 * A Range _MAY_ have the `viewingDirection` property.<br/>
   Clients _MAY_ process `viewingDirection` on a Range.
 * Other types of resource _MUST NOT_ have the `viewingDirection` property.<br/>
   Clients _SHOULD_ ignore `viewingDirection` on other types of resource.

| Value           | Description                                                               |
|-----------------|---------------------------------------------------------------------------|
| `left-to-right` | The object is displayed from left to right. The default if not specified. |
| `right-to-left` | The object is displayed from right to left.                               |
| `top-to-bottom` | The object is displayed from the top to the bottom.                       |
| `bottom-to-top` | The object is displayed from the bottom to the top.                       |
{: .api-table #table-direction}

``` json-doc
{ "viewingDirection": "left-to-right" }
```

> I'm fascinated by this section and this is a notion that might well prove useful, but as it stands I'm not sure there's anything here for us.
> > Me too. I'm generally leaving most of it as is - apart from attempting to deal with the more image-specific bits.

##### behavior

A set of user experience features that the publisher of the content would prefer the client to use when presenting the resource. This specification defines the values in the table below. Others may be defined externally as an [extension][prezi30-ldce].

In order to determine the behaviors that are governing a particular resource, there are four inheritance rules from resources that reference the current resource:
* Collections inherit behaviors from their referencing Collection.
* Manifests **DO NOT** inherit behaviors from any referencing Collections.
* TextFrames inherit behaviors from their referencing Manifest, but **DO NOT** inherit behaviors from any referencing Ranges, as there might be several with different behaviors.
* Ranges inherit behaviors from any referencing Range and referencing Manifest.

Clients should interpret behaviors on a Range only when that Range is selected or is in some other way the context for the user's current interaction with the resources. A Range with the `behavior` value `continuous`, in a Manifest with the `behavior` value `paged`, would mean that the Manifest's TextFrames should be rendered in a paged fashion, unless the range is selected to be viewed, and its included TextFrames would be rendered in that context only as being virtually stitched together. This might occur, for example, when an editor is making a 'sourcebook' that combines excerpts into a single text for use in classes.

> We might want to require that some sort of notice is displayed when stiching them together in this fashion - like an ellipsis when quoting. Otherwise, it would be possible toeasily create 'wicked bibles' (so to speak) of their own making. 
> > (I'm assuming we don't need to worry about quotations being inserted into sentences.) We could specify ellipses (U+2026 is a horizontal ellipsis in Unicode, but there's also a vertical equivalent, U+FE19) but we'll run into the problem that they're language-specific. E.g. In French for an omission you'd expect [...], with square brackets around them, and in Chinese and Japanese it seems you'd expect six dots, each one centred. So this would be a difficult thing to specify unless we just said: 'there should be a character or characters to indicate the omission, either an ellipsis or equivalent'. Or unless there's a list of options to be pointed at somewhere.

The descriptions of the behavior values also lists the other values they are disjoint with, meaning that the same resource _MUST NOT_ have both of two or more from that set. In order to determine which is in effect, the client _SHOULD_ follow the inheritance rules above, taking the value from the closest resource. The user interface effects of the possible permutations of non-disjoint behavior values are client dependent.

The value _MUST_ be an array of strings.

 * Any resource type _MAY_ have the `behavior` property with at least one item.<br/>
   Clients _SHOULD_ process `behavior` on any resource type.

> Concerning 'facing-pages' - do we want to add a property that allows the frame to be identified as a recto or verso? That'd avoid the need for including empty TextFrames for empty pages with no content. We should assume that empty TextFrames are possible since a collection might include not just text (of each page) but also a link to an image. So, the page flip mechanism could deal with a situation where a particular TextFrame has no text but does include the properties necessary to point to the page image._

| Value              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    | **Temporal Behaviors**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `auto-advance`     | Valid on Collections, Manifests, TextFrames, and Ranges that include or are TextFrames with at least the `duration` dimension. When the client reaches the end of a TextFrame, or segment thereof as specified in a Range, with a duration dimension that has this behavior, it _SHOULD_ immediately proceed to the next TextFrame or segment and render it. If there is no subsequent TextFrame in the current context, then this behavior should be ignored. When applied to a Collection, the client should treat the first TextFrame of the next Manifest as following the last TextFrame of the previous Manifest, respecting any `start` property specified. Disjoint with `no-auto-advance`. |
| `no-auto-advance`  | Valid on Collections, Manifests, TextFrames, and Ranges that include or are TextFrames with at least the `duration` dimension. When the client reaches the end of a TextFrame or segment with a duration dimension that has this behavior, it _MUST NOT_ proceed to the next TextFrame, if any. This is a default temporal behavior if not specified. Disjoint with `auto-advance`.                                                                                                                                                                                                                                                                                                                 |
| `repeat`           | Valid on Collections and Manifests that include TextFrames that at least contain a `duration` dimension. When the client reaches the end of the duration of the final TextFrame in the resource, and the `behavior` value `auto-advance` is also in effect, the client _SHOULD_ return to the first TextFrame, or segment of TextFrame, in the resource that has the `behavior` value `repeat` and start playing again. If the `behavior` value `auto-advance` is not in effect, the client _SHOULD_ render a navigation control for the user to manually return to the first TextFrame or segment. Disjoint with `no-repeat`.                                                                      |
| `no-repeat`        | Valid on Collections and Manifests that include TextFrames that at least contain a `duration` dimension. When the client reaches the end of the duration of the final TextFrame in the resource, the client _MUST NOT_ return to the first TextFrame, or segment of TextFrame. This is a default temporal behavior if not specified. Disjoint with `repeat`.                                                                                                                                                                                                                                                                                                                                        |
|                    | **Layout Behaviors**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `unordered`        | Valid on Collections, Manifests and Ranges. The TextFrames included in resources that have this behavior have no inherent order, and user interfaces _SHOULD_ avoid implying an order to the user. Disjoint with `individuals`, `continuous`, and `paged`.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `individuals`      | Valid on Collections, Manifests, and Ranges. For Collections that have this behavior, each of the included Manifests are distinct objects in the given order. For Manifests and Ranges, the included TextFrames are distinct views, and _SHOULD NOT_ be presented in a page-turning interface. This is the default layout behavior if not specified. Disjoint with `unordered`, `continuous`, and `paged`.                                                                                                                                                                                                                                                                                          |
| `continuous`       | Valid on Collections, Manifests and Ranges that include TextFrames. TextFrames included in resources with this behavior and with an appropriate rendering might display all of the TextFrames virtually stitched together, such as joining a series of excerpts or quotes. This behavior has no implication for audio resources. The `viewingDirection` of the Manifest will determine the appropriate arrangement of the TextFrames. Disjoint with `unordered`, `individuals` and `paged`.                                                                                                                                                                                                         |
| `paged`            | Valid on Collections, Manifests and Ranges that include TextFrames that break the text into separate pages. TextFrames included in resources that have this behavior represent views that _SHOULD_ be presented in a page-turning interface if one is available. The first TextFrame is a single view (the first recto) and thus the second TextFrame likely represents the back of the object in the first TextFrame. If this is not the case, see the `behavior` value `non-paged`. Disjoint with `unordered`, `individuals`, `continuous`, `facing-pages` and `non-paged`.                                                                                                                       |
| `facing-pages`     | Valid only on TextFrames that contain the text of a single page _and_ where TextFrames are provided for every recto and verso - even if the a particular frame contains no text because it is blank. TextFrames that have this behavior, in a Manifest that has the `behavior` value `paged`, _MUST_ be displayed by themselves, as they depict both parts of the opening. If all of the TextFrames are like this, then page turning is not possible, so simply use `individuals` instead. Disjoint with `paged` and `non-paged`.                                                                                                                                                                   |
| `non-paged`        | Valid only on TextFrames. TextFrames that have this behavior _MUST NOT_ be presented in a page turning interface, and _MUST_ be skipped over when determining the page order. This behavior _MUST_ be ignored if the current Manifest does not have the `behavior` value `paged`. Disjoint with `paged` and `facing-pages`.                                                                                                                                                                                                                                                                                                                                                                         |
|                    | **Collection Behaviors**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `multi-part`       | Valid only on Collections. Collections that have this behavior consist of multiple Manifests or Collections, which, together, form part of a logical whole or a contiguous set, such as multi-volume books or a set of journal issues. Clients might render these Collections as a table of contents or provide viewing interfaces that can easily advance from one member to the next. Disjoint with `together`.                                                                                                                                                                                                                                                                                   |
| `together`         | Valid only on Collections. A client _SHOULD_ present all of the child Manifests to the user at once in a separate viewing area with its own controls. Clients _SHOULD_ catch attempts to create too many viewing areas. This behavior _SHOULD NOT_ be interpreted as applying to the members of any child resources. Disjoint with `multi-part`.                                                                                                                                                                                                                                                                                                                                                    |
|                    | **Range Behaviors**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `sequence`         | Valid only on Ranges, where the Range is [referenced][prezi30-terminology] in the `structures` property of a Manifest. Ranges that have this behavior represent different orderings of the TextFrames listed in the `items` property of the Manifest, and user interfaces that interact with this order _SHOULD_ use the order within the selected Range, rather than the default order of `items`. Disjoint with `no-nav`.                                                                                                                                                                                                                                                                         |
| `no-nav`           | Valid only on Ranges. Ranges that have this behavior _MUST NOT_ be displayed to the user in a navigation hierarchy. This allows for Ranges to be present that capture unnamed regions with no interesting content, such as the set of blank pages at the beginning of a book, or dead air between parts of a performance, that are still part of the Manifest but do not need to be navigated to directly. Disjoint with `sequence`.                                                                                                                                                                                                                                                                |
|                    | **Miscellaneous Behaviors**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `hidden`           | Valid on Annotation Collections, Annotation Pages, Annotations, Specific Resources and Choices. If this behavior is provided, then the client _SHOULD NOT_ render the resource by default, but allow the user to turn it on and off. This behavior does not inherit, as it is not valid on Collections, Manifests, Ranges or TextFrames.                                                                                                                                                                                                                                                                                                                                                            |
{: .api-table #table-behavior}

``` json-doc
{ "behavior": [ "auto-advance", "individuals" ] }
```

###  3.3. Linking Properties

These properties are references or links between resources, and split into external references where the linked object is outside the ITF space, and internal references where the linked object is an ITF resource. Clients typically create a link to the resource that is able to be activated by the user, or interact directly with the linked resource to improve the user's experience.

#### 3.3.1. External Links

##### homepage

A web page that is about the text that has the `homepage` property. The web page is usually published by the organization responsible for the text, and might be generated by a content management system or other cataloging system. The resource _MUST_ be able to be displayed directly to the user. Resources that are related, but not home pages, _MUST_ instead be added into the `metadata` property, with an appropriate `label` or `value` to describe the relationship.

The value of this property _MUST_ be an array of JSON objects, each of which _MUST_ have the `id`, `type`, and `label` properties, _SHOULD_ have a `format` property, and _MAY_ have the `language` property.

 * Any resource type _MAY_ have the `homepage` property.<br/>
   Clients _SHOULD_ render `homepage` on a Collection, Manifest or TextFrame, and _MAY_ render `homepage` on other types of resource.

__Model Alignment__<br/>
Please note that this specification, like the IIIF specification from which it is derived, has stricter requirements about the JSON pattern used for the `homepage` property than the [Web Annotation Data Model][org-w3c-webanno]. The ITF requirements are compatible, but the home page of an Agent found might only have a URI, or it might be a JSON object with other properties. See the section on [collisions between contexts][prezi30-context-collisions] for more information.
{: .note}

``` json-doc
{
    "homepage": [
        {
            "id": "https://example.com/info/",
            "type": "Text",
            "label": {
                "en": ["Homepage for Example Text"]
            },
            "format": "text/html",
            "language": ["en"]
        }
    ]
}
```

##### logo 

A small image resource that represents the Agent resource it is associated with. The logo _MUST_ be rendered when the resource is displayed or used, without cropping, rotating or otherwise distorting the image.

When more than one logo is present, the client _SHOULD_ pick one of them, based on the information in the logo properties. For example, the client could select a logo of appropriate aspect ratio based on the `height` and `width` properties of the available logos. The client _MAY_ decide on the logo by inspecting properties defined as [extensions][prezi30-ldce].

The value of this property _MUST_ be an array of JSON objects, each of which _MUST_ have `id` and `type` properties, and _SHOULD_ have `format`. The value of `type` _MUST_ be "Image".

 * Agent resources _SHOULD_ have the `logo` property.<br/>
   Clients _MUST_ render `logo` on Agent resources.

``` json-doc
{
  "logo": [
    {
      "id": "https://example.org/img/logo.jpg",
      "type": "Image",
      "format": "image/jpeg",
      "height": 100,
      "width": 120
    }
  ]
}
```

##### service

A service that the client might interact with to gain additional information or functionality when using the resource with the `service` property. The service resource _SHOULD_ have additional information associated with it in order to allow the client to determine how to make appropriate use of it. Please see the [Service Registry][registry-services] document for the details of currently known service types.

The value _MUST_ be an array of JSON objects. Each object will have properties depending on the service's definition, but _MUST_ have either the `id` or `@id` and `type` or `@type` properties. Each object _SHOULD_ have a `profile` property.

 * Any resource type _MAY_ have the `service` property with at least one item.<br/>
   Clients _MAY_ process `service` on any resource type, and _SHOULD_ process the ITF Text Fragment API service.

``` json-doc
{
  "service": [
    {
      "id": "https://example.org/service",
      "type": "ExampleExtensionService",
      "profile": "https://example.org/docs/service"
    }
  ]
}
```

For cross-version consistency, this specification defines the following values for the `type` or `@type` property for backwards compatibility with other IIIF APIs. Future versions of these APIs will define their own types. These `type` values are necessary extensions for compatibility of the older versions.

| Value                | Specification                  |
|----------------------|--------------------------------|
| TextService1         | [Text API version 1]           |
| ImageService1        | [Image API version 1]          |
| ImageService2        | [Image API version 2]          |
| SearchService1       | [Search API version 1]         |
| AutoCompleteService1 | [Search API version 1]         |
| AuthCookieService1   | [Authentication API version 1] |
| AuthTokenService1    | [Authentication API version 1] |
| AuthLogoutService1   | [Authentication API version 1] |
{: .api-table #table-service-types}

Implementations _SHOULD_ be prepared to recognize the `@id` and `@type` property names used by older specifications, as well as `id` and `type`. Note that the `@context` key _SHOULD NOT_ be present within the `service`, but instead included at the beginning of the document. The example below includes both version 2 and version 3 IIIF Image API services.

``` json-doc
{
  "service": [
    {
      "@id": "https://example.org/itf/identifier",
      "@type": "ImageService2",
      "profile": "https://textframe.io/api/level1.json"
    }
  ]
}
```

##### services

A list of one or more service definitions on the top-most resource of the document that are typically shared by more than one subsequent resource. This allows for these shared services to be collected together in a single place, rather than having their information duplicated throughout the document or requiring a client to traverse the entire document structure to find the desired service. The resource that the service applies to _MUST_ still have the `service` property, as described above, where the service resources have at least the `id` and `type` or `@id` and `@type` properties. This allows the client to know that the service applies to that resource. Usage of the `services` property is at the discretion of the publishing system.

A client encountering a `service` property where the definition consists only of an `id` and `type` _SHOULD_ check the `services` property on the top-most resource for an expanded definition. If the service is not present in the `services` list, and the client requires more information in order to use the service, the client _SHOULD_ dereference the `id` (or `@id`) of the service in order to retrieve a service description.

The value _MUST_ be an array of JSON objects. Each object _MUST_ a service resource, as described above.

* A Collection _MAY_ have the `services` property, if it is the topmost Collection in a response document.<br/>
  Clients _SHOULD_ process `services` on a Collection.
* A Manifest _MAY_ have the `services` property.<br/>
  Clients _SHOULD_ process `services` on a Manifest.

``` json-doc
{
  "services": [
    {
      "@id": "https://example.org/itf/auth/login",
      "@type": "AuthCookieService1",
      "profile": "https://textframe.io/api/auth/1/login",
      "label": "Login to Example Institution",
      "service": [
        {
          "@id": "https://example.org/itf/auth/token",
          "@type": "AuthTokenService1",
          "profile": "https://textframe.io/api/auth/1/token"          
        }
      ]
    }
  ]
}
```

##### seeAlso

A machine-readable resource such as an image or RDF description that is related to the resource with the `seeAlso` property. Properties of the resource should be given to help the client select between multiple descriptions (if provided), and to make appropriate use of the document. If the relationship between the resource and the document needs to be more specific, the document should include that relationship rather than the ITF resource. ITF resources are valid targets for `seeAlso`. It can be used, for example, to link to a Manifest for a different version of the same text or for a related text. The URI of the document _MUST_ identify a single representation of the data in a particular format. If the same data exists in JSON and XML, separate resources _SHOULD_ be added for each representation, each with distinct `id` and `format` properties.

The value _MUST_ be an array of JSON objects. Each item _MUST_ have the `id` and `type` properties, and _SHOULD_ have the `label`, `format` and `profile` properties.

 * Any resource type _MAY_ have the `seeAlso` property with at least one item.<br/>
   Clients _MAY_ process `seeAlso` on any resource type.

``` json-doc
{
    "seeAlso": [
        {
            "id": "https://example.org/library/catalog/book1.xml",
            "type": "Dataset",
            "label": {
                "en": ["Bibliographic Description in EAD XML"]
            },
            "format": "text/xml",
            "profile": "https://example.org/profiles/ead"
        }
    ]
}
```

#### 3.3.2. Internal Links

##### partOf

When a client encounters the `partOf` property, it might retrieve the [referenced][prezi30-terminology] containing resource, if it is not [embedded][prezi30-terminology] in the current representation, in order to contribute to the processing of the contained resource. For example, the `partOf` property on a TextFrame can be used to reference an external Manifest in order to enable the discovery of further relevant information. Similarly, a Manifest can reference a containing Collection using `partOf` to aid in navigation.

The value _MUST_ be an array of JSON objects. Each item _MUST_ have the `id` and `type` properties, and _SHOULD_ have the `label` property.

 * Any resource type _MAY_ have the `partOf` property with at least one item<br/>
   Clients _MAY_ render `partOf` on any resource type.

``` json-doc
{
    "partOf": [
        {
            "id": "https://example.org/itf/1",
            "type": "Manifest"
        }
    ]
}
```

##### start

A TextFrame, or part of a TextFrame, which the client _SHOULD_ focus upon or highlight on initialization of the resource that has the `start` property. The reference to part of a TextFrame is handled in the same way that Ranges reference parts of TextFrames. This property allows the client to begin with the first TextFrame that contains interesting content rather than requiring the user to manually navigate to find it.

The value _MUST_ be a JSON object, which _MUST_ have the `id` and `type` properties. The object _MUST_ be either a TextFrame (as in the first example below), or a Specific Resource with a Selector and a `source` property where the value is a TextFrame (as in the second example below).

 * A Manifest _MAY_ have the `start` property.<br/>
   Clients _SHOULD_ process `start` on a Manifest.
 * A Range _MAY_ have the `start` property.<br/>
   Clients _SHOULD_ process `start` on a Range.
 * Other types of resource _MUST NOT_ have the `start` property.<br/>
   Clients _SHOULD_ ignore `start` on other types of resource.

``` json-doc
{
    "start": {
        "id": "https://example.org/itf/1/TextFrame/1",
        "type": "TextFrame"
    }
}
```

``` json-doc
{
  "start": {
    "id": "https://example.org/itf/1/TextFrame-segment/1",
    "type": "SpecificResource",
    "source": "https://example.org/itf/1/TextFrame/1",
    "selector": {
      "mode": "char",
      "fragment": 0,247
    }
  }
}
```

##### supplementary

A link from this Range to an Annotation Collection that includes the `supplementing` Annotations of content resources for the Range. Clients might use this to present additional content to the user from a different TextFrame when interacting with the Range, or to jump to the next part of the Range within the same TextFrame. For example, the Range might represent a newspaper article that spans non-sequential pages, and then uses the `supplementary` property to reference an Annotation Collection that consists of the Annotations that record the text, split into Annotation Pages per newspaper page. Alternatively, the Range might represent the parts of a manuscript that have been transcribed or translated, when there are other parts that have yet to be worked on. The Annotation Collection would be the Annotations that transcribe or translate, respectively.

The value _MUST_ be a JSON object, which _MUST_ have the `id` and `type` properties, and the `type` _MUST_ be `AnnotationCollection`.

 * A Range _MAY_ have the `supplementary` property.<br/>
   Clients _MAY_ process `supplementary` on a Range.
 * Other types of resource _MUST NOT_ have the `supplementary` property.<br/>
   Clients _SHOULD_ ignore `supplementary` on other types of resource.

``` json-doc
{
    "supplementary": {
        "id": "https://example.org/itf/1/annos/1",
        "type": "AnnotationCollection"
    }
}
```

### 3.4. Structural Properties

These properties define the structure of the text being displayed in ITF by allowing the inclusion of child resources within parents, such as a TextFrame within a Manifest, or a Manifest within a Collection. The majority of use cases would use `items`, however there are two special cases for different sorts of structures.

##### items

`items` can be used to record the order in which child resources occur within a parent resource. This could be Collections or Manifests within a parent Collection, or TextFrames within a Manifest. All of these situations are covered with a single property, `items`.

The value _MUST_ be an array of JSON objects. Each item _MUST_ have the `id` and `type` properties. The items will be resources of different types, as described below.

 * A Collection _MUST_ have the `items` property. Each item _MUST_ be either a Collection or a Manifest.<br/>
   Clients _MUST_ process `items` on a Collection.
 * A Manifest _MUST_ have the `items` property with at least one item. Each item _MUST_ be a TextFrame.<br/>
   Clients _MUST_ process `items` on a Manifest.
 * A TextFrame _SHOULD_ have the `items` property with at least one item. Each item _MUST_ be an Annotation Page.<br/>
   Clients _MUST_ process `items` on a TextFrame.
 * An Annotation Page _SHOULD_ have the `items` property with at least one item. Each item _MUST_ be an Annotation.<br/>
   Clients _MUST_ process `items` on an Annotation Page.
 * A Range _MUST_ have the `items` property with at least one item. Each item _MUST_ be a Range, a TextFrame or a Specific Resource where the source is a TextFrame.<br/>
   Clients _SHOULD_ process `items` on a Range.
 * Other types of resource _MUST NOT_ have the `items` property.<br/>
   Clients _SHOULD_ ignore `items` on other types of resource.

``` json-doc
{
  "items": [
    {
      "id": "https://example.org/itf/manifest1",
      "type": "Manifest"
    },
    {
      "id": "https://example.org/itf/collection1",
      "type": "Collection"
    }
    // ...
  ]
}
```

##### structures

The structure of a text represented in a Manifest can be described using a hierarchy of Ranges. Ranges can be used to describe the "table of contents" of the text or other structures that the user can interact with beyond the order given by the `items` property of the Manifest. The hierarchy is built by nesting the child Range resources in the `items` array of the higher level Range. The top level Ranges of these hierarchies are given in the `structures` property.

The value _MUST_ be an array of JSON objects. Each item _MUST_ have the `id` and `type` properties, and the `type` _MUST_ be `Range`.

 * A Manifest _MAY_ have the `structures` property.<br/>
   Clients _SHOULD_ process `structures` on a Manifest. The first hierarchy _SHOULD_ be presented to the user by default, and further hierarchies _SHOULD_ be able to be selected as alternative structures by the user.
 * Other types of resource _MUST NOT_ have the `structures` property.<br/>
   Clients _SHOULD_ ignore `structures` on other types of resource.

``` json-doc
{
    "structures": [
        {
            "id": "https://example.org/itf/range/1",
            "type": "Range",
            "items": [
                { ... },
                { ... },
                ...
            ]
        }
    ]
}
```

##### annotations

An ordered list of Annotation Pages that contain commentary or other Annotations about this resource, separate from the Annotations that are used to supplement content on to a TextFrame. The `motivation` of the Annotations _MUST NOT_ be `supplementing`, and the target of the Annotations _MUST_ include this resource or part of it.

The value _MUST_ be an array of JSON objects. Each item _MUST_ have at least the `id` and `type` properties.

 * A Collection _MAY_ have the `annotations` property with at least one item.<br/>
   Clients _SHOULD_ process `annotations` on a Collection.
 * A Manifest _MAY_ have the `annotations` property with at least one item.<br/>
   Clients _SHOULD_ process `annotations` on a Manifest,.
 * A TextFrame _MAY_ have the `annotations` property with at least one item.<br/>
   Clients _SHOULD_ process `annotations` on a TextFrame.
 * A Range _MAY_ have the `annotations` property with at least one item.<br/>
   Clients _SHOULD_ process `annotations` on a Range.
 * A content resource _MAY_ have the `annotations` property with at least one item.<br/>
   Clients _SHOULD_ process `annotations` on a content resource.
 * Other types of resource _MUST NOT_ have the `annotations` property.<br/>
   Clients _SHOULD_ ignore `annotations` on other types of resource.

``` json-doc
{
    "annotations": [
        {
            "id": "https://example.org/itf/annotationPage/1",
            "type": "AnnotationPage",
            "items": [
                { ... },
                { ... },
                ...
            ]
        }
    ]
}
```

### 3.5. Values

##### Values for motivation

This specification defines two values for the Web Annotation property of `motivation`, or `purpose` when used on a Specific Resource or Textual Body.

While any resource _MAY_ be the `target` of an Annotation, this specification only defines motivations for Annotations that target TextFrames. These motivations allow clients to determine how the Annotation should be rendered, by distinguishing between Annotations that provide the content that are _part of_ the original text and those that are _concern_ the text or its contents.

Additional motivations may be added to the Annotation to further clarify the intent, drawn from [extensions][prezi30-ldce] or other sources. Clients _MUST_ ignore motivation values that they do not understand. Other motivation values given in the Web Annotation specification _SHOULD_ be used where appropriate.

> I notice that the descriptions based on IIIF text pay far more attention to whether and how annotations are to be displayed. We may need to think carefully about what we say on this topic. Do we just need to say for 'asserting', 'classifying' and 'identifying' that they '_MAY_ be presented to the user as part of the representation of the TextFrame, or _MAY_ be presented in a different part of the user interface'? I can't see how we'd be more prescriptive than that, given the variety of things people are likely to want to do.
  
| Value          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `asserting`    | Used when the editor or creation of the annotation is asserting or claiming something about the text or its contents that is described in the target. For example, it could be recording that the original document is damaged and that all (or a particular part) is illegible. This is likely going to be the most common motivation used by modern editors. |
| `classifying`  | Used when the annotation is classifying the Target as something, for example, classifying the text as a certain type of document, such as a letter. The difference between `classifying` and `asserting` is subtle, but significant. If editors are uncomfortable with the epistemological ramifications of using the verb `classifying`, they can easily use `asserting` instead. |
| `identifying`  | Used when assigning an identity to the Target, such as the IRI that identifies a named entity with a mention of that entity within the text. |
| `supplementing` | Resources associated with a TextFrame by an Annotation that has the `motivation` value `supplementing`  _MAY_ be presented to the user as part of the representation of the TextFrame, or _MAY_ be presented in a different part of the user interface. The content can be thought of as being _from_ the TextFrame. The use of this motivation with target resources other than TextFrames is undefined. For example, an Annotation that has the `motivation` value `supplementing`, a body of an Image and the target of part of the TextFrame is an instruction to present that Image to the user either in the TextFrame's rendering area or somewhere associated with it, and could be used to present an easier to read representation of a diagram. Similarly, a textual body is to be presented either in the targeted region of the TextFrame or otherwise associated with it, and might be OCR, a manual transcription or a translation of handwritten text, or captions for what is being said in a TextFrame with audio content. |
| `writing`      | Resources associated with a TextFrame by an Annotation with the `motivation` value of `writing` _MUST_ be presented to the user as part of the visual representation of the TextFrame. The content can be thought of as being _of_ the TextFrame. It should be used, for example, to record text that is written onto the original document, such as page or folio numbers added to the document by a cataloguer, additions or annotations written on the page, etc. The use of this motivation with target resources other than TextFrames is undefined. |
{: .api-table #table-motivations}

##  4. JSON-LD Considerations

This section describes features applicable to all the Presentation API content. For the most part, these are features of the JSON-LD specification that have particular uses within the API.

### 4.1. Case Sensitivity

Terms in JSON-LD are [case sensitive][org-w3c-json-ld-case]. The cases of properties and enumerated values in ITF Presentation API responses _MUST_ match those used in this specification. For example to specify that a resource is a Manifest, the property _MUST_ be given as `type` and not `Type`, `TYPE` or `tYpE`, and the value _MUST_ be given as `Manifest` and not `manifest`, `MANIFEST` or `manIfEsT`.

### 4.2. Resource Representations

Resource descriptions _SHOULD_ be [embedded][prezi30-terminology] within the JSON description of parent resources, and _MAY_ also be available via separate requests from the HTTP(S) URI given in the resource's `id` property. Links to resources _MUST_ be given as a JSON object with the `id` and `type` properties and _SHOULD_ have `format` or `profile` to give a hint as to what sort of resource is being referred to.

``` json-doc
{
  "rendering": [
    {
      "id": "https://example.org/content/book.pdf",
      "type": "Text",
      "label": { "en": [ "Example Book (pdf)" ] },
      "format": "application/pdf"
    }
  ]
}
```

### 4.3. Properties with Multiple Values

Any of the properties in the API that can have multiple values _MUST_ always be given as an array of values, even if there is only a single item in that array.

``` json-doc
{
  "thumbnail": [
    {
      "id": "https://example.org/images/thumb1.jpg",
      "type": "Image",
      "format": "image/jpeg"
    }
  ]
}
```

### 4.4. Language of Property Values
{: #language-of-property-values}

Language _MAY_ be associated with strings that are intended to be displayed to the user for the `label` and `summary` properties, plus the `label` and `value` properties of the `metadata` and `requiredStatement` objects.

The values of these properties _MUST_ be JSON objects, with the keys being the [BCP 47][org-bcp-47] language code for the language, or if the language is either not known or the string does not have a language, then the key _MUST_ be the string `none`. The associated values _MUST_ be arrays of strings, where each item is the content in the given language.

``` json-doc
{
  "label": {
    "en": [
      "French Short Stories Volume 1"
    ],
    "fr": [
      "Nouvelles Françaises Tome 1"
    ],
    "none": [ "Lyon (1966)" ]
  }
}
```

Note that [BCP 47][org-bcp-47] allows the script of the text to be included after a hyphen, such as `ar-latn`, and clients should be aware of this possibility.

In the case where multiple values are supplied, clients _MUST_ use the following algorithm to determine which values to display to the user.

* If all the values are associated with the `none` key, the client _MUST_ display all of those values.
* Else, the client should try to determine the user's language preferences, or failing that use some default language preferences. Then:
  * If any of the values have a language associated with them, the client _MUST_ display all the values associated with the language that best matches the language preference.
  * If all the values have a language associated with them, and none match the language preference, the client _MUST_ select a language and display all the values associated with that language.
  * If some of the values have a language associated with them, but none match the language preference, the client _MUST_ display all the values that do not have a language associated with them.

Note that this does not apply to [embedded][prezi30-terminology] textual bodies in Annotations, which use the Web Annotation pattern of `value` and `language` as separate properties.

### 4.5. HTML Markup in Property Values

Minimal HTML markup _MAY_ be included for processing in the `summary` property and the `value` property in the `metadata` and `requiredStatement` objects. It _MUST NOT_ be used in `label` or other properties. This is included to allow content publishers to add links and simple formatting instructions to blocks of text. The content _MUST_ be well-formed XML and therefore _MUST_ be wrapped in an element such as `p` or `span`. There _MUST NOT_ be whitespace on either side of the HTML string, and thus the first character in the string _MUST_ be a '<' character and the last character _MUST_ be '>', allowing a consuming application to test whether the value is HTML or plain text using these. To avoid a non-HTML string matching this, it is _RECOMMENDED_ that an additional whitespace character be added to the end of the value in situations where plain text happens to start and end this way.

In order to avoid HTML or script injection attacks, clients _MUST_ remove:

  * Tags such as `script`, `style`, `object`, `form`, `input` and similar.
  * All attributes other than `href` on the `a` tag, `src` and `alt` on the `img` tag.
  * All `href` attributes that start with the strings other than "http:", "https:", and "mailto:".
  * CData sections.
  * XML Comments.
  * Processing instructions.

Clients _SHOULD_ allow only `a`, `b`, `br`, `i`, `img`, `p`, `small`, `span`, `sub` and `sup` tags. Clients _MAY_ choose to remove any and all tags, therefore it _SHOULD NOT_ be assumed that the formatting will always be rendered. Note that publishers _MAY_ include arbitrary HTML content for processing using customized or experimental applications, and the requirements for clients assume an untrusted or unknown publisher.

``` json-doc
{
    "summary": {
        "en": ["<p>A short <b>summary</b> of the resource.</p>"]
    }
}
```

### 4.6. Linked Data Context and Extensions

The top level resource in the response _MUST_ have the `@context` property, and it _SHOULD_ appear as the very first key/value pair in the JSON representation. This tells Linked Data processors how to interpret the document. The ITF Presentation API context, below, _MUST_ occur once per response in the top-most resource, and, thus, _MUST NOT_ appear within [embedded][prezi30-terminology] resources. For example, when embedding a TextFrame within a Manifest, the TextFrame will not have the `@context` property.

The value of the `@context` property _MUST_ be either the URI `https://textframe.io/api/presentation/{{ page.major }}/context.json` or a JSON array with the URI `https://textframe.io/api/presentation/{{ page.major }}/context.json` as the last item. Further contexts, such as those for local or [registered extensions][registry], _MUST_ be added at the beginning of the array.

``` json-doc
{
  "@context": "https://textframe.io/api/presentation/{{ page.major }}/context.json"
}
```

Any additional properties beyond those defined in this specification or the Web Annotation Data Model _SHOULD_ be mapped to RDF predicates using further context documents. These extensions _SHOULD_ be added to the top level `@context` property, and _MUST_ be added before the above context. The JSON-LD 1.1 functionality of predicate specific context definitions, known as [scoped contexts][org-w3c-json-ld-scoped-contexts], _MUST_ be used to minimize cross-extension collisions. Extensions intended for community use _SHOULD_ be [registered in the extensions registry][registry], but registration is not mandatory.

``` json-doc
{
  "@context": [
    "http://example.org/extension/context.json",
    "http://iiif.io/api/presentation/{{ page.major }}/context.json"
  ]
}
```

The JSON representation _MUST NOT_ include the `@graph` key at the top level. This key might be created when serializing directly from RDF data using the JSON-LD 1.0 compaction algorithm. Instead, JSON-LD framing and/or custom code should be used to ensure the structure of the document is as defined by this specification.

### 4.7. Term Collisions between Contexts

There are some common terms used in more than one JSON-LD context document. Every attempt has been made to minimize these collisions, but some are inevitable. In order to know which specification is in effect at any given point, the class of the resource that has the property is the primary governing factor. Properties on Annotation-based resources, therefore, use the context from the [Web Annotation Data Model][org-w3c-webanno], whereas properties on classes defined by this specification use the ITF Presentation API context's definition.

There is one property that is in direct conflict: the `label` property is defined by both and is available for every resource. The use of `label` in ITF follows modern best practices for internationalization by allowing the language to be associated with the value using the language map construction [described above][prezi30-languages]. The Web Annotation Data Model uses it only for [Annotation Collections][prezi30-annocoll], and mandates the format is a string. For this property, the API overrides the definition from the Annotation model to ensure that labels can consistently be represented in multiple languages.

The following properties are defined by both, and the ITF representation is more specific than the Web Annotation Data Model but are not in conflict, or are never used on the same resource:

| Property   | Notes on non-conflicting differences with the Web Annotation Data Model                                                                                                                                                                                                               |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `homepage` | In ITF the home page of a resource is represented as a JSON object, whereas in the Web Annotation Data Model it can also be a string. |
| `type`     | In ITF the type is singular, whereas in the Web Annotation Data Model there can be more than one type. |
| `format`   | In ITF the format of a resource is also singular, whereas in the Web Annotation Data Model there can be more than one format. |
| `language` | In ITF the `language` property always takes an array, whereas in the Web Annotation Data Model it can be a single string. |
| `start`    | The `start` property is used on a Manifest to refer to the start TextFrame or part of a TextFrame and thus is a JSON object, whereas in the Web Annotation Data Model it is used on a TextPositionSelector to give the start offset into the textual content and is thus an integer. |

The `rights`, `partOf`, and `items` properties are defined by both in the same way.

### 4.8. Keyword Mappings

The JSON-LD keywords `@id`, `@type` and `@none` are mapped to `id`, `type` and `none` by the Presentation API [linked data context][prezi30-ldce]. Thus in content conforming to this version of the Presentation API, the only JSON key beginning with `@` will be `@context`. However, the content may include data conforming to older specifications or external specifications that use keywords beginning with `@`. Clients should expect to encounter both syntaxes.

##  5. Resource Structure

This section provides a detailed description of the resource types used in this specification. [Section 2][prezi30-type-overview] provides an overview of the resource types and figures illustrating allowed relationships between them, and [Appendix A][prezi30-appendixa] provides summary tables of the property requirements.

### 5.1. Collection

Collections are used to list the Manifests available for viewing. Collections _MAY_ include both other Collections and Manifests, in order to form a tree-structured hierarchy. Collections might align with the curated management of cultural heritage resources in sets, also called "collections", but may have absolutely no such similarity.

The intended usage of Collections is to allow clients to:

  * Load a pre-defined set of Manifests at initialization time.
  * Receive a set of Manifests, such as search results, for rendering.
  * Visualize lists or hierarchies of related Manifests.
  * Provide navigation through a list or hierarchy of available Manifests.

Collections _MAY_ be [embedded][prezi30-terminology] inline within other Collections, such as when the Collection is used primarily to subdivide a larger one into smaller pieces. However, Manifests _MUST NOT_ be [embedded][prezi30-terminology] within Collections. An [embedded][prezi30-terminology] Collection _SHOULD_ also have its own URI from which the JSON description is available.

Manifests or Collections _MAY_ be [referenced][prezi30-terminology] from more than one Collection. For example, an institution might define four Collections: one for modern works, one for historical works, one for newspapers and one for books. The Manifest for a historical newspaper would then appear in both the historical Collection and the newspaper Collection. Alternatively, the institution may choose to have two separate newspaper Collections, and reference each as a sub-Collection of modern and historical.

Collections with an empty `items` property are allowed but discouraged. For example, if the user performs a search that matches no Manifests, then the server _MAY_ return a Collection response with no Manifests.

Collections or Manifests [referenced][prezi30-terminology] in the `items` property _MUST_ have the `id`, `type` and `label` properties.

An example Collection document:

``` json-doc
{
    "@context": "http://iiif.io/api/presentation/{{ page.major }}/context.json",
    "id": "https://example.org/itf/collection/top",
    "type": "Collection",
    "label": {
        "en": ["Collection for Example Organization"]
    },
    "summary": {
        "en": ["Short summary of the Collection"]
    },
    "requiredStatement": {
        "label": {
            "en": ["Attribution"]
        },
        "value": {
            "en": ["Provided by Example Organization"]
        }
    },
    "items": [
        {
            "@context": "https://textframe.io/api/presentation/1/context.json",
            "id": "https://example.org/itf/CASE16313/diplomatic/info.json",
            "type": "Manifest",
            "label": {
                "en": ["CASE16313: Horary consultation concerning Mrs Elizabeth Berry (PERSON9677) (5 September 1604 at 11:40)"]
            },
            "items": [
                {
                    "id": "https://example.org/itf/CASE16313/diplomatic/book/frame/full",
                    "type": "TextFrame",
                    "label": {
                        "en": ["Lorem ipse dolor zzril"]
                    },
                    "items": [
                        {
                            "id": "https://example.org/itf/CASE16313/diplomatic/book/full/1",
                            "type": "AnnotationFrame",
                            "items": [
                                {
                                    "id": "https://example.org/itf/CASE16313/diplomatic/book/annotation/full/1",
                                    "type": "Annotation",
                                    "motivation": "text",
                                    "body": {
                                        "id": "https://example.org/itf/CASE16313/diplomatic/book/full/rich/html",
                                        "type": "html",
                                        "format": "text/html"
                                    }
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ]
}
```

### 5.2. Manifest

The Manifest resource typically represents a single object and any intellectual work or works embodied within that object. In particular, it includes descriptive, rights and linking information for the object. The Manifest embeds the TextFrames that should be rendered as views of the object and contains sufficient information for the client to initialize itself and begin to display something quickly to the user.

The identifier in `id` _MUST_ be able to be dereferenced to retrieve the JSON description of the Manifest, and thus _MUST_ use the HTTP(S) URI scheme.

The Manifest _MUST_ have an `items` property, which is an array of JSON-LD objects. Each object is a TextFrame, with requirements as described in the next section. The Manifest _MAY_ also have a `structures` property listing one or more [Ranges][prezi30-range] which describe additional structure of the content, such as might be rendered as a table of contents. The Manifest _MAY_ have an `annotations` property, which includes Annotation Page resources where the Annotations have the Manifest as their `target`. These will typically be comment style Annotations, and _MUST NOT_ have `writing` as their `motivation`.

``` json-doc
{
    "@context": "https://textframe.io/api/presentation/1/context.json",
    "id": "https://example.org/itf/CASE16313/diplomatic/info.json",
    "type": "Manifest",
    "label": {
        "en": ["CASE16313: Horary consultation concerning Mrs Elizabeth Berry (PERSON9677) (5 September 1604 at 11:40)"]
    },
    "metadata": [
        {
            "label": {
                "en": ["Author"]
            },
            "value": {
                "en": ["Mr Gerence James [Marks]"]
            }
        },
        {
            "label": {
                "en": ["Author"]
            },
            "value": {
                "en": ["Mr Richard Napier [Sandy] [Richard Napier [Senior]]"]
            }
        },
        {
            "label": {
                "en": ["Date"]
            },
            "value": {
                "en": ["5 September 1604 at 11:40"]
            }
        },
        {
            "label": {
                "en": ["DateTimeStamp"]
            },
            "value": {
                "en": ["1604-09-05T11:40:00"]
            }
        },
        {
            "label": {
                "en": ["Source"]
            },
            "value": {
                "en": ["MS Ashmole 207, f. 211r (bottom left part of page)."]
            }
        }
    ],
    "summary": [
        {
            "en": ["A case from 5 September 1604 at 11:40, recorded by Mr Gerence James [Marks] and Mr Richard Napier [Sandy] [Richard Napier [Senior]] in MS Ashmole 207, f. 211r (bottom left part of page)."]
        }
    ],
     
    // Presentation Information
    "navDate": "1604-09-05T11:40:00",

    // Rights Information
    "rights": "https://creativecommons.org/licenses/by/4.0/",
    "requiredStatement": {
        "label": {
            "en": ["Attribution"]
        },
        "value": {
            "en": ["Cite this as: Lauren Kassell, Michael Hawkins, Robert Ralley, John Young, Joanne Edge, Janet Yvonne Martin-Portugues, and Natalie Kaoukji (eds.), ‘CASE16313’, The casebooks of Simon Forman and Richard Napier, 1596–1634: a digital edition, https://casebooks.lib.cam.ac.uk/case/CASE16313, accessed 18 July 2024."]
        }
    },
    "provider": [
        {
            "id": "https://example.org/about",
            "type": "Agent",
            "label": {
                "en": ["Example Organization"]
            },
            "homepage": [
                {
                    "id": "https://example.org/",
                    "type": "Text",
                    "label": {
                        "en": ["Example Organization Homepage"]
                    },
                    "format": "text/html"
                }
            ],
            "logo": [
                {
                    "id": "https://example.org/service/inst1/full/max/0/default.png",
                    "type": "Image",
                    "format": "image/png",
                    "service": [
                        {
                            "id": "https://example.org/service/inst1",
                            "type": "ImageService3",
                            "profile": "level2"
                        }
                    ]
                }
            ],
            "seeAlso": [
                {
                    "id": "https://data.example.org/about/us.jsonld",
                    "type": "Dataset",
                    "format": "application/ld+json",
                    "profile": "https://schema.org/"
                }
            ]
        }
    ],

    // Links
    "homepage": [
        {
            "id": "https://example.org/info/book1/",
            "type": "Text",
            "label": {
                "en": ["Home page for Book 1"]
            },
            "format": "text/html"
        }
    ],
    "service": [
        {
            "id": "https://example.org/service/example",
            "type": "ExampleExtensionService",
            "profile": "https://example.org/docs/example-service.html"
        }
    ],
    "partOf": [
        {
            "id": "https://example.org/collections/books/",
            "type": "Collection"
        }
    ],
    "seeAlso": [
        {
            "id": "https://example.org/library/catalog/book1.xml",
            "type": "Dataset",
            "format": "text/xml",
            "profile": "https://example.org/profiles/ead"
        }
    ],

    // List of Services, referenced from within items, structures or annotations
    "services": [
        {
            "@id": "https://example.org/itf/auth/login",
            "@type": "AuthCookieService1",
            "profile": "https://iiif.io/api/auth/1/login",
            "label": "Login to Example Institution",
            "service": [
                {
                    "@id": "https://example.org/itf/auth/token",
                    "@type": "AuthTokenService1",
                    "profile": "https://iiif.io/api/auth/1/token"
                }
            ]
        }
    ],

    // List of TextFrames
    "items": [
        {
            "id": "https://example.org/itf/CASE16313/diplomatic/book/frame/full",
            "type": "TextFrame",
            "label": {
                "en": ["LABEL GOES HERE"]
            },
            // ...
        }
    ],

    // structure of the resource, described with Ranges
    "structures": [
        {
            "id": "https://example.org/itf/book1/range/top",
            "type": "Range"
      // Ranges members should be included here
        }
    // Any additional top level Ranges can be included here
    ],

    // Commentary Annotations on the Manifest
    "annotations": [
        {
            "id": "https://example.org/itf/book1/annotations/p1",
            "type": "AnnotationPage",
            "items": [
                // Annotations about the Manifest are included here
            ]
        }
    ]
}
```

###  5.3. TextFrame

The TextFrame represents an individual frame or view that acts as the container for assembling the content resources it contains. TextFrames _MUST_ be identified by a URI and it _MUST_ be an HTTP(S) URI. The URI of the TextFrame _MUST NOT_ contain an anchor fragment (a `#` followed by further characters). TextFrames _MAY_ be able to be dereferenced separately from the Manifest via their URIs as well as being [embedded][prezi30-terminology].

Every TextFrame _SHOULD_ have a `label` to display. If one is not provided, the client _SHOULD_ automatically generate one for use based on the TextFrame's position within the `items` property.

Content resources are associated with the TextFrame via Web Annotations. Content that is to be rendered as part of the TextFrame _MUST_ be associated by an Annotation that has the `motivation` value `writing`. These Annotations are recorded in the `items` of one or more Annotation Pages, referred to in the `items` array of the TextFrame. Annotations that do not have the `motivation` value `writing` _MUST NOT_ be in pages [referenced][prezi30-terminology] in `items`, but instead in the `annotations` property. Referenced, external Annotation Pages _MUST_ have the `id` and `type` properties.

Content that is derived from the TextFrame, such as IIIF images for the underlying resource or an audio representation of the text, _MUST_ be associated by an Annotation that has the `motivation` value `supplementing`. Annotations _MAY_ have any other `motivation` values as well. Thus, content of any type may be associated with the TextFrame via an Annotation that has the `motivation` value `writing`, meaning the content is _part of_ the TextFrame; an Annotation that has the `motivation` value `supplementing`, meaning the content is _from_ the TextFrame but not necessarily part of it; or an Annotation with another `motivation` meaning that it is somehow about the TextFrame.

TextFrames _MAY_ be treated as content resources for the purposes of annotating on to other TextFrames. For example, a TextFrame (TextFrame A) containing a commentary on a historical text along with Annotations may be annotated on to another TextFrame (TextFrame B) containing the historical text.

``` json-doc
{
  // Metadata about this TextFrame
  "id": "https://example.org/itf/book1/TextFrame/p1",
  "type": "TextFrame",
  "label": { "none": [ "p. 1" ] },

  "items": [
    {
      "id": "https://example.org/itf/book1/content/p1/1",
      "type": "AnnotationPage",
      "items": [
        // Writing Annotations on the TextFrame are included here
      ]
    }
  ],

  "annotations": [
    {
      "id": "https://example.org/itf/book1/comments/p1/1",
      "type": "AnnotationPage",
      "items": [
        // Non-writing Annotations on the TextFrame are included here
      ]
    }
  ]
}
```

###  5.4. Range

> This may be a question to which there's a simple technical answer. I'm still trying to settle in my mind how Range is useful in the Presentation API when you have custom_modes and custom_mode_definitions in the Text Information API. Those are meant to give you a mechanism to divide up a text into a hierarchical structure of different units (e.g. chapters within a book, or presumably articles in a newspaper). So does the Presentation API need a separate mechanism to record those same hierarchical divisions? Does this allow you to do different things?
> > That's a good point. I can't see any use for it that couldn't be done better with a collection containing a bunch of manifests. Neil, what do you think? Delete Range altogether (here and passim)

Ranges are used to represent structure within an object beyond the default order of the TextFrames in the `items` property of the Manifest, such as newspaper sections or articles, chapters within a book, or movements within a piece of music. Ranges can include TextFrames, parts of TextFrames, or other Ranges, creating a tree structure like a table of contents.

The intent of adding a Range to the Manifest is to allow the client to display a linear or hierarchical navigation interface to enable the user to quickly move through the object's content. Clients _SHOULD_ present only Ranges that have the `label` property and do not have a `behavior` value `no-nav` to the user. Clients _SHOULD NOT_ render TextFrame labels as part of the navigation, and a Range that wraps the TextFrame _MUST_ be created if this is the desired presentation.

If there is no Range that has the `behavior` value `sequence`, and the Manifest does not have the `behavior` value `unordered`, then the client _SHOULD_ treat the order of the TextFrames in the Manifest's `items` array as the default order. If there is one Range that has the `behavior` value `sequence`, then the client _MUST_ instead use this Range for the ordering. If there is more than one Range that has the `behavior` value `sequence`, for example a second Range to represent an alternative ordering of the pages of a manuscript, the first Range _SHOULD_ be used as the default and the others _SHOULD_ be able to be selected. Ranges that have the `behavior` value `sequence` _MUST_ be directly within the `structures` property of the Manifest, and _MUST NOT_ be [embedded][prezi30-terminology] or [referenced][prezi30-terminology] within other Ranges. These Ranges may have limited hierarchical nesting, but clients are not expected to traverse very deep structures in determining the default order. If this Range includes parts of TextFrames, then these parts are the content to render by default and would generate separate entries in a navigation display. This allows for the TextFrame to include content outside of the default view, such as a color bar or ruler.

Ranges _MUST_ have URIs and they _SHOULD_ be HTTP(S) URIs. Top level Ranges are [embedded][prezi30-terminology] or externally [referenced][prezi30-terminology] within the Manifest in a `structures` property. These top level Ranges then embed or reference other Ranges, TextFrames or parts of TextFrames in the `items` property. Each entry in the `items` property _MUST_ be a JSON object, and it _MUST_ have the `id` and `type` properties. If a top level Range needs to be dereferenced by the client, then it _MUST NOT_ have the `items` property, such that clients are able to recognize that it should be retrieved in order to be processed.

All of the TextFrames or parts that should be considered as being part of a Range _MUST_ be included within the Range's `items` property, or a descendant Range's `items`.

The TextFrames and parts of TextFrames need not be contiguous or in the same order as in the Manifest's `items` property or any other Range. Examples include newspaper articles that are continued in different sections, a chapter that starts halfway through a page, or a text that is divided into separate bound manuscript volumes.

Ranges _MAY_ link to an Annotation Collection that has the content of the Range using the `supplementary` property. The [referenced][prezi30-terminology] Annotation Collection will contain Annotations that target areas of TextFrames within the Range and link content resources to those TextFrames.

``` json-doc
{
  "@context": "http://iiif.io/api/presentation/{{ page.major }}/context.json",
  "id": "https://example.org/itf/book1/manifest",
  "type": "Manifest",
  // Metadata here ...

  "items": [
    // TextFrames here ...
  ],

  "structures": [
    {
      "id": "https://example.org/itf/book1/range/r0",
      "type": "Range",
      "label": { "en": [ "Table of Contents" ] },
      "items": [
        {
          "id": "https://example.org/itf/book1/TextFrame/cover",
          "type": "TextFrame"
        },
        {
          "id": "https://example.org/itf/book1/range/r1",
          "type": "Range",
          "label": { "en": [ "Introduction" ] },
          "supplementary": {
            "id": "https://example.org/itf/book1/annocoll/introTexts",
            "type": "AnnotationCollection"
          },
          "items": [
            {
              "id": "https://example.org/itf/book1/TextFrame/p1",
              "type": "TextFrame"
            },
            {
              "id": "https://example.org/itf/book1/TextFrame/p2",
              "type": "TextFrame"
            }
          ]
        },
        {
          "id": "https://example.org/itf/book1/TextFrame/backCover",
          "type": "TextFrame"
        }
      ]
    }
  ]
}
```

###  5.5. Annotation Page

Association of Images and other content with their respective TextFrames is done via Annotations. Traditionally Annotations are used for associating commentary with the resource the Annotation's text or body is about; the [Web Annotation][org-w3c-webanno] model allows any resource to be associated with any other resource, or parts thereof, and it is reused for both commentary and `writing` resources on the TextFrame. Other resources beyond images might include IIIF images of the text, an audio or video performance of the text, commentary Annotations, tags and more.

These Annotations are collected together in Annotation Page resources, which are included in the `items` property from the TextFrame. Each Annotation Page can be [embedded][prezi30-terminology] in its entirety, if the Annotations should be processed as soon as possible when the user navigates to that TextFrame, or a reference to an external page. This reference _MUST_ include `id` and `type`, _MUST NOT_ include `items` and _MAY_ include other properties, such as `behavior`. All the Annotations in the Annotation Page _SHOULD_ have the TextFrame as their `target`. Clients _SHOULD_ process the Annotation Pages and their items in the order given in the TextFrame. Publishers may choose to expedite the processing of [embedded][prezi30-terminology] Annotation Pages by ordering them before external pages, which will need to be dereferenced by the client.

An Annotation Page _MUST_ have an HTTP(S) URI given in `id`, and _MAY_ have any of the other properties defined in this specification or the Web Annotation specification. The Annotations are listed in the `items` property of the Annotation Page.

__Incompatibility Warning__<br/>
The definition of `label` in the Web Annotation specification does not produce JSON conformant with the structure defined in this specification for languages. Given the absolute requirement for internationalized labels and the strong desire for consistently handling properties, the `label` property on Annotation model classes does not conform to the string requirement of the Web Annotation Data Model. This [issue has been filed with the W3C][github-webanno-437] and will hopefully be addressed in a future version of the standard.
{: .warning}

``` json-doc
{
  "@context": "http://iiif.io/api/presentation/{{ page.major }}/context.json",
  "id": "https://example.org/itf/book1/annopage/p1",
  "type": "AnnotationPage",

  "items": [
    {
      "id": "https://example.org/itf/book1/annopage/p1/a1",
      "type": "Annotation"
      // ...
    },
    {
      "id": "https://example.org/itf/book1/annopage/p1/a2",
      "type": "Annotation"
      // ...
    }
  ]
}
```

### 5.6. Annotation

Annotations follow the [Web Annotation][org-w3c-webanno] data model. The description provided here is a summary plus any ITF specific requirements. The W3C standard is the official documentation.

Annotations _MUST_ have their own HTTP(S) URIs, conveyed in the `id` property. The JSON-LD description of the Annotation _SHOULD_ be returned if the URI is dereferenced, according to the [Web Annotation Protocol][org-w3c-webanno-protocol].

When Annotations are used to associate content resources with a TextFrame, the content resource is linked in the `body` of the Annotation. The URI of the TextFrame _MUST_ be repeated in the `target` property of the Annotation, or the `source` property of a Specific Resource used in the `target` property.

Note that the Web Annotation data model defines different patterns for the `value` property, when used within an Annotation. The value of a Textual Body or a Fragment Selector, for example, are strings rather than JSON objects with languages and values. Care must be taken to use the correct string form in these cases.

Additional features of the Web Annotation data model _MAY_ also be used, such as selecting a segment of the TextFrame or content resource, or embedding the comment within the Annotation. The use of these advanced features sometimes results in situations where the `target` is not a content resource, but instead a SpecificResource, a Choice, or other non-content object. Implementations should check the `type` of the resource and not assume that it is always content to be rendered.

### 5.7. Content Resources

Content resources are external web resources that are [referenced][prezi30-terminology] from within the Manifest or Collection. This includes images, video, audio, data, web pages or any other format.

As described in the [TextFrame][prezi30-TextFrame] section, the content associated with a TextFrame (and therefore the content of a Manifest) is provided by the `body` property of Annotations with the `writing` motivation. Content resources can also be [referenced][prezi30-terminology] from `homepage`, `logo`, `rendering`, and `seeAlso` properties.

Content resources _MUST_ have an `id` property, with the value being the URI at which the resource can be obtained.

The type of the content resource _MUST_ be included, and _SHOULD_ be taken from the table listed under the definition of `type`. The `format` of the resource _SHOULD_ be included and, if so, _SHOULD_ be the media type that is returned when the resource is dereferenced. The `profile` of the resource, if it has one, _SHOULD_ also be included. Content resources in appropriate formats _MAY_ also have the `language` property. Content resources _MAY_ also have descriptive and linking properties, as defined in [section 3][prezi30-resource-properties].

If the content resource is a text, and an ITF Image service is available for it, then the `id` property of the content resource _MAY_ be a complete URI to any particular representation supported by the Text Service, such as `https://example.org/itf/CASE20258/diplomatic/book/full/1`, but _MUST NOT_ be just the URI of the ITF Text service. Its `type` value _MUST_ be the string `Text`. The Text _SHOULD_ have the service [referenced][prezi30-terminology] from it using the `service` property.

If there is a need to distinguish between content resources, then the resource _SHOULD_ have the `label` property.

A TextFrame _MAY_ be treated as a content resource for the purposes of annotating it on to other TextFrames. In this situation, the TextFrame _MAY_ be [embedded][prezi30-terminology] within the Annotation, or require dereferencing to obtain its description.

``` json-doc
{
    "@context": "http://textframe.io/api/presentation/{{ page.major }}/context.json",
    "id": "https://example.org/itf/TEXT6/diplomatic/annotation-1",
    "type": "Annotation",
    "motivation": "writing",
    "body": {
        "id": "https://example.org/itf/TEXT6/diplomatic/book/p1/rich/html",
        "type": "Text",
        "label": {
            "en": ["“A treatise touching the Defenc of Astrologie (fragment A)”"]
        },
        "service": [
            {
                "id": "https://example.org/itf/TEXT6/diplomatic/page1",
                "type": "TexService1",
                "profile": "level1",
                "service": [
                    {
                        "@id": "https://example.org/itf/auth/login",
                        "@type": "AuthCookieService1"
                    }
                ]
            }
        ]
    },
    "target": "https://example.org/itf/TEXT6/diplomatic/TextFrame/p1"
}
```

### 5.8. Annotation Collection

Annotation Collections represent groupings of Annotation Pages that should be managed as a single whole, regardless of which TextFrame or resource they target. This allows, for example, all Annotations that make up a particular translation of a text to be collected together. A client might then present a user interface that allows all the Annotations in an Annotation Collection to be displayed or hidden according to the user's preference.

Annotation Collections _MUST_ have a URI, and it _SHOULD_ be an HTTP(S) URI. They _SHOULD_ have a `label` and _MAY_ have any of the other descriptive, linking or rights properties.

``` json-doc
{
    "@context": "http://iiif.io/api/presentation/{{ page.major }}/context.json",
    "id": "https://example.org/itf/book1/annocoll/transcription",
    "type": "AnnotationCollection",
    "label": {
        "en": ["Diplomatic Transcription"]
    },
    "first": {
        "id": "https://example.org/itf/book1/annopage/l1",
        "type": "AnnotationPage"
    },
    "last": {
        "id": "https://example.org/itf/book1/annopage/l120",
        "type": "AnnotationPage"
    }
}
```

For Annotation Collections with many Annotations, there will be many pages. The Annotation Collection refers to the first and last page, and then the pages refer to the previous and next pages in the ordered list. Each page is part of the Annotation Collection.

``` json-doc
{
    "@context": "http://iiif.io/api/presentation/{{ page.major }}/context.json",
    "id": "https://example.org/itf/book1/annopage/l1",
    "type": "AnnotationPage",
    "partOf": {
        "id": "https://example.org/itf/book1/annocoll/transcription",
        "type": "AnnotationCollection"
    },
    "next": {
        "id": "https://example.org/itf/book1/annopage/l2",
        "type": "AnnotationPage"
    },
    "items": [
        {
            "id": "https://example.org/itf/book1/annopage/p1/a1",
            "type": "Annotation"
            // ...
        }
    ]
}
```

## 6. HTTP Requests and Responses

This section describes the _RECOMMENDED_ request and response interactions for the API. The REST and simple HATEOAS approach is followed where an interaction will retrieve a description of the resource, and additional calls may be made by following links obtained from within the description. All requests use the HTTP GET method; creation, update and deletion of resources is not covered by this specification. It is _RECOMMENDED_ that implementations also support HTTP HEAD requests.

### 6.1. URI Recommendations

While any HTTP(S) URI is technically acceptable for any of the resources in the API, there are several best practices for designing the URIs for the resources.

* The URI _SHOULD_ use the HTTPS scheme, not HTTP.
* The URI _SHOULD NOT_ include query parameters or fragments.
* Once published, they _SHOULD_ be as persistent and unchanging as possible.
* Special characters _MUST_ be encoded.

###  6.2. Requests

Clients are only expected to follow links to Presentation API resources. Unlike [ITF Text Fragment API](https://textframe.io/api/) requests, or other parameterized services, the URIs for Presentation API resources cannot be assumed to follow any particular pattern.

###  6.3. Responses

The format for all responses is JSON, as described above. It is good practice for all resources with an HTTP(S) URI to provide their description when the URI is dereferenced. If a resource is [referenced][prezi30-terminology] within a response, rather than being [embedded][prezi30-terminology], then it _MUST_ be able to be dereferenced.

If the server receives a request with an `Accept` header, it _SHOULD_ respond following the rules of [content negotiation][org-rfc-7231-conneg]. Note that content types provided in the `Accept` header of the request _MAY_ include parameters, for example `profile` or `charset`.

If the request does not include an `Accept` header, the HTTP `Content-Type` header of the response _SHOULD_ have the value `application/ld+json` (JSON-LD) with the `profile` parameter given as the context document: `https://textframe.io/api/presentation/1/context.json`.

```
Content-Type: application/ld+json;profile="https://textframe.io/api/presentation/1/context.json"
```
{: .urltemplate}

If the `Content-Type` header `application/ld+json` cannot be generated due to server configuration details, then the `Content-Type` header _SHOULD_ instead be `application/json` (regular JSON), without a `profile` parameter.

```
Content-Type: application/json
```
{: .urltemplate}

The HTTP server _MUST_ follow the [CORS requirements][org-w3c-cors] to enable browser-based clients to retrieve the descriptions. If the server receives a request with one of the content types above in the Accept header, it _SHOULD_ respond with that content type following the rules of [content negotiation][org-rfc-7231-conneg]. Recipes for enabling CORS and conditional Content-Type headers are provided in the [Apache HTTP Server Implementation Notes][notes-apache].

Responses _SHOULD_ be compressed by the server as there are significant performance gains to be made for very repetitive data structures.

## 7. Authentication

It may be necessary to restrict access to the descriptions made available via the Presentation API. As the primary means of interaction with the descriptions is by web browsers using XmlHttpRequests across domains, there are some considerations regarding the most appropriate methods for authenticating users and authorizing their access. The approach taken is described in the [Authentication][itf-auth] specification, and requires requesting a token to add to the requests to identify the user. This token might also be used for other requests defined by other APIs.

It is possible to include Image API service descriptions within the Manifest, and within those it is also possible to include links to the Authentication API's services that are needed to interact with the image content. The first time an Authentication API service is included within a Manifest, it _MUST_ be the complete description. Subsequent references _SHOULD_ be just the URI of the service, and clients are expected to look up the details from the full description by matching the URI. Clients _MUST_ anticipate situations where the Authentication service description in the Manifest is out of date: the source of truth is the Image Information document, or other system that references the Authentication API services.

## Appendices

### A. Summary of Property Requirements

| Icon                       | Meaning     |
| -------------------------- | ----------- |
| ![required][icon3-req]      | Required    |
| ![recommended][icon3-rec]   | Recommended |
| ![optional][icon3-opt]      | Optional    |
| ![not allowed][icon3-na]    | Not Allowed |
{: .api-table #table-reqs-icons}


__Descriptive and Rights Properties__

|                      | label                     | metadata                  | summary                   | requiredStatement      | rights                 | navDate                   | language                  |
| -------------------- | -----------------------   | ------------------------- | ------------------------- | ---------------------- | ---------------------- | ------------------------- | ------------------------- |
| Collection           | ![required][icon3-req]    | ![recommended][icon3-rec] | ![recommended][icon3-rec] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt]    | ![not allowed][icon3-na]  |
| Manifest             | ![required][icon3-req]    | ![recommended][icon3-rec] | ![recommended][icon3-rec] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt]    | ![not allowed][icon3-na]  |
| TextFrame               | ![recommended][icon3-rec] | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt]    | ![not allowed][icon3-na]  |
| Annotation           | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na]  | ![not allowed][icon3-na]  |
| AnnotationPage       | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na]  | ![not allowed][icon3-na]  |
| Range                | ![recommended][icon3-rec] | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt]    | ![not allowed][icon3-na]  |
| AnnotationCollection | ![recommended][icon3-rec] | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na]  | ![not allowed][icon3-na]  |
| Content Resources    | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na]  | ![recommended][icon3-rec] |
{: .api-table #table-reqs-1}

|                      | provider                  | thumbnail                 | placeholderTextFrame        | accompanyingTextFrame       |
| -------------------- | ------------------------- | ------------------------- | ------------------------ | ------------------------ |
| Collection           | ![recommended][icon3-rec] | ![recommended][icon3-rec] | ![optional][icon3-opt]   | ![optional][icon3-opt]   |
| Manifest             | ![recommended][icon3-rec] | ![recommended][icon3-rec] | ![optional][icon3-opt]   | ![optional][icon3-opt]   |
| TextFrame               | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt]\* | ![optional][icon3-opt]\* |
| Annotation           | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| AnnotationPage       | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| Range                | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![optional][icon3-opt]   | ![optional][icon3-opt]   |
| AnnotationCollection | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| Content Resources    | ![optional][icon3-opt]    | ![optional][icon3-opt]    | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
{: .api-table #table-reqs-1a}

\*A TextFrame that is the value of a `placeholderTextFrame` or `accompanyingTextFrame` property may not have either of those properties itself.<br/>

__Technical Properties__

|                       | id                      | type                   | format                   | profile                  | height                   | width                    | duration                  | viewingDirection         | behavior               | timeMode                 |
| --------------------  | ----------------------- | ---------------------- | ------------------------ | ------------------------ | ------------------------ | ------------------------ | ------------------------- | ------------------------ | ---------------------- | ------------------------ |
| Collection            | ![required][icon3-req]  | ![required][icon3-req] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na]  | ![optional][icon3-opt]   | ![optional][icon3-opt] | ![not allowed][icon3-na] |
| Manifest              | ![required][icon3-req]  | ![required][icon3-req] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na]  | ![optional][icon3-opt]   | ![optional][icon3-opt] | ![not allowed][icon3-na] |
| TextFrame                | ![required][icon3-req]  | ![required][icon3-req] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![optional][icon3-opt]\* | ![optional][icon3-opt]\* | ![optional][icon3-opt]    | ![not allowed][icon3-na] | ![optional][icon3-opt] | ![not allowed][icon3-na] |
| Annotation            | ![required][icon3-req]  | ![required][icon3-req] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na]  | ![not allowed][icon3-na] | ![optional][icon3-opt] | ![optional][icon3-opt]   |
| Annotation Page       | ![required][icon3-req]  | ![required][icon3-req] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na]  | ![not allowed][icon3-na] | ![optional][icon3-opt] | ![not allowed][icon3-na] |
| Range                 | ![required][icon3-req]  | ![required][icon3-req] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na]  | ![optional][icon3-opt]   | ![optional][icon3-opt] | ![not allowed][icon3-na] |
| Annotation Collection | ![required][icon3-req]  | ![required][icon3-req] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na]  | ![not allowed][icon3-na] | ![optional][icon3-opt] | ![not allowed][icon3-na] |
| Content Resources     | ![required][icon3-req]  | ![required][icon3-req] | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![optional][icon3-opt]    | ![not allowed][icon3-na] | ![optional][icon3-opt] | ![not allowed][icon3-na] |
{: .api-table #table-reqs-2}

\*If a TextFrame has either of `height` and `width`, it _MUST_ have the other, as described in the [definitions][prezi30-height] of those properties.<br/>


__Linking Properties__

|                       | seeAlso                 | service                 | homepage               | rendering              | partOf                 | start                    | supplementary            | services                 |
| --------------------  | ----------------------- | ----------------------- | ---------------------- | ---------------------- | ---------------------- | ----------------------   | ------------------------ | ------------------------ |
| Collection            | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| Manifest              | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| TextFrame                | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| Annotation            | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| Annotation Page       | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| Range                 | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![not allowed][icon3-na] |
| Annotation Collection | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| Content Resources     | ![optional][icon3-opt]  | ![optional][icon3-opt]  | ![optional][icon3-opt] | ![optional][icon3-opt] | ![optional][icon3-opt] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
{: .api-table #table-reqs-3}


__Structural Properties__

|                       | items                      | structures                 | annotations                |
| --------------------- | -------------------------- | -------------------------- | -------------------------- |
| Collection            | ![required][icon3-req]     | ![not allowed][icon3-na]   | ![optional][icon3-opt]     |
| Manifest              | ![required][icon3-req]     | ![optional][icon3-opt]     | ![optional][icon3-opt]     |
| TextFrame                | ![recommended][icon3-rec]  | ![not allowed][icon3-na]   | ![optional][icon3-opt]     |
| Annotation            | ![not allowed][icon3-na]   | ![not allowed][icon3-na]   | ![not allowed][icon3-na]   |
| Annotation Page       | ![recommended][icon3-rec]  | ![not allowed][icon3-na]   | ![not allowed][icon3-na]   |
| Range                 | ![required][icon3-req]     | ![not allowed][icon3-na]   | ![optional][icon3-opt]     |
| Annotation Collection | ![not allowed][icon3-na]   | ![not allowed][icon3-na]   | ![not allowed][icon3-na]   |
| Content Resources     | ![not allowed][icon3-na]   | ![not allowed][icon3-na]   | ![optional][icon3-opt]     |
{: .api-table #table-reqs-4}


__Behavior Values__

|                 | Collection               | Manifest                 | TextFrame                   | Range                    |
| --------------- | ------------------------ | ------------------------ | ------------------------ | ------------------------ |
| auto-advance    | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![optional][icon3-opt]   |
| continuous      | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| facing-pages    | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![optional][icon3-opt]   | ![not allowed][icon3-na] |
| individuals     | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| multi-part      | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| no-auto-advance | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![optional][icon3-opt]   |
| no-nav          | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| no-repeat       | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| non-paged       | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![optional][icon3-opt]   | ![not allowed][icon3-na] |
| hidden \*       | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| paged           | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| repeat          | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| sequence        | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| thumbnail-nav   | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![optional][icon3-opt]   |
| together        | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![not allowed][icon3-na] | ![not allowed][icon3-na] |
| unordered       | ![optional][icon3-opt]   | ![optional][icon3-opt]   | ![not allowed][icon3-na] | ![optional][icon3-opt] |
{: .api-table #table-reqs-5}

\* `hidden` is allowed on Annotation Collections, Annotation Pages, Annotations, Specific Resources and Choices, as these are the classes that result in rendering content to the user.


### B. Example Manifest Response

```json
{
    "@context": "https://textframe.io/api/presentation/1/context.json",
    "id": "https://example.org/itf/CASE20258/diplomatic/info.json",
    "type": "Manifest",
    "label": {
        "en": ["CASE20258: Horary consultation concerning Goodman Giles Stokes (PERSON16187) (19 January 1602 at 10:00)"]
    },
    "requiredStatement": {
        "label": {
            "en": ["Attribution"]
        },
        "value": {
            "en": ["Cite this as: Lauren Kassell, Michael Hawkins, Robert Ralley, John Young, Joanne Edge, Janet Yvonne Martin-Portugues, and Natalie Kaoukji (eds.), ‘CASE20258’, The casebooks of Simon Forman and Richard Napier, 1596–1634: a digital edition, https://casebooks.lib.cam.ac.uk/case/CASE20258, accessed 18 July 2024."]
        }
    },
    {
    "navDate": ["1602-01-19T10:00:00"]}
    "metadata": [
        {
            "label": {
                "en": ["Author"]
            },
            "value": {
                "en": ["Mr Richard Napier [Sandy] [Richard Napier [Senior]]"]
            }
        },
        {
            "label": {
                "en": ["Author"]
            },
            "value": {
                "en": ["Mr Gerence James [Marks]"]
            }
        },
        {
            "label": {
                "en": ["Date"]
            },
            "value": {
                "en": ["19 January 1602 at 10:00"]
            }
        },
        {
            "label": {
                "en": ["Source"]
            },
            "value": {
                "en": ["MS Ashmole 404, f. 268v (upper left part of page)"]
            }
        }
    ],
    "summary": [],
    "partOf": [],
    "seeAlso": [],
    "items": [
        {
            "id": "https://example.org/itf/CASE20258/diplomatic/book/frame/full",
            "type": "TextFrame",
            "label": {
                "en": ["LABEL GOES HERE"]
            },
            "items": [
                {
                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/1",
                    "type": "AnnotationFrame",
                    "items": [
                        {
                            "id": "https://example.org/itf/CASE20258/diplomatic/book/annotation/full/1",
                            "type": "Annotation",
                            "motivation": "text",
                            "body": {
                                "id": "https://example.org/itf/CASE20258/diplomatic/book/full/rich/html",
                                "type": "html",
                                "format": "text/html"
                            }
                        }
                    ],
                    "annotations": [
                        {
                            "id": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest/1",
                            "type": "AnnotationFrame",
                            "items": [
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/1",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/1/document-type",
                                        "type": "TextualBody",
                                        "value": "Case -- Entry -- Horary",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "DocumentType"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/2",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/2/shelfmark",
                                        "type": "TextualBody",
                                        "value": "MS Ashmole 404",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Shelfmark"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/3",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/3/volume-name",
                                        "type": "TextualBody",
                                        "value": "Napier, vol. 05",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "VolumeName"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/4",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/4/damaged",
                                        "type": "TextualBody",
                                        "value": "No",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Damaged"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/5",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/5/deleted",
                                        "type": "TextualBody",
                                        "value": "No",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Deleted"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/6",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/6/practitioner-identified",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PractitionerIdentified"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/7",
                                    "type": "Annotation",
                                    "motivation": "identifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/7/practitioner",
                                        "type": "TextualBody",
                                        "value": "Mr Richard Napier [Sandy] [Richard Napier [Senior]] (PERSON5218)",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Practitioner"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/8",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/8/practitioner-identified",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PractitionerIdentified"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/9",
                                    "type": "Annotation",
                                    "motivation": "identifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/9/practitioner",
                                        "type": "TextualBody",
                                        "value": "Other -- Mr Gerence James [Marks] (PERSON13172)",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Practitioner"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/10",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/10/number-of-practitioners",
                                        "type": "TextualBody",
                                        "value": "Multiple practitioners",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "NumberOfPractitioners"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/11",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/11/querent-identified",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "QuerentIdentified"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/12",
                                    "type": "Annotation",
                                    "motivation": "identifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/12/querent",
                                        "type": "TextualBody",
                                        "value": "Goodman Giles Stokes (PERSON16187)",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Querent"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/13",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/13/querent-age-band",
                                        "type": "TextualBody",
                                        "value": "050-059 -- 50",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "QuerentAgeBand"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/14",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/14/querent-sex",
                                        "type": "TextualBody",
                                        "value": "Male",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "QuerentSex"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/15",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/15/number-of-querents",
                                        "type": "TextualBody",
                                        "value": "Single querent",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "NumberOfQuerents"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/16",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/16/patient-identified",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PatientIdentified"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/17",
                                    "type": "Annotation",
                                    "motivation": "identifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/17/patient",
                                        "type": "TextualBody",
                                        "value": "Goodman Giles Stokes (PERSON16187)| OR LINK TO PERSON PAGE/URI",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Patient"
                                    },
                                    "target": "ITF-RANGE-INDICATING STRING // WHOLE PAGE FOR US"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/18",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/18/patient-age-band",
                                        "type": "TextualBody",
                                        "value": "050-059 -- 50",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PatientAgeBand"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/19",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/19/patient-sex",
                                        "type": "TextualBody",
                                        "value": "Male",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PatientSex"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/20",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/20/number-of-patients",
                                        "type": "TextualBody",
                                        "value": "Single patient",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "NumberOfPatients"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/21",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/21/astrologer-identified",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "AstrologerIdentified"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/22",
                                    "type": "Annotation",
                                    "motivation": "identifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/22/astrologer",
                                        "type": "TextualBody",
                                        "value": "Mr Richard Napier [Sandy] [Richard Napier [Senior]] (PERSON5218)",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Astrologer"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/23",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/23/practitioner-is-astrologer",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PractitionerIsAstrologer"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/24",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/24/astrologer-identified",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "AstrologerIdentified"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/25",
                                    "type": "Annotation",
                                    "motivation": "identifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/25/astrologer",
                                        "type": "TextualBody",
                                        "value": "Other -- Mr Gerence James [Marks] (PERSON13172)",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Astrologer"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/26",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/26/practitioner-is-astrologer",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PractitionerIsAstrologer"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/27",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/27/number-of-astrologers",
                                        "type": "TextualBody",
                                        "value": "Multiple astrologers",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "NumberOfAstrologers"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/28",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/28/practitioner-is-querent",
                                        "type": "TextualBody",
                                        "value": "No",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PractitionerIsQuerent"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/29",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/29/practitioner-is-patient",
                                        "type": "TextualBody",
                                        "value": "No",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PractitionerIsPatient"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/30",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/30/date",
                                        "type": "TextualBody",
                                        "value": "1600-1609 -- 1602 -- 01 -- 19",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Date"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/31",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/31/time-of-day",
                                        "type": "TextualBody",
                                        "value": "AM -- 10:00-10:59 -- 00:00-14:59",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "TimeOfDay"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/32",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/32/practice",
                                        "type": "TextualBody",
                                        "value": "Napier",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Practice"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/33",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/33/practice",
                                        "type": "TextualBody",
                                        "value": "Napier",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Practice"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/34",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/34/patient-present",
                                        "type": "TextualBody",
                                        "value": "Unclear",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PatientPresent"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/35",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/35/how-did-it-take-place",
                                        "type": "TextualBody",
                                        "value": "Unclear -- At unspecified location",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "HowDidItTakePlace"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/36",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/36/querent-present",
                                        "type": "TextualBody",
                                        "value": "Unclear",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "QuerentPresent"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/37",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/37/patient-knowledge",
                                        "type": "TextualBody",
                                        "value": "Patient asked the question",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PatientKnowledge"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/38",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/38/patient-consent",
                                        "type": "TextualBody",
                                        "value": "Patient asked the question",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PatientConsent"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/39",
                                    "type": "Annotation",
                                    "motivation": "classifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/39/question-asked",
                                        "type": "TextualBody",
                                        "value": "Medical -- Not specified in question",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "QuestionAsked"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/40",
                                    "type": "Annotation",
                                    "motivation": "classifying",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/40/question-asked",
                                        "type": "TextualBody",
                                        "value": "Medical -- Witchcraft and devil -- Witchcraft suffered (medical)",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "QuestionAsked"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/41",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/41/treatment",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Treatment"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/42",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/42/judgment",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Judgment"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/43",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/43/recipe",
                                        "type": "TextualBody",
                                        "value": "No",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Recipe"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/44",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/44/info",
                                        "type": "TextualBody",
                                        "value": "Payment",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Info"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/45",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/45/has-astro-chart",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "HasAstroChart"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/46",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/46/info",
                                        "type": "TextualBody",
                                        "value": "Astrological chart -- Present",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Info"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/47",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/47/has-geo-chart",
                                        "type": "TextualBody",
                                        "value": "No",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "HasGeoChart"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/48",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/48/querent-is-asking-about",
                                        "type": "TextualBody",
                                        "value": "Self",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "QuerentIsAskingAbout"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/49",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/49/patient-assumed",
                                        "type": "TextualBody",
                                        "value": "Yes",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "PatientAssumed"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/50",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/50/hands",
                                        "type": "TextualBody",
                                        "value": "Mr Gerence James [Marks]",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Hands"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/51",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/51/language",
                                        "type": "TextualBody",
                                        "value": "English",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "Language"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                },
                                {
                                    "@context": "http://www.w3.org/ns/anno.jsonld",
                                    "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/52",
                                    "type": "Annotation",
                                    "motivation": "asserting",
                                    "body": {
                                        "id": "https://example.org/itf/CASE20258/diplomatic/book/full/annotation/52/extent-of-transcription",
                                        "type": "TextualBody",
                                        "value": "Excerpt",
                                        "format": "text/html",
                                        "language": "en",
                                        "purpose": "ExtentOfTranscription"
                                    },
                                    "target": "https://example.org/itf/CASE20258/diplomatic/book/full/manifest"
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ]
}
```

### D. Acknowledgements

The base text for the ITF Presentation API was derived from the [IIIF Presentation API](https://github.com/IIIF/api). We’d like to thank all those involved for creating such a great foundation upon which our work could be raised.
