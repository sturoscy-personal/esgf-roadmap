# Event Schema

## metadata
- User/Client ID
- Authorization service
- How the record was generated (publisher version)
- Creation time
- Kafka schema version

## Data
- Type of payload (STAC/SOLR)
- Data version (future proofing for if STAC isn't used)
- Payload (method, parameters, record)

## Example events
### Publish
ESGF Record with STAC Payload
```
{
    "metadata": {
        "auth": {
            "auth_policy_id": "ESGF-Publish-00012",   # We need registered auth policies?
            "target_data": {
              "collection_id": COLLECTION_ID,     # from STAC request
              "data_node_id": DATA_NODE_ID        # from STAC request
            },
            "requester_data": {
              "auth_service": AUTH_SERVICE,       # e.g., "auth.globus.org"    
              "sub": OAUTH_SUB_VALUE,             # e.g., "b16b12b6-d274-11e5-8e41-5fea585a1aa2"
              "username": OAUTH_USERNAME_VALUE,   # e.g., "lliming@uchicago.edu"
              "name": OAUTH_NAME_VALUE,           # e.g., "Lee Liming"
              "email": OAUTH_EMAIL_VALUE,         # e.g., "lliming@uchicago.edu"
              "identity_provider": OAUTH_IDENTITY_PROVIDER_ID,   # e.g., "0dcf5063-bffd-40f7-b403-24f97e32fa47"
              "identity_provider_display_name": OAUTH_IDENTITY_PROVIDER_NAME   # e.g., "University of Chicago"
            },
            "auth_basis_data": {
              "authorization_basis_type": "group",    # e.g., "group" or "attribute"
              "authorization_basis_service": GROUP_SERVICE,  # e.g., "groups.globus.org"
              "authorization_basis": [    # There may be multiple reasons to authorize a request
                {"group_id": GROUP_ID,                # e.g., "89ea3bda-6645-11e8-9427-1ada61684422"
                 "member_id": MEMBER_ID,              # e.g., "4e868912-e4be-11e5-88c5-dbe133110c3e"
                 "member_username": MEMBER_USERNAME   # e.g., "lukasz@uchicago.edu"
                },
                {"group_id": GROUP_ID,                # e.g., "89ea3bda-b60c-11e8-9427-0a5456ef4422"
                 "member_id": MEMBER_ID,              # e.g., "4e868912-e4be-11e5-88c5-dbe133110c3e"
                 "member_username": MEMBER_USERNAME   # e.g., "lukasz@uchicago.edu"
                },
                {"group_id": GROUP_ID,                # e.g., "89ea3bda-32e5-9999-9427-124341684422"
                 "member_id": MEMBER_ID,              # e.g., "4e868912-e4be-11e5-88c5-dbe133110c3e"
                 "member_username": MEMBER_USERNAME   # e.g., "lukasz@uchicago.edu"
                }
              ]
            }
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    },
    "data": {
        "type": "STAC",
        "version": "1.0.0",
        "payload": {
            "method": "POST",
            "collection_id": "cmip6",
            "item": <STAC_record>
        }
    }
}
```



### Update
#### Replacement
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    },
    "data": {
        "type": "STAC",
        "version": "1.0.0",
        "payload": {
            "method": "POST",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
            "item": <STAC_record>
        }
    }
}
```
#### Partial update
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    },
    "data": {
        "type": "STAC",
        "version": "1.0.0",
        "payload": {
            "method": "PUT",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
            "item": <partial_STAC_record>
        }
    }
}
```


### Revoke
#### Soft Delete
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    },
    "data": {
        "type": "STAC",
        "version": "",
        "payload": {
            "method": "PUT",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
            "item": {"hidden": true}
        }
    }
}
```
#### Hard Delete
```
{
    "metadata": {
        "auth": {
            "client_id": "<client_id>",
            "server": "esgf-login.ceda.ac.uk"
        },
        "publisher": {
            "package": "esgf_publisher",
            "version": "1.1.1"
        },
        "time": "2024-07-04T14:17:35Z",
        "schema_version": "1.0.0",
    },
    "data": {
        "type": "STAC",
        "version": "",
        "payload": {
            "method": "DELETE",
            "collection_id": "cmip6",
            "item_id": "CMIP6.ScenarioMIP.UA.MCM-UA-1-0.ssp245.r1i1p1f2.Amon.psl.gn.v20190731",
        }
    }
}
```
