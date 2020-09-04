
## 1. Data provider
Finnish Meteorological Institute FMI

## 2. Thematic scope
Weather observations from approximately 400 weather stations in Finland.

Belongs INSPIRE theme [13. Atmospheric conditions and meteorological geographical features](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:02010R1089-20141231&from=EN#tocId1008)

Features encoded using [Observation and Measurement Simple Feature JSON encoding](https://github.com/opengeospatial/omsf-profile).

## 3. Envisaged use
Web API is a prototype service for testing purposes. The API use real-time data.

## 4. Requirements classes

Requirement class http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/pre-defined

## 5. Server-side technology

The server providing API is implemented using [SOFP server](https://github.com/vaisala-oss/sofp-core) with [Smartmet integration](https://github.com/fmidev/smartmet-sofp-backend).

The data is extracted from database using [SmartMet Server](https://github.com/fmidev/smartmet-server).

All used software is OSS with MIT license.

## 6. Endpoints and client applications

[http://inspire-oapif-demoloadbalancer-1863525030.eu-west-1.elb.amazonaws.com/](http://inspire-oapif-demoloadbalancer-1863525030.eu-west-1.elb.amazonaws.com/)

## 7. Issues

* The main issue is correct choice of the feature type (corresponding issue: https://github.com/INSPIRE-MIF/gp-ogc-api-features/issues/51)
* Another issue is choice of encoding (corresponding issue: https://github.com/INSPIRE-MIF/gp-ogc-api-features/issues/53). This is not a blocker issue but real interoperability requires harmonised encoding.
