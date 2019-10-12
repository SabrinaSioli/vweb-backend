# Node.js

Antes de começarmos a falar de nodejs, precisamos explicar uma das grandes inovações que ele trouxe. Vamos então rebuscar outras linguagens e como elas funcionam. Em um mundo com python, java ou ruby temos uma arquitetura bloqueante. Mas o que significa isso? Um exemplo claro e concreto que pode ser utilizado para entender isso é o caso de requisições feitas para consumir uma determinada API. No mundo dessas linguagens, cada requisição feita é colocada em uma fila, e então elas são processadas uma a uma até a fila estar vazia. Isso pode criar grandes problemas para a gente quando o número de requisições sobe de 20 para 2000, ou de 2000 para 20000. Veja que assim a palavra escalabilidade foge cada vez mais de nossas aplicações. O que para quem quer desenvolver uma aplicação de pequeno porte pode não ser uma grande preocupação, para quem quer algo mais poderoso utilizando menos recurso pode contar muito. Podemos pensar que uma solução para isso é aumentar o poder de nosso hardware. De fato, isso é uma solução, mas é uma solução custosa.

Portanto Ryan Dahl criou o node.js em 2009 como uma forma de conseguir usar todo o processamento do hardware e diminuir os custos para possuir uma aplicação escalável e poderosa. Além disso, temos acesso a protocolos de baixo nível, o que nos oferece um grande poder para criar aplicações com diferentes propositos.

Node.js é um ambiente de tempo de execução que utiliza a engine javascript V8. A mesma que está por baixo dos navegadores chrome, chromium e agora o microsoft edge. Quem diria que a microsoft mudaria tanto a ponto de estar estudando desenvolver seu navegador para linux também. Observando a definição, podemos concluir que o node.js inclui tudo que é preciso para rodar um programa escrito em javascript. Ou seja, você que achava que javascript só pode ser utilizado no navegador pode parar de pensar assim. Instale o node.js, abra seu terminal, crie um arquivo `index.js`, escreva uma mensagem de `console.log` e digite `$node index.js` e rapidamente conseguirá visualizar a mensagem em seu terminal. Mas claro que explicaremos melhor tudo isso. Então fique tranquilo se não conseguir visualizar ainda esse resultado. Apenas saiba que agora podemos fazer muito com javascript e isso é apenas uma agulha para o que pode ser feito com essa linguagem.

## Instalando o node.js

Em nosso curso, obviamente, as máquinas do laboratório tem node.js instalado. Mas caso você queira praticar em seu computador os conhecimento adquiridos, abaixo nós expliaremos como instalar.

Caso você seja um usuário windows, nada precisamos ensinar. Basta você entrar no site [https://nodejs.org/en/](https://nodejs.org/en/) e clicar no botão de download. Ao terminar de baixar, execute o `.exe` e next, next, next.

Mas se você é um usuário linux, o primeiro passo a se fazer é baixar o **nvm** via terminal.

```shellscript
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash
```

Após isso, reinicie seu terminal e teste se tudo deu certo digitando `nvm`. Feito isso, podemo instalar a versão que quisermos para o nosso computador com apenas um comando.

```shellscript
$ nvm install <node-version>
```

Para trocar de vesões instaladas:

```shellscript
$nvm use <node-version>
```

Quando usado um desses comandos é usado, é instalado o gerenciador de pacotes do node conhecido como npm(node packge manager).

Para testar se tudo está correto, digite `node --version` e `npm --version`.

**Observação**: O nvm é um gerenciador de versões do node. O que ele faz é bem explicativo então nos aprofundaremos mais na ferramenta. Caso tenha alguma curiosidade, visite o [repositório no github](https://github.com/nvm-sh/nvm) da tecnologia.

## Criando o Hello World

Agora que temos o node.js instalado em nossos computadores, como vamos utilizá-lo? Vamos então criar um arquivo chamado `index.js`. Depois vamos abrir ele em nosso editor de textos preferido(VSCode, Sublime, Atom, etc) e digitar o seguinte código:

```javascript
console.log("Hello, world!");
```

Então em seu terminal, **no diretório que foi criado o arquivo**, digite:

```shellscript
$ node index.js
```

e veja

```
Hello, world!
```

## Melhorando nosso Hello world

Beleza, mas imprimir um `hello, world!` na tela parece algo bem trivial e até chato comparado ao que podemos fazer com o node.js. Vamos então melhorar essa nossa aplicação criando um arquivo txt via código javascript e escrevendo nesse arquivo a mensagem de `Hello, world!`.

Para fazer isso, utilizamos o módulo `fs` que por padrão, não precisa ser instalado via gerenciador de pacotes. Opa, então como utilizamos ele? Se você vem de python, temos uma palavra-chave responsável por importar as bibliotecas que queremos usar. Em node.js, usamos a função require com um único argumento, que no caso, é o nome da biblioteca que queremos importar. Passamos o valor para uma variavel que agora representará o objeto importado da biblioteca. Veja o código abaixo.

```javascript
const fs = require('fs');
```

Tudo bem, agora vamos criar nosso arquivo com a função `writeFile`.

```javascript

fs.writeFile('myText.txt', 'Hello, world!', (err) => {
  if(err) {
    console.log('Something is wrong');
  } else {
    console.log('Done!');
  }
});
```

Veja que o arquivo foi criado com o texto `Hello, world!`. Se quisermos ler o arquivo e visualizar o texto dentro do arquivo, utilizamos a função `readFile`.

```javascript
fs.readFile('myText.txt', (err, buffer) => {
  if(err) {
    console.log('Something is wrong!');
  } else {
    console.log(buffer.toString());
  }
});
```

Uma solução mais sofisticada é antes de buscar ler o arquivo, ver se ele existe. para fazer isso, usamos a função `statSync`.

```javascript
const fileStats = fs.statSync('myText.txt');

if(fileStats.isFile()) {
  fs.readFile('myText.txt', (err, buffer) => {
    if(err) {
      console.log('Something is wrong!');
    } else {
      console.log(buffer.toString());
    }
  });
}
```

Veja que tudo isso é código javascript. E melhor ainda, não estamos fazendo isso em um browser, estamos realizando funções do sistema como por exemplo, criar, escrever e ler arquivos.

## Pegando input do terminal

Vamos pensar de novo no mundo de outras linguagens. Mais especificamente em Python. Quando você queria capturar uma esrita feita no terminal pelo usuário, era digitado o seguinte código:

```python
x = input('Digite um número: ')
print(x)
```

Ao executar, seria visualizado no terminal da forma como mostramos abaixo:

```shellscript
$ Digite um número: 
```

```shellscript
$ Digite um número: 2
```

```shellscript
$ Digite um número: 2
2
```

Em node.js, podemos fazer o mesmo utilizando o módulo `readline`. Lembre que ele é um módulo Built in do node.js, logo, não é necessário fazer uma instalação via gerenciador de pacotes. O que precisamos é apenas importar o módulo.

```javascript
const readline = require('readline');
```

Antes de começarmos a lógica do programa, precisamos criar uma interface com entrada e saída.

```javascript
const readline = require('readline');

const rlInterface = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
```

Então, criamos uma pergunta ou solicitação de informação para o usuário

```javascript
const readline = require('readline');

const rlInterface = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rlInterface.question('Digite um número: ', (n) => {
  console.log(n);
  rlInterface.close();
});
```

Agora tente fazer um programa que recebe uma mensagem do usuário e grava em um arquivo `txt`.

## Single-thread

Um programa é, de forma direta, um conjunto de instruções com um ou vários propósitos e nada mais do que isso. Um processo, dizemos que é um programa em execução. Quando um processo é criado, surge uma thread. E no decorrer de sua execução, podem ser criadas mais de uma thread para o mesmo processo. Uma Thread é a menor unidade de execução que um processador aloca tempo.

Agora analise a situação dentro do nosso contexto de node.js. Suponha que criamos um programa para ler um arquivo. Quando o executamos, criamos um processo de uma única thread. Ora, você pode pensar agora que podemos criar várias outras thread a partir dessa. Na verdade, a resposta é possívelmente não. Node.js é single-thread, ou seja existe apenas uma tarefa naquele processo.

Um questionamento que deixamos é:

> Node.js tem 10 anos, logo, já é possível criar aplicações node.js com múltiplas threads? Existe alguma tecnologia criada nesse tempo que nos permita fazer isso? 

## Event loop

Quando trabalhamos com servidores, é fácil notar que estamos sujeitos a vários eventos. Seja uma conexão aberta com um banco de dados, uma alteração em um arquivo, uma requisição por determinada informação. Node.js é uma tecnologia orientada a eventos, assim como javascript no client-side tem suas carecteristicas de orientação a eventos, node.js também tem a sua, porém não esta associado a eventos de clique, movimentação do mouse, mas sim a eventos de entrada e saída do servidor. Certo, mas o que isso tem de tão importante?

O event loop é um agente responsável por escutar se os eventos registrados foram completados. Ele é, de forma vaga, um loop infinito que fica verificando se um determinado evento registado ocorreu. Caso tenha ocorrido, enviamos uma função atrelada ao fim do evento para uma fila. Por exemplo, analise o código de criar um arquivo.

```javascript

const fs = require('fs');

fs.writeFile('index.html', data, (err) => {
  console.log('Hello');
});
```

Veja que estamos registrando um evento de criação de arquivo. Logo o event loop ficará escutando se o arquivo foi criado. Quando o arquivo é criado, ele manda a função `(err) => { console.log('Hello') }` para uma fila de execução.

Como já foi explicado, node.js tem arquitetura não bloqueante. Então, ele não vai esperar o evento terminar para executar qualquer código posterior a ele. Por exemplo:

```javascript

const fs = require('fs');

fs.writeFile('index.html', data, (err) => {
  console.log('Feito');
});

console.log('Você gosta de café? :D');
console.log('...');
console.log('...');
console.log('...');

```

Vamos imaginar que esse código é um diálogo entre o senhor Event Isaac Loop e o senhor John JavaScript. Observe:

John JavaScript: Olá, sir. Eu gostária de criar um arquivo chamado `index.html`.

Event Isaac Loop: Tudo bem sir. Vamos criar o seu arquivo. Mas enquanto não está feito, vamos conversar. `Você gosta de café?...`

Algum tempo depois...

Event Isaac Loop: Senhor, o arquivo que o senhor solicitou está `Feito`.

Observe que o código não foi bloqueado em nenhum momento, após a solicitação para criar o arquivo, ele continuou o código ou, como na nossa analogia, a conversa.

[Próxima sessão](./)