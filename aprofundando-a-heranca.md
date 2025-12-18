# Aprofundando a Herança

### 🚀 Aprofundando a Herança: Personalizando Comportamentos

Pessoal, até agora vimos que a herança é fantástica para o **reuso de código**, permitindo que `Carro` e `Moto` aproveitem o método `gerarRelatorio()` que foi escrito lá na classe `Veiculo`.

Mas e se o comportamento padrão do "pai" não for suficiente? E se o filho precisar fazer as coisas do jeito dele? É aqui que entram dois conceitos avançados: **Sobrescrita** e **Abstração**.

### 1. Sobrescrita de Métodos (Overriding)

Nas nossas notas atuais, quando chamamos `meuCarro.gerarRelatorio()`, ele executa exatamente o que está escrito na classe `Veiculo`. Mas um carro tem portas e uma moto tem cilindradas. O relatório genérico não mostra isso.

A **Sobrescrita de Métodos** acontece quando a classe filha (Subclasse) recria um método que já existe na classe pai (Superclasse) para dar a ele um comportamento específico.

**Como fazer no JavaScript?**

Basta declarar o método com o **mesmo nome** dentro da classe filha.

```javascript
class Carro extends Veiculo {
    numeroDePortas;

    constructor(modelo, cor, ano, placa, portas) {
        super(modelo, cor, ano, placa); //
        this.numeroDePortas = portas;
    }

    // SOBRESCRITA: Recriamos o método gerarRelatorio
    gerarRelatorio() {
        // Podemos até reaproveitar a lógica do pai usando 'super.metodo()'
        const relatorioBase = super.gerarRelatorio();

        // E adicionamos a especificidade do filho
        return `${relatorioBase} | Portas: ${this.numeroDePortas}`;
    }
}
```

Agora, quando você chamar `meuCarro.gerarRelatorio()`, o JavaScript vai usar a versão "especializada" do `Carro`, e não a genérica do `Veiculo`. Isso é a especialização do comportamento mencionada nas nossas definições de herança.

***

### 2. Métodos Abstratos (Classes Abstratas)

Às vezes, a classe pai sabe que uma tarefa **precisa** ser feita, mas ela é genérica demais para saber **como** fazer.

Imaginem que precisamos calcular o imposto (IPVA) dos veículos.

* O `Veiculo` genérico não sabe calcular imposto, pois a regra muda se for carro, moto ou caminhão.
* Porém, todo `Veiculo` no sistema **precisa** pagar imposto.

Isso é um **Método Abstrato**: um método que é declarado na classe pai (apenas como um "placeholder" ou contrato), mas que **obrigatoriamente** deve ser implementado (sobrescrito) nas classes filhas.

**Implementação no JavaScript**

Diferente de linguagens como Java, o JavaScript não tem a palavra-chave `abstract`. Nós simulamos isso lançando um erro se o método for chamado diretamente da classe pai ou se a filha esquecer de implementá-lo.

**Na Classe Pai (`Veiculo`):**

```javascript
class Veiculo {
    // ... construtor e atributos

    calcularIPVA() {
        // Se a subclasse não sobrescrever este método, o código explode!
        throw new Error("O método calcularIPVA() deve ser implementado pela classe filha.");
    }
}
```

**Na Classe Filha (`Moto`):**

```javascript
class Moto extends Veiculo {
    // ... construtor e atributos

    // A Moto é OBRIGADA a implementar sua própria lógica
    calcularIPVA() {
        // Regra específica da moto (ex: 2% do valor base fictício)
        return 500.00;
    }
}
```

**Por que usar isso?**

Isso garante segurança no sistema. Se você criar um novo tipo de veículo (ex: `Caminhao`) e esquecer de programar o cálculo do imposto, o sistema vai te avisar com um erro, garantindo que a hierarquia de classes funcione corretamente e que todos os comportamentos esperados estejam presentes.

***

**Resumo da Seção:**

* **Sobrescrita:** O filho diz "Eu sei fazer isso melhor que meu pai" e substitui o método.
* **Abstração:** O pai diz "Eu não sei fazer isso, mas meus filhos são obrigados a saber".

***

### 3. O Conceito de Classe Abstrata (O Molde Intocável)

Pessoal, vamos olhar novamente para a nossa hierarquia. As fontes descrevem que a classe `Veiculo` é a **Classe Base** ou a **Classe mais genérica**. Ela serve de molde para criarmos `Carro` e `Moto`.

Mas pensem comigo: No mundo real, você vê um "Veículo" genérico passando na rua? Não. Você vê um Carro, uma Moto, um Caminhão. O "Veículo" é apenas um conceito, uma categoria.

Na programação, chamamos isso de **Classe Abstrata**.

* **Definição:** Uma Classe Abstrata é uma classe que serve **apenas** para ser herdada. Ela é tão genérica que não faz sentido criar um objeto direto dela.
* **A Regra:** Você **não pode** fazer `new Veiculo()`. Você só pode fazer `new Carro()` ou `new Moto()`.

**Por que transformar `Veiculo` em Abstrata?**

Para segurança do sistema. Se alguém tentar criar um `Veiculo` genérico, o sistema deve impedir, pois um veículo sem especificações (sem saber se é carro ou moto) não deveria existir fisicamente no nosso banco de dados.

**Como implementar no JavaScript?**

_(Nota: As fontes focam na estrutura básica, mas aqui vai o truque padrão do JavaScript para impedir a criação da classe pai)_.

Dentro do `constructor` da classe pai, verificamos quem está tentando criar o objeto.

```javascript
class Veiculo {
    constructor(modelo, cor, ano, placa) {
        // "new.target" nos diz qual classe foi chamada com o 'new'
        if (this.constructor === Veiculo) {
            throw new Error("A classe Veiculo é abstrata e não pode ser instanciada diretamente.");
        }

        this.modelo = modelo;
        this.cor = cor;
        this.anoDeFabricacao = ano;
        this.placa = placa;
    }

    // ... restante dos métodos
}
```

**O que acontece agora?**

1. Se você rodar `let v = new Veiculo(...)`: **ERRO!** 🛑 (O sistema protege a regra de negócio).
2. Se você rodar `let c = new Carro(...)`: **SUCESSO!** ✅ (Pois o construtor chamado foi o de Carro, que é uma classe concreta/específica).

Isso reforça o conceito de **Especialização** visto nas fontes, onde as subclasses concretizam o modelo abstrato da superclasse.

***

### 4. Interface: A Abstração Pura

Fala, turma! Vamos dar mais um passo.

Acabamos de ver que a **Classe Abstrata** (`Veiculo`) é um molde genérico. Ela é útil porque oferece **reuso de código**: ela já entrega prontos os atributos `modelo`, `placa` e o método `gerarRelatorio`. Assim, o `Carro` só precisa preencher o que falta.

Mas... e se quisermos criar uma regra para o nosso sistema onde **não existe código para reutilizar**, apenas uma **obrigação** a cumprir?

É aí que nasce a **Interface**.

Podemos entender a Interface baseada no que aprendemos agora:

> **Se uma Classe Abstrata é um "Molde Parcial" (que já vem com paredes e teto), a Interface é apenas a "Planta Baixa" (o desenho técnico).**

**A Diferença Crucial**

1. **Classe Abstrata (`Veiculo`):**
   * Foca em **O QUE É**.
   * **Tem reuso:** O `Carro` herda a lógica de `gerarRelatorio` que já está pronta na classe pai.
   * **Conceito:** "Um Carro _é um_ tipo de Veículo".
2. **Interface (O Contrato):**
   * Foca em **O QUE FAZ**.
   * **Não tem reuso de código:** Ela não tem lógica nenhuma dentro, é vazia. Ela serve apenas para obrigar a classe a ter certos métodos.
   * **Conceito:** "O Carro _assina o contrato_ de ser Segurável".

**Como imaginar isso no nosso código?**

Imaginem uma "Classe Abstrata Extrema" que não tem nenhum atributo (`cor`, `placa`) e nenhum código dentro dos métodos, apenas os nomes deles. Isso é, conceitualmente, uma Interface.

No JavaScript (que usamos nas fontes), não temos a palavra `interface`. Então, simulamos isso criando uma classe onde todos os métodos lançam erros, obrigando quem herda a reescrever tudo.

```
// ISTO É UMA "INTERFACE" CONCEITUAL
// Não guarda dados, apenas define regras de comportamento.
class SeguravelInterface {
    calcularValorSeguro() {
        throw new Error("A classe deve implementar o método calcularValorSeguro()");
    }

    obterVencimentoSeguro() {
        throw new Error("A classe deve implementar o método obterVencimentoSeguro()");
    }
}
```

**Quando usar qual?**

* Use **Classe Abstrata (Herança)** quando seus objetos têm muito em comum (como `Carro` e `Moto` compartilhando `placa` e `ano`).
* Use **Interface** quando objetos totalmente diferentes precisam ter o mesmo comportamento (ex: uma `Casa` e um `Carro` não têm nada em comum para herdar, mas ambos precisam ter o método `calcularValorSeguro`).

***

**Resumo para levar para casa:** A **Classe Abstrata** é um pai que deixa herança (código). A **Interface** é um fiscal que cobra resultados (comportamentos), sem dar nada em troca além da padronização! 🚀
