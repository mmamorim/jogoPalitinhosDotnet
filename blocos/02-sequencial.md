# Bloco 02 – A primeira rodada

[← Bloco 01](01-variaveis.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 03 →](03-decisao.md)

> Palitos distribuídos, hora de jogar. Cada um esconde os seus, todo mundo dá um palpite e as mãos se abrem. Por enquanto, o seu adversário ainda joga no puro chute.

## Objetivo

Neste bloco você vai aprender a **fazer contas**, a **transformar texto em número** e a **sortear números**, organizando tudo em uma sequência de passos.

Ao final, o programa vai jogar **uma rodada completa**: você esconde palitos, o Computador sorteia os dele, os dois dão palpites e o total é revelado. Quem acertou fica para o próximo bloco.

---

## Conceitos

### Estrutura sequencial

Um programa em **estrutura sequencial** executa os comandos **um depois do outro, de cima para baixo**, sem pular nenhum e sem repetir nenhum. Foi o que você fez no bloco 01, e é assim que vai ser a rodada deste bloco: ler, sortear, calcular e mostrar.

### Operador de atribuição

O sinal `=` em programação **não é igualdade**: ele é o **operador de atribuição**. Ele calcula o que está do lado direito e guarda o resultado na variável do lado esquerdo.

```csharp
int pasteis = 2;
pasteis = pasteis + 1;   // calcula 2 + 1 e guarda 3 em pasteis
```

Na matemática, `pasteis = pasteis + 1` não faria sentido. Em programação, significa "pegue o valor atual, some 1 e guarde de volta".

### Operadores aritméticos

| Operador | Operação | Exemplo | Resultado |
| --- | --- | --- | --- |
| `+` | Soma | `7 + 2` | `9` |
| `-` | Subtração | `7 - 2` | `5` |
| `*` | Multiplicação | `7 * 2` | `14` |
| `/` | Divisão | `7 / 2` | `3` |
| `%` | Resto da divisão | `7 % 2` | `1` |

**Atenção à divisão entre inteiros:** `7 / 2` dá `3`, e não `3.5`. Quando os dois números são `int`, o C# descarta a parte decimal. Para ter `3.5`, pelo menos um dos números precisa ser `double`: `7.0 / 2`.

O operador `%` (resto) parece estranho agora, mas ele vai ser muito útil no bloco 04.

A **precedência** é a mesma da matemática: `*`, `/` e `%` são feitos antes de `+` e `-`. Use parênteses para mudar a ordem:

```csharp
int a = 2 + 3 * 4;     // 14
int b = (2 + 3) * 4;   // 20
```

### Convertendo texto em número

O `Console.ReadLine()` sempre devolve um `string`. Para fazer contas, precisamos converter esse texto em `int` com o `int.Parse`:

```csharp
Console.Write("Quantos pastéis? ");
string texto = Console.ReadLine() ?? "";
int pasteis = int.Parse(texto);
Console.WriteLine($"O dobro seria {pasteis * 2}.");
```

Repare que o código lê o texto primeiro e só depois converte. Fazer em dois passos deixa claro o que está acontecendo.

### Sorteando números

Para o Computador tomar decisões, vamos sortear números com `Random.Shared.Next(minimo, limite)`:

```csharp
int dado = Random.Shared.Next(1, 7);   // sorteia 1, 2, 3, 4, 5 ou 6
```

**Atenção:** o primeiro número **entra** no sorteio, mas o segundo **não entra**. Por isso, para sortear de 1 a 6 escrevemos `Next(1, 7)`. É um erro muito comum esquecer disso.

---

## No jogo

Continue no mesmo `Program.cs`. Primeiro, **apague as duas linhas** do final que mostram os palitos de cada jogador (`Ana: 3 palitos`...), porque elas vão passar para dentro da rodada. Depois, adicione este código **no final do arquivo**:

```csharp
// ---------- Rodada ----------
Console.WriteLine();
Console.WriteLine("============== RODADA 1 ==============");
Console.WriteLine($"{nomeJogador}: {palitosJogador} palitos");
Console.WriteLine($"Computador: {palitosComputador} palitos");
Console.WriteLine();

Console.Write($"Quantos palitos você vai esconder na mão (0 a {palitosJogador})? ");
string texto = Console.ReadLine() ?? "";
// TODO 1: converta o texto para número
int escolhaJogador = /* TODO 1 */;

// TODO 2: sorteie quantos palitos o Computador esconde: de 0 até palitosComputador
int escolhaComputador = /* TODO 2 */;

// TODO 3: o maior total possível é a soma dos palitos dos dois jogadores
int maximoPalpite = /* TODO 3 */;

Console.Write($"Seu palpite para o total de palitos (0 a {maximoPalpite}): ");
texto = Console.ReadLine() ?? "";
int palpiteJogador = int.Parse(texto);

// TODO 4: o Computador chuta um número de 0 até maximoPalpite
int palpiteComputador = /* TODO 4 */;
Console.WriteLine($"O Computador chutou {palpiteComputador}.");

// TODO 5: calcule o total de palitos nas duas mãos
int total = /* TODO 5 */;

Console.WriteLine();
Console.WriteLine("Mãos abertas!");
Console.WriteLine($"{nomeJogador} escondeu {escolhaJogador} e o Computador escondeu {escolhaComputador}.");
Console.WriteLine($"Total na mesa: {total}");
```

Repare na linha `texto = Console.ReadLine() ?? "";`. Ela **não tem** o tipo `string` na frente, porque a variável `texto` já foi declarada antes. Aqui só estamos atribuindo um novo valor a ela.

### Dicas

- **TODO 1:** use `int.Parse(texto)`.
- **TODO 2:** lembre-se de que o segundo número do `Next` não entra no sorteio. Se o Computador tem 3 palitos, ele pode esconder 0, 1, 2 ou 3. Qual deve ser o limite?
- **TODO 3:** some as duas variáveis de palitos.
- **TODO 4:** mesmo raciocínio do TODO 2, agora usando `maximoPalpite`.
- **TODO 5:** some as duas escolhas.

---

## Sua vez

1. Depois de mostrar o total, mostre também quantos palitos **cada jogador deixou fora da mão**. Se você tem 3 e escondeu 2, deixou 1 fora.
2. Rode o programa e, na pergunta de quantos palitos esconder, digite `abc`. O que acontece? Anote a mensagem de erro: vamos resolver esse problema no bloco 04.
3. Rode o programa várias vezes. O Computador às vezes chuta o **mesmo palpite** que você? Pelas regras, isso não poderia acontecer. Vamos corrigir isso no bloco 04 também.

---

## Checkpoint

O programa deve jogar uma rodada inteira. Como o Computador sorteia, os números dele vão variar a cada execução:

```text
============== RODADA 1 ==============
Ana: 3 palitos
Computador: 3 palitos

Quantos palitos você vai esconder na mão (0 a 3)? 2
Seu palpite para o total de palitos (0 a 6): 4
O Computador chutou 5.

Mãos abertas!
Ana escondeu 2 e o Computador escondeu 1.
Total na mesa: 3
```

Funcionou? Faça o *commit* do bloco.

```bash
git add .
git commit -m "Bloco 02 concluído"
```

[← Bloco 01](01-variaveis.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 03 →](03-decisao.md)
