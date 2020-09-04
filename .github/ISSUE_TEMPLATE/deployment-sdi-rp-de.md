## 1. Data provider
* SDI Rhineland-Palatinate - different providers as it works as a simple proxy script on top on existing WFS

## 2. Thematic scope
* Different scopes - see above

## 3. Envisaged use
* It is already in production, service metadata is available and also the export of ckan resource views works to use it in the german open data portal - https://www.govdata.de 

## 4. Requirements classes
* Not tested till now

## 5. Server-side technology
* It is a proxy on top on the older GeoPortal.rlp OWS registry. The new implementation of the registry is started as an new OSGEO project - see https://git.osgeo.org/gitea/GDI-RP/MrMap, when we will implement it, we may migrate to pygeoapi (https://docs.pygeoapi.io) cause MrMap is based on django and this will be straightforward.

## 6. Endpoints and client applications
* https://www.geoportal.rlp.de/spatial-objects/
* examples: https://github.com/opengeospatial/ogcapi-features/blob/master/implementations.md#sdi-rhineland-palatinate---mapbender2

## 7. Issues
* TBD
