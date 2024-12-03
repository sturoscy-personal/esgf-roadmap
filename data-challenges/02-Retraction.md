# ESGF-NG Data Challenge 02: Retraction

**Purpose:** Confirm that when a retraction event occurs in either the East or the West ESGF Catalog, the corresponding entry in the catalog is marked as retracted in both the East and West Catalogs. This is an end-to-end test of the entire ESGF-NG core architecture.

## External Dependencies

* All rounds: ESGF Publisher that can be installed in test environment and that can be configured to publish to either the East or West STAC Catalog.
* Round 2 only: ESGF onboarding procedures implemented in both Globus and EGI Check-in 

## Round 1

Round 1 may be considered to have passed without authentication and authorisation. Items marked with (*) are arbitrary: specific values/types may be decided by the testing teams but must be known to both teams before the challenge is undertaken.

### Prerequisites

1. The ESGF-NG Event Stream is operational and has a set of streams configured for the challenge run. (These streams should be newly created or emptied at the beginning of each challenge run.)
2. East and West STAC Transaction APIs deployed and registered to create events on the Event Stream and specific streams used for this challenge.
3. Two ESGF Publisher installations: one configured to publish to the East STAC Catalog, the other configured to publish to the West STAC Catalog. The ESGF Publisher must have updated dependencies such that it can be installed without issue.
4. Two sets of at least 500(*) CMIP6 dataset metadata payloads, one for the East publisher, one for the West publisher. All metadata payloads should contain acceptable ESGF metadata for a single dataset. Each payload must represent a unique dataset.
5. Two additional sets of at least 50(\*) dataset retraction payloads, one for the East publisher, one for the West publisher. At least 20%(\*) of these should reference datasets that **are not** in the set of datasets published by either the East or the West publisher. The rest should reference datasets that **are** in the set of datasets published.
6. Two back-end indices, one for the East STAC Catalog and one for the West STAC Catalog. These should be empty at the start of each challenge run.
7. East and West event consumer services deployed, registered to use the Event Stream, and registered to write to its corresponding back-end index.
8. Two STAC Discovery APIs, one for the East STAC Catalog and one for the West STAC Catalog.
9. A Jupyter notebook(*) that uses the STAC API to perform catalog searches in East and West catalogs and compares the results.

### Round 1 Procedure

## Round 2

To pass Round 2, the system must perform authentication and authorization.

### Additional Prerequisites

In addition to the prerequisites for Round 1, we'll also need the following for Round 2.

10. Two onboarded publisher identities, one onboarded to the East catalog, one onboarded to the West catalog.
11. At least one other identity that hasn't been onboarded to the East or West catalogs.

### Round 2 Procedure
