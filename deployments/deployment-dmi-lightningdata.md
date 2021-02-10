
## 1. Data provider

Danish Meteorological Institute - Open Data

https://www.dmi.dk/

## 2. Thematic scope

Lightning data from stations located in Denmark

The data belong to the INSPIRE themes [Meteorological geographical features](https://inspire.ec.europa.eu/Themes/142/2892) and [Atmospheric conditions](https://inspire.ec.europa.eu/Themes/141/2892).

## 3. Envisaged use

The API is in production

## 4. Requirements classes

OGC conformance classes:

- http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/core
- http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/oas30
- http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/geojson

Requirements classes implemented from the Good Practice:

- http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/pre-defined

## 5. Server-side technology

The code is self-developed and written in Java as a Spring Boot application using standard products (i.e. Tomcat Webserver and REST implementation).

The data provided through the API is stored in a MapR database. MapR is a big data platform that integrates Hadoop and Spark. The platform includes global event streaming, real-time database capabilities, and enterprise storage.

## 6. Endpoints and client applications

Endpoint: 
https://dmigw.govcloud.dk/v2/lightningdata/

Technical documentation is available at:
https://confluence.govcloud.dk/display/FDAPI/Lightning+data

The technical documentation includes, amongst other things, API endpoints, request and response examples, OpenAPI documentation and user registration guide.

## 7. Issues

- ~~https://github.com/INSPIRE-MIF/gp-ogc-api-features/issues/48~~ (requirements class bulk download is not implemented)
- ~~https://github.com/INSPIRE-MIF/gp-ogc-api-features/issues/47~~ (requirements class bulk download is not implemented)
- ~~https://github.com/opengeospatial/ets-ogcapi-features10/issues/117~~ (issue is closed)
- https://github.com/opengeospatial/ets-ogcapi-features10/issues/137

## Additional information

If you want to download large quantities of historical lightning data, DMI recommends that you use [DMI’s bulk download service](https://confluence.govcloud.dk/pages/viewpage.action?pageId=37355752). The service lets you download .zip files, each containing historic data for a month going back to 2002. You can also download all historic data by selecting the file all.zip.

