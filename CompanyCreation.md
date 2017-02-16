# Company Creation - API Service

## GETTING STARTED WITH IBANFIRST REST COMPANY CREATION API ##

#### 1. Create your company project ####

Please use the following service to create your company project:
```
POST /companies/
```

#### 2. Ask for an IBAN ####

```
PUT /companies/-{id}/iban
```

This service will return an IBAN for the deposit of the "Capital Social" of your future company.
We will require the signature, ID and power of attorney of all founders at this stage. 

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

You can use either this API service, a FTP server or a tailor-made webhook to retrieve your certificate of deposit.

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
| [`PUT /companies/-{id}/projectComplete`](#put_companiesComplete) | Ask for a certificate of deposit |
| [`GET /companies/-{id}/document/certificateDeposit/`](#getDocuments_certificateIncorporation) | Retrieve your certificate of deposit |
| [`PUT /companies/-{id}/document/certificateIncorporation`](#put_companiesCertificateIncorporation) | Upload your Kbis |
| [`PUT /companies/-{id}/releaseDeposit`](#put_companiesReleaseDeposit) | Ask for the release of the deposit |
| [`PUT /companies/-{id}/`](#put_companies) | Update information relative to a company creation project|
| [`GET /companies/-{id}/`](#get_companies) | Retrieve infromation relative to a company creation project |
| [`DELETE /companies/-{id}/`](#delete_companies) | Delete a company creation project |
| [`POST /companies/-{id}/documents/`](#putDocuments_companies) | Submit documents relative to a company creation project |
| [`DELETE /companies/-{id}/documents/-{id}`](#putDocuments_companies) | Delete documents relative to a company creation project |
| [`PUT /companies/-{id}/shareholder/-{id}/`](#put_companies) | Update information relative to a shareholder |
| [`GET /companies/-{id}/shareholder/-{id}/`](#put_companies) | Retrieve information relative to a shareholder |
| [`PUT /companies/-{id}/shareholder/-{id}/documents/`](#put_companies) | Update document relative to a shareholder |

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

On the main founder ([Shareholding Structure Object](#shareholdingStructure_object)):
* type 
* isMainFounder
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* shareholdingStructure (if type = "corporate" and for all shareholders on the 2 level owning +25%)
* email

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
    		"city": "Paris",str
   		"country": "FR",
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"type": "Individual",
		"isMainFounder": true,
		"registeredIndividualName": {
			"civility": "M",
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
| companies | [Companies Object](../conventions/formattingConventions.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
"companies": {companies},
```

<hr />

#### <a id="put_companiesIban"></a> Ask for an IBAN ####

```
Method: PUT 
URL: /companies/-{id}/iban
```
Ok well, at this stage we will require the following datas & documents:

On your future company ([Shareholding Structure Object](#shareholdingStructure_object)):
* legalForm
* registeredName
* registeredAddress
* activityCode
* sharesNumber
* sharesCapital
* liberatedPercentage
* document = "openingAccountAgreement", "projectArticleOfAssociation"

On the main founder ([Shareholding Structure Object](#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* document = "IDProof" (or "certificateOfIncorporation" if type = "corporate")
* birthDate (if type = "individual")
* birthCountry (if type = "individual")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholders on the 2 level owning +25%)

On the other individual founders ([Shareholding Structure Object](#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

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
	"registeredName": "Rocket Startup",
	"registeredAddress": {
		"street": "4 NEW YORK PLAZA, FLOOR 15",
		"postCode": "75008",
		"city": "Paris",
		"country": "FR",
	},
	"activityCode": 334B,
	"legalForm": "EURL",
	"sharesCapital": {
		"value": 100000.00,
		"currency": "EUR",
	}
	"sharesNumber": 100.00,
	"documents": {
		"document": {
			"type": "openingAccountAgreement",
			"id": "Rocket Startup - Opening Account Agreement",
		},
		"document": {
			"type": "projectArticleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"sharesNumber": 50000.00,
		"type": "Individual",
		"isMainFounder": true,
		"email": "mch@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredIndividualCountry": FR,
		"registeredIndivdualNationality": "France",
		"birthDate": 25-06-1991,
		"birthCountry": "France",
		"isPep": true,
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
			},
		},
	},
	"shareholder": {
		"sharesNumber": 50000.00,
		"type": "Corporate",
		"legalForm": "EURL",
		"isMainFounder": false,
		"email": "myHolding@email.com",
		"registeredCorporateAddress": {
			"street": "1 rue de l'université",
			"postCode": "75006",
			"city": "Paris",
			"country": "France",
		},
		"documents": {
			"document": {
				"type": "certificateOfIncorporation",
				"id": "KBIS - myHolding",
			},
			"document": {
				"type": "articleOfAssociation",
				"id": "KBIS - myHolding",
			},
		},
		 "shareholdingStructure": {
			"shareholder": {
				"id": "WE4edA",
				"sharespercentage": 100%,
				"registeredIndividualName": {
					"civility": "M",
					"firstName": "Maxime",
					"lastName": "Champoux",
				},
				"registeredIndividualCountry": FR,
				"registeredIndivdualNationality": "France",
				"birthDate": 25-06-1991,
				"birthCountry": "France",
				"isPep": true,
				"documents": {
					"document": {
						"type": "idProof",
						"id": "Maxime Champoux - CNI",
					},
				},
			},
		},
	},
    },		
},

```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| companies | [Companies Object](../conventions/formattingConventions.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
"companies": {companies},
```

#### <a id="put_companiesComplete"></a> Ask for a certificate of deposit ####

```
Method: PUT 
URL: /companies/-{id}/projectComplete
```
At this stage, we will require additional data and documents:

On your future company ([Shareholding Structure Object](#shareholdingStructure_object)):
* legalForm
* registeredName
* registeredAddress
* activityCode
* sharesNumber
* sharesCapital
* liberatedPercentage
* document = "openingAccountAgreement", "projectArticleOfAssociation", "articleOfAssociation, "businessPlan"

On the main founder ([Shareholding Structure Object](#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* document = "IDProof" (or "certificateOfIncorporation" if type = "corporate")
* birthDate (if type = "individual")
* birthCountry (if type = "individual")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

On the other founders ([Shareholding Structure Object](#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry depending on the type)
* registeredIndividualName (or registeredCorporateName depending on the type)
* is Pep (if type = "individual")
* document = "IDProof" (or "certificateOfIncorporation" if type = "corporate")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

By submitting your project, you consider that your project is complete and we will proceed to a due diligence review of your project, the shareholders and the presence of the right deposits. When we are fine, your project status will be updated to "certificate of deposit ready" and you will be able to retrieve your certificate using the request ```GET ../companies/-{id}/document/certificateDeposit```.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |


**Example of a Call containing anll required information at this stage:**
```js
PUT /companies/NT4edA/iban
{
    "companyCreationDatas": {
	"registeredName": "Rocket Startup",
	"registeredAddress": {
		"street": "4 NEW YORK PLAZA, FLOOR 15",
		"postCode": "75008",
		"city": "Paris",
		"country": "FR",
	},
	"activityType": 334B,
	"legalForm": "EURL",
	"sharesCapital": {
		"value": 100000.00,
		"currency": "EUR",
	}
	"sharesNumber": 100.00,
	"documents": {
		"document": {
			"type": "openingAccountAgreement",
			"id": "Rocket Startup - Opening Account Agreement",
		},
		"document": {
			"type": "projectArticleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"sharesNumber": 50000.00,
		"type": "Individual",
		"isMainFounder": true,
		"email": "mch@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredIndividualCountry": FR,
		"registeredIndivdualNationality": "France",
		"birthDate": 25-06-1991,
		"birthCountry": "France",
		"isPep": true,
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
			},
		},
	},
	"shareholder": {
		"id": "WZ4edA",
		"sharesNumber": 10000.00,
		"type": "Individual",
		"isMainFounder": true,
		"email": "mch@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "John",
			"lastName": "Doe",
		},
		"registeredIndividualCountry": FR,
		"registeredIndivdualNationality": "France",
		"birthDate": 25-06-1991,
		"birthCountry": "France",
		"isPep": true,
		"documents": {
			"document": {
				"type": "idProof",
				"id": "John Doe - CNI",
			},
		},
	},
	"shareholder": {
		"sharesNumber": 40000.00,
		"type": "Corporate",
		"legalForm": "EURL",
		"isMainFounder": false,
		"email": "myHolding@email.com",
		"registeredCorporateAddress": {
			"street": "1 rue de l'université",
			"postCode": "75006",
			"city": "Paris",
			"country": "France",
		},
		"documents": {
			"document": {
				"type": "certificateOfIncorporation",
				"id": "KBIS - myHolding",
			},
			"document": {
				"type": "articleOfAssociation",
				"id": "KBIS - myHolding",
			},
		},
		 "shareholdingStructure": {
			"shareholder": {
				"id": "WE4edA",
				"sharespercentage": 100%,
				"registeredIndividualName": {
					"civility": "M",
					"firstName": "Maxime",
					"lastName": "Champoux",
				},
				"registeredIndividualCountry": FR,
				"registeredIndivdualNationality": "France",
				"birthDate": 25-06-1991,
				"birthCountry": "France",
				"isPep": true,
				"documents": {
					"document": {
						"type": "idProof",
						"id": "Maxime Champoux - CNI",
					},
				},
			},
		},
	},
    },		
},

```

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
    },
   	
},

```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| companies | [Companies Object](../conventions/formattingConventions.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
"companies": {companies},
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

