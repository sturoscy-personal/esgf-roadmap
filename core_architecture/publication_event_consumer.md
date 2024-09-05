# Publication Events Consumer

This document outlines the requirements and acceptance criteria for an event consumer that updates a federation-wide ESGF search index. The event types that will be published are documented in [Publication Actions](publication_actions.md).

- The event consumer MUST subscribe to the ESGF event stream and MUST NOT accept instructions from other sources.
- For each event consumed, the consumer MUST update its associated search index to establish consistency with the event payload.
- Changes to the index MUST be made in the same order as the events on the event stream.
- The event consumer MAY implement index changes as batches, so long as the ordering constraint is satisfied.

## Consistency requirements

The ESGF Federation will operate at least two federation-wide indices: an east index operated by CEDA (UK), and a west index operated by ESGF2-US (USA).

We require eventual consistency between all federation-wide indices.

- Q: Are there any requirements for drift tolerance between federation-wide indices or timeliness of updates, beyond the eventual consistency agreement? (If there are none, we should explicitly state so here.)

## Authentication and authorization

The event consumer will authenticate with the ESGF event stream service using the event service's native authentication mechanism.

The event consumer will authenticate with the search index service using the search service's native authentication mechanism.

The event consumer MUST assume that all events on the ESGF event stream are properly authorized and update its index accordingly. While authorization provenance included in events, this information IS NOT meant to be used by the consumer to enforce stricter authorization policies than those already performed by the STAC Ingest service prior to event publication.

## Event topics

- Q: What topics will the event stream offer to which consumers can subscribe?

- Q: To which topic(s) must a consumer subscribe in order to receive all events needed to guarantee index consistency?

## Contents of search indices

The search indices populated by the event consumer MUST be structured such that they support a STAC-compliant discovery API. (The discovery API MUST be compliant with the STAC API and schema specifications.)

The search indices MUST be configured such that only the event consumer is permitted to make changes to the content governed by ESGF.

The search indices MAY contain additional content that is not governed by ESGF, but this content may not conflict with or cause inconsistency with the content that is governed by ESGF.

## Interaction with search indices

NOTE: The following information doesn't seem relevant to requirements and acceptance criteria, but is preserved until we agree that it doesn't belong here.

Globus Search has done research into STAC and has created some initial scripts for ingesting STAC items into a Globus Search index https://github.com/globusonline/sc-33660-stac-in-search

Through the research that the Globus Search team has done, they have created some scripts to get STAC items from CEDA and place them in a Globus Search index https://github.com/globusonline/sc-33660-stac-in-search Should we modify and use their scripts for a Publish event?

## Logging / audit requirements

The event consumer MUST produce a log of all updates to the search index that can be used to perform an audit against the event stream contents to troubleshoot unexpected index content.
