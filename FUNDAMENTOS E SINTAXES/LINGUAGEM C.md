# Guia de Sintaxe da Linguagem C

> Guia objetivo para aprender a sintaxe de C e escrever programas simples. Feito para quem já entende lógica de programação e algoritmos, mas quer destravar a sintaxe.

---

## 1. O que é C e por que ele importa

C é uma linguagem de **1972** (Dennis Ritchie, nos Laboratórios Bell), criada para reescrever o sistema operacional Unix. Ela é o "ancestral direto" da maioria das linguagens que você já usa:

- Java, C#, JavaScript e Python "roubaram" a sintaxe de blocos `{ }` e `;` de C.
- Python é implementado (CPython) em C.
- O kernel do Linux, o Unix, drivers, sistemas embarcados (microcontroladores, IoT) e boa parte de bancos de dados como PostgreSQL e MySQL são escritos em C.

### Características principais

| Característica | O que significa na prática |
|---|---|
| **Compilada** | Seu `.c` vira um binário executável antes de rodar. Não existe interpretador rodando linha a linha (diferente de JS/Python). |
| **Tipagem estática** | Você declara o tipo da variável e ele não muda. `int x = 5;` sempre será `int`. |
| **Procedural** | Não existe "classe" ou "objeto" nativo. Você organiza o código em funções que manipulam dados. |
| **Baixo nível / próxima do hardware** | Você controla memória manualmente (alocar e liberar). Não existe coletor de lixo (garbage collector). |
| **Ponteiros** | Você acessa e manipula endereços de memória diretamente — é a característica mais marcante e mais difícil de C. |
| **Sem "strings" nativas** | Texto é, na verdade, um vetor (array) de caracteres terminado por um caractere especial (`\0`). |
| **Portável mas não "roda em qualquer lugar" como Java** | Você compila para a plataforma específica (Windows, Linux, ARM etc). Não existe uma "máquina virtual" universal. |

### C vs C++ — as diferenças que importam

C++ nasceu como "C com classes" (Bjarne Stroustrup, 1979) e depois cresceu muito além disso. As melhorias mais relevantes que o C++ trouxe sobre o C:

| C | C++ |
|---|---|
| Procedural puro | Suporta Orientação a Objetos (classes, herança, polimorfismo) |
| Sem `new`/`delete`; usa `malloc`/`free` | Tem `new`/`delete` e, mais importante, **RAII** e ponteiros inteligentes (`unique_ptr`, `shared_ptr`) que gerenciam memória de forma mais segura |
| Sem sobrecarga de função | Permite várias funções com o mesmo nome e assinaturas diferentes (overload) |
| Sem templates | Tem *templates* (genéricos), permitindo funções/estruturas que funcionam com qualquer tipo |
| Sem namespaces | Tem `namespace` para evitar conflito de nomes |
| `struct` só guarda dados | `struct` em C++ pode ter métodos (é quase igual a uma classe) |
| Strings são vetores de `char` | Tem a classe `std::string`, muito mais prática |
| Biblioteca padrão pequena (`stdio.h`, `stdlib.h`...) | Tem a STL (Standard Template Library): vetores dinâmicos (`vector`), mapas, filas, pilhas prontos |
| Compilação mais simples e previsível | Compilação mais complexa, binários geralmente maiores |

**Resumo prático:** C é mais simples, mais "cru" e mais previsível — ótimo para entender como o computador realmente funciona. C++ é C com muito mais ferramentas prontas para projetos grandes, mas com mais complexidade.

### Quando C é usado hoje (dinâmica de uso no mercado)

- **Sistemas embarcados e IoT** (Arduino, microcontroladores, firmware).
- **Sistemas operacionais e drivers** (Linux é majoritariamente C).
- **Bancos de dados e engines de baixo nível** (SQLite, Redis, PostgreSQL).
- **Alta performance** (jogos em engines de baixo nível, processamento de sinais).
- **Disciplina acadêmica**: C é usado para ensinar como memória, ponteiros e compiladores funcionam — a base que depois te ajuda a entender qualquer outra linguagem por baixo dos panos.

Você, vindo de Java/JS/Python, vai sentir falta de: strings prontas, coleta de lixo automática, listas dinâmicas prontas e tratamento de exceções (`try/catch`). C não tem nada disso nativamente — você constrói ou usa bibliotecas.

---

## 2. Estrutura mínima de um programa em C

```c
#include <stdio.h>   // inclui a biblioteca de entrada/saída (printf, scanf)

int main(void) {      // toda execução começa na função main
    printf("Ola, mundo!\n");
    return 0;          // 0 = programa terminou sem erro
}
```

**Explicando linha a linha:**

- `#include <stdio.h>` — uma **diretiva de pré-processador**. Antes de compilar, o compilador "copia e cola" o conteúdo do arquivo `stdio.h` (Standard Input/Output) aqui. É como um `import` do Java/Python, mas mais literal.
- `int main(void)` — toda vez que você roda o programa, o sistema operacional chama a função `main`. `int` é o tipo que ela retorna (um número inteiro para o sistema operacional). `void` entre parênteses significa "não recebe parâmetros".
- `{ }` — delimitam o **bloco** de código da função, igual em Java/JS.
- `;` — toda instrução termina com ponto e vírgula (igual Java, diferente de Python).
- `return 0;` — devolve `0` ao sistema operacional, convenção universal para "executou com sucesso". Qualquer valor diferente de 0 geralmente indica erro.

### Compilando e rodando

```bash
gcc programa.c -o programa   # compila o arquivo .c e gera o executável "programa"
./programa                    # roda o executável (Linux/Mac)
programa.exe                  # roda no Windows
```

Diferente de Python/Node, você **sempre** compila antes de rodar. Erros de sintaxe aparecem na hora de compilar, não na hora de rodar.

---

## 3. Comentários

```c
// comentário de uma linha

/* comentário
   de várias
   linhas */
```

Igual Java e JavaScript.

---

## 4. Tipos de dados primitivos

C tem tipos bem mais "granulares" que Python/JS porque você escolhe exatamente quanto de memória quer usar.

| Tipo | O que guarda | Tamanho típico | Exemplo |
|---|---|---|---|
| `int` | número inteiro | 4 bytes | `int idade = 25;` |
| `float` | número decimal (ponto flutuante, precisão simples) | 4 bytes | `float preco = 9.99f;` |
| `double` | número decimal (precisão dupla, mais preciso) | 8 bytes | `double pi = 3.14159265;` |
| `char` | um único caractere | 1 byte | `char letra = 'A';` |
| `_Bool` / `bool`* | verdadeiro/falso | 1 byte | `bool ativo = true;` |
| `void` | "nenhum tipo" (usado em funções sem retorno ou ponteiros genéricos) | — | `void funcao(void)` |

\* `bool` só existe se você incluir `#include <stdbool.h>` (em C puro, `true` e `false` não são nativos como em Java).

### Modificadores de tipo

Você pode ajustar o alcance (range) e o tamanho:

```c
short int idadeCurta;     // menor, geralmente 2 bytes
long int populacao;       // maior, geralmente 8 bytes
long long int numeroGrande;  // ainda maior, garantido >= 8 bytes

unsigned int contador;    // só aceita valores positivos (0 até o dobro do máximo)
signed int temperatura;   // aceita negativos e positivos (padrão)
```

**Diferença chave para quem vem de outras linguagens:** em Python, `int` não tem limite de tamanho. Em C, `int` estoura (overflow) se você passar do limite — o valor "dá a volta" silenciosamente, sem erro.

```c
#include <stdio.h>

int main(void) {
    int idade = 30;
    float altura = 1.75f;      // o "f" indica que é float, não double
    double distancia = 384400.5;
    char inicial = 'V';         // aspas simples para char!
    
    printf("Idade: %d\n", idade);
    printf("Altura: %.2f\n", altura);
    printf("Distancia: %.1lf\n", distancia);
    printf("Inicial: %c\n", inicial);
    
    return 0;
}
```

### `sizeof` — descobrindo o tamanho de um tipo

```c
printf("Tamanho de int: %zu bytes\n", sizeof(int));       // geralmente 4
printf("Tamanho de double: %zu bytes\n", sizeof(double)); // geralmente 8
```

---

## 5. Variáveis — declaração, inicialização e convenções

```c
int idade;              // declaração (sem valor - "lixo de memória" até você atribuir)
idade = 25;              // atribuição

int altura = 180;        // declaração + inicialização (recomendado sempre fazer assim)

int a = 1, b = 2, c = 3;  // múltiplas variáveis na mesma linha
```

**Regras de nomenclatura:**
- Pode ter letras, números e `_`, mas não pode começar com número.
- Case-sensitive: `idade` e `Idade` são variáveis diferentes.
- Convenção comum: `snake_case` (`nome_completo`) ou `camelCase` (`nomeCompleto`) — C não impõe, mas a comunidade tende a `snake_case`.
- Palavras reservadas (`int`, `return`, `if`...) não podem ser usadas como nome.

### `const` — valor que não muda

```c
const float PI = 3.14159f;
// PI = 3.0;   ISSO DARIA ERRO DE COMPILAÇÃO
```

---

## 6. Entrada e saída (`printf` e `scanf`)

`printf` (print formatted) manda texto para a tela. `scanf` (scan formatted) lê dados digitados pelo usuário.

### Especificadores de formato (essenciais em `printf`/`scanf`)

| Especificador | Tipo |
|---|---|
| `%d` | `int` |
| `%f` | `float` |
| `%lf` | `double` (no `scanf`; no `printf` pode usar `%f` para ambos) |
| `%c` | `char` |
| `%s` | string (vetor de char) |
| `%p` | endereço de memória (ponteiro) |
| `%x` | inteiro em hexadecimal |
| `%%` | imprime o símbolo `%` literal |

```c
#include <stdio.h>

int main(void) {
    int idade;
    char nome[50];   // vetor de até 50 caracteres

    printf("Digite seu nome: ");
    scanf("%s", nome);          // sem & porque array "decai" para ponteiro sozinho

    printf("Digite sua idade: ");
    scanf("%d", &idade);        // & = "endereço de memória de idade"

    printf("Ola, %s! Voce tem %d anos.\n", nome, idade);

    return 0;
}
```

**Ponto crítico que confunde muita gente vinda de outras linguagens:** no `scanf`, você quase sempre passa o **endereço** da variável (`&idade`), não a variável em si — porque `scanf` precisa saber *onde na memória* escrever o valor lido. Isso é a primeira pista de como ponteiros funcionam em C (mais detalhes na seção 11).

`%s` é exceção: como o nome de um vetor já "é" o endereço do seu primeiro elemento, você não usa `&` nele.

---

## 7. Operadores

### Aritméticos

```c
int a = 10, b = 3;

a + b;   // 13  soma
a - b;   // 7   subtracao
a * b;   // 30  multiplicacao
a / b;   // 3   divisao INTEIRA (trunca a parte decimal!)
a % b;   // 1   resto da divisao (modulo)
```

**Armadilha clássica:** `10 / 3` em C dá `3`, não `3.333...`, porque os dois operandos são `int`. Para ter resultado decimal, pelo menos um dos dois precisa ser `float`/`double`:

```c
float resultado = 10.0 / 3;   // 3.333333
float errado = 10 / 3;         // 3.000000 (a divisao inteira acontece ANTES de virar float)
```

### Atribuição composta

```c
int x = 10;
x += 5;   // x = x + 5  -> 15
x -= 3;   // x = x - 3  -> 12
x *= 2;   // x = x * 2  -> 24
x /= 4;   // x = x / 4  -> 6
x %= 4;   // x = x % 4  -> 2
```

### Incremento e decremento

```c
int i = 5;
i++;   // pos-incremento: i vira 6
i--;   // pos-decremento: i vira 5
++i;   // pre-incremento: i vira 6 (a diferenca importa dentro de expressoes)

int a = 5;
int b = a++;  // b recebe 5, DEPOIS a vira 6
int c = ++a;  // a vira 7 PRIMEIRO, DEPOIS c recebe 7
```

### Relacionais (retornam 0 ou 1, pois C não tem `bool` puro sem `stdbool.h`)

```c
a == b   // igual
a != b   // diferente
a > b    // maior
a < b    // menor
a >= b   // maior ou igual
a <= b   // menor ou igual
```

### Lógicos

```c
a && b   // E (AND) - true se ambos forem verdadeiros
a || b   // OU (OR)  - true se pelo menos um for verdadeiro
!a       // NAO (NOT) - inverte o valor
```

**Importante:** em C, **qualquer valor diferente de 0 é considerado verdadeiro**, e **0 é falso**. Não existe erro de "tipo incompatível" como em Python (`if 5:` funciona, `if "texto":` também funciona).

```c
if (5) {
    printf("Isso imprime, porque 5 != 0\n");
}
```

---

## 8. Estruturas condicionais

### `if / else if / else`

```c
int nota = 75;

if (nota >= 90) {
    printf("A\n");
} else if (nota >= 70) {
    printf("B\n");
} else if (nota >= 50) {
    printf("C\n");
} else {
    printf("Reprovado\n");
}
```

### Operador ternário

```c
int idade = 20;
char *status = (idade >= 18) ? "maior de idade" : "menor de idade";
printf("%s\n", status);
```

### `switch`

```c
int dia = 3;

switch (dia) {
    case 1:
        printf("Segunda\n");
        break;         // ESSENCIAL: sem break, o codigo "cai" para o proximo case
    case 2:
        printf("Terca\n");
        break;
    case 3:
        printf("Quarta\n");
        break;
    default:
        printf("Dia invalido\n");
}
```

**Cuidado clássico:** esquecer o `break` faz o programa executar todos os `case` seguintes até achar um `break` ou acabar o `switch` (chamado de "fall-through"). Em Java/JS o comportamento é o mesmo, então se você já usa `switch` nessas linguagens, essa parte é familiar.

---

## 9. Estruturas de repetição

### `for`

```c
for (int i = 0; i < 5; i++) {
    printf("%d\n", i);   // imprime 0 1 2 3 4
}
```

Estrutura: `for (inicializacao; condicao; incremento)`. As três partes são opcionais, mas os dois `;` são obrigatórios.

### `while`

```c
int i = 0;
while (i < 5) {
    printf("%d\n", i);
    i++;
}
```

### `do-while` (executa pelo menos uma vez, testa a condição no final)

```c
int opcao;
do {
    printf("Digite 0 para sair: ");
    scanf("%d", &opcao);
} while (opcao != 0);
```

### `break` e `continue`

```c
for (int i = 0; i < 10; i++) {
    if (i == 5) break;       // sai do loop imediatamente
    if (i % 2 == 0) continue; // pula para a proxima iteracao
    printf("%d\n", i);        // imprime apenas 1 3
}
```


---

## 10. Vetores (arrays)

```c
int numeros[5];                      // vetor de 5 inteiros, valores "indefinidos"
int idades[5] = {20, 25, 30, 35, 40}; // vetor ja inicializado
int notas[] = {8, 9, 7};              // tamanho inferido automaticamente (3)

printf("%d\n", idades[0]);   // 20 - indices comecam em 0!
idades[2] = 99;               // altera o elemento de indice 2

int tamanho = sizeof(idades) / sizeof(idades[0]);  // truque comum para "contar" elementos
```

**Diferente de Python/JS:** o array em C tem **tamanho fixo** definido na criação. Não existe `.push()`, `.append()` ou redimensionamento automático. Se você precisa de uma lista que cresce, precisa gerenciar isso manualmente com ponteiros e `malloc` (seção 12), ou usar bibliotecas de terceiros.

### Percorrendo um vetor

```c
int notas[5] = {7, 8, 5, 9, 10};

for (int i = 0; i < 5; i++) {
    printf("Nota %d: %d\n", i, notas[i]);
}
```

### Matrizes (arrays 2D)

```c
int tabuleiro[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

printf("%d\n", tabuleiro[1][2]);  // imprime 6 (linha 1, coluna 2)

for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        printf("%d ", tabuleiro[i][j]);
    }
    printf("\n");
}
```

---

## 11. Strings — o ponto que mais confunde quem vem de outras linguagens

C **não tem um tipo `string`**. Texto é um vetor de `char` que termina com o caractere nulo `'\0'` (byte de valor zero), que marca "aqui acaba a string".

```c
char nome[20] = "Vinicius";
// na memoria isso fica: 'V','i','n','i','c','i','u','s','\0', lixo, lixo...
```

Isso significa: um vetor de 20 posições guarda no máximo 19 caracteres de texto úteis + o `\0` obrigatório.

### Funções da biblioteca `<string.h>`

Como não existem métodos prontos tipo `.length()` ou `.toUpperCase()`, você usa funções da biblioteca padrão:

```c
#include <string.h>
#include <stdio.h>

int main(void) {
    char nome[50] = "Vinicius";
    char sobrenome[50] = "Silva";

    printf("Tamanho: %zu\n", strlen(nome));      // 8 - conta ate achar o '\0'

    strcat(nome, " ");        // concatena (nome vira "Vinicius ")
    strcat(nome, sobrenome);  // nome vira "Vinicius Silva"
    printf("%s\n", nome);

    char copia[50];
    strcpy(copia, nome);       // copia o conteudo de nome para copia
    printf("%s\n", copia);

    if (strcmp(nome, copia) == 0) {   // strcmp retorna 0 quando sao IGUAIS
        printf("Sao iguais\n");
    }

    return 0;
}
```

| Função | O que faz | Equivalente mental |
|---|---|---|
| `strlen(s)` | retorna o tamanho da string | `len(s)` / `s.length` |
| `strcpy(destino, origem)` | copia uma string para outra | `destino = origem` |
| `strcat(destino, origem)` | concatena origem ao final de destino | `destino += origem` |
| `strcmp(a, b)` | compara (`0` = iguais) | `a == b` |
| `strncpy`, `strncat`, `strncmp` | versões "seguras" que limitam quantos caracteres processar (evitam estourar o buffer) |

**Cuidado sério:** `strcpy` e `strcat` não verificam se o destino tem espaço suficiente. Se você copiar uma string maior do que o vetor de destino comporta, isso corrompe memória (buffer overflow) — um dos bugs mais clássicos e perigosos de C.

### Lendo string com espaços

`scanf("%s", ...)` para no primeiro espaço. Para ler uma linha inteira:

```c
char frase[100];
fgets(frase, sizeof(frase), stdin);   // le ate 99 caracteres ou ate a quebra de linha
```

---

## 12. Ponteiros — o coração de C

Ponteiro é uma variável que guarda **o endereço de memória** de outra variável, em vez de guardar o valor diretamente.

```c
int idade = 25;
int *ponteiro = &idade;   // ponteiro guarda o ENDERECO de idade

printf("%d\n", idade);       // 25          - o valor
printf("%p\n", &idade);      // 0x7ffee...  - o endereco de idade
printf("%p\n", ponteiro);    // 0x7ffee...  - o MESMO endereco
printf("%d\n", *ponteiro);   // 25          - "desreferenciar": pega o valor QUE ESTA naquele endereco
```

**Os dois símbolos que confundem no início:**

- `&variavel` → "me dê o **endereço** dessa variável" (usado ao *declarar* de onde vem um endereço).
- `*ponteiro` → dois significados diferentes dependendo do contexto:
  - Na **declaração** (`int *p;`): "p é um ponteiro para int".
  - No **uso** (`*p = 10;` ou `printf("%d", *p)`): "desreferenciar" — acessa/altera o valor que está *naquele endereço*, não o endereço em si.

```c
int x = 10;
int *p = &x;

*p = 50;              // altera o VALOR de x atraves do ponteiro
printf("%d\n", x);    // imprime 50 - x realmente mudou!
```

### Por que ponteiros existem / para que servem na prática

1. **Passar variáveis por referência para funções** (já que, por padrão, C passa tudo por cópia).
2. **Manipular arrays e strings de forma eficiente** (arrays "decaem" para ponteiros).
3. **Alocar memória dinamicamente** (`malloc`), quando você não sabe o tamanho em tempo de compilação.
4. **Estruturas de dados** como listas ligadas, árvores, e para simular estruturas dinâmicas que Python/JS te dão de graça.

### Passagem por valor vs por referência

```c
#include <stdio.h>

// por valor: a funcao recebe uma COPIA, alteracoes nao afetam o original
void dobrarErrado(int numero) {
    numero = numero * 2;
}

// por referencia (com ponteiro): a funcao recebe o ENDERECO, altera o original
void dobrarCerto(int *numero) {
    *numero = *numero * 2;
}

int main(void) {
    int valor = 10;

    dobrarErrado(valor);
    printf("%d\n", valor);   // ainda 10 - a funcao mexeu so na copia

    dobrarCerto(&valor);
    printf("%d\n", valor);   // 20 - a funcao mexeu no valor de verdade
    
    return 0;
}
```

Isso é o equivalente em C de por que, em Java, alterar um objeto dentro de um método afeta o objeto original, mas alterar um `int` (primitivo) dentro de um método, não — só que em C **você** decide explicitamente se quer "por valor" ou "por referência" usando ponteiros.

### Alocação dinâmica de memória (`malloc`, `free`)

Quando você não sabe o tamanho de algo em tempo de compilação (ex: quantidade de itens que o usuário vai digitar):

```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    int n;
    printf("Quantos numeros? ");
    scanf("%d", &n);

    int *vetor = malloc(n * sizeof(int));  // aloca espaco na memoria "heap"

    if (vetor == NULL) {         // malloc pode falhar (memoria insuficiente)
        printf("Erro de alocacao\n");
        return 1;
    }

    for (int i = 0; i < n; i++) {
        vetor[i] = i * 10;
    }

    for (int i = 0; i < n; i++) {
        printf("%d ", vetor[i]);
    }

    free(vetor);   // OBRIGATORIO liberar a memoria manualmente - C nao tem garbage collector!
    return 0;
}
```

**Essa é a maior diferença mental vindo de Java/Python/JS**: lá, o coletor de lixo libera memória sozinho. Em C, **toda vez que você usa `malloc`, você é responsável por chamar `free`** quando não precisar mais. Esquecer disso causa "memory leak" (vazamento de memória) — o programa vai consumindo mais e mais RAM sem devolver.


---

## 13. Funções

```c
// declaracao/prototipo (avisa o compilador que a funcao existe, antes de usa-la)
int somar(int a, int b);

int main(void) {
    int resultado = somar(5, 3);
    printf("%d\n", resultado);
    return 0;
}

// definicao (o corpo de verdade)
int somar(int a, int b) {
    return a + b;
}
```

**Por que o protótipo existe:** C lê o arquivo de cima para baixo. Se `main` chama `somar` antes de `somar` ser definida no arquivo, o compilador erra "função não declarada" — a menos que você declare o protótipo antes. Em projetos maiores, protótipos ficam em arquivos `.h` (headers).

### Função sem retorno (`void`)

```c
void saudacao(char nome[]) {
    printf("Ola, %s!\n", nome);
    // sem "return" (ou return; sem valor)
}
```

### Parâmetros por valor vs ponteiro (recapitulando função + ponteiro)

```c
void trocar(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main(void) {
    int x = 1, y = 2;
    trocar(&x, &y);
    printf("%d %d\n", x, y);   // 2 1
    return 0;
}
```

### Arrays como parâmetro (sempre "decaem" para ponteiro)

```c
void imprimirVetor(int vetor[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        printf("%d ", vetor[i]);
    }
}

int main(void) {
    int numeros[] = {1, 2, 3, 4};
    imprimirVetor(numeros, 4);   // precisa passar o tamanho, pois o array "esquece" seu tamanho ao virar ponteiro
    return 0;
}
```

### Recursão

```c
int fatorial(int n) {
    if (n <= 1) return 1;          // caso base
    return n * fatorial(n - 1);    // chamada recursiva
}
```

---

## 14. Structs — agrupando dados (o "objeto" simplificado de C)

C não tem classes, mas tem `struct`: um jeito de agrupar variáveis relacionadas em um único tipo composto.

```c
#include <stdio.h>
#include <string.h>

struct Pessoa {
    char nome[50];
    int idade;
    float altura;
};

int main(void) {
    struct Pessoa p1;
    strcpy(p1.nome, "Ana");   // nao pode usar "=" para strings, e struct
    p1.idade = 28;
    p1.altura = 1.65f;

    printf("%s tem %d anos e %.2fm\n", p1.nome, p1.idade, p1.altura);

    // inicializacao direta
    struct Pessoa p2 = {"Carlos", 35, 1.80f};

    return 0;
}
```

### `typedef` — dando um apelido ao tipo (evita repetir `struct` toda hora)

```c
typedef struct {
    char nome[50];
    int idade;
} Pessoa;

int main(void) {
    Pessoa p1 = {"Julia", 22};   // sem precisar escrever "struct Pessoa"
    printf("%s\n", p1.nome);
    return 0;
}
```

### Ponteiro para struct

```c
typedef struct {
    char nome[50];
    int idade;
} Pessoa;

void aniversario(Pessoa *p) {
    p->idade = p->idade + 1;   // "->" acessa campo de struct ATRAVES de um ponteiro
    // equivale a (*p).idade = (*p).idade + 1;
}

int main(void) {
    Pessoa p1 = {"Julia", 22};
    aniversario(&p1);
    printf("%d\n", p1.idade);   // 23
    return 0;
}
```

**Regra prática:** use `.` quando você tem a struct diretamente, use `->` quando você tem um ponteiro para a struct.

---

## 15. Enums — nomes para conjuntos de constantes

```c
enum DiaSemana { SEGUNDA, TERCA, QUARTA, QUINTA, SEXTA, SABADO, DOMINGO };
// por baixo dos panos: SEGUNDA=0, TERCA=1, QUARTA=2... incrementando

enum DiaSemana hoje = QUARTA;

if (hoje == QUARTA) {
    printf("Meio da semana\n");
}
```

---

## 16. Arquivos (`<stdio.h>` para I/O de arquivos)

```c
#include <stdio.h>

int main(void) {
    FILE *arquivo = fopen("dados.txt", "w");  // "w" = write (escrita, cria/sobrescreve)

    if (arquivo == NULL) {
        printf("Erro ao abrir arquivo\n");
        return 1;
    }

    fprintf(arquivo, "Nome: %s\nIdade: %d\n", "Vinicius", 25);
    fclose(arquivo);   // sempre feche o arquivo depois de usar

    // lendo de volta
    FILE *leitura = fopen("dados.txt", "r");   // "r" = read
    char linha[100];

    while (fgets(linha, sizeof(linha), leitura) != NULL) {
        printf("%s", linha);
    }
    fclose(leitura);

    return 0;
}
```

Modos comuns: `"r"` leitura, `"w"` escrita (sobrescreve), `"a"` append (adiciona ao final), `"rb"`/`"wb"` versões binárias.

---

## 17. Erros comuns de quem está começando em C

| Erro | Por que acontece | Como evitar |
|---|---|---|
| Esquecer `;` no fim da linha | C exige em toda instrução | Vá compilando com frequência, não só no final |
| Usar `=` em vez de `==` numa comparação | `if (x = 5)` é uma ATRIBUIÇÃO válida (sempre "verdadeira"), não uma comparação | Preste atenção redobrada em `if`/`while` |
| Esquecer `&` no `scanf` | `scanf` precisa do endereço | Regra: sempre `&variavel`, exceto para vetores/strings |
| Acessar índice fora do array | C **não verifica limites** — `vetor[10]` num array de tamanho 5 não trava, apenas lê memória "lixo" ou corrompe outra variável | Sempre controle os limites manualmente nos loops |
| Esquecer `free()` depois de `malloc()` | Causa vazamento de memória | Todo `malloc` tem que ter um `free` correspondente |
| Comparar strings com `==` | `==` compara os ENDEREÇOS dos vetores, não o conteúdo | Use `strcmp()` |
| Divisão inteira inesperada | `5 / 2` dá `2`, não `2.5` | Use `float`/`double` em pelo menos um dos operandos |
| Esquecer `break` no `switch` | Causa "fall-through" para o próximo `case` | Sempre finalize cada `case` com `break` (a menos que seja intencional) |

---

## 18. Tabela-resumo rápida (consulta relâmpago)

```c
#include <stdio.h>     // biblioteca de entrada/saida
#include <stdlib.h>    // malloc, free, exit
#include <string.h>    // funcoes de string
#include <stdbool.h>   // bool, true, false
#include <math.h>      // sqrt, pow, funcoes matematicas

int main(void) {
    // variaveis
    int i = 0; float f = 1.5f; double d = 2.5; char c = 'A'; 

    // condicional
    if (i == 0) { } else { }

    // laco
    for (int k = 0; k < 10; k++) { }
    while (i < 10) { }
    do { } while (i < 10);

    // vetor
    int v[5] = {1,2,3,4,5};

    // ponteiro
    int *p = &i;
    *p = 10;

    // string
    char s[20] = "texto";

    // funcao (ver secao 13)

    return 0;
}
```

---

## 19. Próximos passos sugeridos de estudo

1. Fixar bem `if/for/while` e vetores com exercícios pequenos (calculadora, tabuada, maior/menor de uma lista).
2. Praticar ponteiros isoladamente antes de misturar com struct (é o maior salto de dificuldade).
3. Implementar uma lista ligada simples (struct + ponteiro para o próximo nó) — clássico exercício que fixa ponteiro + struct + malloc juntos.
4. Só depois disso partir para C++ ou para back-end em linguagens de mais alto nível — você vai entender *por que* elas existem e o que estão poupando você de fazer manualmente (gerenciamento de memória, strings prontas, coleções dinâmicas).
