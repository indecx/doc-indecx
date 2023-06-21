# GUÍA DE UTILIZACIÓN DE API

![Badge](https://img.shields.io/badge/integrations-v3-blue)

La API de IndeCX fue desarrollada para que sea posible realizar diversas integraciones entre su sistema y las funciones disponibles en nuestra plataforma. En esta Versión, la API cuenta con una mejora en el proceso de autenticación y autorización mediante el token Bearer, que es un enfoque seguro, simple, escalable, flexible y estandarizado para garantizar la seguridad en las API.

En esta documentación encontrará ejemplos del uso de cada método.

## Introducción
La API utiliza autenticación y autorización de token de portador para proteger las rutas de acceso restringido. El Bearer Token debe enviarse en el encabezado de Authorization de todas las solicitudes que requieren autenticación.

## Autenticación
Para autenticarse con la API, debe enviar una solicitud de autenticación con sus credenciales de usuario (**company-key**) enviada a través del header, disponible dentro de la configuración de la cuenta en la plataforma Indecx. La API devolverá un Bearer Token, que debe usar para todas las solicitudes futuras que requieran autenticación.

## Endpoint / get
![Badge](https://img.shields.io/badge/GET-authorization--token-orange) 
```bash
https://indecx.com/v3/integrations/authorization/token
```
| parámetros	| Descripción |
| ------------- | ------------- |
| company-key  | $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX  |

## Ejemplo de request 
```javascript
GET /v3/integrations/authorization/token HTTP/1.1
Host: indecx.com
Company-Key: $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX

```

## Ejemplo de response 
```javascript
HTTP/1.1 200 OK
Content-Type: application/json

{
	"authToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVlLU1Rra..."
}

```

## Ejemplo de encabezado de autorización
El Bearer Token debe enviarse en el encabezado de Authorization de todas las solicitudes que requieren autenticación. El token es válido por 30 minutos.

```javascript
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVlLU1Rra...
```

# Desencadenador de invitación POST transaccional 
![Badge](https://img.shields.io/badge/POST-.v3/integrations/%2Fsend%2F-green)

Imagine una situación en la que desea activar una encuesta de satisfacción para cada nueva transacción de cliente dentro de su sistema, ya sea prospección, ventas, interacción, etc. El método de activación de invitación transaccional es posible y muy fácil.

La comunicación se realizará a través del siguiente URL:

```bash
https://indecx.com/v3/integrations/[Identificador_da_acao]
```

Las acciones deben crearse en la plataforma app-indecx.com y los envió se pueden realizar a través de la plataforma (manual) o a través de la API. Cuando se hace la activación a través de la API, debe enviar un JSON a través del body a la URL mencionada anteriormente y su autenticación se realizará a través de la company-key proporcionada y enviada a través de HEADER. Es importante que se envíe el identificador de la acción y que siga un patrón de envío del JSON a través del body.

## Configuración JSON

**Importante:**
El correo electrónico y el teléfono son campos obligatorios, según la configuración del tipo de activador. Consulta la configuración realizada al crear la acción. Para poder enviar datos a las columnas adicionales (indicadores) es necesario crear los indicadores dentro de la acción previamente y usar exactamente los mismos nombres creados dentro de la plataforma.  En nuestro ejemplo a continuación, las columnas adicionales son: Sucursal, Área, Región y Tipo.

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

Para tomas programadas, la invitación tendrá un estado **programado** en el índice y, si es necesario, también es posible cancelar la toma directamente a través de la plataforma en el menú Tomas>>Agendamientos
<h1 align="center">
  <img alt="" title="" src="./assets/scheduling.png" />
</h1>

**Importante:** El campo “scheduling” utiliza el estándar UTC, por lo que es posible programar tomas con horarios locales en otros países. El parámetro de ejemplo **"2020-12-16T20:20:00.000"** se activará en los siguientes horarios: Brasil 17:20:00 (UTC - 3), EE. UU. Nueva York 16:20:00 (UTC - 5)

# POST envíos de invitación por lotes 
![Badge](https://img.shields.io/badge/POST-.v3/integrations/%2Fsend%2Fbulk-green)

Usted también puede realizar los envíos de forma automática para una cantidad de clientes una única vez

La comunicación será realizada a través del siguiente URL:

```bash
https://indecx.com/v3/integrations/send/bulk/[Identificador_da_acao]
```

Las acciones deben crearse en la plataforma app-indecx.com y los envíos se pueden realizar a través de la plataforma o a través de la API. Cuando se activa a través de API, debe enviar un JSON a través del cuerpo a la URL mencionada anteriormente y su autenticación se realizará a través de la company-key proporcionada y enviada a través de HEADER. Es importante que se envíe el identificador de la acción y que siga un patrón de envío del JSON a través del body.

## Configuración JSON
**Importante** 
El correo electrónico y el teléfono son campos obligatorios, según la configuración del tipo de activador. Consulta la configuración realizada al crear la acción. Para poder enviar datos a las columnas adicionales (indicadores) es necesario crear los indicadores dentro de la acción previamente y usar exactamente los mismos nombres creados dentro de la plataforma. **Esta API tiene un límite máximo de 1.000 clientes por solicitud.**

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

## Response 

```bash
{
  "message": "Disparo configurado com sucesso!"
}
```

**Posibles devoluciones** 

| Código | Descripción |
| ------------- | ------------- |
| 200	| ¡Envíos configurados con éxito! |
| 400	| Acción no encontrada. |
| 400	| Falta la clave API. |
| 400	| Informe al menos a un cliente. |
| 401	| No se encontró la compañía para esta clave API. |
| 401	| Clave API no válida. |
| 401	| Esta acción está bloqueada para integraciones. |
| 401	| Esta acción está en pausa. |
| 402	| Saldo de correo electrónico insuficiente. |
| 402	| Saldo de mensaje insuficiente. |
| 500	| Error Interno del Servidor |

# GET Recopilar respuestas 
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Fanswers--info%2F-orange)

También puede integrar las respuestas recopiladas a través de indecx en su sistema. Para ello, deberá utilizar el método "Recopilar respuestas" para acceder a todas las respuestas y detalles de los clientes.

La comunicación se realizará a través de la siguiente URL:

```bash
https://indecx.com/v3/integrations/answers-info/[Identificador_da_acao]?[params]
```

## Query Params

| parámetros	| Descripción |
| ------------- | ------------- |
| page	| Devuelve los resultados de una página específica | 
| limit	| Devuelve un valor límite por página |
| startDate	| Fecha de inicio del parámetro |
| endDate	| Fecha de finalización del parámetro |
| dateType	| Tipo de fecha createdAt (creación de búsqueda) o updatedAt (actualización de búsqueda) |
| email	| Parámetro de correo electrónico del encuestado |
| phone	| Parámetro del teléfono del encuestado |

Nota: Para el valor [Identificador_da_acao], el parámetro "/all" se puede usar para devolver todas las respuestas de todas las acciones disponibles.

Ejemplo de consulta: 
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

## Comprender los campos de devolución 

| parámetros |	Descripción |
| ------------- | ------------- |
| _id |	ID de respuesta |
| active |	Estado |
| date	| Fecha y hora de respuesta |
| answerDate	| Fecha y hora de respuesta |
| anonymousResponse	| Identificación si el cliente quiere identificarse o no. |
| clientId	| Identificador único del cliente |
| name	| Nombre del cliente |
| email	| Correo electrónico del cliente |
| phone	| Teléfono del cliente |
| feedback	| Comentario realizado por el cliente en el momento de la encuesta |
| categories	| Categorización de comentarios (feedback) |
| channel	| Canal de recogida de respuestas |
| companyId	| ID de la compañía |
| actionId	| Identificador de acción |
| inviteId	| Identificación de invitación |
| detailsId	| ID de detalles de importación |
| text	| Pregunta métrica clave |
| review	|Puntuación asignada en la métrica principal |
| metric	| Métrica principal |
| createdAt	| Fecha de creación de la respuesta |
| updateAt	| Fecha de actualización de la respuesta |
| deleted	| Comprueba si la respuesta fue eliminada |

**additionalQuestions**

| parámetros |	Descripción |
| ------------- | ------------- |
| _id	| Identificación de la pregunta |
| type	| Tipo de pregunta REVIEWS,LIKE/DISLIKE,LIKERT,CSAT,EMOTION,MULTIPLE,INPUT |
| text	| Pregunta Descripción |
| review	| Grado asignado |
| answerText	| Retorno de texto en caso de pregunta adicional de tipo **INPUT** |
| multipleValues	| Devolución de respuestas múltiples en caso de tipo de pregunta adicional **MÚLTIPLE** |

**indicators**
| parámetros |	Descripción |
| ------------- | ------------- |
| _id |	ID del indicador |
| column	| Nombre del indicador |
| value	| Valor del indicador |

**sentimientoAnalyzeGCP**
| parámetros	| Descripción | 
| ------------- | ------------- |
| score	| Puntaje de sentimiento |
| keyPhrases	| Palabras claves identificadas en los comentarios de los clientes |

**treatments**
| parámetros	| Descripción |
| ------------- | ------------- |
| status	| Estado de negociación |
| sponsor	| Nombre de la persona a cargo |
| comments	| Comentario registrado por el responsable |
| date	| fecha de negociación |
| categories	| Categorías incluidas en las respuestas |

# GET Recopilar informaciones de invitación

![Badge](https://img.shields.io/badge/GET-invites--info-orange)

Puede acceder a la lista de todos los clientes importados a la plataforma con estado si la invitación ya fue respondida o no.

La comunicación se realizará a través de la siguiente URL:

```bash
https://indecx.com/v3/integrations/invites-info/[Identificador_da_acao]?[params]
```

## Query Params
| parámetros	| Descripción |
| ------------- | ------------- |
| page	| Devuelve los resultados de una página específica |
| limit	| Devuelve un valor límite por página |
| startDate	| Fecha de inicio del parámetro |
| endDate	| Fecha de finalización del parámetro |
| dateType	| Tipo de fecha createdAt (creación de invitación) o updatedAt (actualización de invitación) |
| email	| Parámetro de correo electrónico de invitación |
| phone	| Parámetro de teléfono de invitación |

Nota: Para el valor [Identificador_da_acao] se puede utilizar el parámetro "/all" para devolver todas las invitaciones de todas las acciones disponibles.

Ejemplo de consulta:

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

## Comprender los campos de devolución
| parámetros	| Descripción |
| ------------- | ------------- |
| _id		| ID de invitación |
| answered	| Comprueba si la invitación fue respondida |
| active	| Comprueba si la invitación está activa |
| actionId	| ID de acción |
| name          | Nombre del cliente |
| email 	| Correo electrónico del cliente |
| phone 	| Teléfono del cliente |
| shortUrl	| Dirección del enlace de la encuesta |
| validEmail	| Validador de correo electrónico |
| validPhone	| Validador de teléfono |
| createdAt	| Fecha de creación de la invitación |
| updateAt	| Fecha de actualización de la invitación |
| answeredId	| ID de respuesta |
| deletedAnswer	| Comprueba si la respuesta fue eliminada |

# GET recopilación de clientes que no responden 
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Fno--response-orange)

A través de este método, puede acceder a una lista de todos los clientes que aún no han respondido a su encuesta. La comunicación se realizará a través de la siguiente URL:
```bash
https://indecx.com/v3/integrations/[Identificador_da_acao]?[params]
```

## Query Params
| parámetros	| Descripción |
| ------------- | ------------- |
| page	        | Devuelve los resultados de una página específica |
| limit 	| Devuelve un valor límite por página |
| startDate	| Fecha de inicio del parámetro |
| endDate	| Fecha de finalización del parámetro |
| email 	| Buscar correo electrónico de un usuario en específico |

Ejemplo de consulta:

```javascript
GET /v3/integrations/no-response/[Identificador_da_acao]?page=1&limit=10
GET /v3/integrations/no-response/[Identificador_da_acao]?limit=1000&startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt
GET /v3/integrations/no-response/[Identificador_da_acao]?email=[email do usuário]
GET /v3/integrations/no-response/[Identificador_da_acao]?clienteId=[clientId]
GET /v3/integrations/no-response/[Identificador_da_acao]?indicator=[Nome do Indicador]&indicatorValue=[valor do indicador]

Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

**recopilación de respuestas con un límite de paginación y un retorno de carga útil máximo de 1000 registros.**

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

## Comprender los campos de devolución 
| parámetros	| Descripción |
| ------------- | ------------- |
| _id	| ID de respuesta |
| answered	| estado de respuesta |
| resendDates	| Lista de todas las fechas para las que se reenvió la invitación |
| active	| estado de la invitación |
| reminderDates	| Lista de todas las fechas para las que hubo recordatorios |
| controlId	| Identificador de control de invitación |
| actionId.name	| Nombre de la acción asignada a la invitación |
| actionId.controlId | Identificador de control de acción |
| name	| Nombre del cliente |
| email	| correo electrónico del cliente |
| phone	| teléfono del cliente |
| shortUrl	| enlace de encuesta al cliente |
| validEmail |	Validador de campo de correo electrónico |
| validPhone |	validador de campo de teléfono |
| indicators	| Lista de indicadores asignados a la invitación |
| metric	| Métrica de invitación clave |
| createdAt	| ? |
| page	| paginación de retorno |
| limit	| Límite máximo de devolución de consultas |
| total	| Total, de registros consultados |

# WEBHOOK recopilar respuestas
![Badge](https://img.shields.io/badge/webhook-answers-red)

También puedes registrar tu endpoint dentro de la plataforma Indecx para que cada nueva respuesta sea enviada automáticamente a tu sistema. Ingresando a la plataforma Indecx, vaya a: **Configuración >> Integraciones >> Recopilación de respuestas**
<h1 align="center">
  <img alt="" title="" src="./assets/webhook-answer.png" />
</h1>

Con cada nueva respuesta recibirá una devolución con toda la información del cliente y las respuestas asignadas por él, como se muestra en el siguiente ejemplo:

## **Respuesta**

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

# POST generación de un enlace de encuesta 
![Badge](https://img.shields.io/badge/POST-v3/integrations/%2Factions-green)

Con la ruta a continuación, se podrá integrar con la plataforma Indecx, donde el objetivo es crear una invitación y devolver el enlace en la respuesta de llamada, para que pueda ser utilizado dentro de los medios de contacto con el cliente, este enlace será único para cada nueva invitación generada.

La comunicación se realizará a través de la siguiente URL:

```bash
https://indecx.com/v3/integrations/actions/[Identificador_da_acao]/invites
```

las acciones deben crearse en la plataforma app-indecx.com y los envíos se pueden realizar a través de la plataforma o a través de la API. Cuando se active a través de API, debe enviar un JSON a través del cuerpo a la URL mencionada anteriormente y su autenticación se realizará a través de la clave proporcionada a la empresa y enviada a través de HEADER. Es importante que se envíe el identificador de la acción y que siga un patrón de envío del JSON a través del cuerpo

## **Pedido**

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

## **RESPUESTA**

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

## Generación de un enlace de encuesta con URL de devolución de llamadas 

También es posible enviar un disparador y obtener el enlace a través de la API registrada en la callbackurl.
ruta API **callbackurl**
**maxmsg** Retorno máximo de registros en JSON. Ejemplo: si activa 1000 clientes con maxmsg = 100, recibirá 10 solicitudes con 100 registros cada una.

## **Pedido**

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

## **RESPUESTA**

```javascript
{
  "message": "Request received. The result will be sent to https://webhook.site/58f1d980-89b4-4121-83b9-8cce55b128f4"
}
```

## URL de devolución de llamadas y respuestas

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

# POST Enviar transacciones de respuestas a Indecx 

![Badge](https://img.shields.io/badge/POST-v3/integrations/%2Factions-green)

Con la ruta a continuación, será posible enviar una respuesta recopilada en su APP o sistema de manera transaccional al sistema Indecx. De esta forma, es posible centralizar todos los análisis y respuestas dentro de Indecx y analizar los resultados con los informes disponibles en la plataforma.

La comunicación se realizará a través de la siguiente URL:

```bash
https://indecx.com/v3/integrations/create-answer/[Identificador_da_acao]
```

Para que este recibo por parte de Indecx sea posible, es necesario tener una acción creada con configuraciones de cuestionario compatibles con lo que fue respondido por el cliente.

## **Pedido**

```javascript
POST /v3/integrations/create-answer/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...

  {
  	"inviteId": "6143d38aaf080f75ffbdaCCC",
	"name": "José Paulo",
	"email": "jose@gmail.com.br",
	"phone": "1999999999",
	"review": 9,
	"feedback": "Teste",
	"additionalQuestions": [
		{
			"type": "REVIEWS",
			"text": "Pergunta A",
			"review": 4
		},
		{
			"type": "LIKE/DISLIKE",
			"text": "Pergunta B",	
			"review": 1
		},
		{
			"type": "CSAT",
			"text": "Pergunta C",
			"review": 5
		},
		{
			"type": "LIKERT",
			"text": "Pergunta D",
			"review": 5
		},
		{
			"type": "EMOTION",
			"text": "Pergunta E",
			"review": 1
		},
		{
			"type": "MULTIPLE",
			"text": "Qual o motivo?",
			"review": ["Opção 1", "Opção 2"]
		},
		{
			"type": "INPUT",
			"text": "Qual foi seu vendedor",
			"review": "Francisco da silva"
		}
	],
	"indicators": [
		{
			"column": "UF",
			"value": "SP"
		},
		{
			"column": "Regiao",
			"value": "Campinas"
		}
	]
}
```

## **RESPUESTA**

```javascript
{
  "message": "Successfully integrated answer.",
   "id": "asy8765gasasgydasd"
}
```

## Comprender los campos de solicitud
| Parámetros	| Descripción |
| ------------- | ------------- |
| ID de invitación	| identificación de invitación |
| nombre	| nombre del encuestado |
| correo electrónico	| correo electrónico del encuestado |
| teléfono	| teléfono del encuestado |
| revisar	| Puntuación asignada a la métrica principal de la encuesta |
| comentario	| Campo de comentario del cliente |
| preguntas adicionales/tipo	| Tipo de pregunta adicional, por ejemplo: RESEÑAS, CSAT, ME GUSTA/NO ME GUSTA, EMOCIÓN, MÚLTIPLE, ENTRADA |
| preguntas adicionales/texto	| Descripción adicional de la pregunta |
| Indicadores/columna	| nombre del indicador |
| Indicadores/valor	| valor del indicador |

*Importante: El campo inviteId solo se usa cuando usamos una combinación de API de generación de enlaces + API de respuesta transaccional, y no es obligatorio para casos de inclusión de respuesta simple. Para obtener más información sobre cómo usar el inviteId, comuníquese con el equipo de soporte de Indecx.

## Comprender las opciones de respuesta por tipo
| Parámetros	| Descripción |
| ------------- | ------------- |
| RESEÑAS	| Escala de 5 puntos (1 a 5) en forma de "estrellas" | 
| CSAT	| Escala de 10 puntos (1 a 10) en forma de escala de evaluación |
| GUSTAR DISGUSTAR	| Escala booleana (0 y 1) en forma de icono positivo y negativo |
| ME GUSTA	| Escala de 5 puntos (1 a 5) donde 1 = "Muy Insatisfecho", 2 = "Insatisfecho", 3 = "Indiferente", 4 = "Satisfecho" y 5 = "Muy Satisfecho" |
| EMOCIÓN	| Escala booleana (0 y 1) en forma de icono emoji positivo y negativo |
| MÚLTIPLE	| Escala de selección única o de opción múltiple. |
| APORTE	| Campo abierto para la inclusión de la respuesta. |

***Importante:** Los valores recibidos deben estar de acuerdo con las opciones de respuesta disponibles en cada métrica para que se pueda realizar la integración.

## Lista de posibles errores
| Código	| Descripción |
| ------------- | ------------- |
| 400	| "mensaje": "Tipo de métrica no válido. Permitido: RESEÑAS, CSAT, ME GUSTA/NO ME GUSTA, EMOCIÓN, MÚLTIPLE, ENTRADA, CES" |
| 400	| "mensaje": "Respuesta inválida para la pregunta XXXX..." |
| 400	| "mensaje": "La invitación ya ha sido respondida" |
| 500	| "mensaje": "Error interno del servidor" |

# GET Lista de acciones activas en Indecx
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Factions--info-orange)

Para acceder a la lista de todas las acciones activas disponibles en la cuenta, use la ruta a continuación.

```javascript
GET /v3/integrations/actions-info
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## **RESPUESTA**

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

# OBTENER Recopilar información de la encuesta
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Factions--info-orange)

Acceder a la estructura del cuestionario programado dentro de Indecx. La comunicación se realizará a través de la siguiente URL:

```javascript
GET /v3/integrations/actions-info/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## **RESPUESTA**

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

## Comprender los campos de devolución
| Parámetros	| Descripción |
| ------------- | ------------- |
| ´_identificación´	| identificador de acción |
| ID de control	| Identificador de control de acción |
| Reenviar | Fechas	Lista de todas las fechas de reenvío| 
| tipo de encuesta	| tipo de cuestionario |
| nombre	| Nombre del cuestionario |
| tipo	| Métrica principal del cuestionario |
| pregunta	| Pregunta métrica clave |
| Habilitar Anónimo	| Opción de anonimato de datos habilitada |
| preguntas adicionales/tipo	| Tipo de pregunta adicional, por ejemplo: RESEÑAS, CSAT, ME GUSTA/NO ME GUSTA, EMOCIÓN, MÚLTIPLE, ENTRADA |
| preguntas adicionales/texto	| Descripción adicional de la pregunta |
| preguntas adicionales/tipo múltiple |	Tipo de selección única (radio) u opción múltiple (casilla de verificación) |
| preguntas adicionales/opciones	| Opciones de elección para múltiples tipos. |
| todas las respuestas Requeridas	| Respuestas de tipo obligatorio. |

## Comprender las opciones de respuesta por tipo
| Parámetros	| Descripción |
| ------------- | ------------- |
| RESEÑAS	| Escala de 5 puntos (1 a 5) en forma de "estrella" |
| CSAT	| Escala de 10 puntos (1 a 10) en forma de escala de evaluación |
| GUSTAR DISGUSTAR	| Escala booleana (0 y 1) en forma de icono positivo y negativo |
| ME GUSTA	| Escala de 5 puntos (1 a 5) donde 1 = "Muy Insatisfecho", 2 = "Insatisfecho", 3 = "Indiferente", 4 = "Satisfecho" y 5 = "Muy Satisfecho" |
| EMOCIÓN	| Escala booleana (0 y 1) en forma de icono emoji positivo y negativo |
| MÚLTIPLE	| Escala de selección única o de opción múltiple. |
| APORTE	| Campo abierto para la inclusión de la respuesta. |

# POST Enviar cliente a la lista de bloqueo
![Badge](https://img.shields.io/badge/POST-send--blocklist-green)

Si desea agregar una lista de clientes a la lista de bloqueo para evitar que este correo electrónico reciba nuevos contactos, puede usar esta ruta.

La comunicación se realizará a través de la siguiente URL:
```bash
https://indecx.com/v3/integrations/send-blocklist
```

## **Pedido**

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

## **RESPUESTA**

```javascript
{
	"message": "Sent to the blocklist successfully."
}
```

# OBTENER Recopilar información de la lista de bloqueo (no quiero recibir más contactos)

![Badge](https://img.shields.io/badge/GET-blocklist--info-orange)

También puede tener acceso a la lista de todos los clientes que ingresaron al blacklist:

```javascript
GET /v3/integrations/blocklist-info/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## Parámetros de consulta     

| Parámetros	| Descripción |
| ------------- | ------------- |
| página	| Devuelve los resultados de una página específica |
| límite	| Devuelve un valor límite por página |

## RESPUESTA

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

## Comprender los campos de devolución
| Parámetros	| Descripción |
| ------------- | ------------- |
| actionName	| Compartir nombre |
| actionControlId	| identificador de acción |
| ID de invitación	| identificación de invitación |
| razón	| Motivo de la inclusión en la lista de bloqueo |
| correo electrónico	| correo electrónico del cliente |
| Creado en	| Fecha de inclusión del registro en la lista de bloqueo |

# GET Recopilar una lista de respuestas categorizadas
![Badge](https://img.shields.io/badge/GET-blocklist--info-orange)

También puede acceder a la lista de todas las categorizaciones asignadas.

```javascript
GET /v3/integrations/category-info/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## Parámetros de consulta
| Parámetros	| Descripción |
| ------------- | ------------- |
| página	| Devuelve los resultados de una página específica |
| límite	| Devuelve un valor límite por página |
| fecha de inicio	| Fecha de inicio del parámetro |
| fecha final	| Fecha de finalización del parámetro |
| tipo de fecha	| creado en o actualizado en |

Nota: Para el valor [Identifier_da_acao] se puede utilizar el parámetro "/all" para devolver todas las invitaciones de todas las acciones disponibles.

Ejemplo de consulta:
```javascript
GET /v3/integrations/category-info/all?startDate=01-01-2022&endDate=28-07-2022&dateType=updatedAt
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## **RESPUESTA**

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

## Comprender los campos de devolución
| Parámetros	| Descripción |
| -- | -- |
| ID de respuesta	| ID de respuesta |
| ID de invitación	| identificación de invitación |
| categoría	| Nombre de categoría asignado |
| subcategoría	| Nombre de subcategoría asignado |
| Creado en	| Fecha de inclusión de la respuesta |
| actualizado en	| Fecha de actualización de la respuesta |
| tipo de fecha	| Fecha de inclusión de la respuesta |

## Sucursal de registro POST (IH1)
![Badge](https://img.shields.io/badge/post-branch-green)

También puede registrar una sucursal o unidad de negocio a través de la API. Esta rama, denominada Ih1 en la plataforma, se utilizará para separar datos y niveles de acceso por usuario.
```bash
{
  https://indecx.com/v3/integrations/branches
}
```

## Solicitar json

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

### RESPUESTA

```bash

{
	"_id": "630918be7140e266cafafab7",
	"active": true,
	"corporate_name": "UNIDADE X - CENTRO",
	"type": "ih1"
}
```

### Comprender los campos de devolución
| Parámetros	| Descripción |
| - | - |
| ´_identificación´	| identificación de la sucursal |
| activo	| estado |
| nombre corporativo	| nombre de la sucursal |
| tipo	| Tipo de sucursal |

# GET Lista de Sucursales Registradas (IH1)
![Badge](https://img.shields.io/badge/get-branch-orange)

La API permite el acceso a la lista de todas las unidades registradas en la plataforma.

```javascript
GET /v3/integrations/branches
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## RESPUESTA

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

### Gracias💚

¿Existe alguna ruta que te haga el día a día más fácil?? ¡Ponte en contacto con el equipo de Indecx CX que desarrollamos para ti! =) Actualizado el 20/06/2023
