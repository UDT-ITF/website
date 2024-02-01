# Unlocking Digital Texts - Oxford Workshop


## Background

Much digital text exists in the form of ‘digital paper’, skeuomorphic surrogates for physical documents, such as books, articles and reports, that do little to leverage the potential of the digital world for new forms of analysis and discourse. In these forms, the visual appearance of text to contemporary human readers has primacy over accurate framing of information content and meaning. Consequently, such material is relatively poorly represented in the linked world of the internet where other forms such as images and multimedia may be much more easily enriched by tagging, linking and annotation.       

Other born digital textual forms, often designed for collaborative creation and consumption[^1], largely eschew the notion of a document in the traditional sense. Instead, texts are time-dependent collections of fragments from different authors which may include software agents. The meaning of these fragments depends almost entirely on their relationships to other fragments in the collection, and is consequently also potentially time-dependent. Such material is resistant to document-centric approaches to analysis and criticism.    

Although scholarly and cultural discourse is increasingly taking place using a variety of textual and non-textual digital channels, the ability to analyse such discourse and construct narratives across diverse media has not kept pace. The Unlocking Digital Texts (UDT) project[^2] seeks to tackle some of the technical barriers to addressing this shortfall by proposing an Interoperable Text Framework (ITF). 


## ITF and the International Image Interoperability Framework 

As discussed in our earlier position paper[^3], the ITF approach draws its inspiration from the International Image Interoperability Framework[^4] (IIIF) in that it aims to define two distinct mechanisms:

1. A mechanism for specifying and retrieving a fragment of text from a textual source that does not depend on the format in which the underlying text is stored, analogous to the IIIF Image API[^5]. This mechanism can also be used as the basis for the precise citation of digital texts.  

2. A mechanism analogous to the IIIF Presentation API that allows multiple text fragments from different sources to be brought together for the purposes of display, analysis or narrative construction. 

It would also be useful if ITF were compatible and interoperable with IIIF which introduces a number of additional requirements:

* Many texts are derived from digitised resources, and being able to link to source images would be a valuable element of provenance. 
* ITF should support the increasing number of workflows and tools already existing for transcribing, transliterating and translating IIIF resources supporting both manual and machine-learning approaches.
* The data model for ITF should be based on Linked Data[^6] concepts with JSON[^7] as the preferred serialisation for data transmission and storage
* An ITF text assembly should be amenable to annotation using the Web Annotation Data Model[^8] (WADM) and, in common with IIIF, the mechanism for specifying the relationships between multiple text fragments should also use the same mechanism. 

## The Oxford Workshop

The Oxford Workshop held on 26 Jan 2023 brought together interested parties representing a wealth of text-oriented projects and resources to consider the first mechanism above: “How do we construct a useful text and text fragment reference mechanism that we can use as the basis for an API?”. 

Our attendees were drawn from a wide range of disciplines and institutions to ensure that the discussion could consider the richness of text access mechanisms in current use. Participants presented projects or resources that made use of text fragments, explained how fragments were implemented and the strengths and weaknesses of their approaches. Large textual infrastructure projects such as CLARIN[^9], DARIAH[^10] and Text+[^11] in Germany were also represented.

Attendees included:

    Megan Gooch, Michael Hawkins, Neil Jefferies, Nilo Pedrazzini, Rob Ralley, and Dirk Van Hulle (Unlocking Digital Texts)
    Hennie Brugmann, Dirk Roorda (Koninklijke Nederlandse Akademie van Wetenschappen)
    Nicholas Cole (The Quill, Project, University of Oxford)
    Andrew Cusworth (Bodleian Libraries)
    Yasmin Faghihi, Huw Jones (University of Cambridge)
    Mathias Göbel (Georg-August-Universität Göttingen)
    John Moore (The National Archives, UK)
    Jyrki Niemi, Jussi Piitulainen (University of Helsinki)
    Marina Toumpouri (Church of Cyprus/University of Cyprus)
    Kalle Westerling (The Turing Project)
    Andreas Witt (Leibniz-Institut für Deutsche Sprache, Universität Mannheim)

## What is a Text?      

There was general agreement with the thesis that there is not necessarily a simple correspondence between a textual resource as the target for referencing and retrieval, and a single file. Projects such as Quill[^12] can store multiple concurrent versions of a single document in the form of an editorial event stream, similar to the way Google Docs and Microsoft Office 365 operate. On the other hand, Casebooks[^13] stores fragments pertaining to individual medical cases as a more useful representation of the content than the books as complete documents. Corpus approaches such as Text Fabric[^14] and the Open Corpus Workbench potentially disaggregate documents into even smaller components. 

Consequently, the “Text” that is the target for ITF should represent a an entity more akin to a bibliographic “Work” in the library-focussed BibFrame[^15] model. Each such text should be able to be treated as a sequential series of characters so that locations within the text can be easily specified. More complex documents, such as those with marginalia or other annotations, would comprise several sequential texts. The nature of the mechanism for specifying the relationship between multiple texts will be the subject of the later workshop at Cambridge. There is no reason why a given text fragment may not appear in multiple texts.     

In order to enable long term access and reliable citation, texts should have a persistent identifier such as a DataCite[^16] DOI[^17] although ITF need not prescribe suitable identifiers. How an online resource translates from its underlying storage formats to deliver a requested text is an implementation detail outside the scope of ITF. 

## Versioning

As described in the position paper, and confirmed during discussions at the workshop, versioning is both complex and necessary for an API concerned with providing access to persistent, long-lived text resources. Some resources that do not currently support versioning, such as the Language Bank of Finland[^18] and Fihrist[^19], observed that this meant that some analytical processes were more cumbersome than might otherwise be the case.  The workshop identified two main reasons why a text may occur in multiple versions. 

1. A text that represents the current state of knowledge of a historical document may be updated over time to reflect the ongoing scholarly process. For example, a translation may improve, or a reconstruction based on multiple partial witnesses may change in the light of new discoveries.
2. A text that arises from approaches that seek to reconstruct the processes by which it had been created, will need to be accessed at arbitrary points in its history. These versions may be very granular, as seen in the Quill project, or a coarser level seen in genetic editions commonly encoded using TEI[^20]. A digital text that is part of an ongoing process such as an evolving Twitter stream is the extreme example of this case - additions, deletions and other changes may occur at any time. Our knowledge about the reconstructed process of text creation can change over time, so both reasons may apply in some cases. 
	
The first versioning mechanism indicates the version of an ITF text as a cite-able intellectual entity, since it represents the view we have of the text, its respective versions, and processes, at a particular time. A mechanism for retrieving an online resource as it existed at a specific time already exists in the form of Memento[^21]. As a result of the second mechanism, at any particular time an ITF text may contain various internal versions that represent the stages of its creative process as it is understood at that point. In order to ensure the referential integrity of citations and fragment references, it may be necessary to know both the version of a text (internal) and the date of the citation. ITF will need to define clear versioning nomenclature to avoid confusion.

Furthermore, the Quill project can identify the existence of multiple concurrent versions of a document being worked on by different authors, so internal versions need to be represented by a graph rather than a simple sequence. This also requires a more generic labelling mechanism for internal versions although date should remain an option for simpler cases. Of course, in many cases an ITF text will comprise a single static version so ITF will need an informational API to indicate if multiple versions exist and how they are identified for access.

## Specifying Text Fragments

### Glyph-level Text Coordinates

To simplify matters, ITF texts will be assumed to be encoded using NFKC[^22] normalised Unicode[^23] since this has the broadest software support and is, indeed, the default for most modern systems. A portion of any sequential text can always be specified and  identified in terms of glyph level coordinates, where a glyph is a displayed symbol including any diacritics or other modifiers corresponding to a normalised Unicode code-point. At a file level, it is possible to be more granular with un-normalised Unicode but this is not robust and hard to implement reliably for users.

For ITF, any fragment retrieved using higher level coordinates will be accompanied by an information  “canonical reference” that indicates how to access the same fragment in glyph-level coordinates. This is analogous to the canonical reference mechanism in IIIF which reduces references to pixel-level coordinates.

### Higher-level Text Coordinates

Higher level text coordinate systems can always be specified in terms of the glyph level coordinate system. A logical mechanism to implement these would be to use standoff annotation to indicate how higher level abstractions relate to glyph-level fragments, which has similarities to the mechanisms of Text-Fabric[^24] or VRT[^25]. Such coordinate systems need not be hierarchical and multiple systems may be implemented over an ITF text. However, for users it will be useful for ITF to define ontologies for a few commonly used core systems for both physical (e.g. volume, page, line, token) and semantic (e.g. book, chapter, paragraph, sentence, word) forms. As with versions, ITF will need to be able to indicate which systems are implemented for a given text.

This standoff approach also allows third parties to specify additional systems to reflect specific use cases or non-standard forms, without the need to modify the underlying ITF resource. A number of resources (e.g. CUDL) currently use embedded XML tagging to identify higher level textual structures but the UK National Archives and the BHumanties Cluster in the Netherlands have experimented with code that converts such tagging into Web Annotation, targeting ALTO and TEI respectively.  

## Content Negotiation

As discussed in the position paper, and in common with IIIF, ITF will be agnostic about the source format of the material that it delivers but will also support a variety of delivery formats for the requested text. CUDL[^26], among others, identified this as a requirement for ITF, and already implement this in a limited fashion with their existing API. It is anticipated that ITF will support a number of text delivery formats:

* Plain text (UNICODE UTF-8)
* A constrained subset of TEI
* An HTML fragment
* Raw tagged data from the source material (optional, depends on the source)

In order to generate formats other than plain text there will need to be additional tagging or annotation available to provide the necessary information. The HTML and TEI formats will only include markup relevant to text fragment addressing, indicating higher level text coordinate structures in the returned text. Augmenting the markup further, to display particular aspects of the text, is beyond the scope of the ITF Text APi but would fall to an ITF Text Viewer application.

It is possible that any of the tagged formats could also be returned as plain text plus a small annotation list, as described in the previous section. In any case, an ITF service will need to identify to a requesting application which formats beyond plain-text it supports.

## Metadata

The often overlooked topic of metadata was discussed at several points during the workshop, primarily because the participants recognised the importance of provenance and context for the accurate representation and interpretation of their materials. ITF should include a Metadata API that provides important additional information, which may apply to a text as a whole, or at a version level, and potentially alongside any retrieved fragment too.

Metadata formats vary widely, so while ITF will identify some core standard items it should also support linking (and linked data). In addition to the technical information indicated discussed in previous sections, ITF should also be able to return information concerning:

* Original Sources
    * Author, time, place of original source
    * Location of underlying physical or digital resources, such as IIIF images
* Scholarly Metadata 
    * Details of scholars, projects, funders, motivation and methodology
    * ITF should support standards such as ORCID
* Rights and Permissions
    * ITF must indicate and propagate copyright and reuse permissions from sources
* Related resources such as additional annotation lists as well as publications etc

## Conclusions

While by no means exhaustive, this workshop amounted to a broad comparison of many scholarly approaches to text that seems not to have any precedent. Broadly speaking the participants confirmed that the approach outlined in the position document was rational although the UDT project had perhaps underestimated the complexity of the task. In particular, it emerged that both versions and coordinate systems would need a graph-like rather than a simpler hierarchical or sequential model to adequately reflect current requirements.  

The workshop resulted in a good outline of the requirements for a text fragment API  which leads logically into the next workshop at Cambridge which explores how to assemble and annotate text fragments with a view to implementing current and new text-oriented scholarly workflows.    

## Projects and Infrastructures Discussed

* Beckett Digital Manuscript Project https://www.beckettarchive.org/
    * Genetic Editions, Notebooks, Beckett’s personal library and marginalia
    * Images and custom TEI/XML linked by annotations
* Cambridge Digital Library https://cudl.lib.cam.ac.uk
    * IIIF Images, TEI Transcriptions in various dialects
    * API access to text at document and page level - vis TEI tagging
* Casebooks https://casebooks.lib.cam.ac.uk
    * TEI versions of astrologer-physicians records
    * Multiple concurrent versions - Diplomatic, Normalised, Translated
    * IIIF page images
* CLARIN  Common LAnguage Resources and technology INfrastructure https://www.clarin.eu/
* DARIAH DigitAl Research Infrastructure for the Arts and Humanities https://www.dariah.eu/
* DataCite - https://datacite.org/index.html
* Fihrist Catalogue of Islamic Manuscripts https://fihrist.org.uk
    * TEI encoded catalogue entries, mixed directionality of scripts
    * Raw data maintained in GitHub (https://github.com/fihristorg)
* Harriot Papers https://echo.mpiwg-berlin.mpg.de/content/scientific_revolution/harriot
    * Miix of XML, TeX and images with complex linking narratives
* KNAW Humanities Cluster, Netherlands https://huc.knaw.nl/
    * Globalise: Archives of the Dutch E India Company
    * Republic: all 17th and 18th century Resolutions of Dutch Parliament
    * Editem - Enhanced TEI Editions based on templates, reconciles TEI and Linked-Data
    * CLARIAH Plus - CLARIN+DARIAH - Cross collection annotation using WADM
    * Nederlab - Annotated diachronic corpus, 20b words with 80b automatically generated linguistic annotations
* Language Bank of FInland https://www.kielipankki.fi
    * Many corpora - Newpapers, Social media etc.
    * Korp Web Interface https://github.com/spaakbanken/korp-frontend
    * Korp Back-end https://github.com/spaakbanken/korp-backend
    * Wide variety of source formats: Plain text, Word, JSON, ALTO-XML, TEI-XML
    * VRT intermediate format supports arbitrary addressing
* Living with Machines https://livingwithmachines.ac.uk/
    * The British Newspapers Archive https://www.britishnewspaperarchive.co.uk
    * ALTO-METS
    * Defoe API https://github.com/alan-turing-institute/defoe
* National Archives (UK)
    * Git-based WADM platform
    * ALTO XML to WADM conversion
* Quill Project https://www.quillproject.net
    * Version as a graph representation
    * JSON serialisation of editorial events (aka Google Docs, Microsoft Office365)
* SUB Gottingen TextAPI https://subugoe.pages.gwdg.de/emo/text-api
    * TEI Editions plus IIIF, JSON
    * TIDO Viewer https://gitlab.gwdg.de/subugoe/emo/tido
* Text+ https://www.text-plus.org/en/home/
    * German National Text Infrastructure
* Text Fabric https://github.com/annotation/text-fabric
    * Hebrew Biblical texts (not Unicode)
    * Disaggregated text as a graph representation
* Unlocking Digital Texts https://osf.io/r78gx/

## Glossary of API’s, Standards and Tools Referenced

ALTO - Analyzed Layout and Text Object, an XML format used for OCR output \
	 (https://www.loc.gov/standards/alto/techcenter/structure.html) 

DOI - Digital Object Identifier (https://www.doi.org/)

Git - Software Version Control Platform, handle regular text well too (https://git-scm.com/)

HTR - Handwritten text Recognition

IIIF - International Image Interoperability Framework (https://iiif.io)

JSON - JavaScript Object Notation (https://www.json.org/json-en.html)

Linked Data - (https://www.w3.org/DesignIssues/LinkedData.html)

Memento - Access past versions of a web resource \
	(http://timetravel.mementoweb.org/guide/api/)

METS - Metadata Encoding and Transmission Standard, XML container for other XML \
	 Formats (https://www.loc.gov/standards/mets/) 

OCR - Optical Character Recognition

ORCID - formerly Open Researcher and Contributor ID (https://orcid.org/)

PDF - Portable Document Format (https://pdfa.org/resource/pdf-specification-index/)

TEI - Text Encoding Initiative (https://www.tei.org/) An XML format

TeX - The TeX Typesetting System (https://tug.org/)

Text Fabric (https://github.com/annotation/text-fabric)

Unicode (https://home.unicode.org/)

VRT - Verticalised Text format used by Open Corpus Workbench (https://cwb.sourceforge.io)

WADM - Web Annotation Data  Model (https://www.w3.org/TR/annotation-model/

XML - eXtensible Markup Language (https://www.w3.org/TR/REC-xml/)


<!-- Footnotes themselves at the bottom. -->
## Notes

[^1]:
     For example: email, social media platforms such as Twitter, almost any form of threaded discussion

[^2]:
     Unlocking Digital Texts on the Open Science Framework https://osf.io/r78gx/

[^3]:
     Unlocking Digital Texts Position Paper https://osf.io/u6vb4

[^4]:
     The International Image Interoperability Framework https://iiif.io

[^5]:
     The IIIF Image API https://iiif.io/api/image/3.0/

[^6]:
     Linked Data https://www.w3.org/DesignIssues/LinkedData.html

[^7]:
     JavaScript Object Notation (https://www.json.org/json-en.html)

[^8]:
     Web Annotation Data Model https://www.w3.org/TR/annotation-model/

[^9]:
     Common LAnguage Resources and technology INfrastructure https://www.clarin.eu/

[^10]:
     DigitAl Research Infrastructure for the Arts and Humanities https://www.dariah.eu/

[^11]:
     Text+ https://www.text-plus.org/en/home/

[^12]:
     Quill Project https://www.quillproject.net

[^13]:
     Casebooks https://casebooks.lib.cam.ac.uk

[^14]:
     Text Fabric https://github.com/annotation/text-fabric

[^15]:
     Bibliographic Framework https://www.loc.gov/bibframe/

[^16]:
     DataCite - https://datacite.org/index.html

[^17]:
     Digital Object Identifier https://www.doi.org/

[^18]:
     Language Bank of FInland https://www.kielipankki.fi

[^19]:
     Fihrist Catalogue of Islamic Manuscripts https://fihrist.org.uk

[^20]:
     TEI - Text Encoding Initiative (https://www.tei.org/)

[^21]:
     Memento - Access past versions of a web resource (http://timetravel.mementoweb.org/guide/api/)

[^22]:
     Unicode Normalisation Forms https://www.unicode.org/reports/tr15/

[^23]:
     Unicode (https://home.unicode.org/)

[^24]:
     Text Fabric (https://github.com/annotation/text-fabric)

[^25]:
     Verticalised Text format used by Open Corpus Workbench (https://cwb.sourceforge.io)

[^26]:
     Cambridge Digital Library - https://cudl.lib.cam.ac.uk
