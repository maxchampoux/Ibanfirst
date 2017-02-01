# Company Creation - API Service

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /companies/`](#post_companies) | Create a new company |
| [`PUT /companies/-{id}/confirm`](#put_companies) | Submit a new company |

<hr />

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
| companyCreationDatas | [Company Creation Datas Object](#companyCreationDatas_object) | Required | Standard information on the projet and the future activity of the company. |
| shareholdingStructures | Array[Shareholder Object](#shareholder_object) | Required | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| managerialStructures | Array[Manager Object](#manager_object) | Required | The regulatory list of the representatives, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |

**Example:**
```js
POST /companies/
{
    "companyCreationDatas": {companyCreationDatas}
    "shareholdingStructures": Array [{shareholder}]
    "managerialStructures": Array [{manager}]
}
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The  refering the financial movement. |
| companyCreationDatas | [Company Creation Datas Object](#companyCreationDatas_object) | Standard information on the projet and the future activity of the company. |
| shareholdingStructures | Array[Shareholder Object](#shareholder_object) | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| managerialStructures | [Manager Object](#manager_object) | The regulatory list of the representatives, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |

**Example:**
```js

"companies": {
    "id": "NT4edA",
    "status": "En attente de dépot de capital social",
    "companyCreationDatas": {companyCreationDatas}
    "shareholdingStructures": Array[{shareholder}]
    "managerialStructure": Array [{manager}]
}
```
<hr />

# API Objects  

* [Companies Object](#companies_object)
* [Company Creation Datas Object](#companyCreationDatas_object)
* [shareholder](#shareholder_object)
* [Company Shareholder Datas Object](#companyShareholderDatas_object)
* [Individual Shareholder Datas Object](#individualShareholderDatas_object)
* [managerialStructure](#managerialStructure_object)
* [Individual Managerial Datas Object](#individualManagerialDatas_object)
* [Account Object](#account_object)
* [Phone Object](#phone_object)
* [Individual Name Object](#individualName_object)
* [Amount Object](#amount_object)

## Details ##

#### <a id="companies_object"></a> Companies Object ####

My object to follow where I am in the company creation process.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](#type_id) | The IF code identifying the company to be created. |
| status | [status](#status) | The status of the company file. |
| companyCreationDatas | [companyCreationDatas](#companyCreationDatas) | Specific data required for "attestation de dépôt du capital social" |
| companyRegistrationDatas | [companyRegistrationDatas](#companyRegistrationDatas) | Specific data required for "libération du capital social" |
| shareholdingStructures | [Shareholding Structures Object](#shareholdingStructures_object) | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| managerialStructures | [Managerial Structures Object](#managerialStructures_object) | The regulatory list of the representatives, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |

**Example:**
```js

"companies": {
    "id": "NT4edA",
    "status": "En attente de dépot de capital social",
    "companyCreationDatas": {companyCreationDatas}
    "shareholdingStructures": Array [{shareholdingStructure}]
    "managerialStructure": Array [{managerialStructure}]
}
```
<hr />

#### <a id="companyCreationDatas_object"></a> Company Creation Datas Object ####

Specific information required for submitting a company creation file.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredNumber | String(100) | The registration number that must be provided after registration of the company to be created with the appropriate legal institution. |
| registeredName | String(100) | The legal name of the company to be created. |
| commercialName | String(100) | The commercial name of the company to be created. |
| tag | String(100) | The customized name of the company to be created. (Will only be used internally). |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| commercialAddress | [Address Object](#address_object) | The commercial address of the company to be created. |
| activityCode | [NAFID](../conventions/formattingConventions.md#NAF) | The code identifying the type of business of the company to be created. |
| legalForm | [legalForm](../conventions/formattingConventions.md#legalForm) | The legal form of the company to be created.. |
| authorizedCapital | [amount Object](#amount_object)  | The amount in shareholding capital as mentionned in the status. |

**Example:**

```js
"companyCreationDatas": {
    "registeredNumber": "81445561400010",
    "registeredName": "DJPAD",
    "commercialName": "null",
    "tag":"null",
    "registeredAddress": {address},
    "commercialAddress": {address},
    "activityCode":"6201Z",
    "legalForm":"SARL unipersonnelle",
    "authorizedCapital":{amount},
}
```

<hr />

#### <a id="shareholder_object"></a> Shareholder Object ####

This object shows the shareholder ownership and detailed information.

| Field | Type | Description |
|-------|------|-------------|
| ownership | Percentage | The pourcentage of ownership the shareholder has witha  direct company |
| shareholder | [Company Shareholding Datas Object](#companyShareholdingDatas_object) or [Individual Shareholding Datas Object](#individualShareholdingDatas_object) | Specific data that is required on shareholders |

**Example:**

```js
"shareholderStructures": [
    "shareholders": {
    	"ownership": 20%,
	"shareholderData": {companyShareholdingDatas},
    },
    "shareholders": {
    	"ownership": 80%,
	"shareholderDatas": {individualShareholdingDatas},
    },
}
```

<hr />

#### <a id="companyShareholdingDatas_object"></a> Company Shareholding Datas Object ####

This object shows the shareholder ownership and detailed information.

| Field | Type | Description |
|-------|------|-------------|
| ownership | Percentage | The pourcentage of ownership the shareholder has witha  direct company |
| shareholder | [Company Shareholding Datas Object](#companyShareholdingDatas_object) or [Individual Shareholding Datas Object](#individualShareholdingDatas_object) | Specific data that is required on shareholders |



#### <a id="individualDatas_object"></a> Contact Object ####

What we call a contact can be only an individual registered in France or Belgium.
When an entity is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the contact. |
| registeredNumber | String(100) | The unique legal identifier of the contact. |
| registeredNCountry| String(100) | The registering country of the contact. |
| registeredName | [Individual_Name Object](#individual_name_object) | The individual name of the contact. |
| tag | String(100) | The customized name of the contact. |
| address | [Address Object](#address_object) | The contact address. |
| birthDate | [Date](#type_date) | The birthdate of the contact. |
| phoneNumber | [phone Object](#phone_object)  | The phone number of the entity. |
| position | [position Object](#position_object)  | The position of the entity. |
| idProof | [idProof Object](#idProof_object)  | The url where the file is stored. |
| addressProof | [addressProof Object](#addressProof_object)  | The url where the file is stored. |

**Example:**

```js
"contact": {
    "id": "ND4ue2",
    "registeredNumber": "81445561400010",
    "registeredName": {individual_name},
    "tag":"null",
    "address": {address},
    "birthDate":"1980-11-04",
    "phoneNumber":{phone},
    "position":{position},
}
```

<hr />


#### <a id="account_object"></a> Account Object ####

When an Account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](#type_id) | The code identifying the account. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference of the account. |
| accountNumber | String(40) | The code specifying the account (can be either an Iban or an account number). |
| correspondentBank | [ID](#type_id) | The intermediary bank details, used to reach the beneficiary bank. |
| holderBank | [ID](#type_id) | The recipient bank details, holding the account. |
| holder | [Entity Object](#entity_object) | The recipient details, owner of the account. |

**Example:**

```js
"account": {
    "id": "NT4edA",
    "currency": "EUR",
    "tag": "My wallet account EUR",
    "accountNumber": "516981638516313513",
    "correspondantBank":{correspondentBank}
    "holderBank":{beneficiaryBank}
    "holder":{beneficiary}
}
```

<hr />


#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| street1 | String(255) | The street for the address described. |
| street2 | String(255) | The continuation street for the address described. |
| postCode | String(15) | The ZIP/Post code for the address described. |
| city | String(35) | The city for the address described. |
| state | String(2) | The state code for the address described. This field could be required if the country use a state system, like United States or Canada. To see a full list of state code, please refer to [this site](http://www.mapability.com/ei8ic/contest/states.php). |
| country | String(2) | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

**Example:**

```js
"address": {
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "10004",
	"city": "NEW YORK",
	"state": "NY",
	"country": "US"
}
```

<hr />

#### <a id="phone_object"></a> Phone Object ####

When a phone number is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| countryCode | String(4) | Numeric country code. Optional +, followed by 1-3 digits. |
| phoneNumber | String(15) | Country code and phone number. |

**Example:**

```js
"phone": {
    "countryCode": "+33",
    "phoneNumber": "81445561400010",
}
```

<hr />

#### <a id="individualName_object"></a> Individual Name Object ####

When a phone number is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| first | String(35) | The individual's first name. Truncated after the first 35 characters. |
| first | String(35) | The individual's middle name. Truncated after the first 35 characters. |
| last | String(35) | The individual's last name. Truncated after the first 35 characters. |

**Example:**

```js
"phone": {
    "first": "John",
    "last": "Doe",
}
```

<hr />

#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| value  | [QuotedDecimal](../conventions/formattingConventions.md#type_quoteddecimal) | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. |

**Example:**

```js
"amount": {
	"value": "10000.00",
	"currency": "GBP"
}
```

<hr />

# Formatting Conventions #  

### <a id="type_date"></a> Date Type ###

The Date type represents a date with its year, month and day.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| Date | String | `^[0-9]{4}\-[0-9]{2}\-[0-9]{2}$` - `YYYY-MM-DD` | A String representing a date by its year, month and day in month. | `2015-07-14` |
