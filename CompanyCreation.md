# Company Creation - API Service

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /companies/`](#post_companies) | Create a new company |

## Details ##

#### <a id="post_companies"></a> Create a new company ####

```
Method: POST 
URL: /companies/
```
Create a new company.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| informations | [Informations Object](#informations_object)) | Required | Standard information on the projet and the future activity of the company. |
| shareholdingStructure | String(100) | Required | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| managerialStructure | String(100) | Required | The regulatory list of the representatives, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
