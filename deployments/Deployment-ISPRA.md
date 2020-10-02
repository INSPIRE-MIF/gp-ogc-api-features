# Deployments ISPRA OGC Open Feature API Testing phase

## 1. Data provider

ISPRA – Italian Institute for Environmental Protection and Research

Environmental National Information System (SINA)

## 2. Thematic scope

For the Testing phase we want provide in order to test also possible integration in the new ReportNet 3.0 system for Environmental Reporting and to provide more direct access to monitoring data: 

EUAP and Ramsar Protection area as INSPIRE Protection Sire (PS) theme 

Marine wave monitoring Buoy Network as INSPIRE Environment Monitoring Facility (EF) theme.

Geological Map of Italy 1:1M as GeoSciML 4.1 version and INSPIRE Geology (GE) theme.

## 3. Envisaged use

The Web API is a prototype for test phase only

## 4. Requirements classes

INSPIRE-pre-defined-data-set-download-OAPIF - http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/pre-defined

INSPIRE-OAPIF-GeoJSON - http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/geojson

Features Conformance classes:

http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/core

http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/oas30

http://www.opengis.net/spec/ogcapi-features-1/1.0/req/html

http://www.opengis.net/spec/ogcapi-features-1/1.0/conf/geojsonFrom the draft Good Practice:

## 5. Server-side technology

At the moment we have set-up a public/private server based on Geoserver 2.19-SNAPSHOT server.
The data for EUAP, Ramsar and Geological Map are a simple data store repository POSTGIS linked to Geoserver;
For Marine Network the data will be extract from the Marine INSPIRE LOD (LinkedOpenData) endpoint with specific conversion tool and POSTGRESSQL for sensor data.
A feasibility study is in progress to identify possible second implementation based on PyGeoAPI technology.

## 6. Endpoints and client applications

In progress, we provide endpoint shortly is available at moment only as internal link

http://193.206.192.206:8080/geoapi

## 7. Issues

TBD
