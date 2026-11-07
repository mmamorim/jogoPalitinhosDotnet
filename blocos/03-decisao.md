# Bloco 03 – Quem acertou?

[← Bloco 02](02-sequencial.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 04 →](04-repeticao.md)

> As mãos se abriram e o total está na mesa. Agora alguém precisa decidir quem acertou, quem descarta e o que o Seu Zé vai comentar lá do balcão.

## Objetivo

Neste bloco você vai aprender a fazer o programa **tomar decisões**: executar um trecho de código só quando uma condição for verdadeira e escolher entre caminhos diferentes.

Ao final, a rodada vai dizer **quem acertou**, descontar o palito de quem acertou e recusar valores fora das regras.

---

## Conceitos

### Operadores relacionais

Os **operadores relacionais** comparam dois valores. O resultado é sempre um `bool`: `true` ou `false`.

| Operador | Significado | Exemplo | Resultado |
| --- | --- | --- | --- |
| `==` | Igual a | `3 == 3` | `true` |
| `!=` | Diferente de | `3 != 4` | `true` |
| `>` | Maior que | `2 > 5` | `false` |
| `<` | Menor que | `2 < 5` | `true` |
| `>=` | Maior ou igual a | `5 >= 5` | `true` |
| `<=` | Menor ou igual a | `6 <= 5` | `false` |

**Cuidado:** `=` atribui um valor, `==` compara dois valores. Trocar um pelo outro é um dos erros mais comuns.

### Operadores lógicos

Os **operadores lógicos** combinam condições:

| Operador | Nome | Resultado é `true` quando... |
| --- | --- | --- |
| `&&` | E | as **duas** condições são verdadeiras |
| `\|\|` | OU | **pelo menos uma** das condições é verdadeira |
| `!` | NÃO | a condição é **falsa** (ele inverte o valor) |

### Tabela verdade

A tabela verdade mostra o resultado de cada operador para todas as combinações possíveis de `A` e `B`:

| `A` | `B` | `A && B` | `A \|\| B` | `!A` |
| --- | --- | --- | --- | --- |
| `true` | `true` | `true` | `true` | `false` |
| `true` | `false` | `false` | `true` | `false` |
| `false` | `true` | `false` | `true` | `true` |
| `false` | `false` | `false` | `false` | `true` |

Um exemplo no bar: o Seu Zé só serve pastel se tiver massa **e** recheio.

```csharp
bool temMassa = true;
bool temRecheio = false;
bool podeServir = temMassa && temRecheio;   // false
```

### Decisão simples: `if`

O `if` executa um bloco de código **somente se** a condição for verdadeira:

```csharp
int pasteis = 0;
if (pasteis == 0)
{
    Console.WriteLine("Acabou o pastel!");
}
```

### Decisão composta: `if` e `else`

O `else` define o que fazer quando a condição é **falsa**. Sempre um dos dois caminhos é executado, nunca os dois:

```csharp
if (pasteis > 0)
{
    Console.WriteLine("Pode pedir!");
}
else
{
    Console.WriteLine("Acabou o pastel!");
}
```

### Decisão encadeada: `else if`

Quando há **mais de dois caminhos**, encadeamos condições com `else if`. O C# testa uma por uma, de cima para baixo, e executa **apenas o primeiro** caminho cuja condição for verdadeira:

```csharp
if (pasteis == 0)
{
    Console.WriteLine("Acabou o pastel!");
}
else if (pasteis < 5)
{
    Console.WriteLine("Últimos pastéis!");
}
else
{
    Console.WriteLine("Tem pastel de sobra.");
}
```

### Múltipla escolha: `switch`

Quando a decisão depende do **valor exato** de uma variável, o `switch` deixa o código mais organizado que vários `else if`:

```csharp
int mesa = 2;
switch (mesa)
{
    case 1:
        Console.WriteLine("Mesa da janela.");
        break;
    case 2:
        Console.WriteLine("Mesa perto do balcão.");
        break;
    default:
        Console.WriteLine("Mesa do fundo.");
        break;
}
```

- Cada `case` testa um valor.
- O `break` encerra o `case`. Em C#, ele é obrigatório.
- O `default` é executado quando nenhum `case` corresponde ao valor. Ele funciona como o `else`.

---

## No jogo

Você vai fazer quatro alterações no `Program.cs`.

### 1. Nome vazio

Logo depois da linha que lê o nome do jogador, adicione:

```csharp
// TODO 1: se o nome estiver vazio, use "Jogador" como nome
if (/* TODO 1 */)
{
    nomeJogador = "Jogador";
}
```

### 2. Escolha fora das regras

Logo depois da linha `int escolhaJogador = int.Parse(texto);`, adicione:

```csharp
// TODO 2: a escolha é inválida se for menor que 0 OU maior que os palitos do jogador
if (/* TODO 2 */)
{
    Console.WriteLine("Valor inválido! Vou considerar que você escondeu 0.");
    escolhaJogador = 0;
}
```

### 3. Palpite fora das regras

Logo depois da linha `int palpiteJogador = int.Parse(texto);`, adicione:

```csharp
// TODO 3: o palpite é inválido se for menor que 0 OU maior que maximoPalpite
if (/* TODO 3 */)
{
    Console.WriteLine("Palpite inválido! Vou considerar que você chutou 0.");
    palpiteJogador = 0;
}
```

### 4. Comentário do Seu Zé e resultado da rodada

No **final do arquivo**, depois da linha que mostra o total, adicione:

```csharp
// TODO 4: complete o switch com um comentário para cada caso
switch (total)
{
    case 0:
        Console.WriteLine("Seu Zé: \"Mão vazia geral! Ninguém arriscou nada.\"");
        break;
    case 1:
        // TODO 4: comentário quando há um palito só na mesa
        break;
    default:
        Console.WriteLine("Seu Zé: \"Olha a mesa cheia de palito!\"");
        break;
}

if (palpiteJogador == total)
{
    Console.WriteLine($"{nomeJogador} acertou! Descarta um palito.");
    // TODO 5: tire um palito do jogador
}
else if (/* TODO 6: o Computador acertou? */)
{
    Console.WriteLine("O Computador acertou! Descarta um palito.");
    // TODO 7: tire um palito do Computador
}
else
{
    Console.WriteLine("Ninguém acertou. Ninguém descarta.");
}

Console.WriteLine();
// TODO 8: mostre o placar no formato "Placar: Ana 2 x 3 Computador"
```

Repare no `\"` dentro dos textos do Seu Zé. Como as aspas duplas marcam o começo e o fim do texto, para escrever uma aspa **dentro** do texto colocamos uma barra invertida antes dela.

### Dicas

- **TODO 1:** um texto vazio é escrito como `""`. Compare `nomeJogador` com ele usando `==`.
- **TODO 2:** use `escolhaJogador < 0` e `escolhaJogador > palitosJogador`, ligados pelo operador OU.
- **TODO 3:** mesma ideia do TODO 2, agora com `palpiteJogador` e `maximoPalpite`.
- **TODO 4:** um `Console.WriteLine` com o texto que você quiser. Não apague o `break`!
- **TODO 5 e 7:** use a atribuição que você viu no bloco 02: `variavel = variavel - 1;`.
- **TODO 6:** compare `palpiteComputador` com `total`.
- **TODO 8:** um `Console.WriteLine` com interpolação, usando o nome e as duas variáveis de palitos.

---

## Sua vez

1. Acrescente um `case 2:` no `switch` com um comentário novo do Seu Zé.
2. Depois do placar, se o jogador ficou com **apenas 1 palito**, mostre a mensagem `Só falta um!`.
3. **Para pensar:** como o Computador ainda chuta no sorteio, ele pode dar o mesmo palpite que você. Se os dois acertarem, quem descarta no seu código? Por quê? (Dica: lembre-se de como o `else if` funciona.)

---

## Checkpoint

A rodada agora termina dizendo quem acertou e mostrando o placar:

```text
Quantos palitos você vai esconder na mão (0 a 3)? 7
Valor inválido! Vou considerar que você escondeu 0.
Seu palpite para o total de palitos (0 a 6): 2
O Computador chutou 4.

Mãos abertas!
Ana escondeu 0 e o Computador escondeu 2.
Total na mesa: 2
Seu Zé: "Olha a mesa cheia de palito!"
Ana acertou! Descarta um palito.

Placar: Ana 2 x 3 Computador
```

Teste também um nome vazio (só aperte Enter) e um palpite maior que o permitido.

```bash
git add .
git commit -m "Bloco 03 concluído"
```

[← Bloco 02](02-sequencial.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 04 →](04-repeticao.md)
