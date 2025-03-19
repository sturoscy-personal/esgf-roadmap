# Contributing to ESGF-NG

ESGF is a global federation with participants from many countries. Participants are supported by local government agencies. The ESGF Executive Committee (EC) coordinates contributions from independent participants.

The groundwork for the ESGF-NG initiative was provided by the July 2020 [ESGF Future Architecture Report](https://doi.org/10.5281/zenodo.3928223).
The ESGF-NG initiative was started by participants at the *10th ESGF Conference*, Rockville, MD, April 23–26, 2024.

At the 2024 conference, responsibility for moving ESGF-NG forward was assumed jointly by the UK's Center for Environmental Data Analysis (CEDA) and the US Department of Energy's ESGF2-US project (with participants at Oak Ridge National Laboratory, Lawrence Livermore National Laboratory, and Argonne National Laboratory).
* CEDA had already prototyped a STAC catalogue of ESGF CMIP6 datasets and assumed responsibility for operating an ESGF-East STAC catalog.
* ESGF2-US, in partnership with Globus at the University of Chicago, assumed responsibility for operating an ESGF-West STAC catalog as well as operating the global ESGF event service, by which the East and West catalogs are synchronized.
* CEDA and ESGF2-US have subsequently worked together to design and coordinate ESGF-NG. See the [status updates](../status/README.md) for current status.

## How to Contribute

CEDA and ESGF2-US are providing the East and West STAC catalogs and the global event stream. We are currently executing a series of data challenges to test the end-to-end behavior of the core architecture. Two key ingredients of these data challenges are end users and sample data.
* If you are a modeling center that expects to produce data for the CMIP7 activity, you can volunteer as an end user for one or more of the data challenges. This will help us validate our onboarding, authorization, and publishing features. You'll need to supply one or more people who will be onboarded as a data publisher, and they will need to commit to publishing some ESGF data as part of the data challenge and then verifying that it appears as expected in the East and West catalogs.
* We are particularly interested in sample data that is unique in some way. This will help us test the new features in ESGF-NG that validate metadata schema and controlled vocabularies. If you expect to produce CMIP7 data that is novel in some way, you can volunteer to publish a sample of this data as part of one or more data challenges.

Beyond data publication, the ESGF-NG core architecture (specifically, the STAC catalog) is also used by ESGF data discovery and access applications. Examples of these applications include: Metagrid, ESGValTool, Intake-ESGF, and ESGPull. If you are a software developer and can contribute to any of these applications, you can volunteer to help update one or more of these applications to use the new ESGF-NG STAC catalog.

ESGF-NG's event stream opens the door to significant improvements to the data replication tools used by ESGF data nodes. We have envisioned, but not yet implemented, an [ESGF Replicator](https://github.com/ESGF2-us/esgf-replicator) application that can be used by ESGF data node operators to automate data replication to their local systems as it is published. If you're a software developer with some familiarity with CMIP6/7 datasets, you can volunteer to help design and develop the ESGF Replicator. 
