# Guia de Sintaxe da Linguagem Java

> Guia objetivo para aprender a sintaxe de Java e escrever programas simples. Feito para quem já estudou lógica de programação e a sintaxe de C — várias comparações diretas com C ao longo do texto.

---

## 1. O que é Java e por que ele importa

Java nasceu em **1995**, na Sun Microsystems (hoje Oracle), com o lema **"Write Once, Run Anywhere"** (escreva uma vez, rode em qualquer lugar). É a resposta direta ao maior incômodo de C: em C, você compila para uma plataforma específica; em Java, você compila para uma **máquina virtual** (JVM) que roda igual em Windows, Linux, Mac.

### Como Java executa (diferente de C)

| Etapa | C | Java |
|---|---|---|
| 1 | Código-fonte `.c` | Código-fonte `.java` |
| 2 | Compilador (`gcc`) gera binário nativo da plataforma | Compilador (`javac`) gera **bytecode** `.class` (não é binário de máquina) |
| 3 | Sistema operacional roda o binário diretamente | A **JVM** (Java Virtual Machine) interpreta/compila o bytecode em tempo real para a máquina atual |

Isso tem um custo (Java geralmente é mais lento que C em tarefas pesadas) e um ganho enorme (portabilidade e segurança de memória).

### Características principais

| Característica | O que significa na prática |
|---|---|
| **Orientada a Objetos (quase) pura** | Tudo (exceto tipos primitivos) é objeto. Não existe função "solta" fora de uma classe, diferente de C. |
| **Coleta de lixo automática (Garbage Collector)** | Você **não** chama `free()`. A JVM detecta objetos sem uso e libera a memória sozinha. |
| **Tipagem estática e forte** | Igual C: você declara o tipo e ele não muda. Mas Java é mais rígido — não permite as conversões "soltas" que C permite. |
| **Sem ponteiros explícitos** | Você não manipula endereço de memória diretamente. Existem **referências**, que se comportam parecido, mas de forma protegida (não dá pra fazer aritmética de ponteiro). |
| **Portável (bytecode + JVM)** | Compila uma vez, roda em qualquer SO que tenha JVM instalada. |
| **Gerenciamento de exceções (`try/catch`)** | Diferente de C, que não tem tratamento de erro nativo, Java tem um sistema robusto de exceções. |
| **Biblioteca padrão gigante** | Strings prontas, coleções dinâmicas (`ArrayList`, `HashMap`), threads, rede — tudo que C não tem nativo. |

### Java vs C — as diferenças que mais importam pra você agora

| C | Java |
|---|---|
| Procedural (funções soltas) | Orientado a Objetos (tudo dentro de classes) |
| Você gerencia memória (`malloc`/`free`) | Garbage Collector gerencia sozinho |
| Ponteiros explícitos, `&` e `*` | Referências implícitas, sem aritmética de endereço |
| Sem tratamento de erro nativo | `try/catch/finally` para exceções |
| Array de tamanho fixo, sem verificação de limite | Array fixo, MAS lança exceção se você acessar índice inválido |
| String = vetor de `char` + `\0` | `String` é uma classe pronta, com dezenas de métodos |
| Compila para binário da plataforma | Compila para bytecode, roda na JVM |
| Sem coleções dinâmicas nativas | `ArrayList`, `HashMap`, `HashSet` prontos na biblioteca padrão |
| Você escreve `struct` para agrupar dados | Você escreve `class` — que agrupa dados E comportamento (métodos) |

**Resumo prático:** tudo que era trabalho manual e perigoso em C (memória, limites de array, agrupar dado+comportamento) Java resolve por padrão, trocando controle fino por produtividade e segurança.

### Onde Java é usado hoje (dinâmica de uso no mercado)

- **Back-end corporativo**: é uma das linguagens mais usadas em sistemas bancários, seguradoras, governo, grandes plataformas — normalmente com o framework **Spring / Spring Boot**.
- **Android**: historicamente a linguagem principal para apps Android (hoje dividindo espaço com Kotlin).
- **Sistemas legados e de grande porte**: muita infraestrutura corporativa antiga (e ainda ativa) é Java.
- **APIs REST, microsserviços**: exatamente o seu foco declarado de back-end — Java com Spring Boot é uma das combinações mais comuns do mercado para isso.

---

## 2. Estrutura mínima de um programa em Java

```java
public class Main {                    // TUDO em Java vive dentro de uma classe
    public static void main(String[] args) {   // ponto de entrada do programa
        System.out.println("Ola, mundo!");
    }
}
```

**Comparando com C, linha a linha:**

- Não existe `#include`. Java usa `import` (você vai ver isso quando precisar de classes fora do básico, ex: `import java.util.Scanner;`).
- `public class Main` — o nome do arquivo **precisa** ser `Main.java` (o nome da classe pública tem que bater com o nome do arquivo — regra que C não tem).
- `public static void main(String[] args)` é o equivalente ao `int main(void)` de C, mas com mais peças:
  - `public` — visível de qualquer lugar (mais sobre isso na seção de modificadores de acesso).
  - `static` — o método pertence à classe, não a um objeto específico (você roda sem precisar "criar" um `Main`).
  - `void` — não retorna nada (diferente de C, onde `main` retorna `int`).
  - `String[] args` — vetor de argumentos passados na linha de comando (equivalente a `argv` em C, mas C também tem `argc` separado; em Java, o tamanho do vetor já basta).
- `System.out.println(...)` — o "printf" do Java. `System` é uma classe, `out` é um objeto de saída dentro dela, `println` é um método. Note o ponto (`.`) encadeado — isso é sintaxe de orientação a objetos, algo que C não tem.

### Compilando e rodando

```bash
javac Main.java     # compila .java -> gera Main.class (bytecode)
java Main            # roda o bytecode na JVM (repare: sem ".class" no comando)
```

Diferente de C, onde `gcc` gera um executável nativo pronto pra rodar sozinho, em Java você sempre precisa da JVM instalada na máquina que for rodar.

---

## 3. Comentários

```java
// comentário de uma linha

/* comentário
   de várias linhas */

/**
 * Comentário de documentação (Javadoc) — gera documentação automática.
 * @param nome descrição do parâmetro
 */
```

Os dois primeiros são idênticos a C. O terceiro (Javadoc) não existe em C — é uma convenção de documentação própria do ecossistema Java.

---

## 4. Tipos de dados

Java separa claramente **tipos primitivos** (como em C) de **tipos referência** (objetos).

### Tipos primitivos

| Tipo | Guarda | Tamanho | Exemplo |
|---|---|---|---|
| `int` | inteiro | 4 bytes | `int idade = 25;` |
| `long` | inteiro grande | 8 bytes | `long populacao = 8000000000L;` |
| `short` | inteiro pequeno | 2 bytes | `short ano = 2026;` |
| `byte` | inteiro bem pequeno | 1 byte | `byte idade = 30;` |
| `double` | decimal (precisão dupla) | 8 bytes | `double preco = 9.99;` |
| `float` | decimal (precisão simples) | 4 bytes | `float peso = 70.5f;` |
| `char` | um caractere **Unicode** | 2 bytes | `char letra = 'A';` |
| `boolean` | verdadeiro/falso | 1 bit (na prática) | `boolean ativo = true;` |

**Diferenças importantes em relação a C:**

- `boolean` é **nativo** em Java (`true`/`false` de verdade). Em C você precisava incluir `stdbool.h` e mesmo assim era simulado com `int`.
- O **tamanho de cada tipo é garantido e igual em qualquer plataforma** (um `int` em Java é sempre 4 bytes, seja no Windows, Linux ou Mac). Em C, o tamanho pode variar dependendo do compilador/sistema.
- `char` em Java tem 2 bytes (Unicode/UTF-16), porque pensa em internacionalização desde o início. Em C, `char` tem 1 byte (ASCII/Latin).
- **Não existe `unsigned`** em Java. Todo número com sinal.

### Tipos referência (objetos)

Qualquer coisa que não seja um dos 8 primitivos acima é um **objeto**, incluindo `String`, arrays, e qualquer classe que você criar.

```java
int idade = 25;              // primitivo - guarda o VALOR diretamente
String nome = "Vinicius";    // referência - a variável guarda um "endereço" (gerenciado) para o objeto String
```

Isso é conceitualmente parecido com ponteiro de C, mas **Java não deixa você manipular esse endereço diretamente** — não existe `&nome` nem aritmética de endereço. É uma referência "protegida".

```java
public class Main {
    public static void main(String[] args) {
        int idade = 30;
        double altura = 1.75;
        char inicial = 'V';
        boolean ativo = true;
        String nome = "Vinicius";

        System.out.println("Idade: " + idade);
        System.out.printf("Altura: %.2f%n", altura);
        System.out.println("Inicial: " + inicial);
        System.out.println("Ativo: " + ativo);
        System.out.println("Nome: " + nome);
    }
}
```

Repare: `System.out.println` usa `+` para **concatenar** texto com outros tipos (converte automaticamente). Não existe algo como os especificadores `%d`/`%s` do `printf` de C aqui — embora `System.out.printf` também exista em Java, com sintaxe quase idêntica ao `printf` de C, se você preferir esse estilo.

---

## 5. Variáveis — declaração e convenções

```java
int idade;          // declaracao
idade = 25;          // atribuicao

int altura = 180;    // declaracao + inicializacao (recomendado)

int a = 1, b = 2, c = 3;   // multiplas variaveis, igual C

final double PI = 3.14159;  // "final" e o equivalente do "const" de C - nao pode ser reatribuido
```

**Convenção de nomenclatura em Java (diferente de C):**
- Variáveis e métodos: `camelCase` (`nomeCompleto`, `calcularMedia()`) — é convenção forte, quase regra não escrita.
- Classes: `PascalCase` (`ContaBancaria`, `Pessoa`).
- Constantes: `MAIUSCULO_COM_UNDERSCORE` (`VELOCIDADE_MAXIMA`).

Em C a comunidade tende a `snake_case`; em Java, praticamente todo código segue `camelCase`/`PascalCase` — isso é bom você já fixar porque vai facilitar ler código de terceiros.

---

## 6. Entrada e saída

Java não tem `scanf`. Para ler dados do usuário, usa-se a classe `Scanner`.

```java
import java.util.Scanner;   // precisa importar, diferente de println que já vem pronto

public class Main {
    public static void main(String[] args) {
        Scanner leitor = new Scanner(System.in);   // "new" cria um objeto - conceito que C nao tem

        System.out.print("Digite seu nome: ");
        String nome = leitor.nextLine();     // le uma linha inteira (com espacos)

        System.out.print("Digite sua idade: ");
        int idade = leitor.nextInt();         // le um inteiro

        System.out.println("Ola, " + nome + "! Voce tem " + idade + " anos.");

        leitor.close();   // boa pratica fechar o Scanner (parecido com fechar arquivo em C)
    }
}
```

| Método do `Scanner` | Lê |
|---|---|
| `nextInt()` | um `int` |
| `nextDouble()` | um `double` |
| `nextLine()` | uma linha inteira de texto |
| `next()` | uma palavra (para no espaço, como `%s` do `scanf`) |
| `nextBoolean()` | `true`/`false` |

**Diferença chave de C:** você não passa `&variavel` — não existe conceito de "endereço" explícito aqui. Você simplesmente atribui o retorno do método: `int idade = leitor.nextInt();`.

**Armadilha clássica:** misturar `nextInt()` com `nextLine()` — `nextInt()` não consome a quebra de linha (`\n`) depois de ler o número, então uma chamada de `nextLine()` logo depois pode "ler" uma linha vazia. Solução comum: adicionar um `leitor.nextLine();` extra depois do `nextInt()` para "limpar" o buffer.

---

## 7. Operadores

Praticamente **idênticos a C**:

```java
int a = 10, b = 3;

a + b;   // 13
a - b;   // 7
a * b;   // 30
a / b;   // 3   -> divisao INTEIRA, igual C (trunca)
a % b;   // 1

a += 5; a -= 3; a *= 2; a /= 4; a %= 4;   // atribuicao composta - igual C

a++; a--; ++a; --a;   // incremento/decremento - igual C, mesma pegadinha de pre/pos

a == b; a != b; a > b; a < b; a >= b; a <= b;   // relacionais - igual C

a && b; a || b; !a;   // logicos - igual C
```

**A diferença mais importante:** em Java, `if`, `while`, `for` **exigem** uma expressão `boolean` de verdade. Diferente de C, onde `if (5)` funciona (porque qualquer não-zero é "verdadeiro"), em Java **isso não compila**:

```java
int x = 5;
if (x) { }        // ERRO DE COMPILACAO em Java! precisa ser boolean
if (x != 0) { }    // correto
```

Essa é uma diferença sutil, mas comum de pegar quem vem de C.

### Divisão com decimais — mesma pegadinha de C

```java
int a = 10, b = 3;
System.out.println(a / b);          // 3 (divisao inteira)
System.out.println((double) a / b); // 3.3333333333333335 - CAST explicito para forcar decimal
```

`(double) a` é um **cast** — igual ao que você poderia fazer em C (`(float) a`), sintaxe idêntica.

---

## 8. Estruturas condicionais

Sintaxe **idêntica a C** para `if`/`else`/`switch`.

```java
int nota = 75;

if (nota >= 90) {
    System.out.println("A");
} else if (nota >= 70) {
    System.out.println("B");
} else {
    System.out.println("C");
}

// ternario - identico a C
String status = (nota >= 70) ? "Aprovado" : "Reprovado";
```

### `switch` — igual C, mas com um extra moderno

```java
int dia = 3;

switch (dia) {
    case 1:
        System.out.println("Segunda");
        break;
    case 2:
        System.out.println("Terca");
        break;
    default:
        System.out.println("Outro dia");
}
```

Desde o Java 14, existe também o **switch moderno (arrow syntax)**, que não precisa de `break` (evita o erro clássico de "esquecer o break" que existe em C):

```java
switch (dia) {
    case 1 -> System.out.println("Segunda");
    case 2 -> System.out.println("Terca");
    default -> System.out.println("Outro dia");
}
```

---

## 9. Estruturas de repetição

Sintaxe **idêntica a C** para `for`, `while`, `do-while`, `break`, `continue`.

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

do {
    System.out.println(i);
    i++;
} while (i < 10);
```

### For-each — um recurso que C não tem

```java
int[] numeros = {10, 20, 30};

for (int n : numeros) {          // "para cada n dentro de numeros"
    System.out.println(n);
}
```

Isso evita ter que controlar índice manualmente quando você só quer percorrer todos os elementos — muito mais comum em Java do que o `for` tradicional quando você não precisa do índice.


---

## 10. Arrays

```java
int[] numeros = new int[5];             // vetor de 5 inteiros, com valores default (0 para numeros)
int[] idades = {20, 25, 30, 35, 40};    // ja inicializado
int[] notas = new int[]{8, 9, 7};        // forma explicita equivalente

System.out.println(idades[0]);   // 20 - indices comecam em 0, igual C
idades[2] = 99;

System.out.println(idades.length);   // 5 - "length" e um ATRIBUTO pronto, diferente de C onde vc calcula com sizeof
```

**Diferenças importantes de C:**

- Arrays em Java **guardam o próprio tamanho** (`.length`). Em C, você tinha que calcular ou passar separadamente.
- Java **verifica limites em tempo de execução**. Acessar `idades[10]` num array de tamanho 5 lança uma exceção (`ArrayIndexOutOfBoundsException`) — o programa avisa e (se você não tratar) encerra com erro claro. Em C, isso silenciosamente lia memória inválida, um dos bugs mais perigosos.
- Ainda assim, o **tamanho do array continua fixo** depois de criado, igual C. Para uma lista que cresce, Java tem `ArrayList` (seção 14).

```java
int[] idades = {20, 25, 30, 35, 40};

try {
    System.out.println(idades[10]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Indice invalido!");
}
```

### Matrizes (arrays 2D)

```java
int[][] tabuleiro = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

System.out.println(tabuleiro[1][2]);   // 6

for (int i = 0; i < tabuleiro.length; i++) {
    for (int j = 0; j < tabuleiro[i].length; j++) {
        System.out.print(tabuleiro[i][j] + " ");
    }
    System.out.println();
}
```

---

## 11. Strings — a maior diferença prática em relação a C

Em Java, `String` é uma **classe pronta**, não um vetor de `char` que você gerencia na mão.

```java
String nome = "Vinicius";
String sobrenome = "Silva";

System.out.println(nome.length());              // 8 - metodo pronto, sem precisar de strlen
String completo = nome + " " + sobrenome;         // concatenacao com "+", sem precisar de strcat
System.out.println(completo.toUpperCase());       // "VINICIUS SILVA"
System.out.println(completo.toLowerCase());       // "vinicius silva"
System.out.println(completo.contains("Silva"));   // true
System.out.println(completo.replace("Vinicius", "V.")); // "V. Silva"
System.out.println(completo.substring(0, 3));     // "Vin"
System.out.println(completo.trim());               // remove espacos das pontas
String[] partes = completo.split(" ");              // divide em vetor: ["Vinicius", "Silva"]
```

**Comparando strings — a pegadinha mais importante:**

```java
String a = "Vinicius";
String b = "Vinicius";
String c = new String("Vinicius");

System.out.println(a == b);          // true (coincidencia de implementacao - "string pool")
System.out.println(a == c);          // false! "==" compara REFERENCIA (endereco), nao conteudo
System.out.println(a.equals(c));     // true - equals() compara CONTEUDO, use sempre isso
```

Isso é conceitualmente **igual à pegadinha de C** onde `==` em strings compara ponteiros (endereços), não o texto — só que em Java a regra vale para **qualquer objeto**, não só strings: `==` compara referência, `.equals()` compara conteúdo. Regra prática: **nunca compare `String` com `==`, sempre use `.equals()`**.

**Java te poupa exatamente o trabalho manual de C:** não existe `\0` que você precisa gerenciar, não existe limite de tamanho fixo pra definir na criação, não existe risco de buffer overflow como em `strcpy`.

---

## 12. Referências — o "ponteiro sem aritmética" de Java

Java não tem ponteiros explícitos, mas todo objeto é acessado por **referência** (um "endereço" gerenciado que você não manipula diretamente).

```java
int[] original = {1, 2, 3};
int[] copia = original;   // copia NAO cria um novo array - as duas variaveis apontam para o MESMO array!

copia[0] = 99;
System.out.println(original[0]);   // 99 - o original mudou tambem!
```

Isso é o comportamento de referência: `original` e `copia` são dois "nomes" para o mesmo objeto na memória — bem parecido com dois ponteiros em C guardando o mesmo endereço, exceto que você não escreve `*` nem `&` em lugar nenhum.

### Tipos primitivos são passados por valor / objetos por referência

```java
public class Main {
    static void dobrarInt(int numero) {
        numero = numero * 2;   // altera so a COPIA local
    }

    static void dobrarPrimeiroElemento(int[] vetor) {
        vetor[0] = vetor[0] * 2;   // altera o array de VERDADE, pois array e objeto/referencia
    }

    public static void main(String[] args) {
        int valor = 10;
        dobrarInt(valor);
        System.out.println(valor);   // ainda 10

        int[] numeros = {10, 20};
        dobrarPrimeiroElemento(numeros);
        System.out.println(numeros[0]);   // 20 - mudou!
    }
}
```

Isso é o equivalente, em Java, à mesma lógica que você viu em C com `dobrarErrado`/`dobrarCerto` — só que aqui você **não escolhe** explicitamente passar por referência com `&`; a regra é automática: **primitivo = valor, objeto = referência**.

### Sem `malloc`/`free` — o Garbage Collector

```java
public class Main {
    public static void main(String[] args) {
        int[] vetor = new int[1000000];   // "new" aloca memoria automaticamente (equivale ao malloc)
        // ... uso do vetor ...
        vetor = null;   // quando nada mais referencia o array, o Garbage Collector libera a memoria sozinho, em algum momento
    }
}
```

Você nunca chama algo como `free(vetor)`. A JVM tem um processo (Garbage Collector) que roda em segundo plano e libera memória de objetos que não têm mais nenhuma referência apontando para eles. Isso elimina a categoria inteira de bugs de "esqueci de liberar memória" que existe em C — ao custo de menos controle fino sobre quando exatamente a memória é liberada.


---

## 13. Métodos (o "funções" de Java)

Em Java, toda função vive **dentro de uma classe** — não existe função solta como em C.

```java
public class Calculadora {

    // metodo estatico - pode ser chamado sem criar um objeto Calculadora
    static int somar(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        int resultado = somar(5, 3);
        System.out.println(resultado);
    }
}
```

**Diferença de C:** não existe "protótipo" separado da implementação — você declara e define o método no mesmo lugar, dentro da classe, sem precisar avisar o compilador antes (o compilador Java lê a classe inteira antes de checar, diferente do C que lê de cima para baixo).

### Assinatura de um método

```java
[modificador] [static?] tipoRetorno nomeMetodo(tipoParametro nome1, tipoParametro nome2) {
    // corpo
    return valor;   // se tipoRetorno != void
}
```

```java
static void saudacao(String nome) {   // void = sem retorno, igual C
    System.out.println("Ola, " + nome + "!");
}

static double calcularMedia(double nota1, double nota2) {
    return (nota1 + nota2) / 2;
}
```

### Sobrecarga de método (overload) — recurso que C não tem

Java permite **vários métodos com o mesmo nome**, desde que os parâmetros sejam diferentes (em quantidade ou tipo):

```java
static int somar(int a, int b) {
    return a + b;
}

static double somar(double a, double b) {
    return a + b;
}

static int somar(int a, int b, int c) {
    return a + b + c;
}
```

O compilador decide qual versão chamar de acordo com os argumentos que você passa. Em C isso não existe — cada função precisa ter um nome único.

### Recursão — idêntica a C

```java
static int fatorial(int n) {
    if (n <= 1) return 1;
    return n * fatorial(n - 1);
}
```

---

## 14. Orientação a Objetos — a diferença estrutural mais importante em relação a C

Em C, você tinha `struct` (só dados) e funções soltas que operavam sobre elas. Em Java, uma **classe** junta dados (atributos) e comportamento (métodos) na mesma estrutura — isso é a base de tudo em Java.

```java
public class Pessoa {
    // atributos (equivalente aos campos de uma struct em C)
    String nome;
    int idade;

    // construtor - metodo especial chamado ao criar o objeto com "new"
    public Pessoa(String nome, int idade) {
        this.nome = nome;   // "this" refere-se ao objeto atual, distingue o atributo do parametro
        this.idade = idade;
    }

    // metodo - algo que C NAO permite dentro de uma struct
    public void aniversario() {
        this.idade = this.idade + 1;
    }

    public void apresentar() {
        System.out.println("Sou " + nome + " e tenho " + idade + " anos.");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Pessoa p1 = new Pessoa("Julia", 22);   // "new" cria o objeto de verdade na memoria
        p1.apresentar();          // "Sou Julia e tenho 22 anos."
        p1.aniversario();
        p1.apresentar();          // "Sou Julia e tenho 23 anos."
    }
}
```

**Comparando com o que você já viu em C:**

| C | Java |
|---|---|
| `struct Pessoa { ... };` só guarda dados | `class Pessoa { ... }` guarda dados **e** métodos |
| Função separada: `aniversario(&p1)` | Método dentro do objeto: `p1.aniversario()` |
| `malloc`/inicialização manual | `new Pessoa(...)` chama o construtor automaticamente |
| `p->idade` (ponteiro para struct) | `p1.idade` (Java usa sempre `.`, nunca `->`, pois não há ponteiro explícito) |

### Modificadores de acesso (encapsulamento) — conceito que C não tem

```java
public class ContaBancaria {
    private double saldo;   // "private" - so acessivel DENTRO da propria classe

    public ContaBancaria(double saldoInicial) {
        this.saldo = saldoInicial;
    }

    public void depositar(double valor) {
        if (valor > 0) {
            this.saldo += valor;
        }
    }

    public double getSaldo() {   // metodo "getter" - jeito controlado de ler o dado privado
        return saldo;
    }
}
```

```java
ContaBancaria conta = new ContaBancaria(100);
conta.depositar(50);
System.out.println(conta.getSaldo());   // 150
// conta.saldo = 999999;   ISSO DA ERRO DE COMPILACAO - saldo e private
```

| Modificador | Quem acessa |
|---|---|
| `public` | Qualquer lugar |
| `private` | Só dentro da própria classe |
| `protected` | A própria classe + subclasses (herança) |
| (nenhum, "default") | Só dentro do mesmo pacote |

Isso é o princípio de **encapsulamento** — proteger dados internos para que só sejam alterados de formas controladas (pelos métodos que você escreveu). C não tem esse mecanismo; qualquer código pode acessar qualquer campo de uma `struct`.


### Herança — outro recurso que C não tem

```java
public class Animal {
    String nome;

    public Animal(String nome) {
        this.nome = nome;
    }

    public void emitirSom() {
        System.out.println(nome + " faz um som.");
    }
}

public class Cachorro extends Animal {   // Cachorro HERDA de Animal

    public Cachorro(String nome) {
        super(nome);   // chama o construtor da classe pai (Animal)
    }

    @Override                              // avisa que esta reescrevendo um metodo do pai
    public void emitirSom() {
        System.out.println(nome + " late: Au au!");
    }
}
```

```java
Animal genericoAnimal = new Animal("Bicho");
genericoAnimal.emitirSom();     // "Bicho faz um som."

Cachorro rex = new Cachorro("Rex");
rex.emitirSom();                 // "Rex late: Au au!" - sobrescreveu o comportamento do pai
```

Em C, para simular algo parecido, você precisaria de ponteiros para função dentro de structs — bem mais trabalhoso e manual. Em Java, herança e polimorfismo são recursos nativos da linguagem.

---

## 15. Coleções dinâmicas — o que substitui os arrays de tamanho fixo

C não tinha uma lista que cresce sozinha — você teria que implementar isso na mão com `malloc`/`realloc`. Java já vem com isso pronto na biblioteca `java.util`.

```java
import java.util.ArrayList;

ArrayList<String> nomes = new ArrayList<>();

nomes.add("Ana");
nomes.add("Bruno");
nomes.add("Carlos");

System.out.println(nomes.get(0));       // "Ana"
System.out.println(nomes.size());        // 3 - equivalente ao .length de array, mas para ArrayList e .size()
nomes.remove("Bruno");
System.out.println(nomes.contains("Ana"));  // true

for (String nome : nomes) {
    System.out.println(nome);
}
```

`<String>` é um **generic** — diz que essa `ArrayList` só aceita `String` dentro dela (tipagem segura, o compilador barra se você tentar colocar outro tipo).

### `HashMap` — o "dicionário"/"objeto chave-valor" de Java

```java
import java.util.HashMap;

HashMap<String, Integer> idades = new HashMap<>();

idades.put("Ana", 25);
idades.put("Bruno", 30);

System.out.println(idades.get("Ana"));        // 25
System.out.println(idades.containsKey("Bruno")); // true

for (String chave : idades.keySet()) {
    System.out.println(chave + ": " + idades.get(chave));
}
```

Isso é o equivalente ao `dict` do Python ou objeto `{}` do JavaScript — em C, você teria que construir isso do zero com structs e listas ligadas.

---

## 16. Tratamento de exceções — recurso que C não tem

C não tem `try/catch`. Erros em C geralmente são sinalizados por valores de retorno (`NULL`, `-1`) que você precisa checar manualmente. Java tem um sistema de exceções nativo:

```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] numeros = {1, 2, 3};
            System.out.println(numeros[10]);   // vai lancar excecao
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Erro: indice invalido!");
        } finally {
            System.out.println("Isso sempre executa, com ou sem erro.");
        }
    }
}
```

```java
static int dividir(int a, int b) {
    if (b == 0) {
        throw new ArithmeticException("Divisao por zero!");   // lanca uma excecao manualmente
    }
    return a / b;
}

public static void main(String[] args) {
    try {
        int resultado = dividir(10, 0);
    } catch (ArithmeticException e) {
        System.out.println("Erro capturado: " + e.getMessage());
    }
}
```

| Palavra-chave | Função |
|---|---|
| `try` | bloco onde algo pode dar erro |
| `catch` | captura e trata o erro |
| `finally` | executa sempre, dê erro ou não |
| `throw` | lança uma exceção manualmente |
| `throws` | declara, na assinatura do método, que ele pode lançar uma exceção |

---

## 17. Erros comuns de quem está vindo de C para Java

| Erro | Por que acontece | Como evitar |
|---|---|---|
| Comparar `String` com `==` | `==` compara referência, não conteúdo | Sempre use `.equals()` |
| Esquecer que `main` precisa estar numa classe com o mesmo nome do arquivo | Java exige isso, C não tem esse conceito | `Main.java` precisa conter `class Main` |
| Usar `if (numero)` esperando funcionar como em C | Java exige `boolean` explícito nas condições | Escreva `if (numero != 0)` |
| Misturar `nextInt()` e `nextLine()` no `Scanner` sem cuidado | `nextInt()` não consome o `\n` | Adicione `scanner.nextLine()` extra após ler números |
| Esperar que `array = outroArray` copie o conteúdo | Arrays são referência; a atribuição copia o "endereço", não o conteúdo | Use `Arrays.copyOf()` ou copie elemento por elemento se precisar de uma cópia de verdade |
| Achar que não precisa mais pensar em memória | O Garbage Collector ajuda, mas manter referências desnecessárias (ex: em coleções grandes) ainda pode causar consumo excessivo de memória | Remova referências que não precisa mais (`objeto = null` em casos específicos, ou deixe escopo local resolver sozinho) |
| Esquecer `break` no `switch` tradicional | Mesma pegadinha de C ainda existe no switch clássico | Use `break` ou prefira a sintaxe moderna com `->` |

---

## 18. Tabela-resumo rápida (consulta relâmpago)

```java
import java.util.Scanner;
import java.util.ArrayList;
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        // variaveis
        int i = 0; double d = 1.5; char c = 'A'; boolean ok = true; String s = "texto";

        // condicional
        if (i == 0) { } else { }

        // laco
        for (int k = 0; k < 10; k++) { }
        while (i < 10) { }
        do { } while (i < 10);

        // for-each
        int[] v = {1, 2, 3};
        for (int n : v) { }

        // colecoes
        ArrayList<Integer> lista = new ArrayList<>();
        HashMap<String, Integer> mapa = new HashMap<>();

        // excecao
        try { } catch (Exception e) { } finally { }
    }
}

// classe separada (arquivo Pessoa.java)
class Pessoa {
    private String nome;

    public Pessoa(String nome) { this.nome = nome; }
    public String getNome() { return nome; }
}
```

---

## 19. Próximos passos sugeridos de estudo

1. Fixar bem orientação a objetos: classes, construtores, encapsulamento (`private`/`get`/`set`), herança e polimorfismo — é a base estrutural de praticamente todo código Java que você vai ler no mercado.
2. Praticar `ArrayList` e `HashMap` até ficar natural — são as estruturas que você mais vai usar no dia a dia, muito mais que array puro.
3. Entender bem `try/catch` e quando lançar suas próprias exceções — essencial para código de back-end robusto.
4. Depois disso, o caminho natural para o seu foco declarado é: **Java + Spring Boot** para construir APIs REST, e **JDBC/JPA/Hibernate** para conectar com banco de dados — esse é o próximo guia que faz sentido pedir quando estiver confortável com o conteúdo deste.
