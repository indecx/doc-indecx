<h1 align="center">
  <img alt="" title="" src="./assets/header_indecx_API.png" />
</h1>

![Badge](https://img.shields.io/badge/integrations-v3-blue) 


A API IndeCX foi desenvolvida para que seja possível realizar diversas integrações entre seu sistema e as funcionalidades disponíveis na nossa plataforma. Nessa Versão, a API conta com uma melhoria no processo de autenticação e autorização utilizando Bearer token que é uma abordagem segura, simples, escalável, flexível e padronizada para garantir a segurança em APIs.

Nessa documentação você encontrará exemplos de utilização de cada método.


## Swagger
https://indecx.com/api-docs/#/


## Introdução
A API utiliza autenticação e autorização via Bearer Token para proteger as rotas de acesso restrito. O Bearer Token deve ser enviado no cabeçalho Authorization de todas as requisições que requerem autenticação.


## Autenticação
Para autenticar com a API, você deve enviar um pedido de autenticação com suas credenciais de usuário (**company-key**) enviado via header, disponíveis dentro de configurações da conta na plataforma Indecx. A API retornará um Bearer Token, que você deve usar em todas as solicitações futuras que exigem autenticação.


## Endpoint /get
![Badge](https://img.shields.io/badge/GET-authorization--token-orange)
```bash
https://indecx.com/v3/integrations/authorization/token
```

| Params  | Descrição |
| ------------- | ------------- |
| company-key  | $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX  |


## Exemplo de request
```javascript
GET /v3/integrations/authorization/token HTTP/1.1
Host: indecx.com
Company-Key: $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX

```
## Exemplo de response
```javascript
HTTP/1.1 200 OK
Content-Type: application/json

{
	"authToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVlLU1Rra..."
}

```


## Exemplo de cabeçalho de autorização
O Bearer Token deve ser enviado no cabeçalho Authorization de todas as requisições que requerem autenticação. Token possui validade de 30 minutos.

```javascript
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVlLU1Rra...
```


# POST Disparo de convite transacional
![Badge](https://img.shields.io/badge/POST-.v3/integrations/%2Fsend%2F-green)

Imagine uma situação onde você queira disparar uma pesquisa de satisfação a cada nova transação do cliente dentro do seu sistema, 
seja uma prospecção, vendas, interação etc... No método disparo de convite transacional é possível e de forma bem fácil.

A comunicação será realizada através da seguinte URL:
```bash
https://indecx.com/v3/integrations/send/[Identificador_da_acao]
```
As ações deverão ser criadas na plataforma app-indecx.com e os disparos poderão ser efetuados via plataforma (manual) ou via API. Quando disparado via API, deve-se enviar um JSON via body para a URL mencionada acima e sua autenticação se dará através da company-key fornecida e enviada via HEADER.  Importante que seja enviado o identificador da ação e também que siga um padrão para envio do JSON via body.

## JSON Configuração
**Importante**
E-mail e telefone são campos obrigatórios, dependendo da configuração do tipo de disparo. Consulte as configurações realizadas no momento da criação da ação. Para ser possível enviar dados para as colunas adicionais ( indicadores ) é necessário criar os indicadores dentro da ação anteriormente e utilizar exatamente os mesmos nomes criado dentro da plataforma. No nosso exemplo abaixo as colunas adicionais são: Filial, Área, Região e Tipo. 

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
Para disparos agendados, o convite ficará com status **agendado** na indecx e caso necessário, também é possível cancelar o disparo direto pela plataforma no menu  Disparos>>Agendamentos
<h1 align="center">
  <img alt="" title="" src="./assets/scheduling.png" />
</h1>

**Importante:**
O campo “scheduling” utiliza padrão UTC, dessa forma possibilita agendar disparos com horários locais de outros países.
Exemplo parâmetro **“2020-12-16T20:20:00.000”** será disparos nos seguintes horários:
Brasil 17:20:00 (UTC - 3) , EUA Nova York 16:20:00 (UTC - 5)

# POST Disparo de convite em lote
![Badge](https://img.shields.io/badge/POST-.v3/integrations/%2Fsend%2Fbulk-green)

Você também pode realizar os disparos de forma automática para um lote de clientes de uma única vez 

A comunicação será realizada através da seguinte URL:
```bash
https://indecx.com/v3/integrations/send/bulk/[Identificador_da_acao]
```
As ações deverão ser criadas na plataforma app-indecx.com e os disparos poderão ser efetuados via plataforma ou via API. Quando disparado via API, deve-se enviar um JSON via body para a URL mencionada acima e sua autenticação se dará através da company-key fornecida e enviada via HEADER. 
Importante que seja enviado o identificador da ação e também que siga um padrão para envio do JSON via body.

## JSON Configuração
**Importante**
E-mail e telefone são campos obrigatórios, dependendo da configuração do tipo de disparo. Consulte as configurações realizadas no momento da criação da ação. Para ser possível enviar dados para as colunas adicionais ( indicadores ) é necessário criar os indicadores dentro da ação anteriormente e utilizar exatamente os mesmos nomes criado dentro da plataforma.
**Essa API possui um limite máximo de 1.000 clientes por requisição.**

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
**Retornos possíveis**
| Código  | Descrição |
| ------------- | ------------- |
| 200 | Disparo configurado com sucesso!  |
| 400 | Action not found. |
| 400 | Api key is missing.|
| 400 | Please inform at least one customer. |
| 401 | Company not found for this api key. |
| 401 | Invalid api key. |
| 401 | This action is blocked for integrations. |
| 401 | This action is paused. |
| 402 | Insufficient email balance. |
| 402 | Insufficient sms balance. |
| 500 | Internal Server Error |



# GET Coletar Respostas
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Fanswers--info%2F-orange)

Você também pode integrar as respostas coletadas via indecx no seu sistema. Para isso será necessário utilizar o método "Coletar respostas" para acesso a todas as respostas e detalhes dos clientes.

A comunicação será realizada através da seguinte URL:
```bash
https://indecx.com/v3/integrations/answers-info/[Identificador_da_acao]?[params]
```
## Query Params
| Params  | Descrição |
| ------------- | ------------- |
| page  | Retorna os resultados de uma determinada página em específico  |
| limit  | Retorna o um valor limite por página  |
| startDate  | Data inicial do parâmetro  |
| endDate  | Data final do parâmetro  |
| dateType  | Tipo da data createdAt (criação da pesquisa) ou updatedAt (Atualização da pesquisa)  |
| email  | Parâmetro de email do respondente  |
| phone  | Parâmetro de telefone do respondente  |

Obs: Para o valor [Identificador_da_acao] pode ser utilzado o parâmetro "/all" para retornar todas as resposta de todas as ações disponíveis.

Exemplo de consulta:

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

## Entendendo os campos de retorno
| Params  | Descrição |
| ------------- | ------------- |
| _id  | ID da resposta  |
| active  | Status  |
| date  | Data e hora da resposta  |
| answerDate  | Data e hora da resposta  |
| anonymousResponse  | Identificação se o cliente quis se identificar ou não.  |
| clientId  | Identificador único do cliente   |
| name  | Nome do cliente   |
| email  | E-mail do cliente   |
| phone  | Telefone do cliente  |
| feedback  | Comentário realizado pelo cliente no momento da resposta   |
| categories  | Categorização do comentário (feedback)   |
| channel  | Canal de coleta da resposta   |
| companyId  | ID da empresa   |
| actionId  | ID da ação   |
| inviteId  | ID do convite   |
| detailsId  | ID dos detalhes da importação   |
| text  | Pergunta da métrica principal   |
| review  | Nota atribuida na métrica principal   |
| metric  | Métrica principal   |
| createdAt  | Data da criação da resposta   |
| updateAt  | Data de update da resposta   |
| deleted | Verifica se a resposta foi excluída |

**additionalQuestions**
| Params  | Descrição |
| ------------- | ------------- |
| _id  | ID da pergunta  |
| type  | Tipo da pergunta REVIEWS,LIKE/DISLIKE,LIKERT,CSAT,EMOTION,MULTIPLE,INPUT |
| text  | Descrição da pergunta  |
| review  | Nota atribuída   |
| answerText  | Retorno do texto em caso de pergunta adicional do tipo **INPUT**   |
| multipleValues  | Retorno das respostas multiplas em caso de pergunta adicional do tipo **MULTIPLE**   |
| options  | Retorno das respostas multiplas com subopções **MULTIPLE com SUB**   |

**indicators**
| Params  | Descrição |
| ------------- | ------------- |
| _id  | ID do indicador  |
| column  | Nome do indicador  |
| value  | Valor do indicador  |

**sentimentAnalyzeGCP**
| Params  | Descrição |
| ------------- | ------------- |
| score  | Score de sentimento  |
| keyPhrases  | Palavras chaves identificadas nos comentários do cliente  |

**treatments**
| Params  | Descrição |
| ------------- | ------------- |
| status  | Status da tratativa  |
| sponsor  | Nome do responsável  |
| comments  | Comentário registrado pelo responsável  |
| date  | Data da tratativa  |
| categories  | Categorias incluídas nas respostas   |

# GET Coletar Informações dos convites
![Badge](https://img.shields.io/badge/GET-invites--info-orange)

Você pode ter acesso a lista de todos os clientes importados na plataforma com status se o convite já foi respondido ou não.

A comunicação será realizada através da seguinte URL:
```bash
https://indecx.com/v3/integrations/invites-info/[Identificador_da_acao]?[params]
```
## Query Params
| Params  | Descrição |
| ------------- | ------------- |
| page  | Retorna os resultados de uma determinada página em específico  |
| limit  | Retorna o um valor limite por página  |
| startDate  | Data inicial do parâmetro  |
| endDate  | Data final do parâmetro  |
| dateType  | Tipo da data createdAt (criação do convite) ou updatedAt (Atualização do convite)  |
| email  | Parâmetro de email do convite  |
| phone  | Parâmetro de telefone do convite  |

Obs: Para o valor [Identificador_da_acao] pode ser utilzado o parâmetro "/all" para retornar todos os convites de todas as ações disponíveis.

Exemplo de consulta:
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

## Entendendo os campos de retorno
| Params  | Descrição |
| ------------- | ------------- |
| _id  | ID do convite   |
| answered  | Verifica se o convite foi respondido  |
| active  | Verifica se o convite estar ativo  |
| actionId  | ID da ação   |
| name  | Nome do cliente   |
| email  | E-mail do cliente   |
| phone  | Telefone do cliente  |
| shortUrl  | Endereço do link da pesquisa   |
| validEmail  | Validador de e-mail   |
| validPhone  | Validador do telefone  |
| createdAt  | Data da criação do convite   |
| updateAt  | Data de update do convite   |
| answeredId  | ID da resposta   |
| deletedAnswer | Verifica se a resposta foi excluída |



# GET Coletar clientes não respondentes
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Fno--response-orange)

Através desse método, você pode ter acesso a lista de todos os clientes que ainda não responderam a sua pesquisa.
A comunicação será realizada através da seguinte URL:
```bash
https://indecx.com/v3/integrations/[Identificador_da_acao]?[params]
```

## Query Params
| Params  | Descrição |
| ------------- | ------------- |
| page  | Retorna os resultados de uma determinada página em específico  |
| limit  | Retorna o um valor limite por página  |
| startDate  | Data inicial do parâmetro  |
| endDate  | Data final do parâmetro  |
| email  | Busca por email de usuário específico  |

Exemplo de consulta:

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

**Coletar respostas com limite de paginação e retorno máximo no payload de 1mil registros.**

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

## Entendendo os campos de retorno
| Params  | Descrição |
| ------------- | ------------- |
| _id  | ID da resposta  |
| answered  | Status do resposta  |
| resendDates  | Lista de todas as datas que houveram reenvio do convite  |
| active  | Status do convite  |
| reminderDates  | Lista de todas as datas que houveram lembretes    |
| controlId  | Identificador de controle do convite    |
| actionId.name  | Nome da ação atriuída ao convite   |
| actionId.controlId  | Identificador de controle da ação   |
| name  | Nome do cliente   |
| email  | E-mail do cliente   |
| phone  | Telefone do cliente  |
| shortUrl  | link da pesquisa do cliente  |
| validEmail  | Validador do campo e-mail  |
| validPhone  | Validador do campo telefone |
| indicators  | Lista dos indicadores atribuídos ao convite   |
| metric  | Métrica principal do convite   |
| createdAt  | ???????   |
| page  | Paginação do retorno   |
| limit  | Limite máximo de retorno da consulta    |
| total  | Total de registros da consulta   |

# WEBHOOK Coletar Respostas
![Badge](https://img.shields.io/badge/webhook-answers-red)

Você também pode cadastrar o seu endpoint dentro da plataforma Indecx para que a cada nova resposta seja enviada automaticamente para o seu sistema. Logado na plataforma Indecx, acesse: **Configurações >>Integrações >> Coleta de respostas**
<h1 align="center">
  <img alt="" title="" src="./assets/webhook-answer.png" />
</h1>
A cada nova resposta você receberá um retorno com todas as informações do cliente e respostas atribuídas por ele, conforme mostrado no exemplo abaixo:

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

# POST Geração de link para pesquisa
![Badge](https://img.shields.io/badge/POST-v3/integrations/%2Factions-green)

Com a rota abaixo, será possível realizar a integração com a plataforma Indecx, onde o objetivo é criar um convite e retornar o link no response da chamada, para que seja utilizado dentro das mídias de contato com o cliente e esse link é único para cada novo convite gerado.

A comunicação será realizada através da seguinte URL:
```bash
https://indecx.com/v3/integrations/actions/[Identificador_da_acao]/invites
```
As ações deverão ser criadas na plataforma app-indecx.com e os disparos poderão ser efetuados via plataforma ou via API. Quando disparado via API, deve-se enviar um JSON via body para a URL mencionada acima e sua autenticação se dará através da company-key fornecida e enviada via HEADER.  Importante que seja enviado o identificador da ação e também que siga um padrão para envio do JSON via body

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

## Geração de link para pesquisa com URL Callback
Também é possível enviar um disparo e obter o link através da API cadastrada na callbackurl.

**callbackurl** Rota da API

**maxmsg** Máximo de retorno de registro no JSON. Exemplo: Caso você faça um disparo de 1.000 clientes com maxmsg = 100,  Você vai receber 10 requisições com 100 registros em cada.

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

# POST Envio de respostas transacionais para o Indecx
![Badge](https://img.shields.io/badge/POST-v3/integrations/%2Factions-green)

Com a rota abaixo, será possível enviar uma resposta coletada em seu APP ou sistema de forma transacional para dentro do sistema Indecx. Dessa forma é possível centralizar todas as análises e respostas dentro do Indecx e analisar os resultados com os relatórios disponíveis na plataforma.

A comunicação será realizada através da seguinte URL:

```bash
https://indecx.com/v3/integrations/create-answer/[Identificador_da_acao]
```
Para que esse recebimento por parte do Indecx seja possível, é necessário ter uma ação criada com as configurações de questionários compatíveis com o que foi respondido pelo cliente.

## **Request**
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

## **Response**

```javascript
{
  "message": "Successfully integrated answer.",
   "id": "asy8765gasasgydasd"
}
```



## Entendendo os campos da requisição

| Params  | Descrição |
| ------------- | ------------- |
| inviteId  | id do convite   |
| name  | Nome do respondente   |
| email  | E-mail do respondente  |
| phone  | Telefone do respondente  |
| review  | Nota atribuída para a métrica principal da pesquisa  |
| feedback  | Campo de comentário do cliente    |
| adittionalQuestions.type  | Tipo da pergunta adicional ex:REVIEWS,CSAT,LIKE/DISLIKE,LIKERT,EMOTION,MULTIPLE,INPUT   |
| adittionalQuestions.text  | Descrição da pergunta adicional   |
| indicators.column  | Nome do indicador  |
| indicators.value  | Valor do indicador  |

*Importante: O campo inviteId somente é utilizado quando utilizamos uma conbinação da API de geração de link + API de resposta transacional, não sendo obrigatório para casos de inclusão simples de resposta. Para mais informações de como utilizar o inviteId, entre em contato com o time de suporte da Indecx.

## Entendendo as opções de respostas por tipo

| Params  | Descrição |
| ------------- | ------------- |
| REVIEWS  | Escala de 5 pontos (1 a 5) na forma "estrela"|
| CSAT  | Escala de 10 pontos (1 a 10) na forma escala de avaliação  |
| LIKE/DISLIKE  | Escala boleana (0 e 1) na forma de ícone positivo e negativo  |
| LIKERT  | Escala de 5 pontos (1 a 5) sendo 1 = "Muito insatisfeito" , 2 = "Insatisfeito", 3 = "Indiferente", 4 = "Satisfeito" e 5 = "Muito Satisfeito" |
| EMOTION  | Escala boleana (0 e 1) na forma de ícone emoji positivo e negativo   |
| MULTIPLE  | Escala do tipo seleção única ou múltipla escolha.  |
| INPUT  | Campo aberto para inclusão de resposta.  |

*Importante: Os valores recebidos precisam estar de acordo com as opções de respostas disponíveis em cada métrica para que seja possível a realização da integração.

## Lista de erros possíveis
| Código  | Descrição |
| ------------- | ------------- |
| 400  | "message": "Invalid metric type. Allowed: REVIEWS, CSAT, LIKE/DISLIKE, LIKERT, EMOTION, MULTIPLE, INPUT, CES"|
| 400  | "message": "Invalid answer for the question XXXX..." |
| 400  | "message": "The Invite has already been answered" |
| 500  | "message": "Internal server error" |


# GET Listar ações ativas no Indecx
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Factions--info-orange)

Para ter acesso a lista de todas as ações ativas disponíveis na conta, utilize a rota abaixo.

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


# GET Coletar informações do questionário
![Badge](https://img.shields.io/badge/GET-v3/integrations/%2Factions--info-orange)

Para ter acesso a estrutura do questionário programado dentro do Indecx. A comunicação será realizada através da seguinte URL:


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
## Entendendo os campos de retorno

| Params  | Descrição |
| ------------- | ------------- |
| _id  | ID da ação  |
| controlId  | Identificador de controle da ação   |
| resendDates  | Lista de todas as data que houveram reenvio  |
| surveyType  | tipo do questionário  |
| name  | Nome do questionário    |
| type  | Métrica principal do questionário    |
| question  | Pergunta da métrica principal   |
| enableAnonymous  | Habilitado opção de anonimização dos dados   |
| adittionalQuestions.type  | Tipo da pergunta adicional ex:REVIEWS,CSAT,LIKE/DISLIKE,LIKERT,EMOTION,MULTIPLE,INPUT   |
| adittionalQuestions.text  | Descrição da pergunta adicional   |
| adittionalQuestions.multipleType  | Tipo de seleção única (radio) ou múltipla escolha (checkbox)  |
| adittionalQuestions.choices  | Opções de escolha para tipo múltipla.  |
| allAnswersRequired  | Respostas do tipo obrigatória.  |

## Entendendo as opções de respostas por tipo

| Params  | Descrição |
| ------------- | ------------- |
| REVIEWS  | Escala de 5 pontos (1 a 5) na forma "estrela"|
| CSAT  | Escala de 10 pontos (1 a 10) na forma escala de avaliação  |
| LIKE/DISLIKE  | Escala boleana (0 e 1) na forma de ícone positivo e negativo  |
| LIKERT  | Escala de 5 pontos (1 a 5) sendo 1 = "Muito insatisfeito" , 2 = "Insatisfeito", 3 = "Indiferente", 4 = "Satisfeito" e 5 = "Muito Satisfeito" |
| EMOTION  | Escala boleana (0 e 1) na forma de ícone emoji positivo e negativo   |
| MULTIPLE  | Escala do tipo seleção única ou múltipla escolha.  |
| INPUT  | Campo aberto para inclusão de resposta.  |

# POST Envio de cliente para blocklist
![Badge](https://img.shields.io/badge/POST-send--blocklist-green)

Caso você queira adicionar uma lista de clientes dentro da blocklist para evitar que esse e-mail receba novos contato, você pode realizar através dessa rota.

A comunicação será realizada através da seguinte URL:
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


# GET Coletar informações da blocklist (Não quero receber mais contato)
![Badge](https://img.shields.io/badge/GET-blocklist--info-orange)

Você também pode ter acesso a lista de todos os clientes que entraram em blaclist.:

```javascript
GET /v3/integrations/blacklist-info
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## Query Params
| Params  | Descrição |
| ------------- | ------------- |
| page  | Retorna os resultados de uma determinada página em específico  |
| limit  | Retorna o um valor limite por página  |

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
## Entendendo os campos de retorno

| Params  | Descrição |
| ------------- | ------------- |
| actionName  | Nome da Ação  |
| actionControlId  | ID da ação   |
| inviteId  | ID do convite  |
| reason  | Motivo da inclusão no blocklist  |
| email  | Email do cliente    |
| createdAt  | Data da inclusão do registro em blocklist    |



# GET Coletar lista de respostas categorizadas
![Badge](https://img.shields.io/badge/GET-blocklist--info-orange)

Você também pode ter acesso a lista de todas as categorizações atribuídas.

```javascript
GET /v3/integrations/category-info/T0AXXX
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```

## Query Params
| Params  | Descrição |
| ------------- | ------------- |
| page  | Retorna os resultados de uma determinada página em específico  |
| limit  | Retorna o um valor limite por página  |
| startDate  | Data inicial do parâmetro  |
| endDate  | Data final do parâmetro  |
| dateType  | createdAt ou updatedAt  |


Obs: Para o valor [Identificador_da_acao] pode ser utilzado o parâmetro "/all" para retornar todos os convites de todas as ações disponíveis.

Exemplo de consulta:

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
## Entendendo os campos de retorno

| Params  | Descrição |
| ------------- | ------------- |
| answerId  | ID da resposta  |
| inviteId  | ID do convite  |
| category  | Nome da categoria atribuida  |
| subCategory  | Nome da subcategoria atribuida     |
| createdAt  | Data da inclusão da resposta  |
| updatedAt  | Data da atualização  da resposta  |
| dateType  | Data da inclusão da resposta  |




# POST Cadastrar Filial (IH1)
![Badge](https://img.shields.io/badge/post-branch-green)

Você também pode cadastrar uma filial ou unidade de negócio via API. Essa filial que chamados de Ih1 na plataforma, será utilizada para que seja possível separar os dados e niveis de acesso por usuário.



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
## Entendendo os campos de retorno

| Params  | Descrição |
| ------------- | ------------- |
| _id  | ID da filial  |
| active  | status  |
| corporate_name  | Nome da filial  |
| type  | Tipo da Filial     |


# GET Listar Filiais Cadastradas (IH1)
![Badge](https://img.shields.io/badge/get-branch-orange)

API permite ter acesso a lista de todas as unidades cadastradas na plataforma.

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

# GET Coletar detalhes da ação
![Badge](https://img.shields.io/badge/GET-details--info-orange)

Este endpoint permite que você obtenha detalhes de uma ação específica.

```javascript
GET /v3/integrations/details-info/:controlId
Host: indecx.com
Content-Type: application
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```
## Query Params
| Params  | Descrição |
| ------------- | ------------- |
| page  | Retorna os resultados de uma determinada página em específico  |
| limit  | Retorna o um valor limite por página  |
| startDate | Data inicial do parâmetro | 
| endDate | Data final do parâmetro | 
| status | Status da ação | 
| dateType  | Tipo da data createdAt (criação da pesquisa) ou updatedAt (Atualização da pesquisa)  |

Exemplo de consulta:

```javascript
GET /v3/integrations/details-info/[Identificador_da_acao]?page=1&limit=10
GET /v3/integrations/details-info/[Identificador_da_acao]?startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt HTTP/1.1
GET /v3/integrations/details-info/[Identificador_da_acao]?status=[status da ação]

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

## Entendendo os campos de retorno
| Params  | Descrição |
| ------------- | ------------- |
| _id  | 	ID do detalhe da ação  |
| active  | Se a ação está ativa ou não  |
| companyId  | ID da empresa  |
| actionId  | ID da ação  |
| channel  | Canal da ação (ex: email)    |
| status  | Status da ação (ex: enviado)    |
| controlId  | ID de controle da ação   |
| submitDate  | Data de submissão da ação   |
| createdAt  | Data de criação da ação   |
| updatedAt  | Data de atualização da ação   |
| submitId  | Data de submissão da ação  |
| Email  | Email do destinatário da ação  |
| Name  | Nome do destinatário da ação |
| Phone  | Telefone do destinatário da ação |
| validEmail  | Se o email é válido ou não   |
| validPhone  | Se o telefone é válido ou não   |
| indicators  | Lista dos indicadores atribuídos ao convite   |

# PUT Atualização de Convites
![Badge](https://img.shields.io/badge/PUT-update--invites-green)

Este endpoint permite que você atualize os detalhes de vários convites de uma vez. Ele aceita um array de convites, onde cada convite é um objeto contendo os campos `inviteId`, `name`, `email` e `phone`. O `inviteId` é obrigatório, enquanto `name`, `email` e `phone` são opcionais. Lembrando que não é permitido deixar todos os campos vazios e também não será possivel atualizar convites respondidos.

## Requisição

A comunicação será realizada através da seguinte URL:
```bash
https://indecx.com/v3/integrations/update-invite
```

**Corpo da Requisição:**

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
**Resposta:**

A resposta será um objeto contendo um array message, onde cada item corresponde a um convite enviado na requisição. Cada item será um objeto contendo inviteId e message, indicando o resultado da atualização para aquele convite.

**Exemplo de resposta:**
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

## **Códigos de Resposta Possíveis**
| Código | Descrição |
| ------ | --------- |
| 200    | "message": "Successfully updated invite."
| 400    | "message": "Failure: All fields are empty." |
| 400    | "message": "Failure: Invalid inviteId." |
| 400    | "message": "Failure: Invalid phone number." |
| 400    | "message": "Failure: Invalid email." |
| 400    | "message": "Failure: Invalid name." |
| 400    | "message": "Failure: Invite already answered and cannot be updated." |
| 400    | "message": "Invite not found" |
| 500    | "message": "Failure: [error message]" |

# GET Coletar Informações de Quarentenas
![Badge](https://img.shields.io/badge/GET-quarantines--info-green)

Este endpoint permite que você obtenha informações detalhadas sobre quarentenas.

```javascript
GET /v3/integrations/quarantines-info/:controlId
Host: indecx.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdWJsaWNLZXkiOiIkMmIkMTAkQkxWNENKQVl...
```
## Query Params
| Params  | Descrição |
| ------------- | ------------- |
| page  | Retorna os resultados de uma determinada página em específico  |
| limit  | Retorna o um valor limite por página  |
| startDate | Data inicial do parâmetro | 
| endDate | Data final do parâmetro | 
| status | Status da ação | 
| dateType  | Tipo da data createdAt (criação da pesquisa) ou updatedAt (Atualização da pesquisa)  |
|groupId| ID do grupo de ações|
|clientId| ID do cliente|
|phone|	Número de telefone do destinatário|
|email|	Email do destinatário|
|actionId| ID da ação|
|companyId| ID da empresa |

```javascript
GET /v3/integrations/quarantines-info/?phone=[telefone do destinatário]
GET /v3/integrations/quarantines-info/?email=[email do destinatário]
GET /v3/integrations/quarantines-info/?clientId[id do cliente]
GET /v3/integrations/quarantines-info/?groupId[ID do grupo de ações] ou all(irá buscar em todas as ações)
GET /v3/integrations/quarantines-info/?actionId[ID da ação] ou all (irá buscar em todos os grupos de ação)
GET /v3/integrations/quarantines-info/?page=1&limit=10
GET /v3/integrations/quarantines-info/?startDate=10-01-2022&endDate=10-01-2022&dateType=createdAt
GET /v3/integrations/quarantines-info (trás todas as quarentenas)


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
## Entendendo os campos de retorno
| Params  | Descrição |
| ------------- | ------------- |
| _id | ID do detalhe da ação  |
| expirationDate | Data para o para sair da quarentena  |
| Email | Email do destinatário da ação  |
| Phone | Telefone do destinatário da ação |
| companyId | ID da empresa  |
| actionId | ID da ação  |
| groupId | ID do grupo de ações|
| type | Por onde o cliente entrou na quarentena |
| createdAt | Data de criação da ação   |
| updatedAt | Data de atualização da ação   |



### Obrigado 💚

Sentiu falta de alguma rota que vai facilitar o seu dia a dia?? Entre em contato com o time de CX da Indecx que desenvolvemos para você! = )
Atualizado em 01/01/2023
