_Listing of Reconcilable Data Sources_

With OpenRefine you can perform reconciliation against any web service supporting the [Reconciliation Service Api](Reconciliation+Service+API). Reconciliation against Freebase is built in, but there are several other reconciliation services available as describe on this page. You can alternatively extend your data by [Extending-Data](calling+web+services)

## General

### Reconcile-csv

Reconcile-csv is a reconciliation service for OpenRefine running from a CSV file. It uses fuzzy matching to match entries in one dataset to entries in another dataset, helping to introduce unique IDs into the system - so they can be used to join your data painlessly.

- [http:_okfnlabs.org/reconcile-csv/_](Official+Website)
- [https:_github.com/okfn/reconcile-csv_](GitHub+Project)

### SPARQL endpoints

The RDF Extension by DERI at NUI Galway includes reconciliation against any SPARQL endpoint or RDF dump file and publishing of the results in RDF. [http:_lab.linkeddata.deri.ie/2010/grefine-rdf-extension/_](http://lab.linkeddata.deri.ie/2010/grefine-rdf-extension/)

### conciliator

[https:_github.com/codeforkjeff/conciliator_](conciliator) is a Java framework for creating OpenRefine reconciliation services. It currently offers out of the box support for VIAF, ORCID, Open Library, and any Apache Solr collection. Run your own service, or use the public server at [http:refine.codefork.com](http://refine.codefork.com)

## Librarians

### VIVO Scientific Collaboration Platform

VIVO is a U.S. national interdisciplinary open source scientific collaboration platform funded by the NIH with development led by Cornell. Their reconciliation service allows reconciling against VIVO entities (faculty members, journals, etc) in any VIVO installation. [https:_wiki.duraspace.org/display/VIVO/Extending+Google+Refine+for+VIVO_](Extending+Google+Refine+for+VIVO)

### FundRef

[http:_labs.crossref.org/fundref-reconciliation-service_](The+FundRef+Reconciliation+Service) is designed to help publishers (or anybody) more easily clean-up their funder data and map it to the FundRef Registry. It is built on Open Refine and FundRef Metadata Search.

### JournalTOCs

Use [http:_www.journaltocs.ac.uk/develop.php_](JournalTOCs+API) to create your own cool web applications that integrate content from freely available journal TOCs. Most of JournalTOCs API calls are free and don't require any registration process.

- [https:_github.com/lawlesst/journaltocs-reconcile_](GitHub+Project)

### VIAF

The [http:_viaf.org/_](VIAF%C2%AE+%28Virtual+International+Authority+File%29) combines multiple name authority files into a single OCLC-hosted name authority service. The goal of the service is to lower the cost and increase the utility of library authority files by matching and linking widely-used authority files and making that information available on the Web.

The reconciliation service is hosted by Roderic D. M. Page, more details on [http:_iphylo.blogspot.ca/2013/04/reconciling-author-names-using-open.html_](Reconciling+author+names+using+Open+Refine+and+VIAF).

### FAST (Faceted Application of Subject Terminology)

FAST is derived from the Library of Congress Subject Headings (LCSH), one of the library domain’s most widely-used subject terminology schemas. The development of FAST has been a collaboration of OCLC Research and the Library of Congress. Work on FAST began in late 1998.

- [http:_www.oclc.org/research/activities/fast.html?urlm=159754_](Official+Website)
- [https:_github.com/lawlesst/fast-reconcile_](GitHub+Project)

### Library of Congress Subject Headings

Library of Congress Subject Headings (LCSH) has been actively maintained since 1898 to catalog materials held at the Library of Congress. By virtue of cooperative cataloging other libraries around the United States also use LCSH to provide subject access to their collections.

The reconciliation service is hosted by Free Your Metadata group, more details on [http:_freeyourmetadata.org/reconciliation/_](their+website).

### Sharedshelf Built Work Registry Reconciliation Service

Sharedshelf has offered reconciliation services for its users to enrichment data using Sharedshelf Built Work Registry (BWR) project. Built Work Registry is accessible via [http://builtworksregistry.org/](http://builtworksregistry.org/).

Endpoint Details

| Service Labels | Details |
| Name | Sharedshelf BWR Display Endpoint |
| Endpoint | [http://lod.sharedshelf.artstor.org/sparql/api/reconcile/display](http://lod.sharedshelf.artstor.org/sparql/api/reconcile/display) |
| Graph URI | [http://lod.sharedshelf.artstor.org/sparql/api/project/756](http://lod.sharedshelf.artstor.org/sparql/api/project/756) |
| Type | Generic SPARQL |
| Label properties | rdfs:label && [http://catalog.sharedshelf.artstor.org/display/prefLabel](http://catalog.sharedshelf.artstor.org/display/prefLabel) |

## Open Data

### OpenCorporates

31 million corporate entities (as of Nov. 2011) available for reconciliation through their service.

- [http:_blog.opencorporates.com/tag/google-refine/_](Blog+posts)
- [http:_vimeo.com/17924204_](Screencast)

### dbpedia

DBpedia extension (currently) provides possibility to extend your DBpedia-reconciled data with additional related columns from DBpedia. It is based on similar Freebase extension.

- [https:_github.com/sparkica/dbpedia-extension_](GitHub+Project)

### Ordnance Survey

The Reconciliation API is a simple web service that supports linking of datasets to the Ordnance Survey Linked Data. The API accepts a simple text search, e.g. a label, code or other identifier for a resource and then returns a ranked list of potential matches. Client-side tools may then use these results to either build links to the Linked Data or use the returned identifiers to extract further data to enrich the original dataset.

- [http:_data.ordnancesurvey.co.uk/docs/reconciliation_](Official+Website)

### Organized Crime and Corruption Reporting Project

OCCRP provides a public reconciliation API endpoint which allows reconciliation of data against a comprehensive list of sanctioned persons and companies, politically exposed persons, and other persons of journalistic interest. The service is intended as a first-level "check for interesting entries" for government or private data.

- [https:_data.occrp.org_](Official+Website)
- [https:_data.occrp.org/help/reconcile_](Documentation)
- [https:_data.occrp.org/api/freebase/reconcile_](Endpoint)

### Nomisma

Nomisma provides data about numismatics:

- [https:_numishare.blogspot.co.uk/2017/10/nomisma-launches-openrefine.html_](Blog+post)
- [http:_nomisma.org/apis/reconcile_](Endpoint)

## Biodiversity Researchers

### Taxonomic Databases

Taxonomic databases (EOL,GBIF, NCBI,Global Names Index, uBio, [http:_www.marinespecies.org/aphia.php?p=webservice_](WoRMS)), as documented [http:iphylo.blogspot.com/2012/02/using-google-refine-and-taxonomic.html](here).

Taxonomic names from [http:_www.ipni.org/_](IPNI) via [http:data1.kew.org/reconciliation/](IPNI+Names+Reconciliation+Service). [https:_github.com/RBGKew/Reconciliation-and-Matching-Framework_](Source+code)

## Deprecated

### Freebase

The Freebase Reconciliation Service has been deprecated as of June 2015. There are no guarantees of its future after that date according to Google.

[http://reconcile.freebaseapps.com/](http://reconcile.freebaseapps.com/)

[https://developers.google.com/freebase/v1/reconciliation-overview?hl=en](https://developers.google.com/freebase/v1/reconciliation-overview?hl=en)

### Nomenklatura

Nomenklatura is a simple service that makes it easy to maintain a canonical list of entities such as persons, companies or event streets and to match messy input, such as their names against that canonical list – for example, matching Acme Widgets, Acme Widgets Inc and Acme Widgets Incorporated to the canonical “Acme Widgets”.

With Nomenklatura its a matters of minutes to set up your own set of master data to match against and it provides a simple user interface and API which you can then use do matching (the API is compatible with Open Refine’s reconciliation function).

- [http:_okfnlabs.org/projects/nomenklatura/index.html_](Official+Website)
- [https:_github.com/pudo/nomenklatura_](GitHub+Project)

### Talis Kasabi

The Kasabi reconciliation services provide reconciliation against any database published on the Kasabi platform. [http:_kasabi.com/doc/api/reconciliation_](Documentation)

### Talis Platform reconciliation services

This project has been shut down. [http:_blog.kasabi.com/2012/07/30/so-long-and-thanks-for-all-the-data/_](They+suggest+some+alternatives)

## Wish List

The following are data sources that could provide useful reconciling within OpenRefine. If you would like to help with coding a reconciling extension for any, please contact our mailing list. We would love to see some of these happen!

- [http:_chroniclingamerica.loc.gov/about/api/_](Historical+Newspapers) - Library of Congress' Chronicling America provides JSON, RDF, XML & Linked Data with an easy to use API.
- [http:_cactus.nci.nih.gov/chemical/structure/documentation_](Chemical+Identifier+Resolver) - Over 96 million chemical structures hosted by NCI/NIH, provides names, conversions, & various formats even XML output. 
