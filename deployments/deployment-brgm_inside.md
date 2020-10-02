
## 1. Data provider
BRGM (French Geological Survey)
OFB (French Office for Biodiversity) through "INSIDE - environmental information systems research center"


## 2. Thematic scope
- French Surface Water
  - River gage: 
    - INSPIRE EF:EnvironmentalMonitoringFacility
      - OGC API Feature : GeoJSON, GML, JSON-LD
  - River network 
    - INSPIRE HY: HydroNode, WatercourseLink, WatercourseLinkSequence, Watercourse
      - OGC API Feature : GeoJSON, GML, JSON-LD (not configured yet)
  - Water flow/height Observation
    - ISO/OGC:Observations & Measurements (INSPIRE D2.9) -> OGC SensorThings API (ST API)
  - URI : references between EF <-> ST API


- French Hydrogeological data
  - Environmental Groundwater Quantity Monitoring Facility
    - INSPIRE EF:EnvironmentalMonitoringFacility
      - WFS 2 : DONE
      - OGC API Feature -> GeoJSON, GML, JSON-LD : conf on-going for API4INSPIRE project
  - Hydrogeological unit 
    - OGC:GroundWaterML2.0 (mapped to INSPIRE GE:Hydrogeology package)
      - WFS 2 : DONE
      - OGC API Feature -> GeoJSON, GML, JSON-LD : conf on-going for API4INSPIRE project
  - Groundwater quantity Observation : groundwater level, temperature, conductivity
    - ISO/OGC:Observations & Measurements (INSPIRE D2.9) -> OGC SensorThings API (STA)
  - URI : references between EF <-> ST API


## 3. Envisaged use
Deployment and maturation in test environment on top of production system (data instances) within INSPIRE dynamics for API4INSPIRE project
APIs will be transfered to production afterwards
- French Surface Water : Hub'Eau plateform https://hubeau.eaufrance.fr/ 
- French Hydrogeological data : BRGM information system

## 4. Requirements classes
- Requirement class http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/pre-defined
- Requirement class (with issues - see below) http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/geojson 

TODO SG : X-check with the others

## 5. Server-side technology
Geoserver 2.17.2 for WFS 2.0 and OGC API Feature
- GML:  app-schema plugin
- GeoJSON : 
  - Now : using heuristics introduced in GeoServer core since 2.16.0 (see http://blog.geoserver.org/2019/09/18/geoserver-2-16-released/): GeoJSON generated from GML structure
  - Target : using Features-Templating community module (https://docs.geoserver.org/latest/en/user/community/features-templating/index.html) : available since end of Sept 2020
- JSON-LD : using the Features-Templating community module(initially using community module which is now integrated in the other)
PostGre/GIS  DB 

Note : SensorThings API -> Fraunhofer FROST + PostGre/GIS DB 

## 6. Endpoints and client applications
Test Endpoints are not fully opened. However via URI it partially works. 
- Surface water example :
  - Feature Instance
    - https://iddata.eaufrance.fr/id/HydroStation/A021005050?f=application%2Fgeo%2Bjson 
     - starting with HydroStation (=River gage) allows to also ask for ?f=application%2Fld%2Bjson, ou ?f=application%2Fgml%2Bxml (works also doing real content negotiation in tools like PostMan or cURL)
     - makes possible to traverse the hydro feature graph via featureOfInterest or ultimateFeatureOfInterest choosing between geojson or GML (JSON-LD not implemented yet)
     - makes it possible to access observation data using hasObservation (will provide application/JSON payload according to OGC ST API )
   - API endpoint : https://iddata.eaufrance.fr/api/hydrometryFAPI/ (not opened yet, will update this page as we go)
 - Ground water  example :
   - Feature Instance : TODO SG 
   - API endpoint : TODO SG 

Clients : 
  - Web
    - discussed in API4INSPIRE
    - dev ongoin at BRGM
  - QGIS GML Application Schema toolbox for GML
    - starting to work on allowing to work on GeoJSON (not only GeoJSON serialization but also the 'linked data' part of the plugin)

## 7. Issues
* with http://inspire.ec.europa.eu/id/spec/oapif-download/1.0/req/geojson
  * provides harmonised data according to the [IRs for ISDSS -> needs to respect all the semantic work done in INSPIRE before even when moving to REST API providing other serialization as GML
  * "the Web API SHOULD follow the INSPIRE UML-to-GeoJSON encoding rule." 
    *  examples here https://github.com/INSPIRE-MIF/2017.2/blob/master/GeoJSON/efs/simple-environmental-monitoring-facilities.md 
       *  come from BRGM WFS 2.0 app-schema endpoint (and SOS), not written by us (and not involved)
       *  this is not how we are currently working to expose this in GeoJSON according to INSPIRE EF semantics (which we know pefectly)
       *  moving to GeoJSON does not imply flattening everything ! 
       *  that's why we work on GeoJSON heuristics on Geoserver 2.16.0 then Features-Templating community module within API4INSPIRE project
* need harmonized pattern to provide references to other content in GeoJSON properties for proper client set-up
    * @id/@type/name : JSON-LD flavoured
    * id/type/name : the same but @ neutral
    * @href/@title : GML/XML flavoured (current Geoserver core heuristics for GML -> GeoJSON )


* Another issue is choice of encoding (corresponding issue: https://github.com/INSPIRE-MIF/gp-ogc-api-features/issues/53). This is not a blocker issue but real interoperability requires harmonised encoding.
