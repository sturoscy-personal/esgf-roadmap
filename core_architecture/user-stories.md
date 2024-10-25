# User Stories for the ESGF-NG Core Architecture

## Dataset Publication

Bernard is a member of a climate modeling team at a federal agency that has committed to
producing some of the CMIP7 data. He's already been authorized to publish to a specific ESGF project. (See stories below.) After his team has completed model outputs for a specific
set of variables, Bernard is responsible for publishing the resulting data. Bernard downloads
and configures the ESGF Publisher code, then runs it over the model data. He enters a minimal
set of metadata manually and the code extracts the rest of the metadata automatically. The
Publisher code authenticates Bernard with the **ESGF-East IAM** service. Because Bernard has
been authorized to publish for this project, the Publisher pushes the dataset’s metadata to the **ESGF-East STAC Catalogue** using the catalogue's **STAC Transaction API**. This adds an event to the **ESGF-wide event stream** with
the data’s metadata and availability.

## Registering a New ESGF Data Producer Project

Mary is the project lead for a climate modeling team at a consortium of universities. The team she leads will be publishing data as part of CMIP7.
Because all of the universities in the consortium are in European countries, Mary chooses to use the **ESGF-East STAC Catalogue**. Based on that choice, Mary registers the project with the **ESGF-East IAM service**, EGI Check-In. EGI Check-In establishes a new user attribute that signifies permission to publish to Mary's project and grants Mary (and Mary's designated alternates) the ability to add that attribute to anyone using EGI Check-In to signify that they are authorized to publish datasets to Mary's project. Now, Mary shares the details of her project, including this new EGI Check-In attribute, with the operators of the **ESGF-East STAC Catalogue**, who in turn configure the catalogue's **STAC Transaction API** to require that attribute when someone attempts to publish a dataset to that ESGF project.

## Authorizing an Individual to Publish to a Registered ESGF Project

Eric is a member of Mary's ESGF Project who needs to publish data for the project. When Eric is ready to publish datasets, he contacts Mary and requests permission to publish to the project. Becuse Mary's project has chosen to use the **ESGF-East STAC Catlogue**, Mary uses EGI Check-In to add her project's user attribute to Eric's EGI Check-In account. Now, whenever Eric uses the ESGF Publisher to publish data to the **ESGF-East STAC Catalogue**, the catalogue's **STAC Transaction API** finds the required attribute in Eric's profile and authorizes publication requests he makes against the project.

## Dataset Retraction

Amanda is a member of a climate modeling team at a federal agency that has produced and
published some of the CMIP7 data. The team recently discovered that some of their model
outputs were produced with the wrong input parameters. Amanda needs to retract the datasets
that resulted. Amanda runs the ESGF Publisher code and specifies the IDs of the datasets
to be retracted. The Publisher authenticates Amanda with the **ESGF-West IAM** service and
pushes a retraction request to the **ESGF-West STAC Catalogue** using its **STAC Transaction API**. Because Amanda has been
authorized to initiate retractions for this project, the **STAC Transaction API** adds retraction events to the
**ESGF-wide event stream** with the dataset IDs and explanations. Both the **ESGF-East and ESGF-West
STAC Catalogues** process the event and update their records for that dataset to ensure that
users can no longer be presented with download links. The explanation for the retraction will
be processed by the Errata service listening to the event stream. In addition, each ESGF data
node that subscribes to the event stream can now process the retraction locally if it has a local
replica of the dataset. Furthermore, any other subscribers to the event stream can use this
retraction information to inform their research.

## Replicating Datasets to a Data Node

Luka is an operator of an ESGF data node at a national supercomputing center. He is
responsible for ensuring that his center’s ESGF data node has all of the CMIP7 datasets that
are included in the center’s replication policy. He's already registered his data node with the **ESGF-East STAC Catalogue**. Luka installs the ESGF Replicator service
and configures it to subscribe to the **ESGF-wide event stream** and monitor all publish events.
When it sees a dataset that matches the center's replication policy, Luka's ESGF Replicator
service copies the dataset’s files from any of the data nodes that already have the
dataset and then makes a “replica add” request to the **ESGF-East STAC Catalogue** using its **STAC Transaction API** adding the
new location availability for the dataset. Because Luka is authorized to add his
node as a new location for datasets, the **STAC Transaction API** creates a “replica add” event on the **ESGF-wide event stream** with the new replica information. Each ESGF index that subscribes to
the event stream can now update its local index with the new replica information.

## Registering an ESGF Data Node

Xavier is an operator of an ESGF data node at a national supercomputing center. Because his center is located in the United States, he has decided to use the **ESGF-West STAC Catalogue** to report new replicas available at his data node. The **ESGF-West STAC Catalogue**'s ESGF IAM service is Globus. When Xavier is ready to start creating replicas, he creates a Globus group and adds his Replicator's application ID to the group. He contacts the operators of the **ESGF-West STAC Catalogue** and gives them details about his data node and the Globus group he's using. The Catalogue administrators configure the **ESGF-West STAC Catalogue**'s **STAC Transaction API** to authorize members of the group to perform replica add and replica remove actions for any ESGF dataset. Now, when Xavier's ESGF Replicator creates or removes replicas on his data node, it can update the corresponding entries in the **ESGF-West STAC Catalogue** with the changed replica information.

## Discover and Download Relevant ESGF Datasets

Lisa, a graduate student in ecology, has a hypothesis she wants to test involving precipitation
controls on extremes of gross primary productivity. She logs into an ESGF Metagrid web application and searches for these variables from the E3SM model. The Metagrid application uses the **ESGF-West STAC Catalog** to fulfill search requests. While Lisa did not realize that so many
experiments were part of CMIP7, the search prioritizes the commonly accessed ‘historical’
experiment so it was easy to hone in on the results she needs. The search suggests that she might
also be interested in the model’s cell areas and land fractions, which she had not considered
but will need to take proper summations over land regions of the globe. She builds a data cart in Metagrid and submits a transfer request with Globus to copy the files to her local machine where she has an environment with custom code ready to perform the analysis.
