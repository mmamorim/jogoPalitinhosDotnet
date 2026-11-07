# Bloco 01 – Bem-vindo ao bar

[← Voltar ao início](../README.md) · [Próximo: Bloco 02 →](02-sequencial.md)

> Toda sexta, a primeira coisa que o Seu Zé faz é perguntar o nome de quem chegou e separar três palitos para cada jogador. Seu programa vai começar do mesmo jeito.

## Objetivo

Neste bloco você vai aprender a **guardar informações** em variáveis e constantes, a **mostrar mensagens** na tela e a **ler o que o usuário digita**.

Ao final, o programa vai dar boas-vindas ao jogador e mostrar quantos palitos cada um tem.

---

## Conceitos

### Tipos de dados

Todo valor que o programa usa tem um **tipo**. O tipo diz que espécie de valor é aquele e o que dá para fazer com ele.

| Tipo | Guarda | Exemplos |
| --- | --- | --- |
| `int` | Números inteiros | `3`, `0`, `-15` |
| `double` | Números com casas decimais | `3.5`, `0.25` |
| `string` | Textos | `"Seu Zé"`, `"porrinha"` |
| `char` | Um único caractere | `'A'`, `'?'` |
| `bool` | Verdadeiro ou falso | `true`, `false` |

Repare que textos ficam entre **aspas duplas** e caracteres entre **aspas simples**.

### Variáveis

Uma **variável** é um espaço na memória, com um nome, onde guardamos um valor que **pode mudar** durante o programa. Em C#, declaramos uma variável informando o tipo, o nome e, se quisermos, um valor inicial:

```csharp
int quantidadePasteis = 2;
string bebida = "suco de laranja";
double preco = 7.5;
bool mesaOcupada = true;
```

Depois de declarada, a variável pode receber outro valor:

```csharp
quantidadePasteis = 3;   // pediram mais um pastel
```

### Constantes

Uma **constante** também tem nome e valor, mas o valor **nunca muda**. Usamos a palavra `const`:

```csharp
const int MESAS_DO_BAR = 8;
```

Se você tentar mudar uma constante depois (`MESAS_DO_BAR = 10;`), o programa nem compila. Constantes são úteis para valores fixos que aparecem em vários lugares: se um dia o valor mudar, você altera em um lugar só.

### Identificadores

**Identificador** é o nome que damos a variáveis, constantes e, mais adiante, a funções. Em C#:

- O nome começa com uma letra ou `_` e pode ter letras, números e `_`.
- Não pode ter espaços nem ser uma palavra reservada da linguagem, como `int` ou `if`.
- Letras maiúsculas e minúsculas são diferentes: `palitos` e `Palitos` são dois nomes distintos.

Neste tutorial, seguimos duas convenções:

- **Variáveis** em *camelCase*: a primeira palavra minúscula e as seguintes começando com maiúscula (`palitosJogador`, `nomeJogador`).
- **Constantes** em maiúsculas, com `_` separando as palavras (`PALITOS_INICIAIS`).

Prefira nomes que expliquem o que a variável guarda. `palitosJogador` é muito melhor que `p` ou `x`.

### Saída de dados

Para mostrar algo na tela, usamos:

- `Console.WriteLine(...)`: escreve e **pula para a linha de baixo**.
- `Console.Write(...)`: escreve e **continua na mesma linha**.

```csharp
Console.Write("Pastel de ");
Console.WriteLine("queijo");
Console.WriteLine("Saindo!");
```

```text
Pastel de queijo
Saindo!
```

Para misturar texto e variáveis, usamos a **interpolação de strings**: coloque um `$` antes das aspas e as variáveis entre chaves `{ }`:

```csharp
int quantidadePasteis = 2;
Console.WriteLine($"Você pediu {quantidadePasteis} pastéis.");
```

```text
Você pediu 2 pastéis.
```

### Entrada de dados

Para ler o que o usuário digita, usamos `Console.ReadLine()`. Ele espera o usuário digitar e apertar Enter e devolve o texto digitado:

```csharp
Console.Write("Qual pastel você quer? ");
string sabor = Console.ReadLine() ?? "";
Console.WriteLine($"Um pastel de {sabor} saindo!");
```

O `?? ""` no final é uma garantia. Em situações raras, o `ReadLine()` pode não devolver texto nenhum (o valor especial `null`). O `??` diz: "se não vier nada, use um texto vazio". Assim a variável `sabor` sempre tem um texto.

O `ReadLine()` **sempre devolve um texto** (`string`), mesmo que a pessoa digite um número. No próximo bloco você vai ver como transformar esse texto em número.

---

## No jogo

Abra o arquivo `Porrinha/Program.cs` e **apague** a linha `Console.WriteLine("O Bar do Seu Zé está em construção...");`. Mantenha o comentário do topo e digite o código abaixo, completando cada `// TODO`.

```csharp
// =====================================================
//  PORRINHA – Bar do Seu Zé
// =====================================================

// TODO 1: declare a constante PALITOS_INICIAIS, do tipo int, com o valor 3
const int PALITOS_INICIAIS = /* TODO 1 */;

Console.WriteLine("==========================================");
Console.WriteLine("        BEM-VINDO AO BAR DO SEU ZÉ        ");
Console.WriteLine("==========================================");
Console.WriteLine();

Console.Write("Qual é o seu nome? ");
// TODO 2: leia o nome digitado e guarde na variável nomeJogador
string nomeJogador = /* TODO 2 */;

// TODO 3: os dois jogadores começam com a quantidade da constante
int palitosJogador = /* TODO 3 */;
int palitosComputador = /* TODO 3 */;

Console.WriteLine();
// TODO 4: complete a mensagem usando a variável com o nome do jogador
Console.WriteLine($"Olá, {/* TODO 4 */}! Hoje você vai enfrentar o Computador.");
Console.WriteLine($"Cada jogador começa com {PALITOS_INICIAIS} palitos.");
Console.WriteLine();

// TODO 5: mostre quantos palitos cada jogador tem, no formato
//         "Ana: 3 palitos" e "Computador: 3 palitos"
```

### Dicas

- **TODO 1:** o valor de uma constante vem depois do `=`, como em qualquer variável.
- **TODO 2:** use `Console.ReadLine()` com o `?? ""`, como no exemplo do sabor de pastel.
- **TODO 3:** em vez de escrever `3`, use o nome da constante. Assim, se um dia o jogo começar com 5 palitos, basta mudar a constante.
- **TODO 4:** dentro das chaves vai o **nome da variável**, sem aspas.
- **TODO 5:** são duas linhas com `Console.WriteLine` e interpolação. Use as variáveis `nomeJogador`, `palitosJogador` e `palitosComputador`.

---

## Sua vez

1. Crie uma constante `NOME_DO_BAR` com o texto `"Bar do Seu Zé"` e use essa constante na linha de boas-vindas, com interpolação.
2. Pergunte também de que **cidade** o jogador é e mostre uma saudação como `Olá, Ana, de Campinas!`.
3. Troque um `Console.WriteLine` por `Console.Write` e rode o programa. O que mudou na tela? Por quê?

---

## Checkpoint

Rode o programa com `dotnet run Program.cs` (dentro da pasta `Porrinha`). Ele deve funcionar assim:

```text
==========================================
        BEM-VINDO AO BAR DO SEU ZÉ
==========================================

Qual é o seu nome? Ana

Olá, Ana! Hoje você vai enfrentar o Computador.
Cada jogador começa com 3 palitos.

Ana: 3 palitos
Computador: 3 palitos
```

Funcionou? Faça o *commit* do bloco e siga em frente.

```bash
git add .
git commit -m "Bloco 01 concluído"
```

[← Voltar ao início](../README.md) · [Próximo: Bloco 02 →](02-sequencial.md)
