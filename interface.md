# Interface

## 📜 Aula: Interfaces e Contratos de Código

Fala, turma!

Nas últimas aulas, ficamos craques em **Herança**. Aprendemos que o `Carro` **é um** `Veiculo` e, por isso, ele herda automaticamente códigos como o método `gerarRelatorio()`.

Mas e se precisarmos garantir que objetos completamente diferentes tenham um comportamento em comum, sem que um seja "pai" do outro?

Hoje vamos falar sobre **Interfaces**. Se a Herança é sobre "**Parentesco**", a Interface é sobre "**Contrato**".

***

### 1. O Problema: Nem tudo é "Pai e Filho"

Vamos olhar para o nosso sistema da seguradora. Temos a classe `Veiculo` e suas filhas `Carro` e `Moto`. Imagine que agora a seguradora queira começar a fazer seguros de **Imóveis** (Casas).

* Uma `Casa` **não é um** `Veiculo`. Não faria sentido herdar de `Veiculo` só para aproveitar código.
* Porém, tanto `Carro` quanto `Casa` precisam ser segurados. Ou seja, ambos precisam ter um método, digamos, `calcularValorSeguro()`.

Como garantimos que o programador não esqueça de criar esse método na classe `Casa`? É aqui que entra o conceito de **Interface**.

***

### 2. O Conceito: O que é uma Interface?

Uma **Interface** define um **contrato**. Ela não diz _como_ fazer, ela diz _o que_ deve ser feito.

> **Analogia da Tomada:** Pense em uma tomada elétrica. A "Interface" são os dois (ou três) buracos na parede. Não importa se você liga uma geladeira, um videogame ou um aspirador de pó (classes diferentes). Se o aparelho tiver o plugue no padrão do contrato (a interface), ele vai funcionar.

Na POO, uma interface obriga a classe a ter determinados métodos.

***

### 3. Interfaces no JavaScript (Duck Typing)

Aqui temos um detalhe técnico importante para vocês que são programadores Web: O JavaScript puro (que estamos usando nas fontes) **não possui** a palavra-chave `interface` como Java ou TypeScript.

No JavaScript, usamos o conceito de **Duck Typing** (Tipagem de Pato):

> "Se anda como um pato e faz quack como um pato, então é um pato."

Isso significa que, se o objeto tiver o método que precisamos, o JavaScript aceita. A "Interface" no JS é garantir que o objeto tenha a "forma" correta.

***

### 4. Implementando o Contrato na Prática

Vamos imaginar um contrato chamado `Seguravel`. Qualquer coisa que for `Seguravel` **precisa** ter o método `calcularValorSeguro()`.

Não usamos `extends`. Nós apenas implementamos o método.

#### Classe Carro (Cumprindo o contrato)

Já temos o `Carro` que herda de `Veiculo`. Vamos adicionar a implementação do "contrato".

```js
class Carro extends Veiculo {
    numeroDePortas;

    constructor(modelo, cor, ano, placa, portas) {
        super(modelo, cor, ano, placa); // Inicializa a parte Veiculo
        this.numeroDePortas = portas;
    }

    // Método herdado de Veiculo
    // gerarRelatorio() { ... }

    // --- IMPLEMENTAÇÃO DA INTERFACE "SEGURAVEL" ---
    // O contrato diz: Precisa ter este método!
    calcularValorSeguro() {
        // Lógica: Carros novos são mais caros
        return (2025 - this.anoDeFabricacao) * 100;
    }
}
```

#### Classe Casa (Cumprindo o contrato)

A `Casa` não tem nada a ver com `Veiculo`. Mas ela também assina o contrato `Seguravel`.

```javascript
class Casa {
    endereco;
    metrosQuadrados;

    constructor(endereco, m2) {
        this.endereco = endereco;
        this.metrosQuadrados = m2;
    }

    // --- IMPLEMENTAÇÃO DA INTERFACE "SEGURAVEL" ---
    // O nome do método deve ser IDÊNTICO ao do Carro
    calcularValorSeguro() {
        // Lógica: Casas maiores são mais caras
        return this.metrosQuadrados * 50;
    }
}
```

***

### 5. Por que isso é útil? (Polimorfismo)

Agora vem a mágica. Podemos criar uma função que aceita **qualquer coisa** que siga a interface `Seguravel`. O sistema não precisa saber se é carro ou casa, ele só precisa saber se tem o método.

```js
// Função que processa seguros
function processarSeguro(item) {
    // A função confia que o item tem o método 'calcularValorSeguro'
    console.log("Calculando proposta...");
    console.log(`Valor: R$ ${item.calcularValorSeguro()}`);
}

let meuCarro = new Carro("Corsa", "Prata", 2010, "ABC", 4);
let minhaCasa = new Casa("Rua dos Bobos, 0", 100);

// Funciona para os dois, pois ambos respeitam a "Interface"!
processarSeguro(meuCarro);
processarSeguro(minhaCasa);
```

***

### Resumo Comparativo

Para não confundir com o que vimos nas notas anteriores:

| Conceito      | Pergunta Chave          | No JavaScript                                                           |
| ------------- | ----------------------- | ----------------------------------------------------------------------- |
| **Herança**   | O que o objeto **É**?   | Usa `extends`. Herda código pronto (reuso).                             |
| **Interface** | O que o objeto **FAZ**? | É um acordo de cavalheiros. O objeto _precisa_ ter o método específico. |

**Dica de Ouro:** Use Herança quando quiser reaproveitar código interno (como `modelo` e `cor` de `Veiculo`). Use Interfaces (padronização de métodos) quando quiser que objetos diferentes sejam tratados da mesma forma pelo sistema.

Pratiquem adicionando um método `exibirProprietario()` tanto em `Carro` quanto em `Casa` e vejam a mágica acontecer! 🚀
