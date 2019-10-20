# npm

Os módulos incluidos no node são muitas vezes insuficientes para a construção de certas aplicações. Diante disso, é necessário importar módulos externos, porém realizar manualmente o download desses módulos é complicado e inseguro, pois eles podem rapidamente ficarem desatualizados. Para facilitar a importação de módulos existe o __npm__ (node package manager), que consiste de uma interface de linha de comando homônima e um banco de pacotes para node, chamado de *npm registry*

## O arquivo package.json

O arquivo central do npm é o `package.json`, que é onde ele guarda as informações principais de sua aplicação/pacote. Um exemplo de `package.json` é o seguinte:

```json
{
    "name": "pacote_exemplo",
    "description": "Pacote que serve de exemplo",
    "version": "1.0.0",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "repository": {
      "type": "git",
      "url": "https://github.com/meu_nick/pacote_exemplo"
    },
    "keywords": ["pacote", "exemplo", "node-npm"],
    "author": "Jane Doe",
    "license": "MIT",
    "dependencies": {
      "pacote1": "1.0.x",
      "pacote2": "2.x"
    },
    "homepage": "https://meu_nick.github.io/pacote_exemplo"
}
```

O processo de criação de um arquivo `package.json` pode ser facilitado ao rodar o comando `npm init`, que fornece uma interface de linha de comando para a criação desse arquivo. 

Dentro desse arquivo deve-se destacar a seção dependencies, que consiste de um objeto com parâmetros nomeados iguais às dependências e cujos valores representam as versões dessas dependências. Essas versões seguem um sistema chamado versionamento semântico.

## Versionamento Semântico

A maioria dos pacotes do *npm registry* seguem um sistema de notação de versões chamado de versionamento semântico. Nesse sistema as versões de um programa são denotadas por 3 números no formato `MAJOR.MINOR.PATCH` que são incrementados dependendo da alteração na versão:

1. Alterações em `PATCH` indicam uma nova versão que apenas conserta erros, sem gerar quebras de compatibilidade nem adição de funcionalidades

2. Alterações em `MINOR` indicam uma nova versão que introduz novas funcionalidades porém sem quebrar a retrocompatibilidade

3. Alterações em `MAJOR` indicam uma nova versão que pode quebrar programas que rodam em `MAJOR`s mais antigas

Podemos selecionar todas as versões que correspendem a uma major/minor/patch release ao substituir o maior dígito a mudar por `x`. e.g.: Todos os patches da versão `1.1` podem ser selecionados ao escrever `1.1.x` e todas as versões sem quebra de compatiblidade pela notação `1.x`. Para selecionar todas as versões, independente do `MAJOR`, ao invés de `x` utilizamos `*`, como em `"pacote": *`, mas isso não é recomendado, pois pode ocorrer quebra de compatibilidade e o seu aplicativo não funcionar mais. 

Mais maneiras de selecionar versões de pacotes estão descritas [na documentação do npm-semver](https://docs.npmjs.com/misc/semver)

## Instalando Pacotes

Uma vez selecionados os pacotes desejados no package.json podemos instalar-los facilmente através do comando `npm install` ou, em notação reduzida, `npm i`. Também podemos passar como argumento um pacote a ser instalado que não esteja no `package.json`. 

Existem diversas flags que podem mudar o funcionamento desse comando:

* `--user` __(default)__: instala o pacote a nível de "usuário", podendo ser acessado apenas pelo aplicativo/pacote

* `--global` ou `-g`: Instala o pacote de maneira global, podendo ser utilizado fora do pacote. Essa opção é interessante para ferramentas de linha de comando instaláveis por npm, como o `tsc` do TypeScript ou o `nodemon`, que permite que o node reinicie com qualquer atualização do código fonte do programa

* `--save-prod` __(default)__: salva o pacote na lista de dependências do arquivo `package.json`

* `--save-dev`: salva o pacote na lista de dependências para desenvolvimento do arquivo `package.json`. Conveniente para ferramentas como transpiladores para JavaScript ou linters, que não são necessários em uma versão final mas são requiridos para a edição do código-fonte

* `--save-optional`: salva o pacote na lista de dependências opcionais. O seu aplicativo/pacote pode então verificar se esse pacote está instalado e disponibilizar suas funcionalidades de acordo.

Pacotes podem ser desinstalados utilizando o comando `npm uninstall`, que também podem receber essas flags para realizar os procedimentos inversos (removendo do `package.json` ou globalmente)
