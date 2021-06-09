<h1 align="center">
  <img alt="" title="" src="./assets/header_indecx_API.png" />
</h1>

![Badge](https://img.shields.io/badge/version-Preview-blue)

A API IndeCX foi desenvolvida para que seja possível realizar diversas integrações entre seu sistema e as funcionalidades disponíveis na nossa plataforma.

Nessa documentação você encontrará exemplos de utilização de cada método.

# Autenticação
A autenticação de todas as rotas é feita através da chave **company-key** que fornecida dentro da plataforma no menu dados da conta.

| Params  | Descrição |
| ------------- | ------------- |
| Content-Type  | application/json  |
| company-key  | Exemplo key: $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX  |


# POST Envio de respostas transacionais para o Indecx
![Badge](https://img.shields.io/badge/POST-v2%2Factions-green)

Com a rota abaixo, será possível enviar uma resposta coletada em seu APP ou sistema de forma transacional para dentro do sistema Indecx. Dessa forma é possível centralizar todos as análises e pesquisas dentro do Indecx e analisar os resultados com os relatórios disponíveis na plataforma.

A comunicação será realizada através da seguinte URL:

```bash
https://indecx.com/v2/create-answer/[Identificador_da_acao]
```
Para que esse recebimento por parte do indecx seja possível, é necessário ter uma ação criada com as configurações de questionários compatíveis com o que foi respondido pelo cliente.

## **Request**
```bash
  {
	"name": "José Paulo",
	"email": "jose@gmail.com.br",
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
			"review": 6
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

```bash
{
  "message": "Successfully integrated answer.",
   "id": "asy8765gasasgydasd"
}
```



## Entendendo os campos da requisição

| Params  | Descrição |
| ------------- | ------------- |
| name  | Nome do respondente   |
| email  | E-mail do respondente  |
| review  | Nota atribuída para a métrica principal da pesquisa  |
| feedback  | Campo de comentário do cliente    |
| adittionalQuestions.type  | Tipo da pergunta adicional ex:REVIEWS,CSAT,LIKE/DISLIKE,LIKERT,EMOTION,MULTIPLE,INPUT   |
| adittionalQuestions.text  | Descrição da pergunta adicional   |
| indicators.column  | Nome do indicador  |
| indicators.value  | Valor do indicador  |


## Entendendo as opções de respostas por tipo

| Params  | Descrição |
| ------------- | ------------- |
| REVIEWS  | Escala de 5 pontos (1 a 5) na forma "estrela"|
| CSAT  | Escala de 10 pontos (1 a 10) na forma escala de avaliação  |
| LIKE/DISLIKE  | Escala boleana (0 e 1) na forma de ícone positivo e negativo  |
| LIKERT  | Escala de 5 pontos (1 a 5) sendo 1 = "Muito insatisfeito" , 2 = "Insatisfeito", 3 = "Indiferente", 4 = "Satisfeito" e 5 = "Muito Satisfeito" |
| EMOTION  | Escala boleana (0 e 1) na forma de ícone emoji positivo e negativo   |
| MULTIPLE  | Escala do tipo seleção unica ou multipla escolha.  |
| INPUT  | Capo aberto para inclusão de resposta aberta.  |

*Importante: Os valores recebidos precisam estar de acordo com as opções de respostas disponíveis em cada métrica para que seja possível a realização da integração.

# GET Listar ações ativas no Indecx
![Badge](https://img.shields.io/badge/GET-v2%2Factions--info-orange)

Para ter acesso a lista de todas as ações ativas disponíveis na conta, utilize a rota abaixo.

```bash
{
  https://api-indecx.com/v2/actions-info/
}
```

## **Response**

```bash
[
  {
    "surveyType": "survey",
    "type": "nps-0-10",
    "name": "Ação de pesquisa da área de pós-venda",
    "channel": "emailsms",
    "controlId": "5Q0WB7",
    "createdAt": "2020-01-15T20:11:59.540Z"
  },
  {
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
![Badge](https://img.shields.io/badge/GET-v2%2Factions--info-orange)

Para ter acesso a estrutura do questionário programado dentro o Indecx. A comunicação será realizada através da seguinte URL:

```bash
{
  https://indecx.com/v2/actions-info/[Identificador_da_acao]
}
```

## **Response**

```bash
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
| _id  | Identificador da ação  |
| controlId  | Identificador de controle da ação   |
| resendDates  | Lista de todas as data que houveram reenvio  |
| surveyType  | tipo do questionário  |
| name  | Nome do questionário    |
| type  | Métrica principal do questionário    |
| question  | Pergunta da métrica principal   |
| enableAnonymous  | Habilitado opção de anonimização dos dados   |
| adittionalQuestions.type  | Tipo da pergunta adicional ex:REVIEWS,CSAT,LIKE/DISLIKE,LIKERT,EMOTION,MULTIPLE,INPUT   |
| adittionalQuestions.text  | Descrição da pergunta adicional   |
| adittionalQuestions.multipleType  | Tipo de seleção única (radio) ou multipla escolha (checkbox)  |
| adittionalQuestions.choices  | Opções de escolha para tipo multipla.  |
| allAnswersRequired  | Respostas do tipo obrigatória.  |

## Entendendo as opções de respostas por tipo

| Params  | Descrição |
| ------------- | ------------- |
| REVIEWS  | Escala de 5 pontos (1 a 5) na forma "estrela"|
| CSAT  | Escala de 10 pontos (1 a 10) na forma escala de avaliação  |
| LIKE/DISLIKE  | Escala boleana (0 e 1) na forma de ícone positivo e negativo  |
| LIKERT  | Escala de 5 pontos (1 a 5) sendo 1 = "Muito insatisfeito" , 2 = "Insatisfeito", 3 = "Indiferente", 4 = "Satisfeito" e 5 = "Muito Satisfeito" |
| EMOTION  | Escala boleana (0 e 1) na forma de ícone emoji positivo e negativo   |
| MULTIPLE  | Escala do tipo seleção unica ou multipla escolha.  |
| INPUT  | Capo aberto para inclusão de resposta aberta.  |
