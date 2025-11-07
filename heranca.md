# Herança

## O Conceito de Herança

Até agora, definimos que uma Classe é uma estrutura que abstrai um conjunto de objetos com características similares. Por exemplo, a classe `Veiculo` possui atributos como `modelo`, `cor`, `anoDeFabricacao` e `placa`, e comportamentos como o método `gerarRelatorio()`.

No entanto, no nosso sistema da seguradora, um `Carro` e uma `Moto` são, fundamentalmente, Veículos.

&#x20;Eles compartilham as características básicas (modelo, cor, placa), mas também possuem características ou comportamentos específicos (um carro tem número de portas, uma moto pode ter a cilindrada).

A **Herança** é o mecanismo da Programação Orientada a Objetos (POO) que permite que uma nova classe (chamada **subclasse** ou **classe filha**) **herde os atributos e métodos** de uma classe existente (chamada **superclasse** ou **classe pai**).

Este conceito se baseia na relação "É-UM":

• Um Carro _É-UM_ `Veiculo`.

• Uma Moto _É-UM_ `Veiculo`.

### A Importância da Herança: Reuso de Código

A vantagem notória da Herança é o reuso de código.

Imagine que a classe `Veiculo` possua dez atributos e cinco métodos (incluindo o construtor e o `gerarRelatorio()`). **Sem a herança**, ao criarmos a classe `Carro` e a classe `Moto`, **teríamos que reescrever todos esses quinze membros em cada nova classe**.

Com a herança, a classe filha (como `Carro`) automaticamente ganha acesso a todos os atributos e métodos definidos na classe pai (`Veiculo`), precisando apenas implementar aquilo que é novo ou específico de sua especialização. Isso torna o código mais limpo, fácil de manter e mais consistente.

### Criação de uma Hierarquia de Classes

A Herança estabelece uma Hierarquia de Classes, onde a classe mais genérica está no topo e as classes mais específicas estão abaixo.

A classe `Veiculo` se torna a Classe Base ou Superclasse. As classes `Carro` e `Moto` se tornam as Classes Derivadas ou Subclasses, **especializando** o comportamento e os dados de `Veiculo`.

\==IMAGEM SOBRE GENERALIZAÇÃO E ESPECIALIZAÇÃO==

\==DIAGRAMA UML==

### **Sintaxe da Herança no JavaScript Moderno**

Para implementar a herança no JavaScript moderno (ES6+), utilizamos a palavra-chave **extends** na declaração da classe filha.

**Utilizando a Classe Base `Veiculo`**
