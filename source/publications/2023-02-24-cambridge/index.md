---
layout: page
title: Report for ‘Towards an Interoperable Text Framework - Annotation and the use and reuse of digital texts’ (24 Feb 2023, Cambridge, UK)
subtitle: Digital texts are all around us - as objects of study in scholarly work, to the news, social media and other texts we interact with everyday online. But how long will these digital texts last? The problem with digital texts is that they come in a variety of formats that are hard to reuse. Reusing and sustaining digital texts requires format and infrastructure updates beyond the skill and funding limits that most researchers can access. The Unlocking Digital Texts project aims to scope and define an Interoperable Text Format (ITF) as a means for accessing and delivering text that is stored in a variety of extant formats. Importantly, we are aiming for something that is both human- and computer-readable. If we are successful, this work will allow users to easily create, arrange, annotate, embed and share digital texts (either whole or in part) regardless of how or where they are stored.
permalink: /publications/2023-02-24-cambridge/
anchor_headings: true
---
# Workshop Report for ‘Towards an Interoperable Text Framework: Annotation and the use and reuse of digital texts’ (24 Feb 2023, Cambridge, UK)

## Summary of Unlocking Digital Texts:

Digital texts are all around us – from the objects and outputs of scholarly study, to the news, social media and other texts we interact with everyday online. But how long will these digital texts last? The problem with digital texts is that they come in a variety of formats that are hard to reuse because they require technical skills or infrastructure beyond most researchers’ capabilities and funding. Unlocking Digital Texts ([https://digitalscholarship.web.ox.ac.uk/article/unlocking-digital-texts](https://digitalscholarship.web.ox.ac.uk/article/unlocking-digital-texts)) aims to scope and define an Interoperable Text Format (ITF) as a means for accessing and delivering text that is stored in a variety of extant formats as well as referencing text fragments. Importantly, we are aiming for something that is both human- and computer-readable. ITF only enables access to text; it is not an analytical tool itself.

The goal of the 23 Feb 2023 hybrid workshop held in Cambridge was to bring together people with a variety of experiences of working with text in various formats to encourage community activity around the second aspect of text interoperability that Unlocking Digital Texts (UDT) outlined in our position paper ([https://osf.io/u6vb4](https://osf.io/u6vb4)): “Given that we have an actionable text version and fragment addressing mechanism, what higher level constructs are useful?”

We concentrated on whether existing real-world practices involving the creation and use of textual resources could be mapped into ITF using the Web Annotation Data Model (WADM). If so, would the combination of ITF & WADM simply be mapping existing practices or could it foster the creation of new ones?

Our attendees were drawn from a wide range of disciplines and institutions to ensure that the discussion could consider all the use-cases involving a diverse range of sources and materials.

Attendees included:

- Megan Gooch, Michael Hawkins, Neil Jefferies, Nilo Pedrazzini and Rob Ralley (Unlocking Digital Texts)
- Sepideh Alassi (University of Basel)
- Hennie Brugmann, Dirk Roorda (Koninklijke Nederlandse Akademie van Wetenschappen)
- Nina Cnockaert-Gillou, David McCay (Dindshenchas Project, University of Cambridge)
- Peter Cornwell (Data Futures GmbH)
- Andrew Cusworth (Bodleian Libraries)
- Louisiane Ferlier (The Royal Society)
- Mathias Göbel (Georg-August-Universität Göttingen)
- John Moore (The National Archives, UK)
- Liz Smith (Digital curator for 19th century science collections, Cambridge University Library)
- Marina Toumpouri (Church of Cyprus/University of Cyprus)
- Andreas Witt (Leibniz-Institut für Deutsche Sprache, Universität Mannheim)

## 1. To what extent can we map existing practices to annotations and text fragments?

It was generally felt that the Web Annotation Data Model, used in conjunction with a text fragment API, would work for all the situations listed in the position paper. Annotations point to fragments of text and can be used to record semantic information, such as additions and deletions, change of manuscript hand, named entities or geographical places. These WADM-encoded enrichments could then be used for both presentation and content analysis. Annotations involving named entities would facilitate named entity extraction. It could also be used to link repeated mentions of the same person in the same resource or indeed even external resources. Much the same could be done to geographical annotations. They could even, assuming geographical coordinates were known, be placed on a map.

### i. Connections between texts and physical items

It will also be useful to map the connections between texts and physical items that are now specimens in museums, physically and institutionally separate. Many letters in Charles Darwin’s correspondence were originally sent with enclosures of various kinds, from hair to natural historical specimens. Other correspondents, whose materials are now in Epsilon, similarly enclosed physical artefacts with their letters. Physical artefacts were also enclosed with items from the Sloane collection of letters and texts. Archival practices for objects differ from texts, and some specimens have been moved to museums. WADM would allow the mapping of collections of texts and their associated objects between institutions and between distinct collections.

### ii. Connections between texts and versions of texts

ITF would allow connections to be made between different texts: for instance, mapping out the relationship between multiple copies or versions of the same text or texts that are in some way parallel to each other. For ITF to be feasible, it needs to be capable of dealing with the ways in which texts might differ and at what ‘level’. For example, is one text a translation of the other? A gloss? A draft? An adaptation or retelling? Are the entire texts parallel to each other? Are they only parallel in part? There are also different ways in which one text might map onto another – word by word, sentence by sentence, or paragraph by paragraph. It is even possible for the connection to be made solely at the level of the whole document.

Concrete examples include different copies of a letter. It will need to be able to capture variations between texts that are nearly identical, and to record additions and deletions between successive drafts. There was some discussion about how different versions of a text should be represented, or referred to, as opposed to successive drafts. Among the papers of Charles Darwin there are drafts to be mapped onto printed versions of texts. The same sort of mappings can similarly be made between papers in Samuel Beckett’s archive and his printed works.[1]

Medieval texts frequently survive in multiple versions without any indication that one is the original, or that some are closer to the original than others, nor necessarily any way of determining the differences as the outcome of chronological development. Texts that diverge substantially from each other may be considered variations on the same work.

Some modern texts have a complex array of different versions that do not necessarily map on to a straightforward timeline. In these instances, the problem is not lack of information about their history, but rather the complexity of the processes that generated them. Negotiated texts, such as those studied by the Quill Project, are the results of negotiations between parties as they wend their way through the various legislative and quasi-parliamentary processes. Multiple versions of the text may exist in parallel at any given time, each with its own distinct authors or editors. These processes could be mapped onto specific changes in the text using WADM as an ordered list/sequence of items. It was also suggested that the provider of such texts should provide an annotation explaining the details of the ‘process’ that generated the text so readers could fully understand both it and its development.

One way annotation might facilitate the linking of texts is by indicating the presence of fragments of one text within another: perhaps connecting quotations to their sources, marginalia to body text, or works whose remains have been divided between separate institutions. The main body of a letter from Charles Darwin to Leonard Horner written in late summer 1846 is held in the American Philosophical Society while the enclosure included with it is in Cambridge University Library.[2] In the case of popular print on medicine in the nineteenth century there is considerable recycling of material, which appears as uncited quotations or rehashing of earlier passages.

Comparing different drafts or variant forms of the same text, connecting translations and paraphrases or indicating quotations, involves specifying the many different types of connection possible. The question was raised whether there would be any defined typology for the classification of annotations.

A key family of texts that could be given lots of annotations linking them to other resources are ledgers, indices and other forms of structured data, like the Dutch Parliamentary records. The data in these texts could be connected to other texts, or pages about people or other entities. Library ledgers are another example. A key question is whether such a ledger should be treated as text, or data, or both. The approach proposed was that these may be best treated as databases, delivering textual fragments as necessary, with the possibility of assembling those fragments into longer chunks of text to reproduce a longer excerpt from the ledger or index.

Where different versions of the same text exist, the question arises whether annotations on one version applies to others. It was noted that such a comment *may*, of course, apply to others but that it was not necessarily always the case. One anecdotal example given was of a sentence being changed between two drafts in an early modern text such that ‘Ier.’ shifted in meaning; in one draft it was short for Jerome, in another for Jerusalem. Crucially, the annotated fragment of text itself may not have to change for its meaning to change, because its meaning is dependent on the sentence context. This is a very unusual case, but it points to an important epistemic difficulty: it is hard to see how to determine whether annotations should carry over from one version to another without editorial intervention/annotation.

The solution may simply be transparency about the version that has been annotated and an indication that there are other available versions. An interface might ensure it is clear which version of the text each annotation is anchored to, report if there is another version of the text, and ask if the annotation still applies to that other version. One possible visualisation for investigating whether annotations apply to other versions of a text might be to place the versions on top of each other with the annotation anchored, to make it easier to see whether the annotation still applied in the other version – a sort of Hinman collator for digital texts.[3]

### iii. Annotations and narratives

Being able to select fragments of text and annotate them would provide a tool to create narratives. Those narratives might be from one text or from several. There was recognition of the usefulness of being able to construct intertextual narratives across multiple sources.

A worry was expressed that it would be possible to pull together quotations from a text to compile a version that presented a meaning utterly at variance with the author’s original words and intention. In the spirit of the Wicked Bible, one could quote the ten commandments with the fragments ‘Thou shalt’ and ‘commit adultery’, omitting ‘not’ and reversing the meaning of the commandment. The potential for misleading unwary readers is significant, but it is hardly new. It is difficult to see what can be done about this other than encouraging transparency, perhaps signalling somewhere (such as with a keyword such as ‘excerpt’ in the URL) that the view is of a fragmentary text or series of fragmentary texts, rather than a text in its entirety.

### iv. Controlling access

The delivery of a text in the form of fragments will provide a mechanism for dealing with two common situations in which access to text must be limited. The first is dealing with text behind a paywall, as in the case of certain material at the National Archives, Glenn Roe, the Hathi Trust, and the Royal Society. It was suggested that a form of non-consumptive usage could be employed for some materials. This would make certain actions permissible, such as the retrieval of a set number of small fragments, or that data mining of the text would be allowed, but the retrieval of the full text would not. The second situation would concern text that cannot be made available, either in full or part. This would include documents subject to embargo or containing redacted content. In these cases, the service would only extract what was permitted.

The question of authentication was discussed briefly. There were strong opinions in the room that authentication was difficult territory and should be avoided for the current project.

### v. Things embedded in texts

One area that will need further consideration is what should be done when mathematical passages, musical notation and tables are embedded within text. Can they be dealt with as extractable text? If they cannot be dealt with, how should the display cope with them (or fail)? One solution would be to provide annotations dynamically noting that certain content/passages within the extracted text could not be processed. A concern was raised about the exclusion of tables. While it may be feasible for the project to assert that grappling with mathematics and musical notation are beyond the scope of the current project, tables are common enough occurrences that we should attempt to deal with them. Early Modern texts are especially illustrative of the importance of tables for understanding a document’s broader narrative. While tables often contained concise tabular data, just as a modern reader would expect, they were also capable of containing significant textual content. Their exclusion from ITF would render some texts less sensible. One possible solution might be for ITF to treat this sort of content as a single word.

### vi. Academic work and references

One of the major use-cases for ITF is citations in academic works, particularly given the increase in digital textual resources. Rather than simply giving a URL for the whole text in a reference, the text fragment API will allow references to specify precisely the relevant phrase or passage. It would be useful when following a reference of this type to be able to see somehow the context in which the fragment appears, rather than only being able to choose between seeing the words of the fragment itself, or the whole text in which the fragment appears. A reader following a reference up in a printed book can easily look at the pages either side of a cited page and see the cited passage in context; it would be useful to provide an analogous facility for the readers of digital texts. There was disagreement over whether this was a matter for the API or for implementation.

Links given in footnotes would point at a particular (timestamped) version of the digital resource. If the resource were later changed, the URL in the footnote would still point to the version originally chosen by the author of the footnote and not the updated version. When this happens, however, it would be useful to indicate to the reader that a later version of the resource existed. There will be exceptions to this policy. For example, legally binding retractions, in which all versions of the resource would have to be modified.

Academic work could be carried out in the form of annotations onto texts, as a form of micropublishing. This would demand a mechanism for recording the author of the note being annotated to the text. In an academic environment the use of ORCID identifier ([https://orcid.org/](https://orcid.org/)) would provide that, though that does not answer how it would work outside the academic environment. In theory, this would open up the possibility of creating an edition with multiple editors who may or may not agree within one another. Annotations could be compared as well as the text itself.

One complicating factor is that annotations that add notes to a text are the product of two acts: the act of writing the text of the note, and the act of anchoring it to a text. These do not necessarily have to be done by the same person. Therefore, not only must the comment be ascribed to an author, but its annotation to the text too.

There was also a desire for the ability to record the degree of certainty over an annotation. This could be used, for example, to indicate the certainty of a particular reading or whether the annotation should be anchored to its current location within the text.

WADM allows for these functionalities, but it would be useful if examples were implemented within the ITF test collections as a demonstration.

### vii. Machine-generated text

One area of interest concerned machine generation of annotation. It was pointed out that the software used should be recorded as part of the provenance of the annotations. This facility would be easily supported with WADM, either in the body of the annotation (if the root text was supplied by Handwritten Text Recognition) or the annotation’s agent field (if the HTR text is being supplied in the body of the annotation). An example using on the of the ITF core collections would be useful. If one is not available, then it might be possible to create one using HTR (say, with Transkribus).

### viii. Displaying images alongside text

There was considerable interest in being able to align ITF resources with IIIF resources, and have complex, encoded texts accompanying digitised images.

## 2. What might an ITF manifest look like? And do?

The IIIF specification uses the concept of a ‘manifest.’ It is the resource that records information required to describe and view the images associated with that object. If ITF were to define a manifest for text, what would it need to contain, and what would it need to do?

Not all text is simply a linear stream of characters. ITF needs to be able to handle more complex textual structures and layouts. Scribes writing out medieval Irish quatrains often used available space at the ends of lines further up the page, leading to complex, non-linear layouts. Some medieval texts involved commentaries on commentaries, and therefore appear in manuscript with successive nested layers of notes on the original text.[4] Both modern editions of chronicles, and modern textbooks, sometimes combine marginal notes or glosses with footnotes. Marginal notes are not always anchored at a specific point in the text, and so ITF should be able to handle this (it would be done by treating them as annotations to extended passages in the main text).

The key questions are how complex does the ITF manifest need to be to provide a viable solution for the majority of resources without being too complex or overloaded. While there was no consensus on where the line between just enough and too much complexity should be drawn, there was an agreement that such a line would need to exist.

Scalability was a concern expressed by people with experience of dealing with IIIF on a large scale. On the one hand, there was a feeling that scalability was not a key aspect of the specification, except insofar as the specification needs to avoid hindering or hobbling it. It was admitted that the comparison of different versions of texts at scale might present a problem. On the other hand, a warning was given that the specification should not overload the manifest with more data than is necessary for the text service to function. Other functionalities and pieces of information should be relegated to associated standards, such as WADM and IIIF. The more narrowly constrained the manifest, the more likely that ITF will be a durable and scalable standard.

This makes two things high priorities:

1. ascertaining what core descriptive attributes should be part of the manifest
1. detailing the connections between ITF, WADM and IIIF.

While the question of the core attributes will be dealt with later, some possibilities are: distributor, creator, funder, licence, date/time? Other useful information could be encoded using WADM, such as a methodological statement about the text’s creation might need to be very brief (OCR, double-keyed, scholarly edition, paid-for content) and link to a full statement. Some examples of recommended (but not required) information would be detailed by the ITF specification.

## Use Cases

### Basic Annotations

#### Use Case 2.1: Record basic annotations on a text/textual fragment

**Actor:** Researcher

**Basic Flow:** The researcher adds supplementary information to the text/text fragment, such as explaining detailed concepts, identifying and linking to the source of quotes, denoting the physical features of the text (*e.g.* deletions, additions, and changes of manuscript hand), adding scholarly footnotes, or recording how the digital text was created (*e.g.* editorial principles, generated by text recognition software).


#### Use Case 2.2: Display text and its annotations

**Actor:** Researcher

**Basic Flow:** The researcher can read their text and annotations. They would be able to retrieve their texts either in full or portion thereof.

#### Use Case 2.3: Record and display annotations that might not be anchored to specific points in the text (*e.g*. marginalia)

**Actor:** Researcher

**Basic Flow:** Research adds marginalia on text and wishes to see the results. The marginalia might be anchored to specific points in the text, say through the use of a glyph or marker, or it might be anchored by context (*i.e.* not through the use of glyphs but rather by the fact that the annotation provides supplementary information about something raised in the text at a particular point). A good example of an unanchored annotation would be the mention of a Scriptural passage in the body text and a note written in the margin giving its source. To a knowledgeable reader, it is obvious that that annotation applies to that Scriptural passage. That link, however, is contextual. It is not explicitly indicated through the use of a glyph placed at the end of the passage in the body text and at the start of the annotation (*e.g.* a footnote number).


#### Use Case 2.4: Construct pedagogical narratives using annotations on digital texts

**Actor:** Lecturer

**Basic Flow:** Lecturer selects one or more fragment of text from one or more documents, annotates them with explanatory/narrative materials, arranges the items in the collection and, optionally includes a broader annotation that introduces the collection as whole, say introducing the aims and objectives of the digital lesson.


#### Use Case 2.5: Service converts existing TEI markup to standoff annotations

**Actor:** Software

**Basic Flow:** An automated process converts existing TEI markup (*e.g.* involving additions, deletions, manuscript hands, identification of person/place names, editorial notes, quotes and their sources) to standoff WADM annotations.


#### Use Case: 2.6: Use annotations to create complex text layouts that affect how the text is read

**Actor:** Editor

**Basic Flow:** The editor records complex layouts using annotations, which are then interpreted by software to recreate the layout digitally. For instance, a text including both footnotes and marginalia are encoded as annotations. Those annotations are then displayed in the digital edition as footnotes and marginalia. There is also the case of medieval commentaries in which one piece of text is the subject of a commentary, and that commentary is itself subject to a further commentary. These layers of commentary are recorded as annotations, and used to reconstruct the visual structure of the work in the digital edition. Another example would be manuscripts of medieval Irish quatrains, where the use of available space on the ends of previous lines means that the visual order of words on the page is typically different from the sense order. Readers would expect the text to be displayed in the order from the manuscript, but the computer readable version of the text would present it in sense order for computer processing.

### Interlinking


#### Use Case 2.7: Align an ITF resource with a IIIF resource

**Actor:** Editor

**Basic Flow:** The editor wants to present an encoded manuscript transcription alongside digitised images of the manuscript.


#### Use Case 2.8: Add interlinked semantic adornment to a text/textual fragment

**Actor:** Researcher

**Basic Flow:** The researcher wants to record identified entity/place names and link repeated mentions of the same person/place in the same text/textual fragment as well as points to external resources.


#### Use Case 2.9: Map connections between texts and physical specimens

**Actor:** Researcher

**Basic Flow:** The researcher wants to connect the digital text of a letter written to an individual, such as Charles Darwin, to a museum or archival record for the physical specimen originally enclosed with it. Notes: Letters and physical specimens are often held by different institutions. It is vital to ensure that detailed information about these institutions is recorded in the annotation.


#### Use Case 2.10: Recombine works divided between institutions [or projects]

**Actor:** Researcher

**Basic Flow:** The researcher recombines a work that is online as separate partial texts. This could happen because the physical item is divided between institutions, such as the letter written in late summer 1846 from Charles Darwin to Leonard Horner. The main body of the letter is held by the American Philosophical Society and the enclosure is held by Cambridge University Library. It might also occur because different projects (or teams) are responsible for transcribing different parts of the work.


#### Use Case 2.11: Indicate when two texts are separate transcriptions of the same document

**Actor:** Editor

**Basic Flow:** The editor has a set of texts supplied by different projects. Where two projects have produced transcriptions of the same source, they are labelled as differing editions of the same document.

**Stakeholder:** Epsilon Project (Cambridge University Library)

### Text as Process


#### Use Case 2.12: Map straightforward drafts onto printed versions texts

**Actor:** Editor

**Basic Flow:** The editor records the changes between draft versions of a text and the final printed version. Note: This use case should be specifically constrained to drafts/printed texts by a single author that have a straightforward relationship. A use case involving more complex relations outlined in Cases 2.12-16. Stakeholder: Darwin Correspondence Project (Cambridge University Library), Beckett Project


#### Use Case 2.13: Map the differences between versions of a document

**Actor:** Editor

**Basic Flow:** The editor records the changes in different versions of a text. These changes might involve additions, deletions or even the wholescale rearrangement of the text. Note: This use case should concern itself with texts by a single author that have a more complicated relations (say, repeated/extensive redrafting, no specific version identified by the author as the ‘final version’).

**Stakeholder:** Beckett Project, Casebooks Project


#### Use Case 2.14: Map differences between texts that are related but are not necessarily successive drafts

**Actor:** Editor

**Basic Flow:** The editor records the similarities/differences between texts that are related but not necessarily successive drafts, such as medieval texts, surviving in multiple versions without obvious filiation or chronological development. The texts could range from being nearly identical to strikingly different. Mapping could take place on anything from word by word to passage by passage.

**Stakeholder:** Casebooks Project


#### Use Case 2.15: Map differences between texts that are related but not variant versions of the same text (e.g. A translation and the original; one text being a gloss of another)

**Actor:** Editor

**Basic Flow:** The editor maps the relations between the versions to whatever level of detail is most suitable (e.g. word by word, sentence by sentence, block by block) and views the results.

**Stakeholder:** Darwin Correspondence Project (Cambridge University Library), Casebooks Project


#### Use Case 2.16: Handle texts with complex histories

**Actor:** Editor

**Basic Flow:** The editor records the relationship between different versions of a text with a complex history of production (*e.g.* a negotiated text such as those studied by the Quill Project) and multiple simultaneous versions, each with its own distinct author(s) or editor(s). Maps the processes of production onto specific changes in the text using WADM.

**Stakeholder:** Quill Project


#### Use Case 2.17: Academic micropublishing

**Actor:** Researcher(s)

**Basic Flow:** a group of researchers produce academic work consisting of a series of annotations to an online edition of a text. Authorship of the notes annotated to the text is recorded (perhaps using an ORCID identifier), as is responsibility for linking them to the text. The researchers may sometimes take combined responsibility for an annotation, but other times they might disagree and in those cases each one annotates the text under their own name.


### Projects Mentioned

Darwin Correspondence Project (Cambridge University Library): [https://www.darwinproject.ac.uk/](https://www.darwinproject.ac.uk/)

Epsilon (Cambridge University Library): [https://epsilon.ac.uk/](https://epsilon.ac.uk/)

Quill Project (Pembroke College, University of Oxford): [https://www.quillproject.net/quill](https://www.quillproject.net/quill)

Samuel Beckett Digital Manuscript Project (University of Oxford/University of Antwerp): [https://www.beckettarchive.org/](https://www.beckettarchive.org/)

The Casebooks of Simon Forman and Richard Napier, 1596–1634: A Digital Edition (History and Philosophy of Science, University of Cambridge): [https://casebooks.lib.cam.ac.uk/](https://casebooks.lib.cam.ac.uk/)


[1] https://www.beckettarchive.org/

[2] Darwin Correspondence Project, “Letter no. 993,” accessed on 24 March 2023, https://www.darwinproject.ac.uk/letter/?docId=letters/DCP-LETT-993.xml

[3] This would serve as an excellent example for Use Case: 2.X *below*.

[4] See Erik Kwakkel’s blog post ‘The Architecture of the Medieval Page’: [https://medievalbooks.nl/2018/09/07/the-architecture-of-the-medieval-page](https://medievalbooks.nl/2018/09/07/the-architecture-of-the-medieval-page) (accessed 28 April 2023).
