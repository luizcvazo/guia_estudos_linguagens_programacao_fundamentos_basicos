# Guia de Sintaxe da Linguagem Python

> Guia objetivo e prático para aprender a sintaxe de Python e escrever programas simples. Feito para quem já entende (ou está aprendendo) lógica de programação e algoritmos, mas quer destravar a sintaxe. Todos os exemplos podem ser copiados e executados.

---

## 1. O que é Python e por que ele importa

Python é uma linguagem criada por **Guido van Rossum** e lançada em **1991**. O foco dela sempre foi a **legibilidade**: o código deve ser fácil de ler, quase como texto em inglês. Hoje é uma das linguagens mais usadas do mundo e uma das mais recomendadas para quem está começando.

- É a linguagem dominante em **ciência de dados, inteligência artificial e machine learning** (pandas, NumPy, scikit-learn, PyTorch, TensorFlow).
- É muito usada em **automação e scripts** (renomear arquivos, ler planilhas, integrar sistemas).
- Tem grande presença em **back-end web** (Django, Flask, FastAPI).
- É muito comum em **ensino**, porque a sintaxe sai do caminho e deixa você focar na lógica.

### Características principais

| Característica | O que significa na prática |
|---|---|
| **Interpretada** | Você roda o arquivo `.py` direto, sem etapa separada de compilação. O interpretador executa o código linha a linha. |
| **Tipagem dinâmica** | Você não declara o tipo da variável. `x = 5` cria um `int`; depois `x = "texto"` é permitido (o tipo pertence ao *valor*, não ao nome). |
| **Fortemente tipada** | Apesar de dinâmica, Python não mistura tipos "na surdina": `"5" + 5` dá erro. Você converte explicitamente. |
| **Multiparadigma** | Aceita programação procedural, orientada a objetos e funcional. Você escolhe o estilo. |
| **Indentação obrigatória** | Blocos de código são definidos pelo **recuo** (espaços), não por `{ }`. É a marca registrada do Python. |
| **Alto nível** | Gerencia memória sozinho (coletor de lixo). Listas, dicionários e strings vêm prontos e são muito poderosos. |
| **"Baterias inclusas"** | A biblioteca padrão é enorme (datas, arquivos, JSON, matemática, expressões regulares...) e existe o `pip` para instalar milhares de pacotes de terceiros. |
| **Inteiros sem limite** | `int` cresce conforme a necessidade. Não existe overflow como em C. |

### Python vs C — as diferenças que importam

Se você já viu o guia de C, esta tabela ajuda a fazer a ponte entre as duas linguagens:

| C | Python |
|---|---|
| Compilada | Interpretada |
| Tipagem estática (`int x = 5;`) | Tipagem dinâmica (`x = 5`) |
| Blocos com `{ }` e instruções com `;` | Blocos com **indentação**, sem `;` |
| Memória manual (`malloc`/`free`) | Memória automática (coletor de lixo) |
| Ponteiros explícitos | Não existem ponteiros; tudo é referência a objetos |
| Strings são vetores de `char` com `\0` | `str` é um tipo pronto, com dezenas de métodos |
| Vetores de tamanho fixo | Listas dinâmicas (`append`, `remove`...) |
| `struct` para agrupar dados | Classes (e `dataclass`) |
| Sem exceções (retorna códigos de erro) | `try` / `except` nativo |
| `int` estoura (overflow) | `int` de tamanho ilimitado |
| Muito rápida, próxima do hardware | Mais lenta, mas muito mais produtiva de escrever |

**Resumo prático:** C te ensina *como o computador funciona*. Python te deixa *resolver o problema* sem se preocupar com esses detalhes. Um programa que leva 30 linhas em C muitas vezes leva 5 em Python.

### Quando Python é usado hoje

- **Ciência de dados e IA**: análise de dados, gráficos, treino de modelos.
- **Automação**: scripts para tarefas repetitivas (planilhas, arquivos, e-mails, web scraping).
- **Back-end e APIs**: Django, Flask, FastAPI.
- **Ensino e prototipagem**: testar uma ideia rápido antes de reescrever em algo mais performático.
- **Testes e DevOps**: scripts de infraestrutura e de teste.

Vindo de C, você vai sentir falta de: controle fino de memória e velocidade bruta. Em troca, ganha listas, dicionários, strings e tratamento de erros prontos, e uma sintaxe muito mais curta.

---

## 2. Estrutura mínima de um programa em Python

```python
print("Olá, mundo!")
```

É isso. Uma linha. Não existe `main` obrigatória, `#include` nem `return 0`.

Uma estrutura um pouco mais organizada, que você verá em muitos projetos:

```python
def main():
    print("Olá, mundo!")


if __name__ == "__main__":
    main()
```

**Explicando linha a linha:**

- `def main():` define uma função chamada `main`. Repare nos **dois-pontos** `:` no final: eles abrem um bloco.
- A linha seguinte está **recuada** (4 espaços). Esse recuo é o que diz "isto pertence à função". Sem ele, dá `IndentationError`.
- `if __name__ == "__main__":` significa "só execute isto se o arquivo foi rodado diretamente" (e não apenas importado por outro arquivo). Por enquanto, entenda como o "ponto de entrada" do programa.

### Rodando

```bash
python programa.py      # Windows / geral
python3 programa.py     # Linux / Mac (na maioria dos casos)
```

Também existe o **modo interativo** (REPL): digite `python` no terminal e teste linhas soltas, ótimo para experimentar a sintaxe:

```python
>>> 2 + 3
5
>>> "abc".upper()
'ABC'
```

Diferente de C, **não há compilação**. Erros de sintaxe aparecem quando o interpretador chega no trecho, e erros de lógica ou de tipo só aparecem em tempo de execução.

---

## 3. Comentários

```python
# comentário de uma linha

x = 10  # comentário no fim da linha

"""
Isto é uma string de várias linhas.
Muita gente usa como comentário de bloco,
mas o uso oficial é como 'docstring' (documentação).
"""
```

### Docstring — documentação de funções

```python
def somar(a, b):
    """Retorna a soma de a e b."""
    return a + b

help(somar)   # mostra a docstring
```

Python **não tem** comentário de bloco `/* */` como C. Para vários comentários seguidos, use `#` em cada linha.

---

## 4. Tipos de dados primitivos e básicos

Em Python você não escolhe "quantos bytes" usar. Os tipos principais são poucos:

| Tipo | O que guarda | Exemplo |
|---|---|---|
| `int` | número inteiro (sem limite de tamanho) | `idade = 25` |
| `float` | número decimal | `preco = 9.99` |
| `str` | texto | `nome = "Ana"` |
| `bool` | verdadeiro/falso | `ativo = True` |
| `NoneType` | "nenhum valor" (`None`) | `resultado = None` |
| `complex` | número complexo (raro no dia a dia) | `z = 3 + 4j` |

Repare: `True`, `False` e `None` começam com **letra maiúscula**.

```python
idade = 30
altura = 1.75
nome = "Ana"
ativo = True
nada = None

print(type(idade))    # <class 'int'>
print(type(altura))   # <class 'float'>
print(type(nome))     # <class 'str'>
print(type(ativo))    # <class 'bool'>
print(type(nada))     # <class 'NoneType'>
```

### `type()` e `isinstance()`

```python
type(5)                   # <class 'int'>
isinstance(5, int)        # True
isinstance("a", (int, str))  # True: aceita uma tupla de tipos
```

### Conversão de tipos (casting)

Python não converte tipos incompatíveis sozinho. Você converte explicitamente:

```python
int("42")        # 42
float("3.14")    # 3.14
str(100)         # "100"
bool(0)          # False
bool("texto")    # True
int(3.99)        # 3  (trunca, não arredonda)

# int("abc")     # ValueError: não dá para converter
# "5" + 5        # TypeError: não mistura str com int
```

### Sem limite para inteiros

```python
print(2 ** 100)   # 1267650600228229401496703205376
```

Em C isso estouraria. Em Python funciona normalmente.

### Cuidado com `float`

```python
print(0.1 + 0.2)   # 0.30000000000000004
```

Isso não é bug de Python: é como computadores representam decimais em binário (o mesmo vale em C e Java). Para dinheiro ou precisão exata, use o módulo `decimal`.

---

## 5. Variáveis — criação, atribuição e convenções

Em Python **não há declaração**: a variável passa a existir quando você atribui um valor.

```python
idade = 25              # cria a variável
idade = 26              # muda o valor
idade = "vinte e seis"  # permitido! muda até o tipo (mas evite)

a, b, c = 1, 2, 3       # atribuição múltipla
x = y = 0               # mesmo valor para várias variáveis
a, b = b, a             # troca de valores (sem variável temporária!)
```

**Regras de nomenclatura:**

- Pode ter letras, números e `_`, mas **não pode começar com número**.
- Case-sensitive: `idade` e `Idade` são variáveis diferentes.
- Convenção da comunidade (PEP 8): **`snake_case`** para variáveis e funções (`nome_completo`), **`PascalCase`** para classes (`ContaBancaria`) e **`MAIÚSCULAS`** para constantes (`PI`).
- Palavras reservadas (`if`, `for`, `class`, `def`, `None`...) não podem ser usadas como nome.

### Constantes

Python **não tem** `const`. Por convenção, escreve-se em maiúsculas para indicar "não altere":

```python
PI = 3.14159
LIMITE_MAXIMO = 100
```

Nada impede tecnicamente de reatribuir, mas quem lê o código entende o recado.

### Tipagem dinâmica na prática

```python
x = 10
print(type(x))   # <class 'int'>

x = "dez"
print(type(x))   # <class 'str'>
```

O nome `x` é só uma "etiqueta" que aponta para um valor. Você pode mover a etiqueta para outro valor de outro tipo. (Veja a seção 14 para entender isso melhor.)

### Anotações de tipo (opcional)

Você **pode** documentar o tipo esperado, mas o Python não obriga nem verifica em tempo de execução:

```python
idade: int = 25
nome: str = "Ana"

def somar(a: int, b: int) -> int:
    return a + b
```

Ferramentas como `mypy` e editores usam essas dicas para avisar erros.

---

## 6. Entrada e saída (`print` e `input`)

### `print` — mostrando na tela

```python
print("Olá!")
print("Idade:", 25)                  # separa por espaço automaticamente
print("a", "b", "c", sep="-")        # a-b-c
print("Sem quebra de linha", end="")  # não pula linha no final
print()                               # linha em branco
```

### f-strings — a forma moderna de formatar texto

Coloque um `f` antes das aspas e escreva variáveis e expressões dentro de `{ }`:

```python
nome = "Ana"
idade = 28
altura = 1.6543

print(f"Olá, {nome}! Você tem {idade} anos.")
print(f"Ano que vem: {idade + 1}")
print(f"Altura: {altura:.2f}m")       # 2 casas decimais -> 1.65m
print(f"Preço: R$ {1234.5:,.2f}")     # separador de milhar -> R$ 1,234.50
print(f"{idade:05d}")                 # preenche com zeros -> 00028
print(f"{nome:>10}|")                 # alinha à direita, largura 10
```

É o equivalente do `printf` de C, mas sem decorar `%d`, `%f`, `%s`. Você escreve a variável direto no lugar.

| Formato | O que faz | Exemplo |
|---|---|---|
| `{x}` | valor normal | `f"{5}"` → `5` |
| `{x:.2f}` | float com 2 casas | `f"{3.14159:.2f}"` → `3.14` |
| `{x:d}` | inteiro | `f"{42:d}"` → `42` |
| `{x:05d}` | zeros à esquerda | `f"{7:05d}"` → `00007` |
| `{x:,}` | separador de milhar | `f"{1000000:,}"` → `1,000,000` |
| `{x:>8}` / `{x:<8}` / `{x:^8}` | alinha direita / esquerda / centro | |
| `{x:.1%}` | porcentagem | `f"{0.256:.1%}"` → `25.6%` |

### `input` — lendo do usuário

```python
nome = input("Digite seu nome: ")
idade = int(input("Digite sua idade: "))   # input SEMPRE devolve str!

print(f"Olá, {nome}! Ano que vem você terá {idade + 1} anos.")
```

**Ponto crítico:** `input()` **sempre** devolve texto (`str`). Se você quer número, precisa converter com `int()` ou `float()`. Esquecer isso é o erro mais comum de quem começa:

```python
n = input("Número: ")     # digitou 5
print(n + n)               # "55" (concatenou texto!), não 10
print(int(n) + int(n))     # 10
```

Diferente do `scanf` de C, não existe `&`, nem especificadores de formato, nem buffer para gerenciar.

---

## 7. Operadores

### Aritméticos

```python
a, b = 10, 3

a + b     # 13     soma
a - b     # 7      subtração
a * b     # 30     multiplicação
a / b     # 3.3333333333333335   divisão (SEMPRE devolve float)
a // b    # 3      divisão inteira (descarta a parte decimal)
a % b     # 1      resto da divisão (módulo)
a ** b    # 1000   potência (10 elevado a 3)
```

**Diferença chave em relação a C:** em Python, `10 / 3` dá `3.333...` (float), sem armadilha de divisão inteira. Para a divisão inteira você usa `//` de propósito.

```python
7 // 2      # 3
-7 // 2     # -4  (arredonda para BAIXO, não para zero; diferente de C)
7 % 3       # 1
-7 % 3      # 2   (o resto tem o sinal do divisor)
```

### Atribuição composta

```python
x = 10
x += 5     # x = x + 5   -> 15
x -= 3     # x = x - 3   -> 12
x *= 2     # x = x * 2   -> 24
x /= 4     # x = x / 4   -> 6.0 (vira float)
x //= 4    # x = x // 4  -> 1.0
x **= 2    # x = x ** 2
x %= 4     # x = x % 4
```

### Não existe `++` nem `--`

```python
i = 5
i += 1     # incrementa (i++ não existe em Python!)
i -= 1     # decrementa
```

`i++` dá **erro de sintaxe** em Python. Sempre use `+= 1`.

### Relacionais (retornam `True` ou `False`)

```python
a == b    # igual
a != b    # diferente
a > b     # maior
a < b     # menor
a >= b    # maior ou igual
a <= b    # menor ou igual

# Python permite encadear comparações (muito prático):
1 < x < 10          # equivale a (1 < x) and (x < 10)
18 <= idade <= 65
```

### Lógicos (são **palavras**, não símbolos)

```python
a and b    # E  (AND): True se ambos forem verdadeiros
a or b     # OU (OR):  True se pelo menos um for verdadeiro
not a      # NÃO (NOT): inverte o valor
```

Em C você usa `&&`, `||` e `!`. Em Python: `and`, `or`, `not`.

### Operadores de identidade e pertencimento

```python
x is None            # o MESMO objeto? (use para None)
x is not None

"a" in "banana"      # True: está contido?
3 in [1, 2, 3]       # True
"chave" in {"chave": 1}   # True (procura nas chaves)
5 not in [1, 2, 3]   # True
```

### `==` vs `is`

```python
a = [1, 2, 3]
b = [1, 2, 3]
a == b    # True:  têm o mesmo CONTEÚDO
a is b    # False: são objetos DIFERENTES na memória
```

Use `==` para comparar valores. Use `is` (basicamente) só para `None`, `True` e `False`.

### Verdadeiro e falso (truthiness)

Em Python, valores "vazios" ou "zero" contam como **falsos**; o resto conta como verdadeiro:

| Valores **falsos** | Valores verdadeiros |
|---|---|
| `False`, `None` | `True` |
| `0`, `0.0` | qualquer número diferente de 0 |
| `""` (string vazia) | qualquer texto não vazio |
| `[]`, `()`, `{}`, `set()` (vazios) | coleções com pelo menos 1 elemento |

```python
lista = []
if not lista:
    print("Lista vazia")   # jeito idiomático de testar "está vazia?"

nome = ""
if nome:
    print("Tem nome")       # não imprime, string vazia é falsa
```

---

## 8. Estruturas condicionais

### `if / elif / else`

```python
nota = 75

if nota >= 90:
    print("A")
elif nota >= 70:
    print("B")
elif nota >= 50:
    print("C")
else:
    print("Reprovado")
```

Pontos de atenção (é a **primeira coisa** que muda vindo de C):

- Não há parênteses obrigatórios em volta da condição (`if nota >= 90:`).
- Termina com **dois-pontos** `:`.
- O bloco é definido pelo **recuo** (4 espaços). Nada de `{ }`.
- Escreve-se `elif` (e não `else if`).

### Erro clássico de indentação

```python
if x > 0:
print("positivo")     # IndentationError: falta o recuo!
```

### Operador ternário (expressão condicional)

```python
idade = 20
status = "maior de idade" if idade >= 18 else "menor de idade"
print(status)
```

Lê-se: "valor se a condição for verdadeira, `if` condição, `else` outro valor".

### `match / case` (Python 3.10+)

O equivalente moderno do `switch` de C. **Não precisa de `break`** e não tem "fall-through":

```python
dia = 3

match dia:
    case 1:
        print("Segunda")
    case 2:
        print("Terça")
    case 3:
        print("Quarta")
    case 4 | 5:               # vários valores com |
        print("Quinta ou sexta")
    case _:                   # _ é o "default"
        print("Dia inválido")
```

Se você usa uma versão anterior à 3.10, use `if / elif / else`.

### Condições combinadas

```python
idade = 25
tem_carteira = True

if idade >= 18 and tem_carteira:
    print("Pode dirigir")

if idade < 18 or not tem_carteira:
    print("Não pode dirigir")
```

---

## 9. Estruturas de repetição

### `for` — percorre uma sequência

O `for` de Python **não é** o `for (i = 0; i < n; i++)` de C. Ele percorre os elementos de uma sequência ("for each"):

```python
for i in range(5):
    print(i)          # 0 1 2 3 4
```

`range` gera uma sequência de números:

```python
range(5)          # 0, 1, 2, 3, 4
range(2, 6)       # 2, 3, 4, 5
range(0, 10, 2)   # 0, 2, 4, 6, 8   (início, fim exclusivo, passo)
range(5, 0, -1)   # 5, 4, 3, 2, 1   (contagem regressiva)
```

O valor final é **exclusivo** (`range(5)` termina em 4).

### Percorrendo listas e textos diretamente

```python
frutas = ["maçã", "banana", "uva"]

for fruta in frutas:
    print(fruta)

for letra in "Python":
    print(letra)
```

### `enumerate` — índice e valor juntos

```python
for i, fruta in enumerate(frutas):
    print(f"{i}: {fruta}")          # 0: maçã, 1: banana...

for i, fruta in enumerate(frutas, start=1):
    print(f"{i}º: {fruta}")         # começa a contar do 1
```

### `zip` — percorrendo duas listas juntas

```python
nomes = ["Ana", "Bruno", "Carla"]
notas = [8.5, 7.0, 9.2]

for nome, nota in zip(nomes, notas):
    print(f"{nome}: {nota}")
```

### `while`

```python
i = 0
while i < 5:
    print(i)
    i += 1
```

### Não existe `do-while`

Python não tem `do { } while`. O padrão equivalente é:

```python
while True:
    opcao = int(input("Digite 0 para sair: "))
    if opcao == 0:
        break
```

### `break`, `continue` e o `else` do laço

```python
for i in range(10):
    if i == 5:
        break            # sai do laço imediatamente
    if i % 2 == 0:
        continue         # pula para a próxima iteração
    print(i)             # imprime apenas 1 3
```

Python tem um recurso curioso: o `else` de um laço só executa se o laço terminou **sem** `break`:

```python
for n in [2, 4, 6]:
    if n % 2 != 0:
        print("Achei um ímpar")
        break
else:
    print("Todos são pares")   # este é o que imprime
```

### Laços aninhados

```python
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i} x {j} = {i * j}")
```

### `pass` — bloco vazio

Python exige que um bloco tenha conteúdo. Se quiser deixar "para depois", use `pass`:

```python
if x > 0:
    pass     # TODO: implementar depois
```

---

## 10. Listas, tuplas e matrizes

### Listas — o "vetor" do Python

Uma **lista** guarda vários valores em ordem. É **dinâmica** (cresce e diminui) e **mutável** (pode ser alterada). Pode misturar tipos, mas geralmente guarda itens do mesmo tipo.

```python
numeros = [10, 20, 30, 40]
vazia = []
mista = [1, "dois", 3.0, True]

print(numeros[0])      # 10  (índices começam em 0)
print(numeros[-1])     # 40  (índice negativo: conta a partir do fim)
numeros[2] = 99        # altera o elemento de índice 2
print(len(numeros))    # 4   (tamanho)
```

Diferente de C: sem tamanho fixo, sem `sizeof`, sem estouro silencioso. Acessar um índice inexistente dá `IndexError` (em vez de ler memória "lixo").

### Métodos mais usados

```python
frutas = ["maçã", "banana"]

frutas.append("uva")          # adiciona no fim
frutas.insert(1, "pera")      # insere na posição 1
frutas.remove("banana")       # remove pelo valor
ultimo = frutas.pop()         # remove e devolve o último
frutas.pop(0)                 # remove o de índice 0
frutas.sort()                 # ordena (altera a própria lista)
frutas.reverse()              # inverte
frutas.extend(["kiwi", "manga"])  # junta outra lista no final

"maçã" in frutas              # True/False
frutas.index("uva")           # posição do item
frutas.count("uva")           # quantas vezes aparece

sorted(frutas)                # devolve uma NOVA lista ordenada
len(frutas)  ; max([3, 1, 2]) ; min([3, 1, 2]) ; sum([1, 2, 3])
```

### Fatiamento (slicing)

Uma das melhores ferramentas do Python. Formato: `lista[início:fim:passo]` (o fim é **exclusivo**).

```python
n = [0, 1, 2, 3, 4, 5]

n[1:4]     # [1, 2, 3]
n[:3]      # [0, 1, 2]     (do começo até o índice 3)
n[3:]      # [3, 4, 5]     (do índice 3 até o fim)
n[::2]     # [0, 2, 4]     (de 2 em 2)
n[::-1]    # [5, 4, 3, 2, 1, 0]  (invertida)
n[-2:]     # [4, 5]        (últimos dois)
```

Funciona igual em **strings** e tuplas.

### Percorrendo uma lista

```python
notas = [7, 8, 5, 9, 10]

for nota in notas:
    print(nota)

for i in range(len(notas)):
    print(f"Nota {i}: {notas[i]}")
```

### List comprehension — criar listas em uma linha

```python
quadrados = [x ** 2 for x in range(5)]              # [0, 1, 4, 9, 16]
pares = [x for x in range(10) if x % 2 == 0]        # [0, 2, 4, 6, 8]
maiusculas = [s.upper() for s in ["a", "b"]]        # ["A", "B"]
```

Lê-se: "`x ** 2` para cada `x` em `range(5)`". É o jeito "pythônico" de substituir um `for` que só monta uma lista.

### Tuplas — listas imutáveis

```python
ponto = (3, 4)
x, y = ponto           # desempacotamento
print(ponto[0])        # 3
# ponto[0] = 10        # TypeError: tuplas não podem ser alteradas
```

Use tupla quando os dados não devem mudar (coordenadas, cores RGB, retorno de várias coisas de uma função).

### Matrizes (listas de listas)

```python
tabuleiro = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

print(tabuleiro[1][2])   # 6  (linha 1, coluna 2)

for linha in tabuleiro:
    for valor in linha:
        print(valor, end=" ")
    print()
```

**Cuidado ao criar matrizes vazias:**

```python
errado = [[0] * 3] * 3                    # 3 referências para a MESMA linha!
certo = [[0] * 3 for _ in range(3)]       # 3 linhas independentes
```

---

## 11. Strings — texto pronto para usar

Ao contrário de C, Python **tem um tipo `str` de verdade**. Sem `\0`, sem vetor de `char`, sem `strcpy`. Aspas simples ou duplas funcionam igual:

```python
nome = "Ana"
outro = 'Bruno'
frase = "Ela disse: 'oi'"
multilinha = """Linha 1
Linha 2"""
```

### Strings são imutáveis

```python
s = "casa"
# s[0] = "m"       # TypeError!
s = "m" + s[1:]    # cria uma nova string: "masa"
```

### Operações básicas

```python
nome = "Ana"
sobrenome = "Silva"

nome + " " + sobrenome    # "Ana Silva"  (concatenação)
nome * 3                  # "AnaAnaAna"  (repetição)
len(nome)                 # 3
nome[0]                   # "A"
nome[-1]                  # "a"
nome[0:2]                 # "An"
"na" in "banana"          # True
"Ana" == "Ana"            # True   (== compara o CONTEÚDO; sem strcmp!)
```

### Métodos de string

```python
s = "  Olá, Mundo!  "

s.upper()              # "  OLÁ, MUNDO!  "
s.lower()              # "  olá, mundo!  "
s.strip()              # "Olá, Mundo!"   (remove espaços das pontas)
s.replace("Mundo", "Python")   # "  Olá, Python!  "
s.title()              # "  Olá, Mundo!  "  (primeira letra de cada palavra)

"a,b,c".split(",")            # ["a", "b", "c"]
"-".join(["a", "b", "c"])     # "a-b-c"
"python".startswith("py")     # True
"python".endswith("on")       # True
"banana".find("na")           # 2  (posição; -1 se não achar)
"banana".count("a")           # 3
"123".isdigit()               # True
"abc".isalpha()               # True
```

| Tarefa | C | Python |
|---|---|---|
| Tamanho | `strlen(s)` | `len(s)` |
| Copiar | `strcpy(a, b)` | `a = b` |
| Concatenar | `strcat(a, b)` | `a + b` |
| Comparar | `strcmp(a, b) == 0` | `a == b` |
| Maiúsculas | (loop com `toupper`) | `s.upper()` |

### Caracteres de escape

```python
print("Linha 1\nLinha 2")     # \n = quebra de linha
print("Coluna1\tColuna2")     # \t = tabulação
print("Aspas: \"oi\"")        # \" = aspas
print("Barra: \\")            # \\ = barra invertida
print(r"C:\pasta\novo")       # r"..." = raw string: ignora escapes
```

---

## 12. Dicionários e conjuntos

### Dicionário (`dict`) — pares chave → valor

Um dicionário associa uma **chave** a um **valor**, como uma agenda de contatos. Não existe algo assim pronto em C.

```python
pessoa = {
    "nome": "Ana",
    "idade": 28,
    "cidade": "São Paulo",
}

print(pessoa["nome"])              # Ana
pessoa["idade"] = 29                # altera
pessoa["profissao"] = "Dev"         # adiciona nova chave
del pessoa["cidade"]                # remove

print(pessoa.get("email"))          # None (não dá erro se a chave não existe)
print(pessoa.get("email", "n/d"))   # "n/d" (valor padrão)
# print(pessoa["email"])            # KeyError!

"nome" in pessoa                    # True (procura nas chaves)
len(pessoa)                         # quantidade de pares
```

### Percorrendo um dicionário

```python
for chave in pessoa:
    print(chave)

for valor in pessoa.values():
    print(valor)

for chave, valor in pessoa.items():
    print(f"{chave}: {valor}")
```

### Exemplo prático: contar ocorrências

```python
texto = "banana"
contagem = {}

for letra in texto:
    contagem[letra] = contagem.get(letra, 0) + 1

print(contagem)   # {'b': 1, 'a': 3, 'n': 2}
```

### Dict comprehension

```python
quadrados = {n: n ** 2 for n in range(1, 4)}   # {1: 1, 2: 4, 3: 9}
```

### Conjunto (`set`) — itens únicos, sem ordem

```python
numeros = {1, 2, 2, 3, 3, 3}
print(numeros)               # {1, 2, 3}  (duplicados somem)

numeros.add(4)
numeros.remove(1)

a = {1, 2, 3}
b = {3, 4, 5}
a | b      # {1, 2, 3, 4, 5}  união
a & b      # {3}              interseção
a - b      # {1, 2}           diferença

unicos = list(set([1, 1, 2, 2, 3]))   # truque: remover duplicados de uma lista
vazio = set()                          # {} sozinho cria um dict, não um set!
```

### Qual coleção usar?

| Preciso de... | Use |
|---|---|
| Sequência ordenada e alterável | `list` |
| Sequência ordenada que não muda | `tuple` |
| Buscar valor por uma chave/nome | `dict` |
| Itens únicos / testar pertencimento rápido | `set` |

---

## 13. Funções

```python
def somar(a, b):
    return a + b

resultado = somar(5, 3)
print(resultado)   # 8
```

Estrutura: `def nome(parâmetros):`, dois-pontos, bloco recuado. Sem tipo de retorno obrigatório, sem protótipo (mas a função precisa ser **definida antes de ser chamada** na execução).

### Função sem `return`

```python
def saudacao(nome):
    print(f"Olá, {nome}!")

r = saudacao("Ana")
print(r)    # None (sem return, a função devolve None)
```

### Valores padrão

```python
def saudacao(nome, saudacao="Olá"):
    print(f"{saudacao}, {nome}!")

saudacao("Ana")              # Olá, Ana!
saudacao("Ana", "Bom dia")   # Bom dia, Ana!
```

### Argumentos nomeados

```python
def criar_usuario(nome, idade, ativo=True):
    return {"nome": nome, "idade": idade, "ativo": ativo}

criar_usuario("Ana", 28)
criar_usuario(idade=28, nome="Ana", ativo=False)   # ordem livre com nomes
```

### Retornando vários valores

```python
def min_max(lista):
    return min(lista), max(lista)     # na verdade devolve uma tupla

menor, maior = min_max([3, 1, 4, 1, 5])
print(menor, maior)    # 1 5
```

Em C você precisaria de ponteiros ou de uma `struct` para isso.

### `*args` e `**kwargs`

```python
def somar_tudo(*numeros):        # aceita QUANTOS argumentos quiser
    return sum(numeros)

somar_tudo(1, 2, 3, 4)           # 10

def mostrar(**dados):            # aceita argumentos nomeados extras
    for k, v in dados.items():
        print(f"{k} = {v}")

mostrar(nome="Ana", idade=28)
```

### Funções lambda (anônimas, de uma linha)

```python
dobro = lambda x: x * 2
print(dobro(5))    # 10

nomes = ["carlos", "ana", "bia"]
nomes.sort(key=lambda s: len(s))   # ordena pelo tamanho
```

### Escopo de variáveis

```python
x = 10                 # global

def teste():
    y = 5              # local: só existe dentro da função
    print(x, y)        # lê a global normalmente

teste()
# print(y)             # NameError: y não existe aqui fora

def alterar():
    global x           # necessário para ALTERAR a global
    x = 99
```

Evite `global` sempre que puder. Prefira receber por parâmetro e devolver com `return`.

### Recursão

```python
def fatorial(n):
    if n <= 1:                 # caso base
        return 1
    return n * fatorial(n - 1) # chamada recursiva

print(fatorial(5))   # 120
```

---

## 14. Mutabilidade e referências (a "versão Python" dos ponteiros)

Python **não tem ponteiros**, e essa é uma das maiores diferenças em relação a C. Ainda assim, entender *o que acontece por baixo* evita bugs difíceis.

**Ideia central:** em Python, uma variável é uma **etiqueta que aponta para um objeto**. Atribuir (`b = a`) não copia o objeto: cria outra etiqueta apontando para o mesmo objeto.

```python
a = [1, 2, 3]
b = a            # b aponta para a MESMA lista que a
b.append(4)
print(a)         # [1, 2, 3, 4]  a também mudou!
print(a is b)    # True
```

Para copiar de verdade:

```python
c = a.copy()      # ou list(a) ou a[:]  (cópia "rasa")
c.append(5)
print(a)          # não mudou
print(a is c)     # False

import copy
d = copy.deepcopy(matriz)   # cópia profunda (necessária para listas de listas)
```

### Mutáveis vs imutáveis

| Imutáveis (não mudam "no lugar") | Mutáveis (mudam "no lugar") |
|---|---|
| `int`, `float`, `bool`, `str`, `tuple`, `None` | `list`, `dict`, `set`, objetos de classes |

### Passagem para funções

Python passa o **objeto por referência** (o nome oficial é "*pass by assignment*"). O efeito prático depende de o objeto ser mutável ou não:

```python
def dobrar_numero(n):
    n = n * 2          # cria um NOVO int; o original não é afetado

def adicionar_item(lista):
    lista.append(99)   # altera a MESMA lista do chamador

x = 10
dobrar_numero(x)
print(x)               # 10  (imutável: não mudou)

nums = [1, 2]
adicionar_item(nums)
print(nums)            # [1, 2, 99]  (mutável: mudou!)
```

Compare com C: lá, para alterar o original você precisa passar `&valor` e usar `*ponteiro`. Em Python, o comportamento depende do **tipo do objeto**. Se quiser que uma função "altere" um número, faça ela **devolver** o novo valor:

```python
def dobrar(n):
    return n * 2

x = dobrar(x)
```

### Armadilha: argumento padrão mutável

```python
def adicionar(item, lista=[]):     # ERRADO: a lista é criada UMA vez só
    lista.append(item)
    return lista

print(adicionar(1))   # [1]
print(adicionar(2))   # [1, 2]   surpresa!

def adicionar(item, lista=None):   # CERTO
    if lista is None:
        lista = []
    lista.append(item)
    return lista
```

### Memória

Sem `malloc` e `free`. O Python libera objetos sozinho quando nada mais aponta para eles (coletor de lixo com contagem de referências). Você só se preocupa com a lógica.

---

## 15. Classes e objetos (o "struct" turbinado)

Onde C usa `struct` para agrupar dados, Python usa **classes**, que agrupam dados **e** comportamentos (métodos).

```python
class Pessoa:
    def __init__(self, nome, idade):    # construtor
        self.nome = nome                # atributos
        self.idade = idade

    def apresentar(self):               # método
        return f"Sou {self.nome} e tenho {self.idade} anos."

    def fazer_aniversario(self):
        self.idade += 1


p1 = Pessoa("Ana", 28)
print(p1.nome)              # Ana
print(p1.apresentar())      # Sou Ana e tenho 28 anos.
p1.fazer_aniversario()
print(p1.idade)             # 29
```

**Explicando:**

- `class Pessoa:` define o "molde".
- `__init__` é chamado automaticamente ao criar um objeto (`Pessoa("Ana", 28)`).
- `self` é o próprio objeto. **Todo método precisa dele como primeiro parâmetro**, mas você **não** o passa ao chamar. Ele é o equivalente ao `this` de outras linguagens.
- Acessa-se atributos com `.` (sempre `.`, nunca `->`).

### Herança

```python
class Aluno(Pessoa):                       # Aluno herda de Pessoa
    def __init__(self, nome, idade, curso):
        super().__init__(nome, idade)      # chama o construtor da classe-mãe
        self.curso = curso

    def apresentar(self):                  # sobrescreve o método
        return f"{super().apresentar()} Estudo {self.curso}."

a = Aluno("Bruno", 20, "Engenharia")
print(a.apresentar())
```

### `dataclass` — o jeito curto de criar "structs"

Quando a classe só guarda dados, a `dataclass` gera `__init__`, `__repr__` e comparação automaticamente:

```python
from dataclasses import dataclass

@dataclass
class Ponto:
    x: float
    y: float

p = Ponto(3, 4)
print(p)               # Ponto(x=3, y=4)
print(p.x)             # 3
print(p == Ponto(3, 4))   # True
```

É o mais próximo do `struct` de C, só que bem mais prático.

---

## 16. Enums — nomes para conjuntos de constantes

```python
from enum import Enum

class DiaSemana(Enum):
    SEGUNDA = 1
    TERCA = 2
    QUARTA = 3
    QUINTA = 4
    SEXTA = 5

hoje = DiaSemana.QUARTA

if hoje == DiaSemana.QUARTA:
    print("Meio da semana")

print(hoje.name)     # QUARTA
print(hoje.value)    # 3

for dia in DiaSemana:
    print(dia.name)
```

---

## 17. Arquivos

O jeito recomendado é usar `with`, que **fecha o arquivo automaticamente**, mesmo se der erro (nada de esquecer o `fclose`):

```python
# escrevendo ("w" sobrescreve; cria se não existir)
with open("dados.txt", "w", encoding="utf-8") as arquivo:
    arquivo.write("Nome: Ana\n")
    arquivo.write("Idade: 25\n")

# lendo tudo de uma vez
with open("dados.txt", "r", encoding="utf-8") as arquivo:
    conteudo = arquivo.read()
    print(conteudo)

# lendo linha por linha (bom para arquivos grandes)
with open("dados.txt", "r", encoding="utf-8") as arquivo:
    for linha in arquivo:
        print(linha.strip())     # strip() remove o \n do fim

# adicionando ao final ("a" = append)
with open("dados.txt", "a", encoding="utf-8") as arquivo:
    arquivo.write("Cidade: São Paulo\n")
```

Modos comuns: `"r"` leitura, `"w"` escrita (sobrescreve), `"a"` adicionar ao final, `"rb"` / `"wb"` binário.

Sempre passe `encoding="utf-8"` para textos com acentos, evitando problemas em alguns sistemas.

### Trabalhando com JSON

```python
import json

dados = {"nome": "Ana", "notas": [8, 9, 10]}

with open("dados.json", "w", encoding="utf-8") as f:
    json.dump(dados, f, ensure_ascii=False, indent=2)

with open("dados.json", "r", encoding="utf-8") as f:
    lido = json.load(f)
print(lido["nome"])
```

### Caminhos com `pathlib`

```python
from pathlib import Path

pasta = Path("meus_arquivos")
pasta.mkdir(exist_ok=True)

arquivo = pasta / "nota.txt"       # o / junta caminhos
arquivo.write_text("olá", encoding="utf-8")
print(arquivo.exists())            # True
print(arquivo.read_text(encoding="utf-8"))
```

---

## 18. Tratamento de erros (exceções)

Em C você checava retornos (`if (arquivo == NULL)`). Em Python, erros viram **exceções** que você captura com `try / except`:

```python
try:
    n = int(input("Digite um número: "))
    print(10 / n)
except ValueError:
    print("Isso não é um número!")
except ZeroDivisionError:
    print("Não dá para dividir por zero!")
else:
    print("Tudo certo!")            # só roda se NÃO houve erro
finally:
    print("Isto roda sempre.")      # roda com ou sem erro
```

### Exceções mais comuns

| Exceção | Quando acontece |
|---|---|
| `ValueError` | valor inválido: `int("abc")` |
| `TypeError` | tipo errado: `"5" + 5` |
| `ZeroDivisionError` | `10 / 0` |
| `IndexError` | índice fora da lista |
| `KeyError` | chave inexistente no dicionário |
| `FileNotFoundError` | arquivo não existe |
| `NameError` | variável não definida |
| `IndentationError` / `SyntaxError` | erro de escrita do código |

### Lançando seu próprio erro

```python
def dividir(a, b):
    if b == 0:
        raise ValueError("O divisor não pode ser zero")
    return a / b
```

### Lendo uma mensagem de erro (traceback)

Quando o programa quebra, o Python mostra um *traceback*. **Leia de baixo para cima**: a última linha diz o tipo e o motivo do erro; as de cima mostram por onde o programa passou.

```
Traceback (most recent call last):
  File "programa.py", line 3, in <module>
    print(10 / 0)
ZeroDivisionError: division by zero
```

---

## 19. Módulos, bibliotecas e ambiente virtual

### `import`

Equivale ao `#include` de C, só que mais inteligente:

```python
import math
print(math.sqrt(16))     # 4.0
print(math.pi)

from math import sqrt, pi     # importa só o que precisa
print(sqrt(25))

import random
print(random.randint(1, 6))   # número aleatório entre 1 e 6
print(random.choice(["a", "b", "c"]))

from datetime import datetime
print(datetime.now().strftime("%d/%m/%Y %H:%M"))
```

### Seus próprios módulos

Qualquer arquivo `.py` é um módulo. Se você tem `utils.py` com uma função `somar`:

```python
# em outro arquivo, na mesma pasta:
from utils import somar
print(somar(2, 3))
```

### Instalando pacotes com `pip`

```bash
pip install requests          # instala um pacote de terceiros
pip list                      # lista o que está instalado
pip freeze > requirements.txt # salva as dependências do projeto
pip install -r requirements.txt
```

### Ambiente virtual (`venv`)

Cada projeto deve ter seu "espaço isolado" de pacotes, para não misturar versões:

```bash
python -m venv .venv               # cria o ambiente
source .venv/bin/activate          # ativa (Linux/Mac)
.venv\Scripts\activate             # ativa (Windows)
deactivate                         # desativa
```

---

## 20. Erros comuns de quem está começando em Python

| Erro | Por que acontece | Como evitar |
|---|---|---|
| `IndentationError` | Recuo inconsistente ou ausente | Use sempre 4 espaços; não misture Tab com espaços; configure o editor |
| Esquecer o `:` no fim do `if`/`for`/`def`/`while` | Todo bloco em Python abre com dois-pontos | Confira a linha antes do bloco recuado |
| Usar `=` em vez de `==` numa comparação | `if x = 5:` dá `SyntaxError` (mais seguro que em C) | `=` atribui, `==` compara |
| Esquecer de converter `input()` | `input` sempre devolve `str` | `int(input(...))` ou `float(input(...))` |
| Tentar `i++` | Não existe em Python | Use `i += 1` |
| Misturar `str` e `int` | `"Idade: " + 25` dá `TypeError` | Use f-string: `f"Idade: {25}"` |
| `IndexError: list index out of range` | Índice maior que `len(lista) - 1` | Confira o tamanho; use `for x in lista` |
| Alterar uma lista enquanto percorre | Pula elementos ou dá comportamento estranho | Percorra uma cópia (`lista[:]`) ou crie uma lista nova |
| Esquecer o `self` num método | Método de classe precisa do `self` | `def metodo(self, ...):` |
| Argumento padrão mutável (`lista=[]`) | O objeto é criado uma única vez | Use `None` e crie dentro da função |
| Nome de variável igual ao de uma função nativa | `list = [1,2]` "apaga" o `list()` | Evite `list`, `str`, `sum`, `max`, `input`, `type`... |
| Confundir `=` com cópia | `b = a` cria só outro nome para o mesmo objeto | `b = a.copy()` |
| Comparar com `is` em vez de `==` | `is` compara identidade, não conteúdo | Use `==` (e `is` só para `None`) |
| Chamar a função sem parênteses | `funcao` é o objeto; `funcao()` executa | Não esqueça o `()` |

---

## 21. Tabela-resumo rápida (consulta relâmpago)

```python
# ---------- variáveis e tipos ----------
i = 0                    # int
f = 1.5                  # float
s = "texto"              # str
b = True                 # bool
n = None                 # nada

# ---------- entrada e saída ----------
nome = input("Nome: ")
print(f"Olá, {nome}!")

# ---------- condicional ----------
if i == 0:
    pass
elif i > 0:
    pass
else:
    pass

# ---------- laços ----------
for k in range(10):
    pass
while i < 10:
    i += 1

# ---------- coleções ----------
lista = [1, 2, 3]                 # list
tupla = (1, 2, 3)                 # tuple
dicio = {"a": 1, "b": 2}          # dict
conj = {1, 2, 3}                  # set

# ---------- comprehension ----------
quadrados = [x ** 2 for x in range(5)]

# ---------- função ----------
def somar(a, b=0):
    return a + b

# ---------- classe ----------
class Pessoa:
    def __init__(self, nome):
        self.nome = nome

# ---------- erros ----------
try:
    x = 10 / 0
except ZeroDivisionError:
    print("erro")

# ---------- arquivos ----------
with open("arq.txt", "w", encoding="utf-8") as arq:
    arq.write("oi")

# ---------- módulos ----------
import math
from random import randint
```

### Cola de sintaxe: C → Python

| Tarefa | C | Python |
|---|---|---|
| Imprimir | `printf("%d\n", x);` | `print(x)` |
| Ler inteiro | `scanf("%d", &x);` | `x = int(input())` |
| Se / senão se | `if () {} else if () {}` | `if: ... elif: ...` |
| Repetição contada | `for (i=0; i<n; i++)` | `for i in range(n):` |
| Bloco | `{ ... }` | indentação após `:` |
| Fim de instrução | `;` | quebra de linha |
| Vetor | `int v[5];` | `v = [0] * 5` |
| Tamanho do vetor | `sizeof(v)/sizeof(v[0])` | `len(v)` |
| Incremento | `i++` | `i += 1` |
| E / OU / NÃO | `&&`  `\|\|`  `!` | `and`  `or`  `not` |
| Comparar strings | `strcmp(a, b) == 0` | `a == b` |
| Alocar memória | `malloc` / `free` | automático |
| Agrupar dados | `struct` | `class` / `@dataclass` |
| Constante | `const int X = 5;` | `X = 5` (convenção) |
| Comentário | `//`  `/* */` | `#` |

---

## 22. Próximos passos sugeridos de estudo

1. **Fixar o básico com exercícios pequenos**: calculadora, tabuada, par ou ímpar, maior/menor de uma lista, contador de palavras, conversor de temperatura, jogo de adivinhar número.
2. **Dominar listas e dicionários**: são o coração do Python. Pratique fatiamento, comprehensions e os métodos mais comuns.
3. **Treinar leitura e escrita de arquivos**: monte um programa que salva e lê uma lista de tarefas (`.txt` ou `.json`).
4. **Aprender a ler erros (tracebacks)**: quase todo bug se resolve lendo a última linha da mensagem com calma.
5. **Fazer um mini-projeto completo**: uma agenda de contatos no terminal (dicionários + funções + arquivos + `try/except`) junta quase tudo deste guia.
6. **Só depois, escolher um caminho**: análise de dados (pandas, matplotlib), automação (requests, openpyxl), web (Flask/FastAPI), jogos (pygame) ou IA.

### Onde praticar e consultar

- Documentação oficial: <https://docs.python.org/pt-br/3/>
- Tutorial oficial em português: <https://docs.python.org/pt-br/3/tutorial/>
- Guia de estilo (PEP 8): <https://peps.python.org/pep-0008/>
- Praticar problemas: Exercism, HackerRank, Codewars, Beecrowd (em português)
- Testar código rápido no navegador: [Python Tutor](https://pythontutor.com) (mostra passo a passo o que acontece na memória, ótimo para entender variáveis e referências)
