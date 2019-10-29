# Good Practice – Setting up an INSPIRE Download based on the OGC-API Features standard


## Introduction


- Rationale: why another TG/good practice for download services in INSPIRE?

- Very short overview of the main features and ideas behind SDW OGC-API Features and how it is different from the other OGC standards used for INSPIRE download services


## Glossary


## How to set up an INSPIRE-compliant web API compliant with OGC API – Feature


### Mapping core concepts (INSPIRE data set distribution = API landing page, OAF collections)


### Reference to OAF conformance classes (and extensions) – Core & CRS

               do we want to mandata Open API, GeoJSON, …? Recommendations?

               Filter – might be needed for direct access download service – could be a recommendation


### INSPIRE-specific requirements

- language support – HTTP content negotiation

- supported languages – Could use alternate representations or in OpenAPI definition of the API

- For each INSPIRE data set, a separate landing page (=API) shall be provided. (each landing page shall provide access to exactly one INSPIRE data set).

- To enable bulk download of the whole data set, an enclosure link shall be included.

- a collection shall contain only one spatial object type

- a collection shall have as its id the language-neutral name of the spatial object type as specified in the legislation

- enclosure link for bulk download – recommendation?

- licence link – recommendation?


### INSPIRE-specific recommendations


## Annex A: Abstract Test Suite

- could be done by reference to a repository in the INSPIRE validation Github space


## Annex B: Introduction to OGC-API Features

- Do we need this? Reference to Github repo…


## Annex C: Mapping the requirements from the IRs to the OGC-API Features standard (and extensions)


## Annex D: An example – link to github page
