# ESGF-NG Data Challenges

Preparing for the first wave of data publication from CMIP7 Fast Track, these
data challenges test the key functionality and behavior of the new ESGF-NG
core architecture.

* The data challenges are meant to be executed by the ESGF
core architecture operations team, as they require operator privileges.
* The data challenges **are not** meant to be undertaken sequentially. Any
of the data challenges may be undertaken by the team as soon as the
prerequisites are available, and as long as the infrastructure setup for each
challenge is kept separate, they may be run in parallel.
* Each challenge may be run as many times as necessary until successful
results are achieved, as long as the prerequisite conditions are
re-established for each run.

## Challenge Phases

Each of the following tests should be performed in three rounds, or "stages," with increasing degrees of operational realism.

* Round 1 may be considered to have passed without authentication, authorisation, or validation.
* Round 2 may be considered to have passed with authentication, “type level” validation, and without authentication.
* Round 3 (final round) must include authentication, authorisation, and full “CV level validation.”

## Planned Data Challenges

1. [Publication with Index Synchronization](./01-Publication.md)
2. [Retraction](./02-Retraction.md)
3. [Update](./03-Update.md)

## Schedule Goals

* First week of December 2024: Round 1 for 01, 02, 03
* End of December 2024: Rounds 1 and 2 for 01, 02, 03
* End of January 2025: Rounds 1, 2, and 3 for 01, 02, 03

## Additional Challenges (TBD)

4. Replication
5. External Publishers
6. Client Applications (Metagrid, intake-esgf, others?)
7. Replica Remove (unless it's already been covered by 03-Update?)
8. Onboarding, aka Registration (Projects, Data Nodes)
9. Error/exception Validation (as many error conditions as we can think of)
