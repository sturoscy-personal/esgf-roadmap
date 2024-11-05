# User Stories for the ESGF-NG Core Architecture

## 1. Dataset Publication

Bernard is a member of a climate modeling team at a federal agency that has committed to
producing some of the CMIP7 data. He's already been authorized to publish to a specific ESGF project. (See stories below.) After his team has completed model outputs for a specific
set of variables, Bernard is responsible for publishing the resulting data. Bernard downloads
and configures the ESGF Publisher code, then runs it over the model data. He enters a minimal
set of metadata manually and the code extracts the rest of the metadata automatically. The
Publisher code authenticates Bernard with the **ESGF-East IAM** service. Because Bernard has
been authorized to publish for this project, the Publisher pushes the dataset’s metadata to the **ESGF-East STAC Catalogue** using the catalogue's **STAC Transaction API**. This adds an event to the **ESGF-wide event stream** with
the data’s metadata and availability.

## 2. Registering a New ESGF Data Producer Project

Mary is the project lead for a climate modeling team at a consortium of universities. The team she leads will be publishing data as part of CMIP7.
Because all of the universities in the consortium are in European countries, Mary chooses to use the **ESGF-East STAC Catalogue**. Based on that choice, Mary registers the project with the **ESGF-East IAM service**, EGI Check-In. EGI Check-In establishes two new user attributes that signifies permission to (1) publish and (2) retract within Mary's project and grants Mary (and Mary's designated alternates) the ability to add those attributes to anyone using EGI Check-In to signify that they are authorized to publish and/or retract datasets in Mary's project. Now, Mary shares the details of her project, including this new EGI Check-In attribute, with the operators of the **ESGF-East STAC Catalogue**, who in turn configure the catalogue's **STAC Transaction API** to require the corresponding attribute when someone attempts to publish or retract a dataset in that ESGF project.

## 3. Authorizing an Individual to Publish to a Registered ESGF Project

Eric is a member of Mary's ESGF Project who needs to publish data for the project. When Eric is ready to publish datasets, he contacts Mary and requests permission to publish to the project. Becuse Mary's project has chosen to use the **ESGF-East STAC Catlogue**, Mary uses EGI Check-In to add her project's publication user attribute to Eric's EGI Check-In account. Now, whenever Eric uses the ESGF Publisher to publish data to the **ESGF-East STAC Catalogue**, the catalogue's **STAC Transaction API** finds the required attribute in Eric's profile and authorizes publication requests he makes against the project.

## 3b. Authorizing an Individual to Retract Datasets from a Registered ESGF Project.

Eric is a member of Mary's ESGF Project who occasionally needs to retract data from the project. The first time Eric is ready to retract a dataset, he contacts Mary and requests permission to retract in the project. Because Mary's project has chosen to use the **ESGF-East STAC Catlogue**, Mary uses EGI Check-In to add her project's retraction user attribute to Eric's EGI Check-In account. Now, whenever Eric uses the ESGF Publisher to publish data to the **ESGF-East STAC Catalogue**, the catalogue's **STAC Transaction API** finds the required attribute in Eric's profile and authorizes retraction requests he makes against the project.

## 4. Dataset Retraction

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

## 5. Replicating Datasets to a Data Node

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

### 6. Replication request for a Data Node

**a**: direct connection to ESGF Replicator Webapp

Vickie is a researcher who uses a National High Performance Computer (NHPC) to perform her analysis and wishes to do some research with
ESGF data that isn't currently hosted at NHPC. Vickie signs into Metagrid, searches for relevant datasets and adds them to her cart.
She selects to issue download request for these datasets to NHPC which redirects her to the request page of NHPC's ESGF Replicator Webapp
(logging in if required). She submits the request and awaits action from the data manager at NHPC.

Alternatively, she refines a search to datasets of interest and issues a query based replication request via a button at the top of the
search results page

**b**: manual connection to ESGF Replicator Webapp

Vickie is a researcher who uses a National High Performance Computer (NHPC) to perform her analysis and wishes to do some research with
ESGF data that isn't currently hosted at NHPC. Vickie signs into Metagrid, searches for relevant datasets and adds them to her cart.
She selects to download a replication request file which she then uploads to the request page of NHPC's ESGF Replicator Webapp (logging
in if required). She submits the request and awaits action from the data manager at NHPC.

Alternatively, she refines a search to datasets of interest and downloads a query based replication request via a button at the top of
the search results page

### 7. Updating replicator replication policy from requests

Ashley is a Data Manager at a national supercomputing center.  He is responsible for moderating data replication requests that are received
on the ESGF Replicator Webapp. Ashley logs into the webapp to make a decision whether to approve or decline each request based on if the request
came from someone who is a registered user at the center, the request size (bytes and #files) is within policy, (other TBC).  Optionally, he
could contact the user for clarification (outside of the webapp) using contact details displayed in the webapp. The ESGF Replicator Webapp will
send a message to the user submitting the request notifying them of the outcome.  For approvals, the ESGF webapp will connect to the ESGF
Replicator Service API and add the requested replication policy.

## 8. Registering an ESGF Data Node for replication

Xavier is an operator of an ESGF data node at a national supercomputing center. Because his center is located in the United States, he has decided to use the **ESGF-West STAC Catalogue** to report new replicas available at his data node. The **ESGF-West STAC Catalogue**'s ESGF IAM service is Globus. When Xavier is ready to start creating replicas, he creates a Globus group and adds his Replicator's application ID to the group. He contacts the operators of the **ESGF-West STAC Catalogue** and gives them details about his data node and the Globus group he's using. The Catalogue administrators configure the **ESGF-West STAC Catalogue**'s **STAC Transaction API** to authorize members of the group to perform replica add and replica remove actions for any ESGF dataset. Now, when Xavier's ESGF Replicator creates or removes replicas on his data node, it can update the corresponding entries in the **ESGF-West STAC Catalogue** with the changed replica information.

## 9. Discover and Download Relevant ESGF Datasets (metagrid example)

Lisa, a graduate student in ecology, has a hypothesis she wants to test involving precipitation
controls on extremes of gross primary productivity. She logs into an ESGF Metagrid web application and searches for these variables from the E3SM model. The Metagrid application uses the **ESGF-West STAC Catalog** to fulfill search requests. While Lisa did not realize that so many
experiments were part of CMIP7, the search prioritizes the commonly accessed ‘historical’
experiment so it was easy to hone in on the results she needs. The search suggests that she might
also be interested in the model’s cell areas and land fractions, which she had not considered
but will need to take proper summations over land regions of the globe. She builds a data cart in Metagrid, logins, and submits a transfer request with Globus to copy the files to her local machine where she has an environment with custom code ready to perform the analysis.

## 10. Dataset Immigration

The Financially Optimistic Organization (FOO) joins the ESGF federation as a data node and publishes 100 datasets. The operation of
their node is funded using project funding. At the end of the project, money dries up and FOO places a message on the ESGF slack group
advising of their intention to leave the federation. Boston Atmospheric Repository (BAR) agree to the immigration request for their
datasets. BAR replicates all FOO's datasets. FOO issues a change of ownership publication event for all of their datasets to BAR,
which BAR accepts. Finally, FOO issues remove replica message, shuts down its data node and requests to leave the federation.

## 11. Orientation to ESGF-NG, aka Documentation

Marc is any of the following.
* a current or prospective operator of an ESGF data node
* a team lead or team member of an ESGF data-producing project
* a current or prospective IPCC author
* an ESGF WIP member
* a current or prospective software engineer who needs to work on ESGF software
* a member of the public who's curious about how ESGF works

Marc needs to find out how part of the ESGF system works. Marc consults the ESGF documentation and finds the information he needs on any of the following topics.
* How to establish a new ESGF data node
* How to operate an ESGF data node
* How to set up automated data replication for an ESGF data node
* How to remove replicas of ESGF datasets from an ESGF data node
* What ESGF does (and does not do) for ESGF data nodes
* How to establish/register a new ESGF data-producing project
* How to authorize team members in an ESGF data-producing project
* How to publish data in ESGF as a member of a registered data-producing project
* How to (and when to, and when not to) retract data in ESGF
* How to (and when to, and when not to) update a dataset in ESGF
* How to cite ESGF datasets
* How to contribute to ESGF software engineering efforts
* How ESGF is organized, both technically (architecture, software, systems) and socially (teams, communication channels, relationship with other WCRP bodies)

## 12. Setting up a new ESGF Data node

Xavier has been asked to establish a new ESGF data node at a national supercomputing center. After consulting ESGF documentation (use case 11), he provisions hardware/hosts and sets up Ansible for storage and services,
uses the ESGF Ansible definition to download and deploy containers, registers his data node with the ESGF Executive Committee (EC) and either the East or West ESGF STAC Catalog and ESGF Event Stream and configures the resulting credentials in his deployed services.
* If his Data Node is to be used for publishing new ESGF datasets, he installs and configures the ESGF Publisher application,  tells the ESGF projects using the Data Node for publishing how to add his Data Node to their registered Data Nodes so they can publish datasets from it.
* If his Data Node is to be used for replicating existing datasets from other Data Nodes, he installs the ESGF Replicator and configures it with a replication policy and subsequently monitors its logs to confirm that it is behaving as expected.

## 13. Registering an ESGF Community Service that Uses the ESGF Event Stream

Judith is building a new ESGF Community Service that will subscribe to events on the ESGF Event Stream and act on them. After consulting the ESGF documentation (use case 11), Judith contacts the operators of the ESGF Event Stream and provides a statement of intent for her service. The operators review her request and agree that the service is a legitimate use of the Event Stream. The operators issue credentials that Judith can use in her Community Service to subscribe to topics in the Event Stream and consume events.
