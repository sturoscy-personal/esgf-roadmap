# ESGF-NG Data Challenge 03: Update

**Purpose:** Confirm that when an update event occurs in either the East or the West ESGF Catalog, the corresponding entry in both East and West Catalogs shows the expected update. This is an end-to-end test of the entire ESGF-NG core architecture.

## External Dependencies

* All rounds: ESGF Publisher that can be installed in test environment and that can be configured to publish to either the East or West STAC Catalog.
* Rounds 2 and 3 only: ESGF onboarding procedures implemented in both Globus and EGI Check-in, and Final ESGF metadata schema (expected keys and value types)
* Round 3 only: ESGF CV service

## Round 1

Round 1 may be considered to have passed without authentication, authorisation, or validation. Items marked with (*) are arbitrary: specific values/types may be decided by the testing teams but must be known to both teams before the challenge is undertaken.

### Prerequisites

1. The ESGF-NG Event Stream is operational and has a set of streams configured for the challenge run. (These streams should be newly created or emptied at the beginning of each challenge run.)
2. East and West STAC Transaction APIs deployed and registered to create events on the Event Stream and specific streams used for this challenge.
3. Two ESGF Publisher installations: one configured to publish to the East STAC Catalog, the other configured to publish to the West STAC Catalog. The ESGF Publisher must have updated dependencies such that it can be installed without issue.
4. Two sets of at least 500(*) CMIP6 dataset metadata payloads, one for the East publisher, one for the West publisher. All metadata payloads should contain acceptable ESGF metadata for a single dataset. Each payload must represent a unique dataset.
5. Two additional sets of at least 50(\*) dataset update payloads, one for the East publisher, one for the West publisher. Each set of updates should be a mix of:
  * Adding an asset of a new type. (E.g., Globus transfer link, Kerchunk endpoint.)
  * Publish an updated version of a retracted dataset. (Should work.)
  * Publish an update to a retracted version of a dataset. (Should generate an error unless the update is a replica remove.)
  * Updates to datasets that don't exist in the catalog. (Should generate an error.)
  * Q: Are there any other types of updates we should test? (\*)
6. Two back-end indices, one for the East STAC Catalog and one for the West STAC Catalog. These should be empty at the start of each challenge run.
7. East and West event consumer services deployed, registered to use the Event Stream, and registered to write to its corresponding back-end index.
8. Two STAC Discovery APIs, one for the East STAC Catalog and one for the West STAC Catalog.
9. A Jupyter notebook(*) that uses the STAC API to perform catalog searches in East and West catalogs and compares the results.

### Round 1 Procedure

## Round 2

To pass Round 2, the system must perform authentication, authorization, and “type level” validation. Controlled vocabulary validation is not required.

### Additional Prerequisites

In addition to the prerequisites for Round 1, we'll also need the following for Round 2.

10. Two onboarded publisher identities, one onboarded to the East catalog, one onboarded to the West catalog.
11. At least one other identity that hasn't been onboarded to the East or West catalogs.
12. CMIP6 metadata schema (expected keys and corresponding value types) is available to both East and West teams.
13. Two additional sets of at least 10(\*) CMIP6 dataset update payloads, one for the East publisher, one for the West publisher. All of these update payloads should contain new keys or new values that **do not** conform to the CMIP6 metadata schema.

### Round 2 Procedure

## Round 3

To pass Round 3 (final round), the system must perform authentication, authorization, “type level” validation, and controlled vocabulary validation.

### Additional Prerequisites

In addition to the prerequisites for Rounds 1 and 2, we'll also need the following for Round 3.

14. ESGF-NG Controlled Vocabulary service is available for CV queries.
15. Two additional sets of at least 10(\*) update payloads, one for the East publisher, one for the West publisher. All of the payloads in each set must contain at least one value that doesn't conform to the ESGF controlled vocabulary for its corresponding key.

### Round 3 Procedure
