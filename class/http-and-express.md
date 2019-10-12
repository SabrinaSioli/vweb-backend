# Protocolo HTTP

Agora vamos entrar na explicação de um dos protocolos que regem a web atualmente. O HTTP(**H**yper**T**ext **T**ransfel **P**rotocol) é um protocolo na camada de aplicação do modelo OSI que permite o envio de arquivos HTML, vídeos, imagens, JSON, etc entre computadores. Isso nos diz que existe um cliente que requisita/pede uma informação/ação e então o servidor analisa o pedido e envia a resposta adequada para o cliente.

Analise um site qualquer. Por exemplo, o github. Quando você acessa a página do github, é pedido informações para visualização. Então, quando você digita, [github.com](https://github.com/), é mandado uma página com as informações do github referentes àquela rota.

O protocolo HTTP define um conjunto de métodos de requisição responsáveis por indicar a ação a ser executada para um dado recurso. Os que mais utilizaremos são `GET`, `POST`, `PUT`, `PATCH` e `DELETE`.

| Método | Descrição |
| ------ | --------- |
| `GET`  | Solicita a representação de um recurso específico |
| `POST` | Utiliado para enviar alguma entidade a um recurso específico causando uma mudança de estado ou efeitos colaterais no servidor |
| `PUT`  | Substitui toda a representação de um recurso específico por uma nova representação enviada na requisição |
| `PATCH` | Substitui uma determinada representação de um recurso específico por outro enviado na requisição |
| `DELETE` | Remove um recurso específico |

Para entender melho os outros métodos, acesse o [MDN](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Methods)

**Observação**: Uma resposta HTTP possui um status code que nos permite saber se a resposta para determinada requisição ocorreu da maneira esperada. Seria muito tedioso explicarmos todos o status code do HTTP, por isso deixamos a cargo dos sistes abaixo a explicação dos status HTTP.

[HTTP Dog status](https://httpstatusdogs.com/)
[HTTP Cat status](https://http.cat/)

## Utilizando o protoco HTTP no node.js

Como já explicamos, o node nos permite utiliza muitos protocolos. Não é diferente para o `HTTP`. É preciso apenas importar o módulo `http` para o seu código.

```javascript
const http = require('http');
```

Certo, importamos nosso módulo http sem precisar instalar qualquer coisa via gerenciador de pacotes. Mas só isso não é muito útil para a gente. Então vamos aprender a instanciar o servidor HTTP. A primeira que devemos fazer é criar um servidor.

```javascript
const http = require('http');

const server = http.createServer((request, response) => {

});
```

No código acima, não fazemos nada ainda. Vamos escrever alguma lógica dentro da função que será chamada quando o servidor for criado(Lembre da explicação de event loop). Primeiro vamos retornar um status `200` para o usuário. Depois vamos passar um cabeçalho dizendo que tipo de recurso estamos enviado para ele. No caso, vamos enviar um JSON. Após isso, definimos o recurso que queremos enviar. E por fim, terminamos a nossa resposta.

```javascript
const http = require('http');

const server = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'application/JSON' });
  response.write(JSON.stringify({ msg: 'Hello, world!' }));
  response.end();
});
```

Terminado, certo? Não. Ainda precisamos dizer que o servidor estará sendo escutado na porta 8080.

```javascript
const http = require('http');

const server = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'application/JSON' });
  response.write(JSON.stringify({ msg: 'Hello, world!' }));
  response.end();
});

server.listen(8080, () => {
  console.log('Servidor rodando na porta 8080');
});
```

Agora é so abrirmos nosso navegador e acessar `localhost:8080` Facilmente levantamos um servidor http em node.js.

## Construindo rotas

Nas aplicações da vida real, existem várias rotas que retornam determinada informação. Queremos fazer isso de forma que:

- Ao realizar uma requisição para a rota `/`, seja retornado uma mensagem de `Hello, world!`

- Ao realizar uma requisição para a rota `/dog`, seja retornado um objeto com o nome e a raça do cachorro.

- Ao realizar uma requisição para uma rota diferente das citadas acima, seja retornado uma mensagem de `rota não encontrada`.

A dica é que conseguimos saber qual a rota foi requisitada através da propriedade `url` de `request`. Vamos então construir as rotas da lista acima.

```javascript
const http = require('http');

const server = http.createServer((request, response) => {
  if (request.url == '/') {
    response.writeHead(200, { 'Content-Type': 'application/JSON' });
    response.write(JSON.stringify({ msg: 'Hello, world!' }));
  } else if (request.url == '/dog') {
    response.writeHead(200, { 'Content-Type': 'application/JSON' });
    response.write(JSON.stringify({ type: 'Talvez um doguinho vira-lata caramelo', name: 'zig' }));
  } else {
    response.writeHead(404, { 'Content-Type': 'application/JSON' });
    response.write(JSON.stringify({ msg: `Rota ${request.url} não encontrada` }));
  }

  response.end();
});

server.listen(8080, () => {
  console.log('Servidor rodando na porta 8080');
});
```

Execute o código com `node <nome-arquivo>.js` e veja os resultados em seu navegador acessando `localhost:8080`, `localhost:8080/dog` e `localhost:8080/cat`.

Observe que para um sistema muito grande, pode se tornar inviável manter o código com varios `if ... else if ... else ...`.

# Express.js

