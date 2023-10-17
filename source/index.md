---
layout: default
title: Home
banner: false
hero:
  image: "/assets/uploads/Bodleian_MS Auct F 4 32.jpeg"
  title: "Unlocking Digital Texts:<br>Towards an Interoperable Text Framework"
  subtitle: Unlocking Digital Texts (UDT) aims to lay the foundation for the creation of the Interoperable Text Framework (ITF). Just as the International Image Interoperability Framework (IIIF) enables users to present, annotate and reuse digital images easily without having to worry about the underlying infrastructure, ITF will empower users to create a richer and more layered approach to the presentation, analysis and reuse of textual resources.
  button:
    label: "Get started"
    link: "/api"
---

{{ theme.block-center-start }}

## Funders

{% assign funders = site.data.institutions | where_exp: "org", 'org.status == 0' | where_exp: "org", 'org.logo and org.logo != nil' | sample: 2 %}
{% assign logos = funders %}

{% include blocks/logo-grid.html items=logos %}

UDT-ITF is an output of the Unlocking Digital Texts project. Unlocking Digital Texts is funded by the NEH/AHRC New Directions for Digital Scholarship in Cultural Institutions program, a joint initiative between the National Endowment for the Humanities and the UK Arts and Humanities Research Council (AHRC).

## Partners

{% assign new = site.data.institutions | where_exp: "org", 'org.new == true' %}
{% assign consortium = site.data.institutions | where_exp: "org", 'org.iiifc == 1 or org.iiifc == 2 or org.iiifc == 3' | where_exp: "org", 'org.logo and org.logo != nil and org.new != true' | sample: 4  %}
{% assign logos = new | concat: consortium %}

{% include blocks/logo-grid.html items=logos %}
