````md
# GoPay Power BI Extractor

Getting metadata and administration data from Power BI via REST API.

---

# Functionality Notes

This component uses Azure AD Service Principal authentication (`client_credentials` OAuth2 flow) to access the Power BI REST API.

The component extracts information about:

- Workspaces
- Users
- Datasets
- Dashboards
- Reports
- Gateways
- Datasources
- Dataset refreshes
- Dataset refresh schedules

---

# Prerequisites

Before using this extractor, create an Azure App Registration with:

- Power BI Service API permissions
- Client Secret
- Tenant access enabled for Service Principals

The Service Principal must also have access to the Power BI workspaces.

Required configuration parameters:

| Parameter | Description |
|---|---|
| `client_id` | Azure Application (Client) ID |
| `client_secret` | Azure Client Secret VALUE |
| `tenant_id` | Azure Directory (Tenant) ID |
| `incremental` | Enables incremental loading |

---

# Authentication

The component authenticates using OAuth2 Client Credentials flow.

Token endpoint:

```text
https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token
````

Request body:

```text
grant_type=client_credentials
client_id=<client_id>
client_secret=<client_secret>
scope=https://analysis.windows.net/powerbi/api/.default
```

---

# Features

The component exports Power BI metadata into Keboola output tables.

Supported entities:

* Groups (Workspaces)
* Workspace Users
* Datasets
* Dashboards
* Reports
* Gateways
* Gateway Datasources
* Dataset Datasources
* Dataset Refreshes
* Dataset Refresh Schedules

---

# Output

Generated output tables:

```text
data/out/tables/pbi_dashboards.csv
data/out/tables/pbi_dashboards_refresh.csv
data/out/tables/pbi_datasets.csv
data/out/tables/pbi_datasets_refresh.csv
data/out/tables/pbi_datasets_datasources.csv
data/out/tables/pbi_datasets_refresh_schedule_days.csv
data/out/tables/pbi_datasets_refresh_schedule_enable.csv
data/out/tables/pbi_datasets_refresh_schedule_times.csv
data/out/tables/pbi_datasets_refreshes.csv
data/out/tables/pbi_datasources_gateway.csv
data/out/tables/pbi_gateways.csv
data/out/tables/pbi_groups.csv
data/out/tables/pbi_groups_refresh.csv
data/out/tables/pbi_reports.csv
data/out/tables/pbi_reports_actual.csv
data/out/tables/pbi_users.csv
```

---

# Used APIs

Authentication:

```text
https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token
```

Power BI REST APIs:

```text
GET /v1.0/myorg/groups
GET /v1.0/myorg/groups/{groupId}/users
GET /v1.0/myorg/groups/{groupId}/datasets
GET /v1.0/myorg/groups/{groupId}/dashboards
GET /v1.0/myorg/groups/{groupId}/reports
GET /v1.0/myorg/gateways
GET /v1.0/myorg/gateways/{gatewayId}/datasources
GET /v1.0/myorg/groups/{groupId}/datasets/{datasetId}/refreshes
GET /v1.0/myorg/groups/{groupId}/datasets/{datasetId}/datasources
GET /v1.0/myorg/groups/{groupId}/datasets/{datasetId}/refreshSchedule
```

---

# Development

If required, change local data folder path in `docker-compose.yml`:

```yaml
volumes:
  - ./:/code
  - ./CUSTOM_FOLDER:/data
```

Clone repository and run locally:

```bash
git clone https://github.com/gopaydata/gopay.ex-pbi-extractor gopay.ex-pbi-extractor
cd gopay.ex-pbi-extractor
docker-compose build
docker-compose run --rm dev
```

Run tests:

```bash
docker-compose run --rm test
```

---

# Integration

For deployment and Keboola integration documentation:

https://developers.keboola.com/extend/component/deployment/

```
```
