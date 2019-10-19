# JavaScript

## Sintaxe

O JavaScript não é uma linguagem que procurar reinventar a programação. Consequentemente sua sintaxe é familiar para alguem que já tenha trabalhado com linguagens como C, C++ ou Java

```javascript
function buscaBinaria(vetor, inicio, fim, k){
    if(inicio > fim) return -1;

    let mediana = Math.floor((inicio + fim)/2);

    if(vetor[mediana] == k) return mediana;
    if(vetor[mediana] < k) return buscaBinaria(vetor, mediana, fim, k);
    if(k < vetor[mediana]) return buscaBinaria(vetor, inicio, mediana, k);
}
```

No entando o JavaScript também tem suas particularidades e abstrações, pensadas especialmente para o desenvolvimento Web. Em destaque estão:

* Orientação a Eventos

* Tipagem Dinâmica e Fraca

* `var`, `let` e `const`

* Orientação a Objetos

* Anonymous & Callback Functions

* Controle de erros

---

## Orientação a Eventos

A característica que torna o JavaScript extremamente versatil para o desenvolvimento para a web é sua orientação a eventos. Mas o que é essa orientação a eventos?

A orientação a eventos é um paradigma onde o caminho da execução do programa é controlada por eventos. Esses eventos podem ser entradas de I/O, como cliques de mouse (que é um evento comum no desenvolvimento front-end) ou, no nosso caso, o recebimento de uma requisição HTTP

## Tipagem

A tipagem do JavaScript é classificada como dinâmica e fraca. O que isso significa?

1. A tipagem dinâmica significa que uma variável não é coagida a um tipo, podendo assumir diferentes tipos em diferentes locais do programa. Ou seja, uma variável pode ser instanciada como numérica (`let x = 3`) e posteriormente alterada para textual (`x = "Lorem Ipsum"`)

2. A tipagem fraca por sua vez significa que o programa é capaz de realizar a conversão de tipos implicitamente. Isto é, podemos realizar a soma de 5 com "Lisp" (`5 + "Lisp"`) e receber uma string "5Lisp". Em linguagems com tipagens mais fortes a operação retornaria um erro.

## Declaração de valores com `var`, `let` e `const`

Enquanto a declaração de uma variável no JavaScript não é complexa como em Java ou C, onde é necessário escolher o tipo entre dezenas, ainda existem 3 diferentes maneiras de declarar um valor, cada uma com suas particularidades:

1.  `var`: O metodo padrão de declarar uma variável, não limitado em escopo ou alteração

2. `const`: Uma constante. O seu valor não pode ser mudado, sendo qualquer tentativa de alteração de seu valor resultante em um erro

3. `let`: Uma adição recente ao JavaScript, o `let` se diferencia do `var` por respeitar melhor os escopos do programa. Isso significa que se uma variável for declarada com `let` dentro de uma função, iteração(`for`) ou estrutura de controle (`if`) ela não exisitirá fora desse escopo. 

## Orientação a Objetos

O JavaScript, assim como diversas linguagens modernas é orientado a objetos. Objetos são abstrações, compostas de 2 tipos de coisas: propriedades e métodos.

Propriedades são valores, como variáveis e constantes, enquanto métodos são como funções atreladas a um objeto. Os objetos da programação são análogos aos da vida real. Um carro por exemplo, pode ser representado por um objeto

```javascript
{
    cor: "#FFFFFF",
    posicao: 0,
    escada_no_teto: true, // Propriedades
    acelerar: function(){
        if(escada_no_teto) return this.posicao += 2;
        return this.posicao++;
    } // Método
}
```

## Anonymous & Callback Functions

Funções em JavaScript não requerem nome e podem receber como parâmetro outras funções.

Funções sem nomes são chamadas de funções anônimas, e podem ser declaradas ao simplesmente não receberem nome tampouco serem atreladas a uma variável, no estilo `function(params){ /*corpo*/ }`. Enquanto essa declaração não faz sentido sozinha no código, ela é extremamente interessante para callback functions.

Callback functions são funções passadas como parâmetros de outras funções, podendo serem usadas ao longo da função "pai". Um exemplo muito comum do uso de callback functions é o metodo `map` de arrays. Esse método executa a função passada como parâmetro em cada elemento do array.  Nesse sentido, para evitar declarações desnecessária de variáveis, **anonymous functions** se mostram convenientes.

Uma maneira mais simples de declara funções anônimas é através da sintaxe `(params) => { /*corpo*/ }`, que permite uma declaração *inline* e curta.

## Controle de erros

Uma característica potente de linguagens modernas como JavaScript é o controle de erros. Erros são inevitaveis, e é importante saber como tratá-los. Muitas vezes é desejável cuidar de um erro fora do escopo em que esse erro ocorreu. Diante disso temos o `throw`, que permite que o erro "pule" de escopo.

Suponha uma função que recebe apenas strings. Caso ela receba algo diferente de uma string podemos querer diferentes ações dependendo do contexto. Podemos então fazer o seguinte:

```javascript
function funcao_que_recebe_strings(variavel){
    if(typeof(variavel) != String){
        throw "Tipo inaceitável"
    }
}
```

Podemos rodar essa função dentro de uma estrutura `try {} catch(err) {}` para poder tratar esse erro.

```javascript
try {
    funcao_que_recebe_strings(variavel)
} catch(erro) {
    console.error(erro); // Imprime "Tipo inaceitável"
}
```

Esse "empurrão com a barriga" do erro pode ser bastante conveniente para operações que possam dar erros inesperados, como comunicações da web e acessos a bancos de dados


