# Progress meeting of INSPIRE MIG Action 2020.1 "Use of OGC API-Features as an INSPIRE Download service"

## Date and time
02/10/2020 10:00 - 12:00 CEST (via WebEx)

## Agenda

1. Review of the updated TG
2. Deployment evidence (if not already done, please provide additional examples by using the GitHub template)
3. What is (possibly) still missing
4. Planning of future activities for submission of the INSPIRE Good practice
5. AOB (if needed)

  
## Main decisions

### 1. Review of the updated TG
* An updated Technical spec is made available through PR #63 reflecting all changes discussed during the previous call of the group as documented [here](https://github.com/INSPIRE-MIF/gp-ogc-api-features/blob/master/action%202020.1/meetings/progress%20meeting%20040920207/minutes.md). 

    * **ACTION:** Following a round of discussion within the group all the modifications are accepted, and the pull request will be merged. Action to merge the PR.

* The way in which `/rec/geojson/geojson-inspire` is written is imprecise. The text should be amended to better reflect the work done in the [GitHub space of MIWP Action 2017.2 on GeoJSON as INSPIRE encoding](https://github.com/INSPIRE-MIF/2017.2). Additional text (outside the actual recommendation) should be provided to remind that 'In order to conform to the legal requirements, the encoding rules different from the default should be properly documented'. 

   * **ACTION** Update the spec on GeoJSON through a new PR.

### 2. Deployment evidence
* Presentation by BRGM on work done under the API4INSPIRE and INSIDE. The overview is available [here](https://github.com/INSPIRE-MIF/gp-ogc-api-features/blob/master/deployments/deployment-brgm_inside.md).
* Presentation on work done for multriple data themes in NRW based on ldproxy. The overview is available [here](https://github.com/INSPIRE-MIF/gp-ogc-api-features/blob/master/deployments/deployment-nrwde.md).
   
### 3. What is missing
* The currently existing spec is too narrow and not capturing specific use cases, e.g. those involving data retrieval on the fly from multidimensional arrays. Such cases would require work on the standardisation level (e.g. by OGC) that can then be converted to INSPIRE specs. @tervo will insert an issue to raise this.

* A dedicated label is created for issues that have a `MIG implication`.  **ACTION** Issues that are relevant to the MIG (e.g. for possible updates of the IR) to be labeled accordingly.


### 4. Future activities
* Following a discussion, the group agrees to use the already fixed regular meeting slot (6 November 10:00 - 12:00) and organise a webinar to promote the work done in preparation for submission of the good practice. In the meantime the following will be completed:
    * Move the defined ATS to an annex of the spec. **ACTION**
    * Provide an Annex with mapping of the operations required by the NS Regulation to the 'OGC API - Features standard' **ACTION**
