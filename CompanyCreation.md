# Company Creation - API Service

## GETTING STARTED WITH IBANFIRST REST COMPANY CREATION API ##

#### 1. Create your company project ####

Please use the following service to create your company project :
```
POST /companies/
```

#### 2. Ask for an IBAN ####

```
PUT /companies/-{id}/iban
```

This service will return an IBAN for the deposit of the "Capital Social" of your future company.

#### 3. Ask for a certificate of deposit ####

```
PUT /companies/-{id}/projectComplete
```

Until your project is not complete, the following service will return errors mentionning the missings information.
When your project is complete, the analysis of your project and the generation of the certificate can take up to 48 hours. In case you have implemented a webhook, you will be notified as soon as this certificate is available. In other cases, you may call our API to get the status and retrieve the document when available.

#### 4. Retrieve your certificate of deposit ####

```
GET /companies/-{id}/documents/certificateDeposit
```

You can use either this service of a taylor-made webhook to retrieve your certificate of deposit.

#### 5. Upload your Kbis ####

```
PUT /companies/-{id}/certificateIncorporation
```

Until your project is not complete, the following service will return errors mentionning the missings information.
When your project is complete, you must upload the official certificate of incorporation.

#### 6. Ask for the release of the deposit ####

```
PUT /companies/-{id}/releaseDeposit
```

Until your project is not complete, the following service will return errors mentionning the missings information.
When your project is complete, you must upload the official certificate of incorporation.

#### XX. Know where you are in the process ####

```
GET /companies/-{id}/
```
You want to know if all shareholders has deposited funds to the IBAN? You can use this service for having more information on the status of your project.

#### XX. Update information in your project ####

```
PUT /companies/-{id}/
```
You have new information you want to share with us. That's great! Updating your project shall move it to the next step.

#### XX. Delete your project ####

```
DELETE /companies/-{id}/
```
We are so sad you are doing that. See you next time!

<hr />

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /companies/`](#post_companies) | Start a company creation project |
| [`PUT /companies/-{id}/iban`](#put_companiesIban) | Ask for an IBAN |
| [`PUT /companies/-{id}/complete`](#put_companiesComplete) | Ask for a certificate of deposit |
| [`GET /companies/-{id}/document/certificateDeposit/`](#getDocuments_certificateIncorporation) | Retrieve your certificate of deposit |
| [`PUT /companies/-{id}/document/certificateIncorporation`](#put_companiesCertificateIncorporation) | Upload your Kbis |
| [`PUT /companies/-{id}/releaseDeposit`](#put_companiesReleaseDeposit) | Ask for the release of the deposit |
| [`PUT /companies/-{id}/`](#put_companies) | Update the informations on my project |
| [`GET /companies/-{id}/`](#get_companies) | Get the status of my project |
| [`DELETE /companies/-{id}/`](#delete_companies) | Delete information on my project |
| [`POST /companies/-{id}/documents/`](#putDocuments_companies) | Submit documents to a company creation |
| [`DELETE /companies/-{id}/documents/-{id}`](#putDocuments_companies) | Submit documents to a company creation |

<hr />

## Details ##

#### <a id="post_companies"></a> Start a company creation project ####

```
Method: POST 
URL: /companies/
```
You want to create your company? That's great! Let's start you project now, only a minimum of information is needed to open a file:

On your future company ([Company Creation Data Object](#companyCreationData_object)):
* registeredName
* registeredAddress

On the founders' team ( [Shareholder Object](#shareholder_object) |):
* shareholder: type, isMainFounder, registeredIndividualName, registeredCountry, email

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| companyCreationData | [Company Creation Data Object](#companyCreationData_object) | Required | Standard information on the projet and the future activity of the company. |
| shareholdingStructure | Array<[Shareholder Object](#shareholder_object)> | Required | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. 

**Example:**
```js
POST /companies/
{
    "companyCreationData": {
    	"registeredName": "Rocket Startup",
	"registeredAddress": {
		"street": "4 NEW YORK PLAZA, FLOOR 15",
    		"postCode": "75008",
    		"city": "Paris",
   		"country": "FR",
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"type": "Individual",
		"isMainFounder": 1,
		"registeredIndividualName": {
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredIndividualCountry": FR,
		"email": "mch@ibanfirst.com",
	},
    },			
},
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](#type_id) | The internal reference for this company creation project. |
| status | [Status](#type_status) | The stage of your company creation project. |
| companyCreationData | [Company Creation Data Object](#companyCreationData_object) | Standard information on the projet and the future activity of the company. |
| shareholdingStructure | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| account | [Account Object](#account_object) | The IBAN that has been open for the purpose of creating the company. |

**Example:**
```js
"companies": {
    "id": NT4edA,
    "status": "Not yet submitted",
    "companyCreationData": {
    	"registeredName": "Rocket Startup",
	"registeredAddress": {
		"street": "4 NEW YORK PLAZA, FLOOR 15",
    		"postCode": "75008",
    		"city": "Paris",
   		"country": "FR",
	},
	"commercialName": null,
	"commercialAddress": null,	
	"tag": null,
	"productDescription": null,
	"activityCode": null,
	"legalForm": null,
	"authorizedCapital": null,
	"documents": null,
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"type": "Individual",
		"isMainFounder": 1,
		"ownershipPourcentage": null,
		"registeredIndividualName": {
			"firstName": "Maxime",
			"middleName": null,
			"lastName": "Champoux",
		}
		"registeredCorporateName": null,
		"registeredIndividualCountry": FR,
		"registeredIndividualNumber": null,
		"registeredCorporateNumber": null,
		"tag": null,
		"email": "mch@ibanfirst.com",
		"birthDate": null,
		"phoneNumber": null,		
		"position": "Astronaute",
		"documents": null,
		"shareholdingStructure": null,
	},
	"account": {
	    "currency": null,
	    "accountNumber": null,
	    "holderName": null,
	    "financialMovements": null,
        },
    },		
},
```
<hr />

#### <a id="put_companiesIban"></a> Ask for an IBAN ####

```
Method: PUT 
URL: /companies/-{id}/iban
```
Ok well, at this stage we will require some data and documents to be already specified in the project:

On your future company ([Shareholding Structure Object](#shareholdingStructure_object)):
* legalForm
* authorizedCapital

On the founders' team ( [Shareholder Object](#shareholder_object) |):
* shareholder: id, type, isMainFounder, ownership, email, individualCountry (or corporateCountry depending on the type), individualName (or corporateName depending on the type), document (type: IDProof and status: uploaded).

By submitting your project, you will have in return an IBAN that you can share with the co-founders for collecting the deposit of each one.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation. |

**Example:**
```js
PUT /companies/NT4edA/iban
{
    "companyCreationDatas": {
	"legalForm": "EURL",
	"authorizedCapital": {
		"value": "100000.00",
		"amount": "EUR",
	"documents": {
		"document": {
			"type": "businessPlan",
			"id": "Rocket Startup - Business Plan",
		},
		"document": {
			"type": "articleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
		},
	},
    }
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
			},
		}
	},
    },		
},

```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The internal reference for this company creation. |
| status | [Status](#type_status) | The stage of your company creation project. |
| companyCreationData | [Company Creation Data Object](#companyCreationData_object) | Standard information on the projet and the future activity of the company. |
| shareholdingStructure | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| account | [Account Object](#account_object) | The IBAN account that has been open for the purpose of creating the company. |

**Example:**
```js
"companies": {
    "id": NT4edA,
    "status": "Awaiting deposits",
    "companyCreationDatas": {
    	"activityCode": null,
	"legalForm": "EURL",
	"authorizedCapital": {
		"value": "100000.00",
		"amount": "EUR",
	"documents": {
		"document": {
			"type": "businessPlan",
			"id": "Rocket Startup - Business Plan",
			"status": "not uploaded",
		},
		"document": {
			"type": "articleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
			"status": "not uploaded",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"type": "Individual",
		"isMainFounder": 1,
		"ownershipPourcentage": 100%,
		"registeredIndividualName": {
			"firstName": "Maxime",
			"middleName": null,
			"lastName": "Champoux",
		}
		"registeredCorporateName": null,
		"registeredIndividualCountry": FR,
		"registeredIndividualNumber": null,
		"registeredCorporateNumber": null,
		"tag": null,
		"email": "mch@ibanfirst.com",
		"birthDate": null,
		"phoneNumber": null,		
		"position": "Astronaute",
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
				"status": "uploaded",
			},
		},
	},
    },
    "account": {
	"currency": EUR,
	"accountNumber": "FR914516981638516313513",
	"holderName": "Rocket Startup [En cours de création]",
	"financialMovements": null,
    },
}
```
<hr />

#### <a id="put_companiesComplete"></a> Ask for a certificate of deposit ####

```
Method: PUT 
URL: /companies/-{id}/complete
```
At this stage, we will require additional data and documents:

On your future company ([Company Creation Datas Object](#companyCreationDatas_object)):
* documents: type (articleOfAssociation, businessPlan), status: uploaded.
* activityCode

On the founders' team [Shareholding Structure Object](#shareholdingStructure_object):
* shareholdingStructure/ shareholder: type, isMainFounder, ownership, email, individualCountry (or corporateCountry depending on the type), individualName (or corporateName depending on the type), document (type: IDProof and status: uploaded).

By submitting your project, you consider that your project is complete and we will proceed to a due diligence review of of your project, the shareholders and the presence of the right deposits. When we are fine, your project status will be updated to "certificate of deposit ready" and you will be able to retrieve your certificate using the request ```GET ../companies/-{id}/document/certificateDeposit```.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |

**Example:**
```js
PUT /companies/NT4edA/certificateDeposit
{
    "companyCreationDatas": {
    	"activityCode": "8542Z",
	"documents": {
		"document": {
			"type": "businessPlan",
			"id": "Rocket Startup - Business Plan",
		},
		"document": {
			"type": "articleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
		},
	},
    }
   	
},

```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The internal reference for this company creation. |
| status | [Status](#type_status) | The stage of your company creation project. |
| companyCreationData | [Company Creation Data Object](#companyCreationData_object) | Standard information on the projet and the future activity of the company. |
| shareholdingStructure | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| account | [Account Object](#account_object) | The IBAN account that has been open for the purpose of creating the company. |

**Example**
```js
"companies": {
    "id": NT4edA,
    "status": "Project being reviewed",
    "companyCreationDatas": {
    	"activityCode": "8542Z",
	"legalForm": "EURL",
	"authorizedCapital": {
		"value": "100000.00",
		"amount": "EUR",
	"documents": {
		"document": {
			"type": "businessPlan",
			"id": "Rocket Startup - Business Plan",
			"status": "uploaded",
		},
		"document": {
			"type": "articleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
			"status": "uploaded",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"type": "Individual",
		"isMainFounder": 1,
		"ownershipPourcentage": 100%,
		"registeredIndividualName": {
			"firstName": "Maxime",
			"middleName": null,
			"lastName": "Champoux",
		}
		"registeredCorporateName": null,
		"registeredIndividualCountry": FR,
		"registeredIndividualNumber": null,
		"registeredCorporateNumber": null,
		"tag": null,
		"email": "mch@ibanfirst.com",
		"birthDate": null,
		"phoneNumber": null,		
		"position": "Astronaute",
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
				"status": "uploaded",
			},
		},
	},
    },
    "account": {
	"currency": EUR,
	"accountNumber": "FR914516981638516313513",
	"holderName": "Rocket Startup [En cours de création]",
	"financialMovements": {
		"financialMovement" {
			"id": "FX4edA",
			"bookingDate": "2017-07-14",
			"valueDate": "2017-07-14",
			"authorizedCapital": {
				"value": "100000.00",
				"amount": "EUR",
			}
			"sourceAccount": {
				"accountNumber": "FR650516981638516313513",
				"holderName": "Maxime Champoux",
			}
			"targetAccount": {
				"accountNumber": "FR914516981638516313513",
				"holderName": "Rocket Startup [En cours de création]",
			}
		}
	},
    },
}
```

<hr />

#### <a id="put_companiesCertificateIncorporation"></a> Upload your Kbis ####

```
Method: PUT 
URL: /companies/-{id}/document/CertificateIncorporation
```

At this stage, basically your company should be registered. Congratulation! Therefore, we will require the registration information on your company:
* Document: certificate of incorporation, signed article of association, status: uploaded.
* Registration number

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation. |

**Example:**
```js
PUT /companies/NT4edA/document/certificateIncorporation
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The internal reference for this company creation. |
| status | [Status](#type_status) | The stage of your company creation project. |
| companyCreationData | [Company Creation Data Object](#companyCreationData_object) | Standard information on the projet and the future activity of the company. |
| shareholdingStructure | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| account | [Account Object](#account_object) | The IBAN account that has been open for the purpose of creating the company. |

**Example:**
```js
"companies": {
    "id": NT4edA,
    "status": "Registration documents being reviewed",
    "companyCreationDatas": {
    	"activityCode": "8542Z",
	"legalForm": "EURL",
	"authorizedCapital": {
		"value": "100000.00",
		"amount": "EUR",
	"documents": {
		"document": {
			"type": "businessPlan",
			"id": "Rocket Startup - Business Plan",
			"status": "uploaded",
		},
		"document": {
			"type": "articleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
			"status": "uploaded",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"type": "Individual",
		"isMainFounder": 1,
		"ownershipPourcentage": 100%,
		"registeredIndividualName": {
			"firstName": "Maxime",
			"middleName": null,
			"lastName": "Champoux",
		}
		"registeredCountry": FR,
		"registeredNumber": null,
		"tag": null,
		"email": "mch@ibanfirst.com",
		"birthDate": null,
		"phoneNumber": null,		
		"position": "Astronaute",
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
				"status": "uploaded",
			},
		},
	},
    },
    "account": {
	"currency": EUR,
	"accountNumber": "FR914516981638516313513",
	"holderName": "Rocket Startup [En cours de création]",
    },
}
```
}
```
}
```
<hr />

# API Objects  

* [Companies Object](#companies_object)
* [Company Creation Datas Object](#companyCreationDatas_object)
* [Company Submit Datas Object](#companySubmitDatas_object)
* [Shareholder Object](#shareholder_object)
* [Founders Object](#founder_object)
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
| companyCreationDatas | [Company Creation Datas](#companyCreationDatas) | Specific data required for "attestation de dépôt du capital social" |
| companyRegistrationDatas | [Company Registration Datas](#companyRegistrationDatas) | Specific data required for "libération du capital social" |
| shareholdingStructures | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| managerialStructures | Array<[Founder Object](#founder_object)> | The regulatory list of the representatives, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. |
| accounts | [Account Object](#account_object) | The IBAN account that has been open for the purpose of creating the company. |

**Example:**
```js
"companies": {
    "id": NT4edA,
    "status": "En attente de dépot de capital social",
    "companyCreationDatas": {companyCreationDatas}
    "shareholdingStructures": [{shareholder}]
    "managerialStructures": [{founder}]
    "account": {account},	    
}
```
<hr />

#### <a id="companyCreationDatas_object"></a> Company Creation Datas Object ####

Specific information required for opening a company creation file.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredName | String(100) | The legal name of the company to be created. |
| commercialName | String(100) | The commercial name of the company to be created. |
| tag | String(100) | The customized name of the company to be created. (Will only be used internally). |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| commercialAddress | [Address Object](#address_object) | The commercial address of the company to be created. |
| activityCode | [NAFID](#NAF) | The code identifying the type of business of the company to be created. |
| legalForm | [Legal Form](#legalForm) | The legal form of the company to be created.. |
| authorizedCapital | [Amount Object](#amount_object)  | The amount in shareholding capital as mentionned in the status. |
| documents | Array<[Document Object](#document_object)> | The required documents for creating a company. |

**Example:**

```js
"companyCreationDatas": {
    "registeredName": "DJPAD",
    "tag":"null",
    "registeredAddress": {address},
    "commercialAddress": {address},
    "activityCode":"6201Z",
    "legalForm":"SARL unipersonnelle",
    "authorizedCapital":{amount},
    "documents": [
    	"document": {
		"type": "article of association",
		"tag": "NameOfTheDocument",
	}
	"document": {
		"type": "kbis",
		"tag": "NameOfTheDocument",
	}
    ]
}
```

<hr />


#### <a id="companySubmitDatas_object"></a> Company Creation Datas Object ####

Specific information required for submitting a company creation file.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredName | String(100) | The legal name of the company to be created. |
| commercialName | String(100) | The commercial name of the company to be created. |
| tag | String(100) | The customized name of the company to be created. (Will only be used internally). |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| commercialAddress | [Address Object](#address_object) | The commercial address of the company to be created. |
| activityCode | [NAFID](#NAF) | The code identifying the type of business of the company to be created. |
| legalForm | [Legal Form](#legalForm) | The legal form of the company to be created.. |
| authorizedCapital | [Amount Object](#amount_object)  | The amount in shareholding capital as mentionned in the status. |
| documents | Array<[Document Object](#document_object)> | The required documents for creating a company. |

**Example:**

```js
"companyCreationDatas": {
    "registeredName": "DJPAD",
    "registeredAddress": {address},
    "activityCode": "6201Z",
    "legalForm": "SARL unipersonnelle",
    "authorizedCapital": {
    	"value": 1000.00,
	"curency": EUR,
    }	
    "documents": [
    	"document": {
		"type": "article of association",
		"tag": "NameOfTheDocument",
	}
	"document": {
		"type": "kbis",
		"tag": "NameOfTheDocument",
	}
    ]
}
```

<hr />

#### <a id="companyRegistrationDatas_object"></a> Company Registration Datas Object ####

Additional information required for releasing "capital social".

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredNumber | String(100) | The unique legal identifier of the entity opening the account. |
| registrationDate | [Date](#type_date) | The legal date of creation of the entity. |

**Example:**

```js
"companyRegistrationDatas": {
    "registeredParentNumber": "	814455614",
    "registrationDate":"2015-11-04",
}
```

<hr />

#### <a id="shareholder_object"></a> Shareholder Object ####

This object shows the shareholder ownership and detailed information.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| ownership | Percentage | The pourcentage of ownership the shareholder has witha  direct company |
| shareholder | [Company Shareholding Datas Object](#companyShareholdingDatas_object) or [Individual Shareholding Datas Object](#individualShareholdingDatas_object) | Specific data that is required on shareholder. |


**Example:**

```js
"shareholderStructures": [
    "shareholder": {
    	"ownership": 20%,
	"shareholderData": {companyShareholdingDatas},
    },
    "shareholder": {
    	"ownership": 80%,
	"shareholderDatas": {individualShareholdingDatas},
    },
}
```

<hr />

#### <a id="companyShareholdingDatas_object"></a> Company Shareholding Datas Object ####

This object shows the shareholder ownership and detailed information.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredName | String(100) | The legal name of the company to be created. |
| commercialName | String(100) | The commercial name of the company to be created. |
| tag | String(100) | The customized name of the company to be created. (Will only be used internally). |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| commercialAddress | [Address Object](#address_object) | The commercial address of the company to be created. |
| activityCode | [NAFID](../conventions/formattingConventions.md#NAF) | The code identifying the type of business of the company to be created. |
| legalForm | [Legal Form](../conventions/formattingConventions.md#legalForm) | The legal form of the company to be created.. |

<hr />

#### <a id="individualShareholdingdataDatas_object"></a> Individual Shareholding Datas Object ####

This object shows the shareholder ownership and detailed information.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredNumber | String(100) | The unique legal identifier of the contact. |
| registeredCountry| String(100) | The registering country of the contact. |
| registeredName | [Individual Name Object](#individual_name_object) | The individual name of the contact. |
| tag | String(100) | The customized name of the contact. |
| address | [Address Object](#address_object) | The contact address. |
| birthDate | [Date](#type_date) | The birthdate of the contact. |
| phoneNumber | [Phone Object](#phone_object)  | The phone number of the entity. |
| position | [Position Object](#position_object)  | The position of the entity. |


**Example:**

```js
"contact": {
    "id": ND4ue2,
    "registeredNumber": "81445561400010",
    "registeredName": {individualName},
    "tag":"null",
    "address": {address},
    "birthDate":"1980-11-04",
    "phoneNumber":{phone},
    "position":{position},
}
```
<hr />

#### <a id="founder_object"></a> Founder Object ####

The Founder object.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredNumber | String(100) | The unique legal identifier of the contact. |
| registeredCountry| String(100) | The registering country of the contact. |
| registeredName | [Individual Name Object](#individual_name_object) | The individual name of the contact. |
| tag | String(100) | The customized name of the contact. |
| address | [Address Object](#address_object) | The contact address. |
| birthDate | [Date](#type_date) | The birthdate of the contact. |
| phoneNumber | [Phone Object](#phone_object)  | The phone number of the entity. |
| position | [Position Object](#position_object)  | The position of the entity. |

<hr />

#### <a id="account_object"></a> Account Object ####

When an Account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference of the account. |
| accountNumber | String(40) | The code specifying the account (can be either an Iban or an account number). |
| holderBank | [ID](#type_id) | The recipient bank details, holding the account. |
| holder | [Holder Object](#holder_object) | The recipient details, owner of the account. If the company is in pending creation, then [pending creation] will be added aside the owner name. |

**Example:**

```js
"account": {
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
| middle | String(35) | The individual's middle name. Truncated after the first 35 characters. |
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
| value  | [Quoted Decimal](../conventions/formattingConventions.md#type_quoteddecimal) | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. |

**Example:**

```js
"amount": {
	"value": "10000.00",
	"currency": "GBP"
}
```

<hr />


#### <a id="document_object"></a> Document Object ####

The list of document that can be submitted for a shareholder/ individual.


**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| idProof | [Id Types](#idTypes_object)  | The url where the file is stored. |
| addressProof | [Proof of Address Object](#addressProof_object)  | The url where the file is stored. |

**Example:**

```js
TBD
```


<hr />
# Formatting Conventions #  

### <a id="type_date"></a> Date Type ###

The Date type represents a date with its year, month and day.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| Date | String | `^[0-9]{4}\-[0-9]{2}\-[0-9]{2}$` - `YYYY-MM-DD` | A String representing a date by its year, month and day in month. | `2015-07-14` |
