---
name: Deployment template
about: Describes a deployment of the OGC API - Features as an INSPIRE Download service
title: "[DEPLOYMENT]"
labels: deployment
assignees: ''

---

## 1. Data provider
* National Land Survey of Finland

## 2. Thematic scope

Geographic Names: The Geographic Names Register contains approximately 800,000 named places and their names in different languages. There are also map names with cartographical information on the register. Please see more information about place, place name and map name products and their data sources on the product description or meta data(link is external)

Buildings:  Buildings from municipalities and the National Land Survey of Finland stored in the National Terrain Database (KMTK). So far, no property information has been added for the buildings offered through the service.

INSPIRE AD and INSPIRE BU are beta services following INSPIRE simple encodings.

## 3. Envisaged use

Geographic Names and Buildings are in production.
INSPIRE AD and BU are in beta.

## 4. Requirements classes

Web API conforms to following requirements classes

[INSPIRE-pre-defined-data-set-download-OAPIF](https://github.com/INSPIRE-MIF/gp-ogc-api-features/blob/master/spec/oapif-inspire-download.md#req-pre-defined)

[INSPIRE-OAPIF-GeoJSON](github.com/INSPIRE-MIF/gp-ogc-api-features/blob/master/spec/oapif-inspire-download.md#req-oapif-json)



OGC API Features - Conformance classes:

* http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/core
* http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/oas30
* http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/geojson
* http://www.opengis.net/spec/ogcapi-features-1/1.0/req/gmlsf0 (only Geographic Names)
* http://www.opengis.net/spec/ogcapi-features-2/1.0/conf/crs (INSPIRE BU)

## 5. Server-side technology

An own server implementation by NLS Finland. 

Java development (servlet) backed by a PostGIS database, configurable with a config file.


## 6. Endpoints and client applications

**Geographic Names** ([Finnish Geographic Names Register](https://www.maanmittauslaitos.fi/en/maps-and-spatial-data/expert-users/product-descriptions/geographic-names)):
 
https://avoin-paikkatieto.maanmittauslaitos.fi/geographic-names/features/v1/

Encoding: GeoJSON and GML 3.2.1

Status: In production (requires [authentication key](https://omatili.maanmittauslaitos.fi/?lang=en))

[comment]: # (you must login with key, no password: 74b4dffa-30bd-46b6-9b20-28cd811ff6fe)

**Buildings**

https://avoin-paikkatieto.maanmittauslaitos.fi/buildings/features/v1/

Encoding: GeoJSON

Status: In production (requires [authentication key](https://omatili.maanmittauslaitos.fi/?lang=en))

**Adresses (INSPIRE AD)**

https://beta-paikkatieto.maanmittauslaitos.fi/simple-addresses/features/v1/

Encoding: GeoJSON

Status: Beta (prototype)

**Buildings (INSPIRE BU)**

https://beta-paikkatieto.maanmittauslaitos.fi/inspire-buildings/features/v1/

Encoding: GeoJSON

Status: Beta (prototype)

## 7. Issues
 [comment]: # (Link to other GitHub issues, or describe here issues that you encounter with the guidelines for the use of the OGC API - Features as an INSPIRE Download service.)
TBD