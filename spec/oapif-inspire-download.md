# Setting up an INSPIRE Download service based on the OGC API-Features standard

`Version: 0.1`
`Date: 2019-12-04`

## Table of Contents
* [1. Introduction](#introduction)
* [2. Scope](#scope)
* [3. Conformance](#conformance)
* [4. Normative references](#normative-references)
* [5. Terms and definitions](#terms-and-definitions)
* [6. Symbols and abbreviated terms](#symbols-and-abbreviated-terms)
* [7. INSPIRE Download Services based on OAPIF](#inspire-oapif)
    * [7.1. Main principles](#main-principles)
    * [7.2. Requirements class “INSPIRE-pre-defined-dataset-download-OAPIF”](#req-pre-defined)
    * [7.3. Requirements class “INSPIRE-multilinguality”](#req-multilinguality)
    * [7.4. Requirements class “INSPIRE-OAPIF-GeoJSON”](#req-oapif-json)
* [8. Bibliography](#bibliography)
* [Annex A: Abstract Test Suite](#ats)
* [Annex B: Mapping the requirements from the IRs to the OGC  API - Features standard (and extensions)](#ir2oapif)
* [Annex C: Mapping between INSPIRE Network services metadata and OpenAPI definitions](#inspire-ns-openapi)
* [Annex D: Supported languages](#supported-lang)

## 1. Introduction <a name="introduction"></a>
This document proposes a technical approach for implementing the requirements set out in the [INSPIRE Implementing Rules for download services](http://data.europa.eu/eli/reg/2009/976/oj) based on the newly adopted [OGC API - Features standard](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html). 

Several possible solutions for implementing download services are already endorsed by the INSPIRE Maintenance and Implementation (MIG) group. [Technical guidelines documents](https://inspire.ec.europa.eu/Technical-Guidelines2/Network-Services/41) are available that cover implementations based on ATOM, WFS 2.0, WCS and SOS. 

While all of these approaches use the Web for providing access to geospatial data, the new family of OGC API standards aim to be more developer friendly by requiring less up-front knowledge of the standard involved. The rapid emergence of Web APIs provide a flexible and easily understandable means for access to data, as recommended by the W3C Data on the Web Best Practices [DWBP Best Practice 23](https://www.w3.org/TR/dwbp/#accessAPIs) and [DWBP Best Practice 24](https://www.w3.org/TR/dwbp/#APIHttpVerbs). 

Therefore, this document describes an additional option for the implementation of INSPIRE download services. 

In order to facilitate the use of off-the-shelf software implementing the OGC API - Features standard to meet the requirements in this document, INSPIRE-specific extensions are limited to the absolute minimum. Where several implementation options exist, this document describes the specific way of application of the OAPIF and associated standards to meet the requirements of the INSPIRE Implementing Rules for download services.

### OGC API - Features - a brief overview

OGC API standards define modular API parts that spatially enable Web APIs in a consistent way. The [OpenAPI specification](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#OpenAPI) is used to define the API building blocks.

OGC API - Features provides API building blocks to create, modify and query features on the Web. OGC API - Features is comprised of multiple parts, each of them is a separate standard. The ["Core"](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) part specifies the core capabilities and is restricted to retrieving features where geometries are represented in the WGS 84 coordinate reference system with axis order longitude/latitude. Additional capabilities that address more advanced needs will be specified in additional parts. 

By default, every API implementing this standard will provide access to a single dataset. Rather than sharing the data as a complete dataset, the OGC API - Features standards offer direct, fine-grained access to the data at the feature (object) level. Query operations enable clients to retrieve features from the underlying data store based upon simple selection criteria, defined by the client.

For further details about the standard, see the official [OGC API - Features website] (https://www.opengeospatial.org/standards/ogcapi-features). 

For a description of the main differences between WFS 2.0 and OGC API - Features, see [this section in the Guide on OGC API - Features](https://github.com/opengeospatial/ogcapi-features/blob/master/guide/section_8_WFS_2_0_v_3_0.adoc).

## 2. Scope <a name="scope"></a>
This document proposes a technical approach for implementing the requirements set out in the [INSPIRE Implementing Rules for download services](http://data.europa.eu/eli/reg/2009/976/oj) based on the [OGC API - Features standard](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html). The approach described here is not legally binding and shows one of several ways of implementing INSPIRE Download services.
## 3. Conformance <a name="conformance"></a>
This specification defines the following requirements classes:

- [INSPIRE-pre-defined-dataset-download-OAPIF (mandatory)](#req-pre-defined)
- [INSPIRE-multilinguality (conditional)<sup> 1</sup>](#req-multilinguality)
- [INSPIRE-OAPIF-GeoJSON (optional)](#req-oapif-json)

<sup>1 </sup>The INSPIRE-multilinguality RC is mandatory for all data sets that contain information in more than one natural language.

Future versions of this specification may include further conformance classes, in particular for 
- direct access download, and
- quality of service.

The target of all requirements classes are “Web APIs”. Conformance with this specification shall be assessed using all the relevant conformance test cases specified in [Annex A (normative)](#ats) of this specification.

## 4. Normative references <a name="normative-references"></a>

- **[OAPIF](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html)** - OGC API - Features - Part 1: Core<sup> 2</sup>
- **[OpenAPI 3.0](https://swagger.io/docs/specification/about)** - OpenAPI specification v3.0 
- **[IRs for NS](https://eur-lex.europa.eu/eli/reg/2009/976/oj)** - Regulation 976/2009 implementing Directive 2007/2/EC as regards the Network Services 
- **[IRs for ISDSS](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex:32010R1089)** - Regulation 1089/2010 implementing Directive 2007/2/EC as regards interoperability of spatial data sets and services 
- **[RFC 7231](https://tools.ietf.org/html/rfc7231)** - Hypertext Transfer Protocol (HTTP/1.1)
- **[RFC 4647](https://tools.ietf.org/html/rfc4647)** - Phillips, A. and David, M. (eds.). Matching of Language Tags [online]. Internet Engineering Task Force, September 2006.
- **[RFC 6838](https://tools.ietf.org/html/rfc6838)** - Media Type Specifications and Registration Procedures
- **[RSS 2.0](http://www.rssboard.org/rss-draft-1)** - Really Simple Syndication Specification (RSS 2.0) Specification 

<sup>2 </sup> The standard is in the process of being released as [ISO 19168-1](https://www.iso.org/standard/32586.html).
## 5. Terms and definitions <a name="terms-and-definitions"></a>
For the purposes of this document, the following terms and definitions apply:

| Term | Definition | Source
| --- | --- | ---|
| content negotiation | HTTP/1.1 allows web site authors to put multiple versions of the same information under a single resource URI.  Each of these versions is called a `variant'. Content negotiation is the process by which the best variant is selected if the resource is accessed. | [RFC 2295](https://tools.ietf.org/html/rfc2295)
| data set | Identifiable collection of data. | [ISO 19115]()
| distribution (of a data set) | A specific representation of a dataset. A dataset might be available in multiple serializations that may differ in various ways, including natural language, media-type or format, schematic organization, temporal and spatial resolution, level of detail or profiles (which might specify any or all of the above). | [DCAT](https://www.w3.org/TR/vocab-dcat-2/#Class:Distribution)
| direct access download service | Service that enables copies of spatial data sets, or parts of such sets, to be downloaded. | [INSPIRE](http://inspire.ec.europa.eu/glossary/DownloadService) |
| encoding | Conversion of data into a series of codes. | [ISO 19118](https://www.iso.org/standard/44212.html) |
| encoding rule | Identifiable collection of conversion rules that define the encoding for a particular data structure. | [ISO 19118](https://www.iso.org/standard/44212.html) |
| feature | Abstraction of real world phenomena. **NOTE** The concept of a `feature` is synonymous to a `spatial object` in INSPIRE | [OAPIF](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_feature) |
| feature collection | A set of features from a dataset. | [OAPIF](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_feature_collection)
| feature type | **NOTE** The concept of a `feature type` is synonymous to a `spatial object type` in INSPIRE | [INSPIRE](https://inspire.ec.europa.eu/glossary/SpatialObject) |
| pre-defined data set download service | Service that enables copies of spatial data sets, or parts of such sets, to be downloaded and, where practicable, accessed directly. | [INSPIRE](http://inspire.ec.europa.eu/glossary/DownloadService) |
| Web API | API using an architectural style that is founded on the technologies of the Web. | [DWBP](https://www.w3.org/TR/dwbp) |


**NOTE** ISO and the European Commission maintain comprehensive terminological databases at the following addresses:
- [ISO Online browsing platform](https://www.iso.org/obp)
- [INSPIRE glossary](http://inspire.ec.europa.eu/glossary)

## 6. Symbols and abbreviated terms <a name="symbols-and-abbreviated-terms"></a>
| Abbreviation | Term |
| --- | --- |
| API |	Application Programming Interface |
| DCAT | Data Catalog Vocabulary |
| GML | Geography Markup Language |
| JSON | JavaScript Object Notation |
| OAPIF | OGC API - Features |
| URL |	Uniform Resource Locator |
| WFS | Web Feature Service |
## 7. INSPIRE Download Services based on OAPIF <a name="inspire-oapif"></a>
This section describes the requirements a Web API shall fulfill in order to be compliant with both ‘OGC API – Features’ and INSPIRE requirements for download services.
### 7.1. Main principles <a name="main-principles"></a>

- The Web API provides download access to one INSPIRE data set. For example, two data sets (with their own metadata records), one on buildings and one on addresses will have two landing pages (http://my-org.eu/addresses/ and http://my-org.eu/buildings/) rather than one landing page for the Web API (http://my-org.eu/oapif/) and two feature collections, one for each data set (http://my-org.eu/oapif/collections/addresses and http://my-org.eu/oapif/collections/buildings).

&#x1F538; OPEN QUESTION: Would such an approach be feasible from an implementation point of view?

- The data set is structured into one or several feature collections. Аll feature collections available in one API (under the `/collections` path) are considered to be part of the data set provided by the Web API.

- Every collection contains features of only one feature type.

| INSPIRE resources | OAPIF resource | Sample path | Document reference |
| ------------- | ------------- | ------------- |-------------: |
| Data set (all distributions) | Landing page | http://my-org.eu/addresses/ <br> http://my-org.eu/buildings/ | [OAPIF 7.2 API landing page](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_api_landing_page) |
| Data set description | Feature collections |
| Spatial object type | Feature collection | http://my-org.eu/addresses/collections/address | [OAPIF 7.14 Feature collection](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_collection_) |
| Spatial objects | Features | http://my-org.eu/addresses/collections/address/items | [OAPIF 7.15 Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_items_) |
| Spatial object | Feature | http://my-org.eu/addresses/collections/address/items/{featureId} | [OAPIF 7.16 Feature](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_feature_) |
 
### 7.2. Requirements class “INSPIRE-pre-defined-dataset-download-OAPIF”  <a name="req-pre-defined"></a>

| Requirements class | http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/pre-defined |
| --- | --- |
| Target type | Web API |
| Dependency | [OAPIF Requirements class "Core"](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_requirements_class_core) |

#### 7.2.1. OAPIF requirements

| **Requirement** | **/req/pre-defined/oapif-core** |
| --- | --- |
| A | The Web API SHALL comply with the [OAPIF-core](http://www.opengis.net/spec/ogcapi-features-1/1.0/req/core) requirements class.|

**TEST** 
1. Tests defined by for [OAPIF CC Core](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_conformance_class_core) shall be satisfied.

**NOTE** The Web API shall return collections and features in the default OAPIF coordinate reference system (WGS 84 longitude and latitude). However, the `enclosure` link for bulk download could still provide access to a data set in a different CRS.

| **Requirement** | **/req/pre-defined/openapi** |
| --- | --- |
| A | The Web API SHALL comply with OAPIF requirements class OpenAPI 3.0. |

**NOTE 1** In the OAPIF standard, the [OpenAPI 3.0 Requirements class](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_requirements_class_openapi_3_0) is optional. This specification proposes to make it a mandatory requirement for INSPIRE in order to facilitate the development of client applications, and in particular adding support in the European INSPIRE geoportal.

**NOTE 2** There are plans to add additional requirements classes for other API description standards (or standard versions) in the future (e.g. for OpenAPI v3.1). When additional requirements classes become available, this specification will be reviewed and possibly revised to include these as additional options.

#### 7.2.2. INSPIRE-specific requirements
##### Download of the whole data set

| **Requirement** | **/req/pre-defined/enclosure** |
| --- | --- |
| A | The response of the `/collections` operation SHALL include at least one `enclosure` link that allows requesting a representation of the whole data set. |

**TEST**

1. Issue an HTTP GET request to {root}/collections.
2. Validate that at least one of the links returned in the response has `rel` link parameter `enclosure`.
3. For each of the links returned in the response having a `rel` link parameter equal to `enclosure`, issue an HTTP HEAD request to the path given in the `href` link parameter of that link.
4. For each of the responses:
    - If the HTTP status code is 405 (Method Not Allowed), the test verdict is inclusive.
    - If HTTP status code 200 is returned and HTTP header Content-Length > 0, the test verdict is “pass”.
    - Otherwise, the test verdict is “fail”.


| **Requirement** | **/req/pre-defined/enclosure-type** |
| --- | --- |
| A | A link with the relation type `enclosure` SHALL include the `type` link parameter containing a media type valid that is valid according to [RFC 6838](https://tools.ietf.org/html/rfc6838). |

**TEST**
1. Issue an HTTP GET request to `{root}/collections`.
2. For each of the links returned in the response having a `rel` link parameter equal to `enclosure`,validate that the `type` parameter is present and the media type is valid according to [RFC 6838](https://tools.ietf.org/html/rfc6838).


**NOTE** Requirements for downloads of a whole data set available in more than one natural language are included in the requirements class INSPIRE-multilinguality.


| **Recommendation** | **/rec/pre-defined/enclosure-length** |
| --- | --- |
| A | A link with the link relation type `enclosure` SHOULD include the `length` link parameter containing the length in bytes. |

| **Recommendation** | **/rec/pre-defined/enclosure-title** |
| --- | --- |
| A | The link(s) with the link relation type `enclosure` SHOULD include the `title` link parameter containing a human-friendly name. |


**EXAMPLE** Feature collections response document (adapted from [OAPIF](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_response_4))

- This feature collections example response in JSON is for a dataset with a single collection "buildings". It includes links to the features resource in all formats that are supported by the service (link relation type: `items`).

- Representations of the resource in other formats are referenced using link relation type `alternate`.

- An additional link is to a GML application schema for the dataset - using link relation type `describedBy`.

- There are also links to the license information for the building data (using link relation type `license`).

- Finally, the link with the link relation type `enclosure` provides a reference to another distribution of the data set as a GeoPackage download of the complete data set (pre-defined download). The `length` property includes the size in bytes of the data set.

```json
{
  "links": [
	{ "href": "http://my-org.eu/collections.json",
  	"rel": "self", "type": "application/json", "title": "this document" },
	{ "href": "http://my-org.eu/collections.html",
  	"rel": "alternate", "type": "text/html", "title": "this document as HTML" },
	{ "href": "http://inspire.ec.europa.eu/schemas/bu-core2d/4.0/BuildingsCore2D.xsd",
  	"rel": "describedBy", "type": "application/xml", "title": "The 2D application schema for INSPIRE theme buildings." },
	{ "href": "http://my-org.eu/buildings.gpkg",
  	"rel": "enclosure", "type": "application/geopackage+sqlite3", "title": "Pre-defined data set download (GeoPackage)", "length": 472546 }
  ],
  "collections": [
	{
  	"id": "buildings",
  	"title": "Buildings",
  	"description": "Buildings in the city of Bonn.",
  	"extent": {
    	"spatial": {
      	"bbox": [ [ 7.01, 50.63, 7.22, 50.78 ] ]
    	},
    	"temporal": {
      	"interval": [ [ "2010-02-15T12:34:56Z", null ] ]
    	}
  	},
  	"links": [
    	{ "href": "http://my-org.eu/collections/buildings/items",
      	"rel": "items", "type": "application/geo+json",
      	"title": "Buildings" },
    	{ "href": "https://creativecommons.org/publicdomain/zero/1.0/",
      	"rel": "license", "type": "text/html",
      	"title": "CC0-1.0" },
    	{ "href": "https://creativecommons.org/publicdomain/zero/1.0/rdf",
      	"rel": "license", "type": "application/rdf+xml",
      	"title": "CC0-1.0" }
  	]
	}
  ]
}
```
##### Organisation of a dataset in feature collections

| **Requirement** | **/req/pre-defined/spatial-object-type** |
| --- | --- |
| A | Every collection SHALL contain features of only one feature type
 
**NOTE** According to the OAPIF standard a collection could also contain more than one feature type.
 
**TEST**
1. Manual check for every collection, that all its features belong to the same feature type.

| **Requirement** | **/req/pre-defined/collection-naming** |
| --- | --- |
| A | For each `collection` that provides data that is harmonised according to the [IRs for ISDSS](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex:32010R1089), the name of the collection SHALL be the language-neutral name of the feature type as specified in the [IRs for ISDSS](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex:32010R1089). |

**TEST** 
1. Check all collection names for a recognised language neutral name
2. If no language-neutral name is available - MANUAL TEST: Check that the collections have not yet been harmonised.

| **Requirement** | **/req/pre-defined/licence** |
| --- | --- |
| A | The Web API shall contain a link to the licence of the data set made available. |

| **Recommendation** | **/rec/pre-defined/license-openapi** |
| --- | --- |
| A | The license information for the exposed data set SHOULD be provided in accordance with OpenAPI 3.0. A proposal for mapping between INSPIRE NS Metadata elements and OpenAPI definition fields is available in [Annex C.](#inspire-ns-openapi) |

### 7.3. Requirements class INSPIRE-multilinguality <a name="req-multilinguality"></a>
| Requirements class | http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/multilinguality |
| --- | --- |
| Target type | Web API |
| Dependency | [INSPIRE-pre-defined-dataset-download-OAPIF](#req-pre-defined)  |

This requirements class is mandatory for all data sets that contain information in more than one natural language.

#### 7.3.1. INSPIRE-specific requirements
##### Internationalisation: request and response language

The requirements from the [IRs for NS] to support requests in different natural languages are met in the Web API through HTTP language negotiation, using HTTP headers as specified in [RFC 7231](https://tools.ietf.org/html/rfc7231), and language tags as specified in [RFC 4647](https://tools.ietf.org/html/rfc4647).


| **Requirement** | **/req/multilinguality/accept-language-header** |
| --- | --- |
| A | The Web API SHALL support the `Accept-Language` header in requests to the landing page (/), /collections, /collections/{collectionId}, /collections/{collectionId}/items and /collections/{collectionId}/items/{featureId} in accordance with [RFC 7231](https://tools.ietf.org/html/rfc7231) and [RFC 4647](https://tools.ietf.org/html/rfc4647). The Web API SHALL return HTTP status code 406 when an Accept-Language HTTP header set to `*;q=0.0` is sent. |

**TEST**
1. Issue an HTTP GET request with an Accept-Language HTTP header containing a valid language tag to the following URLs: {root}/ and {root}/collections. For every feature collection identified in the response of {root}/collections, issue an HTTP request with an Accept-Language HTTP header containing a valid language tag to {root}/collections/{collectionId}, {root}/collections/{collectionId}/items.
2. For every response, validate that the HTTP status code is 200.
3. Issue an HTTP GET request with the Accept-Language header set to `*;q=0.0` to the following URLs: {root}/ and {root}/collections. For every feature collection identified in step 1, issue an HTTP GET request with the Accept-Language header set to `*;q=0.0` to {root}/collections/{collectionId} and {root}/collections/{collectionId}/items.
4. For every response, validate that the HTTP status code is 406.

&#x1F538; OPEN QUESTION: How to test for /collections/{collectionId}/items/{featureId}? It would give way too much overhead to test every single feature. One feature? One feature in every collection?

| **Requirement** | **/req/multilinguality/content-language-root** |
| --- | --- |
| A | The Web API SHALL include the `Content-Language` HTTP header in the response for a request to its landing page (/). |

**TEST**
1. Issue an HTTP GET request with an Accept-Language HTTP header containing a valid language tag to URL {root}/.
2. Validate that a response is returned with a `Content-Language` HTTP header.
3. Issue an HTTP GET request without a Accept-Language HTTP header to URL {root}/.
4. Validate that a response is returned with a `Content-Language` HTTP header.

| **Recommendation** | **/rec/multilinguality/content-language-paths** |
| --- | --- |
| A | The Web API SHOULD take the language specified in the `Accept-Language` HTTP header of a request to all paths into account. The Web API SHOULD include the `Content-Language` HTTP header in the response for a request to all paths. |

##### Internationalization: supported languages

&#x1F538; OPEN QUESTION: Would the proposed approach based on HTTP standards be easily implementable by existing solutions. 

[RFC 7231](https://tools.ietf.org/html/rfc7231) does not define what such a response body exactly should look like, see also Annex B, and no other existing specifications have been identified that define this. As one of the principles in this specification is not to have any INSPIRE-specific extensions or requirements, this specification therefore does not give a stronger recommendation. This specification may be updated when the response body returned with HTTP status code 406 is standardised.

| **Recommendation** | **/rec/pre-defined/accept-language-header** |
| --- | --- |
| A | The Web API SHOULD return a response with a response body containing a list of all supported languages for requests with the Accept-Language HTTP header set to `*;q=0.0` to the landing page (/), /collections, /collections/{collectionId}, /collections/{collectionId}/items and /collections/{collectionId}/items/{featureId}. |

When HTML is acceptable for the client and the Web API conforms to OAPIF Conformance Class HTML, this could look as follows:

```
# Request
GET {root}/ HTTP/1.1
Accept: text/html
Accept-Language: *;q=0.0
```

```
# Response
406 Not Acceptable
Content-Type: application/html
Content-Language: en

<html>
  <head>
    <title>Supported languages</title>
  </head>
  <body>
     <p>This server supports the following languages: English, Danish.</p>
  </body>
</html>
```

As long as the response body supplied in a machine-readable format, such as JSON, sent with HTTP status code 406, is not standardised, it is not possible to build applications, such as the [INSPIRE Geoportal](https://inspire-geoportal.ec.europa.eu/), that can programmatically access a list of supported languages and display that information to an end user. A workaround for this is the following procedure:
1. Retrieve a list of languages of interest
    - For a EU-wide application, this could be a list of all EU official languages.
    - For another application, this could be a list of languages chosen by an end user.
2. For every all language of interest, issue an HTTP HEAD request to the landing page (/) having HTTP header Accept-Language set to `language_of_interest_tag,*;q=0.0` (e.g. `en,*;q=0.0`).
3. For every response: if the HTTP status code of the response is 200, add the language specified in Content-Language to the list of supported languages.

This workaround presumes that the following requirements and recommendations are followed:
- [Recommendation 2 of OAPIF](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_http_1_1), regarding the support of HTTP method HEAD
- **/req/multilinguality/accept-language-header**, and **/rec/multilinguality/content-language-paths** regarding support for Accept-Language and Content-Languages


&#x1F538; OPEN QUESTION: Do you have any other proposals for how to implement the requirement for the download service to advertise the natural languages it supports?

| **Requirement** | **/req/multilinguality/hreflang** |
| --- | --- |
| A | A link with the link relation type “enclosure” SHALL include the `hreflang` link parameter containing the language of that distribution. The value of `hreflang` SHALL follow [RFC 4647]. |

**TEST**

1. Issue an HTTP GET request to `{root}/collections`.
2. For each of the links returned in the response having a `rel` link parameter equal to `enclosure`, validate that the `hreflang` parameter is present.
3. Check that the `hreflang` parameter  contains a language encoded in accordance with [RFC 4647]

### 7.4. Requirements class “INSPIRE-OAPIF-GeoJSON” <a name="req-oapif-json"></a>

| Requirements class | http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/geojson |
| --- | --- |
| Target type | Web API |
| Dependency | [INSPIRE-pre-defined-dataset-download-OAPIF](#req-pre-defined)  |
| Dependency | [OAPIF requirements class GeoJSON](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_requirements_class_geojson)  |

#### 7.4.1. INSPIRE-specific requirements
This requirements class is relevant when providing access to INSPIRE data encoded as (Geo-)JSON. (e.g. those developed in 2017.2 for Addresses and Environmental Monitoring Features).


| **Requirement** | **/req/geojson/oapif-geojson** |
| --- | --- |
| A | The Web API SHALL comply with [OAPIF requirements class GeoJSON](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html#_requirements_class_geojson). |

| **Recommendation** | **/rec/geojson/geojson-inspire** |
| --- | --- |
| A | For each `collection` which is encoded as GeoJSON and provides harmonised data according to the [[IRs for ISDSS]](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex:32010R1089), the Web API SHOULD follow the [INSPIRE UML-to-GeoJSON encoding rule.](https://github.com/INSPIRE-MIF/2017.2/blob/master/GeoJSON/geojson-encoding-rule.md) |

## 8. Bibliography <a name="bibliography"></a>
- [W3C Data on the Web Best Practices](https://www.w3.org/TR/dwbp/)
- [W3C Spatial Data on the Web Best Practices](https://www.w3.org/TR/sdw-bp/)
- [INSPIRE UML-to-GeoJSON encoding rule](https://github.com/INSPIRE-MIF/2017.2/blob/master/GeoJSON/geojson-encoding-rule.md)
- [Alla] Allamaraju, S. RESTful Web services cookbook. O’Reilly Media, 2010. ISBN 978-0-596-80168-7.
- [SO1] [How to properly send 406 status code?](https://stackoverflow.com/questions/4422980/how-to-properly-send-406-status-code)
- [SO2] [Format for 406 Not Acceptable payload?](https://stackoverflow.com/questions/50102277/format-for-406-not-acceptable-payload)

# Annex A: Abstract Test Suite <a name="ats"></a>
**NOTE** Detailed ATS will be developed in the next version of the document, based on the descriptions of tests included in the main body of the specification for each requirement.
# Annex B: Mapping the requirements from the IRs to the OGC-API Features standard (and extensions) <a name="ir2oapif"></a>
**NOTE** A [draft mapping]() between the requirements from the IRs to the OGC-API Features standard has been proposed and discussed in the INSPIRE MIG-T. This section will be completed based on this discussion paper once this specification is stable. 

# Annex C: Mapping between INSPIRE NS Metadata elements and OpenAPI definition fields  <a name="inspire-ns-openapi"></a>

This guidance document proposes a lightweight mapping approach between INSPIRE Network service metadata elements and OpenAPI definition. No extensions of OpenAPI terms are foreseen.

| INSPIRE NS Metadata element | OpenAPI field names |
| ------------ | ------------ |
Resource Title (M) | info/title
Resource Abstract (M) | info/description
Resource Type (M) |
Resource Locator (C) | 
Coupled Resource (C) | 
Spatial Data Service Type (M) 
Keyword (M) | 
Geographic Bounding Box (M)
Temporal Reference (M)
Spatial Resolution (C)
Conformity* (M)
Conditions for Access and Use (M) | info/termsOfService or info/license
Limitations on Public Access (M) | info/termsOfService or info/license
Responsible Organisation (M) info/contact/name
Metadata Point of Contact (M) | info/contact/name
Metadata Date (M) 
Metadata Language (M) 
Unique Resource Identifier (M)

&#x1F538; OPEN QUESTION: Would the proposed lightweight mapping to OpenAPI i.e. without extensions of OpenAPI terms (terms beginning with ‘x-’ in accordance with the [OpenAPI specs](https://swagger.io/docs/specification/openapi-extensions)) be sufficient?

--- 
**NOTE** Additional metadata elements can be added to an OpenAPI definition through [extensions](https://swagger.io/docs/specification/openapi-extensions/), implemented through the introduction of fields beginning with `x-`. However, in order to streamline the implementation of metadata, this document does not propose any INSPIRE-specific extensions. 
# Annex D. Supported languages  <a name="supported-lang"></a>
According to [RFC 7231](https://tools.ietf.org/html/rfc7231):

>   The 406 (Not Acceptable) status code indicates that the target resource does not have a current representation that would be acceptable to the user agent, according to the proactive negotiation header fields received in the request (Section 5.3), and the server is unwilling to supply a default representation.

>  The server SHOULD generate a payload containing a list of available representation characteristics and corresponding resource identifiers from which the user or user agent can choose the one most appropriate.  A user agent MAY automatically select the most appropriate choice from that list.  However, this specification does not define any standard for such automatic selection, as described in Section 6.4.1.

> [...]

> A specific format for automatic selection is not defined by this specification because HTTP tries to remain orthogonal to the definition of its payloads.  In practice, the representation is provided in some easily parsed format believed to be acceptable to the user agent, as determined by shared design or content negotiation, or in some commonly accepted hypertext format.


Similar to the example on to [SO2], this could look as follows when JSON is acceptable for the client:

```
# Request
GET {root}/ HTTP/1.1
Accept: application/json
Accept-Language: *;q=0.0
```

```
# Response
406 Not Acceptable
Content-Type: application/json

{
  { "acceptable" : [ "en", "da" ] }
}
```

[RFC 7807] defines a "problem detail" as a way to carry machine-readable details of errors in an HTTP response to avoid the need to define new error response formats for HTTP APIs.

If [RFC 7808] is to be used, the problem details object would have to be extended with additional members.

&#x1F538; OPEN QUESTION: Would the approach discussed in  https://github.com/opengeospatial/oapi_common/issues/75 be a good way of dealing with errors (incl. INSPIRE-specific ones).

```
# Request
GET {root}/ HTTP/1.1
Accept: application/json
Accept-Language: *;q=0.0
```

```
# Response
406 Not Acceptable
Content-Type: application/problem+json
Content-Language: en

{
    "type": "https://example.com/to/be/decided",
    "title": "Not Acceptable",
    "status": 406,
    "detail": "This server supports the following languages: English, Danish.",
    "instance": "/collections",
    "acceptable_languages": [ "en", "da" ]
   }

```

In summary, there is currently no standard way to supply a list of supported languages.

The parts in this specification regarding the 406 HTTP status code are inspired by discussions on Stack Overflow ([SO1], [SO2]) and on chapter 7 in [Alla].
