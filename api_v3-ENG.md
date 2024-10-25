<h1 align="center">
  <img alt="" title="" src="./assets/header_indecx_API.png" />
</h1>

![Badge](https://img.shields.io/badge/integrations-v3-blue) 

The IndeCX API was developed to enable various integrations between your system and the features available on our platform. In this version, the API includes an improved authentication and authorization process using a Bearer token, which is a secure, simple, scalable, flexible, and standardized approach to ensuring API security.

In this documentation, you will find usage examples for each method.

## Swagger
https://indecx.com/api-docs/#/

## Introduction
The API uses Bearer Token-based authentication and authorization to protect restricted access routes. The Bearer Token must be sent in the Authorization header of all requests that require authentication.

## Authentication
To authenticate with the API, you must send an authentication request with your user credentials (**company-key**) sent via header, available within the account settings on the Indecx platform. The API will return a Bearer Token, which you must use in all future requests that require authentication.

## Endpoint /get
![Badge](https://img.shields.io/badge/GET-authorization--token-orange)
```bash
https://indecx.com/v3/integrations/authorization/token

| Params  | Description |
| ------------- | ------------- |
| company-key  | $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX  |


## Request Example
```javascript
GET /v3/integrations/authorization/token HTTP/1.1
Host: indecx.com
Company-Key: $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX

```
## Response Example
```javascript
HTTP/1.1 200 OK
Content-Type: application/json

{
	"authToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVlLU1Rra..."
}

```


## Authorization Header Example
The Bearer Token must be sent in the Authorization header of all requests that require authentication. The token is valid for 30 minutes.

```javascript
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVlLU1Rra...
```


# POST Transactional Invite Trigger
![Badge](https://img.shields.io/badge/POST-.v3/integrations/%2Fsend%2F-green)

Imagine a scenario where you want to trigger a satisfaction survey for each new client transaction in your system, whether it’s a prospecting, sales, or interaction process, etc. With the transactional invite trigger method, this is possible and very straightforward.

The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/send/[Action_Identifier]
```
Actions must be created on the platform app-indecx.com, and the triggers can be carried out via platform (manual) or via API. When triggered via API, a JSON must be sent in the body to the URL mentioned above, and your authentication will be handled through the company-key provided and sent via HEADER. It's important to send the action identifier and follow a specific JSON format in the body.

## JSON Configuration
**Important**
Email and phone number are mandatory fields, depending on the configuration of the trigger type. Check the configurations set when creating the action. To be able to send data to additional columns (indicators), it's necessary to create the indicators within the action beforehand and use exactly the same names created on the platform. In our example below, the additional columns are: Branch, Area, Region, and Type. 

## **Request**
```javascript
POST /v3/integrations/send/T0AXXX HTTP/1.1
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

{
   "nome":"Jose Paulo da Silva",
   "email":"clientejosepaulo@gmail.com",
   "telefone":"11998720000",
   "Filial":"Filial B",
   "Area":"SP",
   "Regiao":"Campinas",
   "Tipo":"Vendas",
   "scheduling":""
}
```
## **Response**
```javascript
{
  "message": "Disparo configurado com sucesso!"
}
```
For scheduled triggers, the invite will have the status **scheduled** in IndeCX, and if necessary, it is also possible to cancel the trigger directly on the platform in the menu Triggers >> Schedules.
<h1 align="center">
  <img alt="" title="" src="./assets/scheduling.png" />
</h1>

**Important:**
The “scheduling” field uses UTC format, allowing trigger scheduling with local times from other countries.
Example parameter **“2020-12-16T20:20:00.000”** will trigger at the following times:
Brazil 17:20:00 (UTC -3), USA New York 16:20:00 (UTC -5)

# POST Batch Invite Trigger
![Badge](https://img.shields.io/badge/POST-.v3/integrations/%2Fsend%2Fbulk-green)

You can also trigger invites automatically for a batch of clients all at once.

The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/send/bulk/[Action_Identifier]
```
Actions must be created on the platform app-indecx.com, and the triggers can be carried out via platform or API. When triggered via API, a JSON must be sent in the body to the URL mentioned above, and your authentication will be handled through the company-key provided and sent via HEADER. It's important to send the action identifier and follow a specific JSON format in the body.



## JSON Configuration
**Important**
Email and phone number are mandatory fields, depending on the trigger type configuration. Check the configurations set when creating the action. To send data to additional columns (indicators), it is necessary to create the indicators within the action beforehand and use exactly the same names created on the platform. This API has a maximum limit of 1,000 clients per request.

```javascript
POST /v3/integrations/send/bulk/T0AXXX HTTP/1.1
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
{
   "customers":[
      {
         "nome":"ClienteA",
         "email":"ClienteD@gmail.com",
         "telefone":""
      },
      {
         "nome":"ClienteB",
         "email":"ClienteD@gmail.com",
         "telefone":""
      },
		  {
         "nome":"ClienteC",
         "email":"ClienteD@gmail.com",
         "telefone":""
      },
		  {
         "nome":"ClienteD",
         "email":"ClienteD@gmail.com",
         "telefone":""
      }
   ]
}
```
## **Response**
```bash
{
  "message": "Disparo configurado com sucesso!"
}
```
**Possible Returns**
| Code  | Description |
| ------------- | ------------- |
| 200 | Trigger configured successfully! |
| 400 | Action not found. |
| 400 | API key is missing. |
| 400 | Please inform at least one customer. |
| 401 | Company not found for this API key. |
| 401 | Invalid API key. |
| 401 | This action is blocked for integrations. |
| 401 | This action is paused. |
| 402 | Insufficient email balance. |
| 402 | Insufficient SMS balance. |
| 500 | Internal Server Error |




# GET Collect Responses
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Fanswers--info%2F-orange)

You can also integrate responses collected via IndeCX into your system. To do this, you will need to use the "Collect responses" method to access all responses and customer details.

The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/answers-info/[Action_Identifier]?[params]

```
## Query Params
| Params  | Description |
| ------------- | ------------- |
| page  | Returns results from a specific page |
| limit  | Returns a limit value per page |
| startDate  | Initial date parameter |
| endDate  | Final date parameter |
| dateType  | Date type: createdAt (survey creation) or updatedAt (survey update) |
| email  | Respondent's email parameter |
| phone  | Respondent's phone parameter |


Note: For the [Action_Identifier] value, you can use the "/all" parameter to return all responses for all available actions.

Query example:

```javascript
GET /v3/integrations/answers-info/[Identificador_da_acao]?limit=1000&startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt HTTP/1.1
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## **Response**
```javascript
 {
   "page": 1,
   "limit": 50,
   "total": 825,
   "answers": [
      "_id": "60b680790be5ec68f9aaa8ac",
      "active": true,
      "date": "2021-06-01T18:46:17.374Z",
      "answerDate": "2021-06-01T18:46:17.374Z",
      "anonymousResponse": false,
      "clientId": null,
      "categories": [
         {
            "category": "Atendimento Insatisfatorio",
            "subCategory": "Gerente"
         },
         {
            "category": "Conteúdo programático"
         }
      ],
      "tags": [],
      "subTags": [],
      "deleted": false,
      "name": "Nome do Cliente A",
      "email": "clientea@gmail.com",
      "phone": 1199999222,
      "feedback": "O atendimento poderia ser melhor",
      "additionalQuestions": [
        {
          "multipleValues": [],
          "_id": "60b690786b90df6a1c0c25c7",
          "type": "REVIEWS",
          "text": "Atendimento",
          "review": 5
        },
	{
          "multipleValues": [],
          "_id": "60b690786b90df6a1c0c25c8",
          "type": "CSAT",
          "text": "Prazo",
          "review": 8
        },
	{
					
	"type": "MULTIPLE",
	"_id": "6494506193567c001327e4c4",
	"text": "Qual motivo da sua avaliação",
	"options": [
		{
		"option": "devolução e cancelamento da venda",
		"subOptions": ["comissão"]
		} 
		]
	}
      ],
      "channel": "email",
      "companyId": "5edfb0342e03a449708e67df",
      "actionId": "6074572fa732a85e3683bf9f",
      "inviteId": "60b66441e4c5236925287995",
      "detailsId": "60b66440e4c523692528689a",
      "text": "Em uma escala de 0 a 10, o quanto você indicaria a Indecx para amigos ou familiares?",
      "review": 7,
      "indicators": [
        {
          "_id": "60b66440e4c523692528689b",
          "column": "sent_date",
          "value": "2021-06-01",
          "indicatorId": "60999b6846cfe15d3e276b67",
          "key": "2021-06-01"
        },
	{
          "_id": "60b66440e4c523692528689c",
          "column": "seller_id",
          "value": "18/05/3038",
          "indicatorId": "6099701094450d5d31861a87",
          "key": "415785"
        },
        {
          "_id": "60b66440e4c523692528689d",
          "column": "seller_type",
          "value": "biz",
          "indicatorId": "609970140ad6c05d4c8ae98e",
          "key": "biz"
        },
	],
      "controlId": "S8C0VE",
      "metric": "nps-0-10",
      "createdAt": "2021-06-01T18:46:17.380Z",
      "updatedAt": "2021-06-01T18:46:17.780Z",
      "sentimentAnalyzeGCP": {
        "score": 0,
        "keyPhrases": [
          "Atendimento",
          "Melhor"
        ]
	},
      "treatments": {
        "status": "aberto",
        "sponsor": "Ninguém",
        "comments": [],
        "date": "2021-06-01T18:46:17.374Z",
        "categories": []
      }
    }
```

## Understanding Return Fields
| Params  | Description |
| ------------- | ------------- |
| _id  | Response ID |
| active  | Status |
| date  | Date and time of the response |
| answerDate  | Date and time of the response |
| anonymousResponse  | Indicates if the client chose to remain anonymous |
| clientId  | Unique identifier for the client |
| name  | Client's name |
| email  | Client's email |
| phone  | Client's phone number |
| feedback  | Comment provided by the client at the time of response |
| categories  | Feedback categorization |
| channel  | Response collection channel |
| companyId  | Company ID |
| actionId  | Action ID |
| inviteId  | Invite ID |
| detailsId  | Import details ID |
| text  | Primary metric question |
| review  | Score assigned to the primary metric |
| metric  | Primary metric |
| createdAt  | Date the response was created |
| updatedAt  | Date the response was updated |
| deleted | Indicates if the response was deleted |


**additionalQuestions**
| Params  | Description |
| ------------- | ------------- |
| _id  | Question ID |
| type  | Question type: REVIEWS, LIKE/DISLIKE, LIKERT, CSAT, EMOTION, MULTIPLE, INPUT |
| text  | Question description |
| review  | Assigned score |
| answerText  | Text response for additional question type **INPUT** |
| multipleValues  | Response for multiple answers in additional question type **MULTIPLE** |
| options  | Response for multiple answers with sub-options **MULTIPLE with SUB** |


**indicators**
| Params  | Description |
| ------------- | ------------- |
| _id  | Indicator ID |
| column  | Indicator name |
| value  | Indicator value |

**sentimentAnalyzeGCP**
| Params  | Description |
| ------------- | ------------- |
| score  | Sentiment score |
| keyPhrases  | Key phrases identified in the client comments |

**treatments**
| Params  | Description |
| ------------- | ------------- |
| status  | Treatment status |
| sponsor  | Responsible person's name |
| comments  | Comment recorded by the responsible person |
| date  | Treatment date |
| categories  | Categories included in the responses |


# GET Collect Invite Information
![Badge](https://img.shields.io/badge/GET-invites--info-orange)

You can access the list of all clients imported into the platform, with a status indicating whether the invite has been responded to or not.

The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/invites-info/[Action_Identifier]?[params]

```
## Query Params
| Params  | Description |
| ------------- | ------------- |
| page  | Returns results from a specific page |
| limit  | Returns a limit value per page |
| startDate  | Initial date parameter |
| endDate  | Final date parameter |
| dateType  | Date type: createdAt (invite creation) or updatedAt (invite update) |
| email  | Invite email parameter |
| phone  | Invite phone parameter |


Note: For the [Action_Identifier] value, you can use the "/all" parameter to return all invites for all available actions.

Query example::
```javascript
GET /v3/integrations/invites-info/[Identificador_da_acao]?limit=1000&startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt HTTP/1.1
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## **Response**
```javascript
 {
	"page": 1,
	"limit": 500,
	"total": 409,
	"invites": [
		{
			"_id": "61e078eba3320b33e7332888",
			"answered": true,
			"active": false,
			"actionId": "61534bf90f9599741e703000",
			"name": "Paulo",
			"email": "contato_1@indecx.com.br",
			"phone": "5511955555555",
			"shortUrl": "https://id-cx.co/XiYyYxxx",
			"validEmail": true,
			"validPhone": true,
			"createdAt": "2022-01-13T19:09:31.635Z",
			"updatedAt": "2022-01-13T19:09:31.635Z",
			"answeredId": "61df50e72d5402779195a001",
			"deletedAnswer": false,
		},
		{
			"_id": "61e078eba3320b1122232799",
			"answered": false,
			"active": true,
			"actionId": "61534bf90f9599741e703000",
			"name": "Marcos",
			"email": "contato_2@indecx.com.br",
			"phone": "5599999999999",
			"shortUrl": "https://id-cx.co/XmVAsdTg",
			"validEmail": true,
			"validPhone": true,
			"createdAt": "2022-01-13T19:09:31.634Z",
			"updatedAt": "2022-01-13T19:09:31.634Z",
			"deletedAnswer": false,
		},
		{
			"_id": "61e078eba3321111e7332779",
			"answered": false,
			"active": true,
			"actionId": "61534bf90f9599741e703000",
			"name": "Fernando",
			"email": "contato_3@indecx.com.br",
			"phone": "55119800000000",
			"shortUrl": "https://id-cx.co/XfuIIuaD",
			"validEmail": true,
			"validPhone": true,
			"createdAt": "2022-01-13T19:09:31.633Z",
			"updatedAt": "2022-01-13T19:09:31.633Z",
			"deletedAnswer": false,
		}
	]
}
```

## Understanding Return Fields
| Params  | Description |
| ------------- | ------------- |
| _id  | Invite ID |
| answered  | Indicates if the invite was answered |
| active  | Indicates if the invite is active |
| actionId  | Action ID |
| name  | Client's name |
| email  | Client's email |
| phone  | Client's phone number |
| shortUrl  | Survey link address |
| validEmail  | Email validator |
| validPhone  | Phone validator |
| createdAt  | Invite creation date |
| updatedAt  | Invite update date |
| answeredId  | Response ID |
| deletedAnswer | Indicates if the response was deleted |




# GET Collect Non-Responding Clients
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Fno--response-orange)

Using this method, you can access a list of all clients who have not yet responded to your survey.
The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/[Action_Identifier]?[params]
```

## Query Params
| Params  | Description |
| ------------- | ------------- |
| page  | Returns results from a specific page |
| limit  | Returns a limit value per page |
| startDate  | Initial date parameter |
| endDate  | Final date parameter |
| email  | Search by a specific user's email |

Query example:


```javascript
GET /v3/integrations/no-response/[Action_Identifier]?page=1&limit=10
GET /v3/integrations/no-response/[Action_Identifier]?limit=1000&startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt
GET /v3/integrations/no-response/[Action_Identifier]?email=[user's email]
GET /v3/integrations/no-response/[Action_Identifier]?clientId=[clientId]
GET /v3/integrations/no-response/[Action_Identifier]?indicator=[Indicator Name]&indicatorValue=[indicator value]

Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

**Collect responses with pagination limit and a maximum payload return of 1,000 records.**

## **Response**
```javascript
{
  "invites": [
    {
      "_id": "60b0e0430ca11b72bbba6616",
      "answered": false,
      "resendDates": [
        "2021-05-28T12:22:32.488Z"
      ],
      "active": true,
      "reminderDates": [],
      "controlId": "1M311J",
      "actionId": {
        "name": "Teste ClientID",
        "controlId": "PJ48WM"
      },
      "name": "Caio fernando do Nascimento",
      "email": "caio.nasc@ymail.com",
      "phone": "5519993133850",
      "shortUrl": "https://id-cx.co/8JoD5sLL/s",
      "validEmail": true,
      "validPhone": true,
      "indicators": [],
      "metric": "nps-0-10",
      "createdAt": "2021-05-28T12:21:23.668Z"
    }
  ],
  "page": 1,
  "limit": 20,
  "total": 5
}
```

## Understanding Return Fields
| Params  | Description |
| ------------- | ------------- |
| _id  | Response ID |
| answered  | Response status |
| resendDates  | List of all dates when the invite was resent |
| active  | Invite status |
| reminderDates  | List of all reminder dates |
| controlId  | Control identifier for the invite |
| actionId.name  | Name of the action assigned to the invite |
| actionId.controlId  | Control identifier for the action |
| name  | Client's name |
| email  | Client's email |
| phone  | Client's phone number |
| shortUrl  | Client's survey link |
| validEmail  | Email field validator |
| validPhone  | Phone field validator |
| indicators  | List of indicators assigned to the invite |
| metric  | Main metric for the invite |
| createdAt  | Invite creation date |
| page  | Pagination of the return |
| limit  | Maximum return limit for the query |
| total  | Total records for the query |


# WEBHOOK Collect Responses
![Badge](https://img.shields.io/badge/webhook-answers-red)

You can also register your endpoint within the IndeCX platform so that each new response is automatically sent to your system. While logged into the IndeCX platform, go to: **Settings >> Integrations >> Collect responses**
<h1 align="center">
  <img alt="" title="" src="./assets/webhook-answer.png" />
</h1>
With each new response, you will receive a return with all the client information and responses provided, as shown in the example below:

## **Response**
```bash
{
    "_id": "60ae3842a81a5d534cbcfb21",
    "active": true,
    "date": "2022-10-06T17:43:47.367Z",
    "answerDate": "2022-10-06T17:43:47.368Z",
    "inviteDate": "2022-10-06T12:54:55.730Z",
    "anonymousResponse": true,
    "clientId": null,
    "categories":[],
    "subCategories":[],
    "tags":[],
    "subTags":[],
    "subCategoriesIA":[],
    "deleted": false,
    "name": "****",
    "email": "****",
    "phone": "****",
    "feedback": "Demora na entrega do veículo. ",
    "additionalQuestions": [
      {
        "multipleValues": [],
        "_id": "60ae3842a81a5d534cbcfb22",
        "type": "REVIEWS",
        "text": "Como você avalia sua experiência geral?",
        "review": 3
      },
      {
        "multipleValues": [],
        "_id": "60ae3842a81a5d534cbcfb23",
        "type": "REVIEWS",
        "text": "Qual a possibilidade de voltar a utilizar os serviços?",
	 "review": 3
      },
    ],
    "channel": "email",
    "companyId": "5e18ccd0dc36a60022491bdf",
    "actionId": "5e3b51300de04300f04eae90",
    "inviteId": "60ae358f05b1b32c15a389b1",
    "detailsId": "60ae358f05b1b32c15a386ae",
    "review": 5,
    "indicators": [
      {
        "_id": "60ae358f05b1b32c15a386af",
        "column": "subsegmento",
        "value": "****",
        "indicatorId": "5f25907d6be88015eea3a6e1",
        "key": "****"
      },
      {
        "_id": "60ae358f05b1b32c15a386b0",
        "column": "estadocliente",
        "value": "****",
        "indicatorId": "5f25907d6be88015eea3a6e2",
        "key": "****"
      },
    ],
    "controlId": "KUHC68",
    "metric": "nps-0-10",
    "createdAt": "2021-05-26T12:00:02.557Z",
    "updatedAt": "2021-05-26T12:00:02.557Z"
}
```

# POST Generate Survey Link
![Badge](https://img.shields.io/badge/POST-v3/integrations/%2Factions-green)

With the route below, you can integrate with the IndeCX platform, aiming to create an invite and return the link in the response of the call. This link can be used in customer contact media, and it is unique for each new invite generated.

The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/actions/[Action_Identifier]/invites

```
Actions must be created on the platform app-indecx.com, and the invites can be sent through the platform or via API. When triggered via API, a JSON must be sent in the body to the URL mentioned above, and your authentication will be handled through the company-key provided and sent via HEADER. It is important to send the action identifier and follow a specific JSON format in the body.

## **Request**

```javascript
POST /v3/integrations/actions/T0AXXX/invites
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

{
   "customers":[
      {
         "nome":"client A",
         "email":"clientec@indecx.com",
         "telefone":""
      },
      {
         "nome":"Cliente B",
         "email":"clienteb@indecx.com",
         "telefone":""
      }
		 
   ]
}
```

## **Response**
```javascript
{
  "customers": [
    {
      "nome": "client A",
      "email": "clientec@indecx.com",
      "telefone": "",
      "inviteId": "6143d38aaf080f75ffbdacf2",
      "shortUrl": "https://id-cx.co/ZT4wLBEL",
      "created_at": "2021-06-02T22:07:33.977Z",
      "actionId": "DHNS7U",
      "status": "integrado"
    },
    {
      "nome": "Cliente B",
      "email": "clienteb@indecx.com",
      "telefone": "",
      "inviteId": "6143d38aaf080f75ffbdacf3",
      "shortUrl": "https://id-cx.co/XgHQ3z7z",
      "created_at": "2021-06-02T22:07:34.172Z",
      "actionId": "DHNS7U",
      "status": "integrado"
    }
```  

## Generating Survey Link with Callback URL
It is also possible to send an invite and obtain the link through the API registered in the callback URL.

**callbackurl** API route

**maxmsg** Maximum number of records returned in the JSON. Example: If you send an invite to 1,000 clients with maxmsg = 100, you will receive 10 requests with 100 records each.

## **Request**

```javascript
POST /v3/integrations/actions/T0AXXX/invites
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

{
    "webhook": {
        "callbackurl": "https://webhook.site/58f1d980-89b4-4121-83b9-8cce55b128f4",
        "maxmsg": 100
    },
   "customers":[
      {
         "nome":"client A",
         "email":"clientec@indecx.com",
         "telefone":""
      },
      {
         "nome":"Cliente B",
         "email":"clienteb@indecx.com",
         "telefone":""
      }
		 
   ]
}
```

## **Response**
```javascript
{
  "message": "Request received. The result will be sent to https://webhook.site/58f1d980-89b4-4121-83b9-8cce55b128f4"
}
```

## **Response callbackurl**
```javascript
{
  "customers": [
    {
      "nome": "client A",
      "email": "clientec@indecx.com",
      "telefone": "",
      "inviteId": "6143d38aaf080f75ffbdacf2",
      "shortUrl": "https://id-cx.co/sH2zmIhY",
      "created_at": "2021-06-02T21:57:09.239Z",
      "actionId": "DHNS7U",
      "status": "integrado"
    },
    {
      "nome": "Cliente B",
      "email": "clienteb@indecx.com",
      "telefone": "",
      "inviteId": "6143d38aaf080f75ffbdacf3",
      "shortUrl": "https://id-cx.co/PV0DCQtS",
      "created_at": "2021-06-02T21:57:09.421Z",
      "actionId": "DHNS7U",
      "status": "integrado"
    }
  ]
}
```
## **Sending Reminders **
If you want INDECX to send automatic reminders to clients who have received the link generated by the API, follow these steps:

1 - Configure the number of reminders and the delivery channels directly in the action.

2 - Add the parameter include-reminder:true in the API header. This way, whenever a new link is generated via API, an automatic reminder will also be created based on the settings defined in the action.


# POST Sending Transactional Responses to IndeCX
![Badge](https://img.shields.io/badge/POST-v3/integrations/%2Factions-green)

With the route below, you can send a response collected in your APP or system transactionally into the IndeCX system. This way, it is possible to centralize all analyses and responses within IndeCX and analyze the results with the reports available on the platform.

The communication will be done through the following URL:

```bash
https://indecx.com/v3/integrations/create-answer/[Action_Identifier]

```
For IndeCX to receive this data, an action with questionnaire settings compatible with what the client responded to must be created.

## **Request**
```javascript
POST /v3/integrations/create-answer/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

  {
   "inviteId":"6143d38aaf080f75ffbdaCCC",
   "name":"José Paulo",
   "email":"jose@gmail.com.br",
   "phone":"1999999999",
   "review":9,
   "feedback":"Teste",
   "additionalQuestions":[
      {
         "type":"REVIEWS",
         "text":"Pergunta A",
         "review":4
      },
      {
         "type":"LIKE/DISLIKE",
         "text":"Pergunta B",
         "review":1
      },
      {
         "type":"CSAT",
         "text":"Pergunta C",
         "review":5
      },
      {
         "type":"LIKERT",
         "text":"Pergunta D",
         "review":5
      },
      {
         "type":"EMOTION",
         "text":"Pergunta E",
         "review":1
      },
      {
         "type":"MULTIPLE",
         "text":"Qual o motivo?",
         "review":[
            "Opção 1",
            "Opção 2"
         ]
      },
      {
         "type":"MULTIPLE-SUBS",
         "text":"Qual o motivo da sua nota?",
         "review":[
            {
               "text":"Atendimento",
               "subOptions":[
                  {
                     "text":"Tempo de resposta"
                  },
                  {
                     "text":"Qualidade do atendimento"
                  }
               ]
            },
            {
               "text":"Produto",
               "subOptions":[
                  {
                     "text":"Qualidade do produto"
                  },
                  {
                     "text":"Preço"
                  }
               ]
            }
         ]
      },
      {
         "type":"INPUT",
         "text":"Qual foi seu vendedor",
         "review":"Francisco da silva"
      }
   ],
   "indicators":[
      {
         "column":"UF",
         "value":"SP"
      },
      {
         "column":"Regiao",
         "value":"Campinas"
      }
   ]
}
```

## **Response**

```javascript
{
  "message": "Successfully integrated answer.",
   "id": "asy8765gasasgydasd"
}
```



## Understanding Request Fields

| Params  | Description |
| ------------- | ------------- |
| inviteId  | Invite ID |
| name  | Respondent's name |
| email  | Respondent's email |
| phone  | Respondent's phone number |
| review  | Score assigned to the survey's main metric |
| feedback  | Client comment field |
| additionalQuestions.type  | Type of additional question, e.g., REVIEWS, CSAT, LIKE/DISLIKE, LIKERT, EMOTION, MULTIPLE, INPUT |
| additionalQuestions.text  | Description of the additional question |
| indicators.column  | Indicator name |
| indicators.value  | Indicator value |

*Important: The inviteId field is only used when combining the Link Generation API with the Transactional Response API; it is not mandatory for simple response inclusion. For more information on how to use inviteId, contact the IndeCX support team.

## Understanding Response Options by Type

| Params  | Description |
| ------------- | ------------- |
| REVIEWS  | 5-point scale (1 to 5) in "star" format |
| CSAT  | 10-point scale (1 to 10) in rating scale format |
| LIKE/DISLIKE  | Boolean scale (0 and 1) with positive and negative icons |
| LIKERT  | 5-point scale (1 to 5) where 1 = "Very dissatisfied," 2 = "Dissatisfied," 3 = "Neutral," 4 = "Satisfied," and 5 = "Very Satisfied" |
| EMOTION  | Boolean scale (0 and 1) in positive and negative emoji format |
| MULTIPLE  | Single or multiple-choice selection scale |
| MULTIPLE-SUBS  | Single or multiple-choice selection scale with sub-options |
| INPUT  | Open field for response inclusion |

*Important: Received values must match the available response options for each metric to enable integration.

## List of Possible Errors
| Code  | Description |
| ------------- | ------------- |
| 400  | "message": "Invalid metric type. Allowed: REVIEWS, CSAT, LIKE/DISLIKE, LIKERT, EMOTION, MULTIPLE, INPUT, CES" |
| 400  | "message": "Invalid answer for the question XXXX..." |
| 400  | "message": "The Invite has already been answered" |
| 500  | "message": "Internal server error" |


# GET List Active Actions on IndeCX
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Factions--info-orange)

To access the list of all active actions available in the account, use the route below.

```javascript
GET /v3/integrations/actions-info
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

```

## **Response**

```javascript
[
  {
    "_id": "61a6704c35927d3d11fe18f8"
    "surveyType": "survey",
    "type": "nps-0-10",
    "name": "Ação de pesquisa da área de pós-venda",
    "channel": "emailsms",
    "controlId": "5Q0WB7",
    "createdAt": "2020-01-15T20:11:59.540Z"
  },
  {
    "_id": "61a6704c35927d3d11fe18f9"
    "surveyType": "survey",
    "type": "nps-0-10",
    "name": "Ação de pesquisa da área de Vendas",
    "channel": "emailsms",
    "controlId": "5YSZVS",
    "createdAt": "2020-01-20T17:16:14.951Z"
  }
]
```


# GET Collect Questionnaire Information
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Factions--info-orange)

To access the structure of the questionnaire programmed within IndeCX. The communication will be done through the following URL:



```javascript
GET /v3/integrations/actions-info/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## **Response**

```javascript
{
  "_id": "60ae764cec27e759d6268563",
  "controlId": "PJ48WM",
  "surveyType": "survey",
  "name": "Teste ClientID",
  "type": "nps-0-10",
  "question": "Em uma escala de 0 a 10, quanto você indicaria a IndeCX para um amigo ou familiar?",
  "enableAnonymous": null,
  "adittionalQuestions": [
    {
      "type": "REVIEWS",
      "text": "Atendimento"
    },
    {
      "type": "CSAT",
      "text": "Prazo"
    },
    {
      "type": "LIKE/DISLIKE",
      "text": "Satisfação com o produto"
    },
    {
      "type": "LIKERT",
      "text": "Qual sua avaliação sobre o tema A?"
    },
    {
      "type": "EMOTION",
      "text": "Foi entregue no prazo?"
    },
    {
      "type": "MULTIPLE",
      "text": "o que você acha?",
      "multipleType": "radio",
      "choices": [
        {
          "text": "Opção 1"
        },
        {
          "text": "Opção 2"
        },
        {
          "text": "Opção 3"
        },
        {
          "text": "Opção 4"
        }
      ]
    },
    {
      "type": "INPUT",
      "text": "pergunta Input"
    }
  ],
  "allAnswersRequired": false
}
```
## Understanding Return Fields

| Params  | Description |
| ------------- | ------------- |
| _id  | Action ID |
| controlId  | Action control identifier |
| resendDates  | List of all resend dates |
| surveyType  | Type of questionnaire |
| name  | Questionnaire name |
| type  | Main metric of the questionnaire |
| question  | Main metric question |
| enableAnonymous  | Option to enable data anonymization |
| additionalQuestions.type  | Type of additional question, e.g., REVIEWS, CSAT, LIKE/DISLIKE, LIKERT, EMOTION, MULTIPLE, INPUT |
| additionalQuestions.text  | Description of the additional question |
| additionalQuestions.multipleType  | Type of selection: single (radio) or multiple choice (checkbox) |
| additionalQuestions.choices  | Choice options for multiple type questions |
| allAnswersRequired  | Specifies if answers are mandatory |


## Understanding Response Options by Type

| Params  | Description |
| ------------- | ------------- |
| REVIEWS  | 5-point scale (1 to 5) in "star" format |
| CSAT  | 10-point scale (1 to 10) in rating scale format |
| LIKE/DISLIKE  | Boolean scale (0 and 1) with positive and negative icons |
| LIKERT  | 5-point scale (1 to 5) where 1 = "Very dissatisfied," 2 = "Dissatisfied," 3 = "Neutral," 4 = "Satisfied," and 5 = "Very Satisfied" |
| EMOTION  | Boolean scale (0 and 1) in positive and negative emoji format |
| MULTIPLE  | Single or multiple-choice selection scale |
| INPUT  | Open field for response inclusion |

# POST Send Client to Blocklist
![Badge](https://img.shields.io/badge/POST-send--blocklist-green)

If you want to add a list of clients to the blocklist to prevent this email from receiving new contacts, you can do so through this route.

The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/send-blocklist

```
## **Request**
```javascript
POST /v3/integrations/send-blacklist
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

{
   "email":"contato_3@indecx.com.br",
   "reason":"Não quer receber contato"
	
}
```
## **Response**
```javascript
{
	"message": "Sent to the blocklist successfully."
}
```


# GET Collect Blocklist Information (Do Not Contact)
![Badge](https://img.shields.io/badge/GET-blocklist--info-orange)

You can also access the list of all clients who have been added to the blocklist:

```javascript
GET /v3/integrations/blacklist-info
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

```

## Query Params
| Params  | Description |
| ------------- | ------------- |
| page  | Returns results from a specific page |
| limit  | Returns a limit value per page |


## **Response**

```bash
{
	"page": 1,
	"limit": 1000,
	"data": [
		{
			"actionName": "Ação A",
			"actionControlId": "74HSBB",
			"inviteId": "60ef454559d33741586e0000",
			"reason": Não desejo mais receber esses emails,
			"email": "contato_1@indecx.com",
			"createdAt": "2021-07-25T00:23:58.735Z"
		},
		{
			"actionName": "Ação B",
			"actionControlId": "74HSBB",
			"inviteId": "60ef454559d33741586e0001",
			"reason": "Outros",
			"email": "contato_2@indecx.com",
			"createdAt": "2021-07-22T17:11:32.877Z"
		},
		{
			"actionName": "Ação C",
			"actionControlId": "74HSBB",
			"inviteId": "60ef454559d33741586e0002",
			"reason": "Não desejo mais receber esses emails",
			"email": "contato_3@indecx.com",
			"createdAt": "2021-08-05T10:19:23.419Z"
		}
	]
}
```
## Understanding Return Fields

| Params  | Description |
| ------------- | ------------- |
| actionName  | Action name |
| actionControlId  | Action ID |
| inviteId  | Invite ID |
| reason  | Reason for inclusion in the blocklist |
| email  | Client's email |
| createdAt  | Date of record inclusion in the blocklist |




# GET Collect Categorized Response List
![Badge](https://img.shields.io/badge/GET-blocklist--info-orange)

You can also access the list of all assigned categorizations.

```javascript
GET /v3/integrations/category-info/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

```

## Query Params
| Params  | Description |
| ------------- | ------------- |
| page  | Returns results from a specific page |
| limit  | Returns a limit value per page |
| startDate  | Initial date parameter |
| endDate  | Final date parameter |
| dateType  | createdAt or updatedAt |

Note: For the [Action_Identifier] value, you can use the "/all" parameter to return all invites for all available actions.

Query example:


```javascript
GET /v3/integrations/category-info/all?startDate=01-01-2022&endDate=28-07-2022&dateType=updatedAt
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## **Response**

```javascript

	"page": 1,
	"limit": 50,
	"total": 5648,
	"answers": [
		{
			"answerId": "6258af1bec2fae3ee56d461a",
			"inviteId": "62570d80d788a74c0e05aab7",
			"categories": [
				{
					"category": "Elogio",
					"subCategory": "Atendimento"
				}
			],
			"createdAt": "2022-04-14T23:32:43.288Z",
			"updatedAt": "2022-07-26T21:19:52.303Z"
		},
		{
			"answerId": "625810a83b3c5d3ea7234302",
			"inviteId": "62570d81d788a74c0e05cd97",
			"categories": [
				{
					"category": "App/Painel",
					"subCategory": "Quer Cartão"
				},
				{
					"category": "Conta",
					"subCategory": "Desativar / Cancelar"
				}
			],
			"createdAt": "2022-04-14T12:16:40.714Z",
			"updatedAt": "2022-07-26T21:19:52.303Z"
		}
	]
```
## Understanding Return Fields

| Params  | Description |
| ------------- | ------------- |
| answerId  | Response ID |
| inviteId  | Invite ID |
| category  | Assigned category name |
| subCategory  | Assigned subcategory name |
| createdAt  | Response inclusion date |
| updatedAt  | Response update date |
| dateType  | Response inclusion date |





# POST Register Branch (IH1)
![Badge](https://img.shields.io/badge/post-branch-green)

You can also register a branch or business unit via API. This branch, referred to as IH1 on the platform, will be used to enable data separation and user access levels.

```bash
{
  https://indecx.com/v3/integrations/branches
}

```


## **request json**


```javascript
POST /v3/integrations/branches
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

{
   "corporate_name":"UNIDADE X - CENTRO",
   "type":"ih1",
   "secondary_fields":[
      {
         "key":"Codigo",
         "value":"1"
      }
   ]
}

```

## **Response**

```bash

{
	"_id": "630918be7140e266cafafab7",
	"active": true,
	"corporate_name": "UNIDADE X - CENTRO",
	"type": "ih1"
}
```
## Understanding Return Fields

| Params  | Description |
| ------------- | ------------- |
| _id  | Branch ID |
| active  | Status |
| corporate_name  | Branch name |
| type  | Branch type |


# GET List Registered Branches (IH1)
![Badge](https://img.shields.io/badge/get-branch-orange)

This API allows access to the list of all registered units on the platform.

```javascript
GET /v3/integrations/branches
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

```

## **Response**

```javascript

[
	{
		"_id": "62bdd0cb37a01172d1dcf65a",
		"corporate_name": "101",
		"type": "ih1",
		"Grupo": "AA"
	},
	{
		"_id": "62bdd0d91213ea72bc85fdfe",
		"corporate_name": "100",
		"type": "ih2",
		"Grupo": "YY"
	},
	{
		"_id": "62bdd12755052372d78d0251",
		"corporate_name": "1002",
		"type": "ih1",
		"Grupo": "XX"
	}
	]
```

# GET Collect Action Details
![Badge](https://img.shields.io/badge/GET-details--info-orange)

This endpoint allows you to obtain details of a specific action.


```javascript
GET /v3/integrations/details-info/:controlId
Host: indecx.com
Content-Type: application
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```
## Query Params
| Params  | Description |
| ------------- | ------------- |
| page  | Returns results from a specific page |
| limit  | Returns a limit value per page |
| startDate | Initial date parameter | 
| endDate | Final date parameter | 
| status | Action status | 
| dateType  | Date type: createdAt (survey creation) or updatedAt (survey update) |

Query example:


```javascript
GET /v3/integrations/details-info/[Action_Identifier]?page=1&limit=10
GET /v3/integrations/details-info/[Action_Identifier]?startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt HTTP/1.1
GET /v3/integrations/details-info/[Action_Identifier]?status=[action status]

Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

```


## **Response**

```javascript

{
    "page": 1,
    "limit": 30,
    "total": 5648,
    "details": [
        {
            "_id": "6258af1bec2fae3ee56d461a",
            "active": true,
            "companyId": "62570d80d788a74c0e05aab7",
            "actionId": "62570d80d788a74c0e05aab7",
            "channel": "email",
            "status": "sent",
            "controlId": "T0AXXX",
            "submitDate": "2022-04-14T23:32:43.288Z",
            "createdAt": "2022-04-14T23:32:43.288Z",
            "updatedAt": "2022-07-26T21:19:52.303Z",
            "submitId": "625810a83b3c5d3ea7234302",
            "Email": "user@example.com",
            "Name": "User Name",
            "Phone": "+5511999999999",
            "validEmail": true,
            "validPhone": true,
            "indicators": []
        }
    ]
}

```

## Understanding Return Fields
| Params  | Description |
| ------------- | ------------- |
| _id  | Action detail ID |
| active  | Indicates if the action is active or not |
| companyId  | Company ID |
| actionId  | Action ID |
| channel  | Action channel (e.g., email) |
| status  | Action status (e.g., sent) |
| controlId  | Action control ID |
| submitDate  | Action submission date |
| createdAt  | Action creation date |
| updatedAt  | Action update date |
| submitId  | Action submission date |
| Email  | Action recipient's email |
| Name  | Action recipient's name |
| Phone  | Action recipient's phone number |
| validEmail  | Indicates if the email is valid |
| validPhone  | Indicates if the phone number is valid |
| indicators  | List of indicators assigned to the invite |


# PUT Invite Update
![Badge](https://img.shields.io/badge/PUT-update--invites-green)

This endpoint allows you to update the details of multiple invites at once. It accepts an array of invites, where each invite is an object containing the fields `inviteId`, `name`, `email`, and `phone`. The `inviteId` is mandatory, while `name`, `email`, and `phone` are optional. Remember that all fields cannot be left empty, and it is not possible to update invites that have already been answered.

## Request

The communication will be done through the following URL:
```bash
https://indecx.com/v3/integrations/update-invite

```

**Request Body:**

```javascript
POST /v3/integrations/update-invite
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...


{
  "invites": [
    {
      "inviteId": "60d2f3b2f789b2128c761abc",
      "name": "Novo Nome",
      "email": "novoemail@example.com",
      "phone": "5511999999999"
    },
    {
      "inviteId": "60d2f3b2f789b2128c761def",
      "name": "Outro Nome",
      "email": "outroemail@example.com",
      "phone": "5511888888888"
    },
     {
      "inviteId": "60d2f3b2f789b2128c76dse4",
      "name": "Outro Nome",
      "email": "outroemail",
      "phone": "5511888888888"
    }
  ]
}

```
**Response:**

The response will be an object containing a message array, where each item corresponds to an invite sent in the request. Each item will be an object containing inviteId and message, indicating the result of the update for that invite.

**Response Example:**

```
{
  "message": [
    {
      "inviteId": "60d2f3b2f789b2128c761abc",
      "message": "Successfully updated invite."
    },
    {
      "inviteId": "60d2f3b2f789b2128c761def",
      "message": "Failure: Invalid email."
    },
    {
      "inviteId": "60d2f3b2f789b2128c76dse4",
      "message": "Failure: Invite already answered and cannot be updated."
    }
  ]
}
```

## **Possible Response Codes**
| Code | Description |
| ------ | --------- |
| 200    | "message": "Successfully updated invite." |
| 400    | "message": "Failure: All fields are empty." |
| 400    | "message": "Failure: Invalid inviteId." |
| 400    | "message": "Failure: Invalid phone number." |
| 400    | "message": "Failure: Invalid email." |
| 400    | "message": "Failure: Invalid name." |
| 400    | "message": "Failure: Invite already answered and cannot be updated." |
| 400    | "message": "Invite not found" |
| 500    | "message": "Failure: [error message]" |


# GET Collect Quarantine Information
![Badge](https://img.shields.io/badge/GET-quarantines--info-green)

This endpoint allows you to obtain detailed information about quarantines.


```javascript
GET /v3/integrations/quarantines-info/:controlId
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```
## Query Params
| Params  | Description |
| ------------- | ------------- |
| page  | Returns results from a specific page |
| limit  | Returns a limit value per page |
| startDate | Initial date parameter | 
| endDate | Final date parameter | 
| status | Action status | 
| dateType  | Date type: createdAt (survey creation) or updatedAt (survey update) |
| groupId | Action group ID |
| clientId | Client ID |
| phone | Recipient's phone number |
| email | Recipient's email |
| actionId | Action ID |
| companyId | Company ID |


```javascript
GET /v3/integrations/quarantines-info?phone=[recipient's phone number]
GET /v3/integrations/quarantines-info?email=[recipient's email]
GET /v3/integrations/quarantines-info?clientId=[client ID]
GET /v3/integrations/quarantines-info?groupId=[action group ID] or all (retrieves all actions)
GET /v3/integrations/quarantines-info?actionId=[action ID] or all (retrieves all action groups)
GET /v3/integrations/quarantines-info?page=1&limit=10
GET /v3/integrations/quarantines-info?startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt
GET /v3/integrations/quarantines-info (retrieves all quarantines)



Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

```

## **Response**

```javascript

{
    "page": 1,
    "limit": 50,
    "total": 1419,
    "quarantines": [
        {
            "_id": "4234defa8901hijk2345lmno",
            "expirationDate": "2024-07-01T11:30:00.000Z",
            "type": "alert",
            "clientId": "98012345000123",
            "companyId": "5678abcd9012efgh3456ijkl",
            "createdAt": "2024-06-01T11:30:00.000Z",
            "updatedAt": "2024-06-01T11:30:00.000Z"
        },
        {
            "_id": "5234efab9012ijkl3456mnop",
            "expirationDate": "2024-07-01T12:00:00.000Z",
            "type": "notification",
            "clientId": "12345678000145",
            "actionId": "abcd1234efgh5678ijkl9012",
            "createdAt": "2024-06-01T12:00:00.000Z",
            "updatedAt": "2024-06-01T12:00:00.000Z"
        },
        {
            "_id": "6234fabc0123jklm4567nopq",
            "expirationDate": "2024-07-01T12:30:00.000Z",
            "type": "reminder",
            "email": "example2@example.com",
            "groupId": "7654dcba3210kjih8765mnop",
            "createdAt": "2024-06-01T12:30:00.000Z",
            "updatedAt": "2024-06-01T12:30:00.000Z"
        }
    ]
}

```
## Understanding Return Fields
| Params  | Description |
| ------------- | ------------- |
| _id | Action detail ID |
| expirationDate | Date to exit quarantine |
| Email | Recipient's email |
| Phone | Recipient's phone number |
| companyId | Company ID |
| actionId | Action ID |
| groupId | Action group ID |
| type | How the client entered quarantine |
| createdAt | Action creation date |
| updatedAt | Action update date |




### Thank you 💚

Missing any route that would make your daily tasks easier? Contact the CX team at Indecx, and we’ll develop it for you! = )
Updated on 01/01/2023

