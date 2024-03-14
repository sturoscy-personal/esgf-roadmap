# DKRZ / CEDA ESGF search futures sprints

## Sprint 1: installing new CEDA STAC at DKRZ

*Vision*: To bring DKRZ's STAC stack up to date with CEDA's.

### Tasks
* Install Radiant Earth's API (uninstalling CEDA's deprecated API) at DKRZ
* Install / update STAC generator (from CEDA) at DKRZ
* Update DKRZ recipes to reference their existing CMIP6 data
* Run STAC generator on their CMIP6 assets

### Definition of done
* A STAC catalogue exists at DKRZ containing references to their CMIP6 data
* A user can search the DKRZ STAC catalogue and get results for data held at DKRZ

## Sprint 2: authentication and authorisation of STAC endpoints

*Vision*: Secure the transaction endpoint to allow trusted partners to write into a STAC catalogue.

### Tasks

* Investigate replacing requests in STAC generator with HTTPX
* Decide which OAuth2 flow to use
* Write FastAPI routers and dependancies for securing endpoints

### Definition of done
* CEDA and DKRZ transaction endpoints are secured via OAuth2 (flow to be determined in task 2)
* CEDA can externally write to DRKZ's STAC catalogue transaction endpoint
* DRKZ can externally write to CEDA's STAC catalogue transaction endpoint

## Sprint 3: replicating assets from DKRZ to CEDA and vice-versa

*Vision*: To replicate catalogues of assets between DKRZ and CEDA.

### Tasks
* Determine methodlogy for deciding what to import and what to export - i.e. what does
the other institution already have?
* Determine how to represent the same data at two different URLs.
* Export assets from CEDA to DKRZ 
    * Write a harvester
    * Improve performance with asyncIO & dask to parallelise
* Import assets from DKRZ to CEDA
    * Write an importer
    * Improve performance with asyncIO & dask to parallelise

### Definition of done:
* User's can search for assets at either institution and get correct and consistent results.  
* Some assets may not be present at both institutions.  For example, users should be able to search 
for a CMIP asset at DKRZ but get a URI for the data at CEDA, as the asset does not exist at DKRZ.

## Sprint 4: generate Kerchunk files from netCDF item references
