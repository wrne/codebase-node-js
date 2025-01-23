# Objetivo e motivação
Projeto para servir de base para outras API´s Node.

Desenvolvido do zero com muita pesquisa e algum apoio do GPT.

O projeto visa cobrir os principais pontos arquiteturais do projeto de uma API Rest.
- Roteamento
- Autenticação
- Arquitetura
- Message Broker para comunicação assíncrona
- Documentação
- Exemplos de implementação de rotas GET e POST


# Tecnologias utilizadas

| Tecnologia | Ferramenta |
| --- | --- | 
| **Back-end** | NodeJS | 
| **Message Broker** | [RabbitMQ](https://www.notion.so/RabbitMQ-fb2c9d12b7e642bb86dffd6841c1c0e1?pvs=21) | 
| **Banco de Dados** | SQL Server | 


# Métodos e Rotas implementados

Todas as rotas devem estar documentadas no Swagger da API:

http://localhost:3000/docs

# Componentes e Boas Práticas

## Padrão de resposta

Foi incluído um middleware na propriedade *response* do **Express** em todas as rotas que permite que todas as respostas sejam padronizadas, sejam elas de sucesso ou de erro.

### Mensagens de sucesso

Execução do método ***res.success(data, message)***

Onde:

- data: objeto opcional em JSON com qualquer dado que deva ser retornado na API.
- message: Mensagem opcional que será retornada na API.

Formato de saída:

```jsx
{
    "status": 200,
    "message": "task added successful.", // Mensagem Padrão
    "data": null,
    "error": null
}
```

### Mensagens de erro

Execução do método ***res.error(statusCode, errorObj, errorTitle)***

Onde:

- statusCode: código http do status code do erro. O *default* é 500.
- error: objeto com instancia da classe Error(). A propriedade error.message é retornada na API na propriedade message.
- titleError: Mensagem opcional que será enviada na propriedade error no retorno da API.

Formato de saída:

```jsx
{
    "status": 401,
    "message": "jwt expired",
    "error": "Not autorized"
}
```

Para saber mais sobre os status code que você pode utilizar, acesse: 

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

## Logging

Foi criado um componente para auxiliar na padronização dos logs de console.

```jsx
import {logMessage, logError} from "./utils/log-generator.js";

logMessage(`Servidor escutando na porta: ${port}`)
logError(`Falha no servidor!`)
```


## Autenticação

A autenticação utilizao método Bearer com token JWT para garantir a identidade do requisitante.
A gravação das senhas no banco é criptografa utilizando salt de 64 bits.

## Boas práticas

Práticas recomendadas:

- Evite ao máximo Acoplar componentes! Isso quer dizer, não pode haver dependências de agentes externos à rotina.
- Todas as rotas que tratam dados sensíveis ou de negócio devem ser autenticadas. Para isso temos o middleware de autenticação que valida o token enviado na requisição.
- Para manter a integridade do sistema todos os retornos devem ser padronizados. Para isso temos componentes.
