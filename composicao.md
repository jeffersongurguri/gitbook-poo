# Composição

Na última aula, exploramos a Herança, onde aprendemos que o `Carro` é uma versão especializada do `Veiculo`. Vimos que isso cria uma relação "É-UM" (Carro _É-UM_ Veículo).

Nesta página, vamos falar sobre outra forma poderosa de reutilizar código e modelar o mundo real: a Composição.

Enquanto a Herança foca no que o objeto **É**, a Composição foca no que o objeto **TEM** (ou do que ele é feito).

• Herança: Relação _**É-UM**_ (Ex: Moto é um Veículo).

• Composição: Relação _TEM-UM_ (Ex: Veículo _tem um_ Motor).

Na Composição, construímos objetos complexos juntando objetos menores e mais simples, como peças de LEGO.

**Por que usar Composição?**

Nas notas sobre Herança, vimos que a vantagem principal é o reuso de código para evitar reescrever atributos comuns. A Composição também oferece reuso, mas com mais flexibilidade.

Imagine que queremos detalhar o motor dos nossos veículos. Poderíamos criar subclasses como `VeiculoMotorV8` ou `VeiculoEletrico`, mas isso criaria uma hierarquia gigante e confusa. Em vez disso, criamos uma classe `Motor` separada e fazemos com que o `Veiculo` utilize essa classe. Isso é uma associação de Classes, levada a um nível estrutural.

**Implementando a Composição**

Vamos usar o mesmo cenário da seguradora. Em vez de colocar os detalhes do motor soltos dentro da classe `Veiculo`, vamos criar uma classe dedicada.

1\. Criando a "Peça" Motor (Classe Componente)

Primeiro, definimos a classe que será parte do todo.

```javascript
class Motor {
    tipo;     // ex: "Elétrico", "Combustão"
    potencia; // ex: "150cv"

    constructor(tipo, potencia) {
        this.tipo = tipo;
        this.potencia = potencia;
    }

    ligar() {
        return `Motor ${this.tipo} de ${this.potencia} fazendo barulho!`;
    }
}
```

2\. Criando o "Todo" (Classe Composta)

Agora, atualizamos nossa classe `Veiculo`. Em vez de herdar características, ela vai conter um objeto.

```js
class Veiculo {
    modelo;
    cor;
    placa;
    motor; // NOVO: Este atributo vai guardar um OBJETO inteiro

    // O construtor recebe o objeto motor como parâmetro
    constructor(modelo, cor, placa, motorRecebido) {
        this.modelo = modelo;
        this.cor = cor;
        this.placa = placa;
        
        // COMPOSIÇÃO: O veículo "guarda" o motor dentro de si
        this.motor = motorRecebido; 
    }

    // Podemos usar o método do objeto motor
    acionar() {
        return `${this.modelo} diz: ${this.motor.ligar()}`;
    }
}
```

**Herança vs. Composição: O Comparativo**

Para não confundir as coisas, vamos colocar lado a lado com o que aprendemos na aula passada:

| Característica                    | Herança (Aula Anterior)                                                                                                                      | Composição (Aula de Hoje)                                                                                                                                             |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Palavra-chave                     | `extends` e `super()`                                                                                                                        | `new` (instanciação) e atribuição via `this`                                                                                                                          |
| Relação                           | É-UM (Subclasse é um tipo da Superclasse)                                                                                                    | TEM-UM (Classe tem uma instância de outra)                                                                                                                            |
| Objetivo                          | Especializar comportamento (criar uma versão mais específica)                                                                                | Montar objetos complexos usando partes menores                                                                                                                        |
| Flexibilidade (Momento) :warning: | Tempo de Programação (Estático): Definido no código. Um `Carro` sempre será filho de `Veiculo` a menos que reescrevamos o arquivo da classe. | Tempo de Execução (Dinâmico): Definido ao rodar. Podemos trocar o `Motor` que passamos para o `Veiculo` a cada nova criação de objeto, sem mexer no código da classe. |
| Exemplo                           | `Carro extends Veiculo`                                                                                                                      | `Veiculo` possui um atributo `motor`                                                                                                                                  |

**Testando a Composição**

Assim como testamos a hierarquia instanciando um `Carro`, vamos ver como instanciar um objeto composto. O processo acontece em duas etapas: fabricar a peça e depois montar o veículo.

```
// 1. Criamos a peça (Instância de Motor)
let motorPotente = new Motor("V8", "500cv");

// 2. Criamos o veículo passando a peça
let meuCarro = new Veiculo("Mustang", "Azul", "ABC-1234", motorPotente);

// Testando o acesso
console.log(meuCarro.acionar());
// Saída: Mustang diz: Motor V8 de 500cv fazendo barulho!
```

A Composição nos permite **mudar as "peças" do nosso software sem quebrar o sistema todo**. Se precisarmos mudar como o `Motor` funciona, não precisamos mexer na classe `Veiculo` ou na classe `Carro`. Cada classe cuida da sua responsabilidade!
