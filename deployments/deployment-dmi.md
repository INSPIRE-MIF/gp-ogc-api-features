
## 1. Data provider

Danish Meteorological Institute - Open Data

https://www.dmi.dk/

## 2. Thematic scope

Point measurements from tide gauge stations in Denmark owned by the Danish Meteorological Institute and the Danish Coastal Authority.

The API serves the following features:
•	Tide gauge stations
•	Sea level observations
•	Water temperature observations

The API belongs to the INSPIRE theme “Oceanographic geographical features”:
https://inspire.ec.europa.eu/Themes/143/2892

## 3. Envisaged use

The API is in production

## 4. Requirements classes

- http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/core
- http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/oas30
- http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/geojson

## 5. Server-side technology

The code is self-developed and written in Java as a Spring Boot application using standard products (i.e. Tomcat Webserver and REST implementation).

The data provided through the API is stored in a MapR database. MapR is a big data platform that integrates Hadoop and Spark. The platform includes global event streaming, real-time database capabilities, and enterprise storage.

## 6. Endpoints and client applications

Endpoint: 
http://dmigw.govcloud.dk/v2/oceanObs/

Technical documentation is available at:
https://confluence.govcloud.dk/display/FDAPI/Oceanographic+Observation

The technical documentation includes, amongst other things, API endpoints, request and response examples, OpenAPI documentation and user registration guide.

## 7. Issues

- https://github.com/INSPIRE-MIF/gp-ogc-api-features/issues/48
- https://github.com/INSPIRE-MIF/gp-ogc-api-features/issues/47
- https://github.com/opengeospatial/ets-ogcapi-features10/issues/117

