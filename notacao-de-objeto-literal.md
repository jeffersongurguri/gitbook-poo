# Notação de Objeto Literal

Por exemplos, imagine que estamos desenvolvendo um software que apresenta como uma de suas funcionalidades a **gestão de usuários. Os usuários devem apresentar, necessariamente, as seguintes informações:**

```mermaid
erDiagram
    USUARIO {
        TEXTO nome "Nome completo do usuário"
        INT idade "Idade do usuário"
        TEXTO email "Endereço de email único"
        BOOLEAN ativo "Indica se a conta do usuário está ativa"
    }
```

### Uma abordagem de representação utilizando variáveis escalares

```javascript
// Definindo variáveis escalares para representar um usuário

// Nome do usuário (String)
let nomeUsuario = "Alice";

// Idade do usuário (Número)
let idadeUsuario = 25;

// Email do usuário (String)
let emailUsuario = "alice@exemplo.com";

// Status de atividade da conta do usuário (Booleano)
let usuarioAtivo = true;

```

Embora as variáveis escalares sejam fundamentais e úteis para dados individuais, elas apresentam várias desvantagens quando usadas para representar um conceito complexo como um "usuário".

***

#### Desvantagens da Representação de Usuário com Variáveis Escalares

1. **Dificuldade de Agrupamento e Coesão:**
   * **Perda de Semântica**: As variáveis `nomeUsuario`, `idadeUsuario`, `emailUsuario`, `usuarioAtivo` estão soltas. Embora saibamos que se referem ao mesmo "usuário", o JavaScript não tem um mecanismo intrínseco que as agrupe logicamente. A relação entre elas é apenas por convenção no nome, o que é frágil.
   * **Espalhamento de Dados**: Se você tiver 100 usuários, precisaria de 400 variáveis separadas (100 `nomeUsuario1`, 100 `idadeUsuario1`, etc.), o que é inviável e gera um código extremamente desorganizado.
2. **Dificuldade na Manipulação e Passagem de Dados:**
   * **Passagem de Argumentos em Funções**: Se você precisar passar as informações de um usuário para uma função, teria que passar cada variável escalar como um argumento individual: `function exibirUsuario(nome, idade, email, ativo) { ... }`. Com muitos atributos, a lista de argumentos se torna longa e difícil de gerenciar.
   * **Retorno de Funções**: De forma similar, se uma função precisasse "retornar" um usuário, ela teria que retornar múltiplas variáveis ou um array, o que não é tão elegante ou claro quanto retornar um objeto único.
3. **Maior Risco de Erros e Inconsistências:**
   * Atribuição Incorreta: É muito fácil atribuir o `email` de um usuário à variável de `nome` de outro usuário acidentalmente, pois não há um contêiner que garanta que esses dados permaneçam juntos para uma mesma "entidade".
   * Falta de Tipagem Forte (em outras linguagens): Embora JavaScript seja fracamente tipado, em linguagens com tipagem forte, a falta de uma estrutura de objeto significaria a perda de verificações de tipo que poderiam prevenir erros.
4. **Reutilização de Código Limitada:**
   * **Replicação de Lógica**: Se você precisar de funções para "validar nome", "calcular idade", etc., e essas funções forem aplicadas a diferentes "usuários", você teria que repetir a lógica ou criar funções genéricas que aceitem múltiplos argumentos, o que novamente dificulta a manutenção.
   * **Falta de Encapsulamento**: Não há como associar comportamentos (métodos) diretamente a um conjunto específico de dados. As funções que operam sobre o usuário ficariam "soltas" no código, sem uma relação explícita com os dados que manipulam.
5. **Manutenção e Escalabilidade Complexas:**
   * **Adicionar Novos Atributos**: Se você precisar adicionar um novo atributo (por exemplo, `endereco` ou `dataCadastro`), teria que criar uma nova variável escalar para cada usuário e modificar todas as funções que trabalham com usuários para incluir essa nova variável.
   * **Dificuldade em Modelar Relações**: Se o usuário tiver um "endereço" que por sua vez tem "rua", "cidade", "CEP", representar isso com variáveis escalares se tornaria uma bagunça gigantesca (e.g., `usuario1Rua`, `usuario1Cidade`, `usuario1CEP`).
6. **Código Menos Legível e Intuitivo**:
   * O código que lida com dados de um usuário se torna menos natural de ler e entender, pois a relação lógica entre as variáveis não é imposta pela estrutura da linguagem.

<mark style="background-color:$success;">Em suma, enquanto variáveis escalares são ótimas para dados atômicos, elas falham miseravelmente em fornecer a estrutura, a coesão e a organização necessárias para representar entidades complexas com múltiplos atributos e comportamentos, que é exatamente o problema que os objetos (e, por extensão, a Programação Orientada a Objetos) se propõem a resolver.</mark>

### Primeiras ferramentas de Orientação a Objetos em Javascript

### A Anatomia de um Objeto Literal

A sintaxe é simples e poderosa. Usamos chaves `{}` para definir o objeto.

```javascript
const usuario = {
  // Chave : Valor
  nome: "Alice",
  idade: 25,
  email: "alice@exemplo.com",
  ativo: true
};
```

* `{ }`: As chaves definem o início e o fim do objeto.
* `nome`: É a chave (ou _key_). Geralmente uma string sem aspas, se for um identificador válido.
* `:`: O dois-pontos separa a chave do valor.
* `"Alice"`: É o valor (_value_) associado à chave.
* `,`: A vírgula separa os pares de chave-valor.

***

### Flexibilidade dos Valores

O valor de uma propriedade pode ser de qualquer tipo de dado em JavaScript!

```javascript
const perfilDev = {
  nome: "Beto",
  idade: 32,
  salario: 12345.67,
  ehAdmin: false,
  tecnologias: ["JavaScript", "SQL", "PowerShell"], // Array
  contato: null, // Nulo
  // Objeto aninhado (um objeto dentro de outro)
  endereco: {
    cidade: "Pacatuba",
    estado: "CE"
  }
};
```

***

### Adicionando Comportamentos (Métodos)

Quando o valor de uma propriedade é uma função, chamamos isso de método. Ele define um comportamento para o objeto.

```javascript
const saudacao = {
  mensagem: "Olá, pessoal!",

  // Método (Sintaxe moderna ES6+)
  falar() {
    console.log(this.mensagem);
  },

  // Método (Sintaxe tradicional)
  mudarMensagem: function(novaMensagem) {
    this.mensagem = novaMensagem;
  }
};

saudacao.falar(); // Saída: Olá, pessoal!
saudacao.mudarMensagem("Tudo bem?");
saudacao.falar(); // Saída: Tudo bem?
```

* `this` refere-se ao próprio objeto (`saudacao`).

***

### Acessando Propriedades: Ponto vs. Colchetes

Existem duas formas de ler o valor de uma propriedade.

```javascript
const produto = {
  nome: "Notebook Gamer",
  preco: 5000.00,
  "em-estoque": true // Chave com caracteres especiais
};
```

* 1\. Notação de Ponto (`.`): Mais comum e mais limpa.
  * `console.log(produto.nome); // Saída: Notebook Gamer`
* 2\. Notação de Colchetes (`[]`): Mais poderosa e flexível. A chave deve ser uma string.
  * `console.log(produto["preco"]); // Saída: 5000.00`

***

### Quando Usar a Notação de Colchetes?

A notação de colchetes é obrigatória em duas situações:

1\. Quando a chave contém espaços, hifens ou caracteres especiais:

```javascript
// A notação de ponto falharia aqui: produto.em-estoque
console.log(produto["em-estoque"]); // Saída: true
```

2. Quando a chave é dinâmica (vem de uma variável):

```javascript
let propriedadeDesejada = "nome";
console.log(produto[propriedadeDesejada]); // Saída: Notebook Gamer

propriedadeDesejada = "preco";
console.log(produto[propriedadeDesejada]); // Saída: 5000.00
```

Isso é extremamente útil para acessar propriedades de forma programática.

***

### Modificando Objetos

Objetos literais são dinâmicos. Podemos adicionar, alterar e remover propriedades a qualquer momento.

```javascript
const config = {
  tema: "dark",
  linguagem: "pt-br"
};

// 1. Adicionando uma nova propriedade
config.notificacoes = true;
console.log(config); // { tema: 'dark', linguagem: 'pt-br', notificacoes: true }

// 2. Alterando uma propriedade existente
config.tema = "light";
console.log(config.tema); // Saída: light

// 3. Removendo uma propriedade
delete config.linguagem;
console.log(config); // { tema: 'light', notificacoes: true }
```

***

### Dica Bônus (ES6+): _Shorthand Properties_

Se o nome da variável que você quer atribuir a uma propriedade é o mesmo que o nome da chave, você pode abreviar!

* **Forma tradicional:**

```javascript
const nome = "Guri";
const email = "guri@moodle.com";

const professor = {
  nome: nome,
  email: email
};
```

* **Com&#x20;**_**Shorthand**_**&#x20;(muito mais limpo):**

```javascript
const nome = "Guri";
const email = "guri@moodle.com";

const professor = {
  nome, // Abreviação de nome: nome
  email // Abreviação de email: email
};

console.log(professor.nome); // Saída: Guri
```

***

### Resumo da Aula

* A Notação de Objeto Literal (`{}`) é a forma mais direta de criar objetos em JS.
* As chaves podem ser simples ou strings complexas.
* Os valores podem ser de qualquer tipo, incluindo outros objetos e funções (métodos).
* Use a Notação de Ponto (`.`) para acesso simples e direto.
* Use a Notação de Colchetes (`[]`) para chaves com caracteres especiais ou para acesso dinâmico com variáveis.
* Objetos são mutáveis: podemos adicionar, alterar e deletar propriedades facilmente.

Agora, mãos ao código! Tente criar seus próprios objetos para modelar coisas do seu dia a dia.
