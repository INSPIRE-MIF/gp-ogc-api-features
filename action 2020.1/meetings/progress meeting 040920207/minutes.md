# Progress meeting of INSPIRE MIG Action 2020.1 "Use of OGC API-Features as an INSPIRE Download service"

## Date and time
04/09/2020 10:00 - 12:00 CEST (via WebEx)

## Agenda

  1. Deployments of OGC API-Features
  2. Outstanding issues for improvement of the existing spec
  3. AOB
  
## Main decisions

### 1. Documenting the deployments of OGC API - Features
* A dedicated folder 'deployments' will be created for capturing examples of APIs in a structured manner (as *.md documents). 
    * **ACTION:** JRC to tidy the repository 
* The existing template will be updated to capture under section 5. licensing conditions for the server-side technology that is in use. Already documented examples by DE and FI will be updated to reflect the licensing of the software.
    * **ACTION:** Colleagues who  already provided descriptions of deployments to update Section 5. of their description with the licensing information.
### 2. Outstanding issues (condensed from all GitHub issues):
* Strict naming convention of collections.
    * The existing requirement based on the IRs for ISDSS has limitations and is not universally applicable (e.g. the Specialised Observation types have generic names that are reused in different themes that are not helpful for describing the content that is served).
	 **Decision:** Convert the existing '/req/pre-defined/collection-naming' to a recommendation, and ask for a justification for any deviation from the naming convention defined by the IRs for ISDSS. To be investigated how the API will expose such justifications.
* Metadata link as html
    * **Decision:** It is agreed to add a recommendation for provision of the metadata link as html (in addition to xml)
* Bulk download and paging
    * **Decision:** We agree to allow paging for cases where datasets are too big for retrieval with a single request. This would also help address the issues with the frequently updated data.
* Bulk download per collection or for the whole dataset
    * **Decision:** Either of the two options (or both) should be allowed. This should be reflected in the guideline.
* Requirement and recommendations on 'enclosure'
    * **Decision:** We will keep 'enclosure' as the name of the relation, as it is a link relation type registered with the [IANA](https://www.iana.org/assignments/link-relations/link-relations.xhtml). Recommendations for CRS, title (something more digestible that 'enclosure'), and 'length' attributes to be considered.
* OGC API-Features that can satisfy the requirements for an INSPIRE View service.
    * the NS Regulation requirement for supporting at least png or gif is outdated. Vector tiles should be considered as a more modern approach. The group will capture such recommendations to be provided to the MIG.
* A recommendation to be added (possibly by reusing exiting material from the OGC) on CORS.
### 3. Way forward
* Issues to be updated according with decisions made during the meeting and briefly captured above.
* Co-editing session for the spec is planned for the week of 7-11 September. A Doodle will follow for those who volunteered.
* Next virtual meeting of the group is scheduled for 2nd October at 10:00. All are encouraged to contribute to the spec and provide deployment evidence.
