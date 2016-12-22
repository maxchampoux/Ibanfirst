# Ibanfirst
Onboarding - account opening API

# API Objects #  

* [Entity Object](#entity_object)

## Details ##

#### <a id="entity_object"></a> Entity Object ####

What we call a Entity can be only an company incorporate in France or Belgium.
When an entity is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the entity. |
| registeredParentNumber | String(100) | The unique legal identifier of the entity mother house. |
| registeredNumber | String(100) | The unique legal identifier of the entity opening the account. |
| registeredName | String(100) | The legal name of the entity. |
| commercialName | String(100) | The commercial name of the entity. |
| tag | String(100) | The customized name of the entity. |
| address | [Address Object](#address_object) | The account owner address. |
| activityCode | [NAFID](../conventions/formattingConventions.md#NAF) | The code identifying the type of business. |
| registrationDate | [Date](../conventions/formattingConventions.md#type_date) | The legal date of creation of the entity. |
| legalForm | [legalForm](../conventions/formattingConventions.md#legalForm) | The legal form of the entity. |
| authorizedCapital | [Amount Object](#amount_object)  | The amount in shareholding capital. |

**Example:**

```js
"entity": {
    "id": "ND4ue2",
    "registeredParentNumber": "	814455614",
    "registeredNumber": "81445561400010",
    "registeredName": "DJPAD",
    "commercialName": "null",
    "tag":"null",
    "address": {address},
    "activityCode":"6201Z",
    "registrationDate":"2015-11-04",
    "legalForm":"SARL unipersonnelle",
    "authorizedCapital":{amount},
}
```

<hr />
