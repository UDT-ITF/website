---
layout: page
title: Unlocking Digital Texts Position Paper
subtitle: Digital texts are all around us - from the objects and outputs of scholarly study, to the news, social media and other texts we interact with everyday online. But how long will these digital texts last? The problem with digital texts is that they come in a variety of formats that are hard to reuse because they require technical skills or infrastructure beyond most researchers’ capabilities and funding. This project aims to scope and define an Interoperable Text Format (ITF) as a means for accessing and delivering text that is stored in a variety of extant formats as well as referencing text fragments. Importantly, we are aiming for something that is both human- and computer-readable.
permalink: /publications/2022-position-paper/
hero:
  image: ""
anchor_headings: true
---
(% include blocks/tabs.html  tabs=page.top_tabs %})
(( theme.block-start ))
# Project Approach

The project will take place in three phases.

The initial phase, *research*, will conduct a series of workshops, interviews and meetings with researchers and technologists from other text-based projects with the aim of expanding the experience of the project partners and starting the process of community-building. In particular:
- Collecting additional use cases and requirements for a text-fragment application Programming Interface (API) over and above those from the projects currently involved. This will enable us to gain a broader and more representative picture of the needs of the target audience, and will also help focus the projects on outcomes that are most useful to the community.
- Identifying useful technologies in use by other projects that could be re-used or adapted to help deliver the project’s aims, to avoid repeating existing work.
- Identifying potential collaborations to co-develop or co-test aspects of the project in order to strengthen the validity of the proof-of-concept deliverables.

The second phase, *design*, will construct the specification for an Interoperable Text Framework (ITF) based on the outputs of the initial phase. The framework will include both data structures and API’s, and aim to be compatible with existing standards such as the International Image Interoperability Framework ([IIIF](https://iiif.io/)), and the Text Encoding Initiative ([TEI](https://tei-c.org/)) as much as possible. As a community endeavour, this work will be conducted in an open manner using [GitHub](https://github.com/) and related tools and encouraging contributions and suggestions from others. 

The final phase, *development*, will develop proof-of-concept implementations of the core parts of ITF, based on the resources and software stacks of the participating projects. The ultimate aims of this phase are two-fold:
- To demonstrate the technical feasibility and implementability of ITF.
- To establish that it meets enough of the scholarly and sustainability needs of the participating resources (and the wider textual scholarship community) to be a worthwhile ongoing investment.  

In order to frame this work, the aims of Unlocking Digital Texts can be distilled into answering two fundamental questions which approach the same problem from two different angles, detailed in the next sections. We would anticipate running some workshops that focus on only one of these questions, while other workshops  might consider both angles. 

# Bottom up: How do we construct a useful text and text fragment reference mechanism that we can use as the basis for an API?

## Text Referencing

Texts evolve over time, and digital texts especially so. At the whole-text level, we therefore need to be able to store and address multiple versions. There are actually potentially two dates that can be used to define a text version:
- The Citation Date – which reflects our knowledge about a textual entity at a particular point in time
- The Version Date – which reflects the state of a text at a particular point in its own evolutionary timeline.

These dates may coincide for a contemporary, evolving text, but will diverge for a historical text series that is being actively researched. This case may be infrequent enough that it does not warrant the additional complexity at this stage. There are existing mechanisms for handling referencing by Citation Date, such as Memento and DataCite DOI versioning (with caveats, since versions have text labels rather than being strictly date-based).
    
## Fragment Addressing
This bottom-up approach starts by considering the range of text file formats under consideration for this proof-of-concept phase. These include:
- Plain text in Unicode formats, in the expectation that other encodings can be losslessly converted to Unicode
- Structured texts with embedded markup which remain interpretable with a basic text editor. The following are in scope for this project:  
  - TEI XML, focussing on the variants used by the contributing projects although collaborating projects are welcome to experiment
  - TeX, used when mathematical material needs to be correctly formatted  
  - Markdown, increasingly commonly used to drive both websites and online documentation
- Complex formats, which can include embedded binary objects, requiring specialised software to display or otherwise interact with.  
  - Because of its ubiquity for digital publishing, PDF/A is favoured for archival copies of documents since it avoids some of the complexities of handling arbitrary PDF versions and feature sets.
  
From these formats, it is definitely possible to introduce a “glyph-level”[^1] fragment addressing scheme, comprising an offset from the start of the file. This effectively reduces all text formats to plain-text by stripping away any additional tagging and non-textual components. This is not an entirely trivial exercise, since some additional complexities around Unicode normalisation rules and white-space handling will need to be dealt with, in order to ensure that plain-text conversions are carried out in a consistent manner.

However, at this stage, it appears that it would be advantageous to also have a higher level scheme that operates in a more “human-friendly” way, with word (or token) granularity and some sense of semantic structure at a level similar to Markdown or a light-TEI schema. The extent to which this is technically possible, and whether there might need to be multiple such addressing modes based on common textual forms, needs further investigation. This could define a minimal structural TEI subset that we could deliver regardless of source format. This needs to be carefully thought out since digitised texts have an inherent dichotomy between physical (page/line) and semantic (chapter/paragraph) modes of reference. A notable peculiarity of text-referencing is that there is a need to target the spaces between words or characters, to identify where material may have been inserted and deleted.  

Text-miners have produced tools for reducing specific formats to plain-text, but those that seek to retain some semantic structure are far less common.

## Content Negotiation  
Given the ability to reference text fragments in a consistent manner that can be implemented as a RESTful[^2] API, we must consider what the results of invoking such an API should be. Taking inspiration from the way IIIF handles images, some form of content negotiation would enable text to be returned in a form suitable for subsequent processing. Plain-text, HTML and a suitably defined minimal XML[^3] would seem to be useful forms that can be readily generated from most sources (and Markdown perhaps?). The option to return “the original format” would seem to make sense only at a complete text level since formats such as PDF, TeX and general TEI-XML only really work at that level.     

# Top Down: Given that we have an actionable text version and fragment addressing mechanism, what higher level constructs are useful:

## Web Annotation as the Basic Extension Mechanism
Once a basic fragment addressing scheme is implemented as a RESTful API, text fragments can be specified online via suitably structured web addresses. Thus, they can be referenced as Linked Data entities, and specifically, as targets for annotations using the Web Annotation Data Model (WADM). As the annotations themselves can be RDF nodes, this mechanism can form the basis for both layered hierarchies of annotations and threaded modes of discourse, enabling many of the higher level constructions needed to address the various scholarly and presentational use cases identified by the project.

WADM also defines the idea of annotation-list, which provides an aggregation mechanism for individual annotations. This is an important feature for sorting and grouping annotations from different sources, or that address different use-cases. 

An initial selection of these use-cases are expanded in the following sections: 

## Basic Scholarly Annotation

The simplest use of the WADM is to provide a mechanism for representing existing annotation paradigms, such as footnotes, marginalia or embedded references. However, WADM is capable of handling considerably more flexible and complex use cases:
- Annotations can include markup, formatting and links to other resources including images, videos and other content.
- They are capable of indicating a precise fragment of text for any given annotation
- They can be classified to determine both how and when they might be displayed
  - If appropriate, they can be rendered in a more conventional form, such as a footnote, depending on the form in which the text is accessed
- Separate annotations can reference blocks of text that overlap or contain one-another
- An annotation can reference several disparate locations within a text
- Annotations can be attributed individually or as a set defined by an annotation-list
- An annotation-list can be published separately from the underlying text as a derivative work. It does, however, need to be re-integrated with the text in order to be understood.    

## Higher-Level Fragment Addressing
For an effective generic protocol, fragment addressability needs to be sufficiently granular to accommodate a broad range of sources and applications. However, for specific use-cases, it may be useful to define higher-level addressable constructs based on aggregating such lower-level fragments. Annotations specifying multiple target fragments can be used to identify these larger fragments. Multiple schemes can be coincident on an underlying text, representing different approaches to textual structure, each aggregated into annotation-lists for management, display and usage.

## Machine-generated Annotations
Many analytical approaches to texts operate by identifying particular indicative textual components through the application of linguistic models constructed from processing larger relevant corpora. Examples include named-entity extraction, stylometry and sentiment analysis. Rather than using these approaches to embed tags into a text document, making it difficult to combine several analyses, an alternative is to instead generate annotation-lists that leave the underlying text untouched. Segregating annotations in this way allows them to be treated as separate layers of information overlying the text beneath,each of which can be viewed or hidden as required. Subsequent statistical processing of these lists is much simpler than extracting and processing tagged entities from marked-up text.

In addition, new machine-learning models can be iteratively created starting with human-annotated texts as an initial training set, using the algorithms to annotate new texts, then correcting the new annotations and, in turn, using them as an improved training set. Because annotations can indicate their source, a researcher can always understand whether information has been provided by a human or an algorithm, and potentially any corrections that may have taken place (which may, for example, indicate edge-cases where the accuracy of a model diminishes).       

## Parallel Texts
The previous examples have focussed on annotating texts individually, regardless of the source. However, a couple of common use cases concern the linking of parallel versions of texts.

The first case concerns linking different versions of the same text in different scripts/languages – often as a result of a digitisation process. It would be useful to navigate between the same locations in each of the text versions, both at the editorial stage, but also for subsequent scholarly commentary. However, the granularity and nature of the mapping between versions varies. Literal transcriptions could map to a transliteration at the level of an individual glyph or small group of glyphs if there is a well defined and reversible set of transliteration rules. Other translations would only be capable of mapping to a higher level, such as token, word or even sentence.  

The second case arises when using genetic approaches that bring together diverging witnesses to a hypothetical underlying original text. Here, different text fragments may appear in one or more of the witnesses and be absent in others. Analytical tools such as CollateX could readily generate annotation-lists that identify common and distinct fragments ready for further quantitative analysis. 
## Text as Process

Another emerging approach attempts to reconstruct the processes by which texts are created, considering any given witness to be the aggregate of a series of text editing actions performed on a previous version (or a blank document). Original manuscripts with extant revisions can be expressed through an editorial narrative that not only links different versions together but also can be expanded within a version. However, the strength of this approach really emerges when considering negotiated documents such as contracts and treaties where multiple authors and versions may exist concurrently. Online document editing systems that support multiple concurrent authors and “unlimited-undo”, such as Google Docs, store exactly this sort of editorial event stream, “flattening” the data into a more conventional document format only when a copy needs to be downloaded or printed.   

Editorial event streams can be readily mapped to our annotation model since annotations can reference locations within one or more texts and include time and attribution information. In annotation form, these edits can be readily rearranged to suit analytical or presentational needs – for example, sorting them temporally, thematically or by author.   

## Fragment Lists as Datasets
Earlier sections alluded to the use of WADM annotation-lists to organise annotations in various ways. Implicit in each list is an additional dataset comprising the set of text fragments referenced by the annotations in that list. Depending on the manner in which the annotations were generated, these fragment collections may form useful pre-filtered Sources  suitable for further analysis. Thus, there may be a case for providing access to fragment lists in a more efficient manner than parsing the associated annotation lists.  

## Intertextual Narratives
The previous sections have focussed on the use of WADM as a framework for close-reading or analysis of texts. Annotations are based on Linked Data[^4], and thus allow, even encourage, linking to external entities. By using WADM, ITF will allow users to create richly contextualised materials. For example, linking textual fragments associated with named-entities in a text to the relevant entries in authority sources or gazetteers[^5]. The possibilities are almost endless since users can make links to a wide range of information, such as:
- Historical, biographical or prosopographical events
- Definitions or explanations
- Expansions of references
- Related documents, or parts thereof. 

In particular, the ability to link between related parts of documents over time allows the construction of narrative threads that tie them together. This mechanism can be used to analyse and map the development of an idea or theme over time and space through epistolic conversation, published works or private writings. 

## Transitivity
Where multiple related texts are linked via fragment annotations, it can be useful for annotations on one text fragment to also refer to corresponding fragments in other texts. While it is possible for these annotations to refer to all relevant text fragments, updating every annotation whenever a new text is added to the collection can be cumbersome. It might even be impossible if the annotations belong to someone else. It is essential, therefore, that the annotations linking the texts together are parseable so that they can be used to identify all the potentially relevant annotations that refer to the fragment in question. The utility of this capability is naturally dependent on the type of information involved, and is probably more useful for more ‘distant’ information, such as contextualising links discussed previously.     

## IIIF Alignment and Integration
Another key use case for ITF is in conjunction with images and other media made available via the International Image Interoperability Framework (IIIF). IIIF uses a conceptually similar fragment and annotation model to make image collections easier to access and reuse. Currently, however, display of textual content in conjunction with IIIF images is limited to the body text in an annotation and linking to specially coded TEI material.

ITF aims to greatly expand the scope for linking texts and IIIF resources, enriching them with the results of textual methods and taking advantage of the presentational strengths of IIIF. With this in mind, ITF aims to be compatible with, but not dependent on, IIIF. 

Neil Jefferies (PI), Bodleian Libraries, University of Oxford

Michael Hawkins (Co-PI), Digital Humanities, University of Cambridge

Robert Ralley, Cambridge Digital Humanities, University of Cambridge

Scott B. Weingart (Co-PI), Navari Family Center for Digital Scholarship, University of Notre Dame

Robert Goulding (Co-PI), Reilly Center for Science, Technology, and Values, University of Notre Dame.

Natalie Meyers (Co-PI), Lucy Family Institute for Data & Society, University of Notre Dame

Dirk Van Hulle (Co-PI), Oxford Centre for Textual Editing and Theory (OCTET), University of Oxford

Caterina Agostini, Navari Family Center for Digital Scholarship, John J. Reilly Center for Science, Technology and Values, University of Notre Dame

Arnaud Zimmern, Navari Family Center for Digital Scholarship, University of Notre Dame

[^1]: A glyph is defined for our purposes as a distinct symbol or mark corresponding to a defined normalised Unicode code-point. Normalisation ensures that modifier and diacritic codes have been applied to the base characters to give a consistent Unicode representation. 
[^2]: Representational State transfer, see [https://en.wikipedia.org/wiki/Representational_state_transfer](https://en.wikipedia.org/wiki/Representational_state_transfer) 
[^3]: One of the lighter TEI variants would be an obvious candidate here.
[^4]: “Linked Data” rather than “Open Linked Data” is the term used here since textual content does not need to be Open for these methods to be applicable.
[^5]: Indices of geospatial names,relating them to geospatial coordinates and, sometimes, epoch and/or context  
(( theme.block-end ))
