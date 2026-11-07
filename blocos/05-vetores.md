# Bloco 05 – Organizando a mesa

[← Bloco 04](04-repeticao.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 06 →](06-funcoes.md)

> O jogo já funciona, mas o Seu Zé olhou o seu código e torceu o nariz: "Tudo em dobro! Uma variável para você, outra para o Computador... E se um dia entrar um terceiro jogador na mesa?" Hora de organizar.

## Objetivo

Neste bloco você vai aprender a usar **vetores** para guardar vários valores em uma única variável e a percorrê-los com o `for`.

Este é um bloco de **refatoração**: o jogo vai continuar funcionando **exatamente igual** ao bloco 04, mas com o código mais organizado. Melhorar a estrutura de um programa sem mudar o que ele faz é uma tarefa muito comum no dia a dia de quem programa.

---

## Conceitos

### O que é um vetor

Um **vetor** (ou *array*) é uma variável que guarda **vários valores do mesmo tipo**, um ao lado do outro. Cada valor ocupa uma **posição**, identificada por um número chamado **índice**.

```csharp
string[] sabores = { "queijo", "carne", "palmito" };
```

| Índice | 0 | 1 | 2 |
| --- | --- | --- | --- |
| Valor | `"queijo"` | `"carne"` | `"palmito"` |

**Atenção:** os índices **começam em 0**. Em um vetor de 3 posições, os índices vão de 0 a 2.

### Declarando vetores

Há duas formas de criar um vetor:

```csharp
// 1. Já com os valores
int[] precos = { 7, 8, 9 };

// 2. Só com o tamanho: todas as posições começam com 0
int[] pedidos = new int[3];
```

O `[]` depois do tipo indica que a variável é um vetor. Na segunda forma, `new int[3]` é o jeito do C# de reservar espaço para 3 números inteiros.

### Lendo e alterando posições

Usamos o índice entre colchetes:

```csharp
Console.WriteLine(sabores[0]);   // queijo
sabores[2] = "frango";            // troca "palmito" por "frango"
pedidos[1]++;                     // soma 1 na posição 1
```

Se você usar um índice que não existe, como `sabores[3]`, o programa quebra com o erro `IndexOutOfRangeException`.

### Percorrendo um vetor com `for`

A propriedade `Length` diz quantas posições o vetor tem. Juntando com o `for`, visitamos todas as posições:

```csharp
for (int i = 0; i < sabores.Length; i++)
{
    Console.WriteLine($"Sabor {i}: {sabores[i]}");
}
```

Um uso muito comum é **somar** todos os valores. A variável que vai juntando a soma é chamada de **acumulador**:

```csharp
int totalPedidos = 0;
for (int i = 0; i < pedidos.Length; i++)
{
    totalPedidos += pedidos[i];   // atalho para totalPedidos = totalPedidos + pedidos[i]
}
```

### Constantes como índices

Índices soltos como `palitos[0]` e `palitos[1]` são difíceis de ler. Quem é o 0? Quem é o 1? Para deixar claro, criamos constantes com nomes:

```csharp
const int JOGADOR = 0;
const int COMPUTADOR = 1;

palitos[JOGADOR]--;   // muito mais claro que palitos[0]--;
```

### Vetores "em paralelo"

No jogo, vamos usar vários vetores em que **a mesma posição se refere ao mesmo jogador**:

| Vetor | Posição `JOGADOR` (0) | Posição `COMPUTADOR` (1) |
| --- | --- | --- |
| `nomes` | `"Ana"` | `"Computador"` |
| `palitos` | `3` | `3` |
| `escolhas` | quanto escondeu | quanto escondeu |
| `palpites` | o palpite | o palpite |

Assim, `nomes[j]`, `palitos[j]` e `palpites[j]` sempre falam do mesmo jogador.

### Matrizes

Uma **matriz** é um vetor com **duas dimensões**: linhas e colunas. Ela é declarada com uma vírgula dentro dos colchetes:

```csharp
int[,] vendas = new int[2, 3];   // 2 linhas e 3 colunas
vendas[0, 2] = 15;               // linha 0, coluna 2
```

Para percorrer uma matriz, usamos um `for` dentro de outro, um para as linhas e um para as colunas. Você vai usar uma matriz no desafio do final deste bloco.

---

## No jogo

Primeiro, veja como as variáveis antigas viram posições dos vetores:

| Antes (bloco 04) | Depois (bloco 05) |
| --- | --- |
| `nomeJogador` (ao mostrar na tela) | `nomes[JOGADOR]` |
| `"Computador"` (ao mostrar na tela) | `nomes[COMPUTADOR]` |
| `palitosJogador` | `palitos[JOGADOR]` |
| `palitosComputador` | `palitos[COMPUTADOR]` |
| `escolhaJogador` | `escolhas[JOGADOR]` |
| `escolhaComputador` | `escolhas[COMPUTADOR]` |
| `palpiteJogador` | `palpites[JOGADOR]` |
| `palpiteComputador` | `palpites[COMPUTADOR]` |

A variável `nomeJogador` continua existindo, porque é nela que lemos o nome digitado. Depois, ela é usada para preencher o vetor `nomes`.

### 1. Constantes e vetores

Logo abaixo da constante `PALITOS_INICIAIS`, crie as constantes dos índices:

```csharp
const int JOGADOR = 0;
const int COMPUTADOR = 1;
```

Depois, **substitua** as declarações de `palitosJogador` e `palitosComputador` por:

```csharp
// TODO 1: o vetor de nomes tem o nome lido e o texto "Computador"
string[] nomes = { /* TODO 1 */ };
// TODO 2: os dois jogadores começam com PALITOS_INICIAIS
int[] palitos = { /* TODO 2 */ };
int[] escolhas = new int[2];
int[] palpites = new int[2];
```

Na mensagem de boas-vindas, troque os nomes pelos do vetor:

```csharp
Console.WriteLine($"Olá, {nomes[JOGADOR]}! Hoje você vai enfrentar o {nomes[COMPUTADOR]}.");
```

### 2. Condição da partida

```csharp
// TODO 3: reescreva a condição do while usando o vetor palitos
while (/* TODO 3 */)
```

### 3. Desenho dos palitos

Os dois `for` que desenhavam os palitos viram **um `for` dentro de outro**: o de fora passa por cada jogador, o de dentro desenha os palitos daquele jogador. Substitua o desenho antigo por:

```csharp
for (int j = 0; j < nomes.Length; j++)
{
    Console.Write($"{nomes[j]}: ");
    // TODO 4: desenhe um "| " para cada palito do jogador j
    /* TODO 4 */
    Console.WriteLine();
}
Console.WriteLine();
```

### 4. Escolhas e palpites

Agora você vai fazer a **troca de nomes** seguindo a tabela lá de cima, em todo o trecho das escolhas e dos palpites. Por exemplo:

```csharp
// Antes
valido = int.TryParse(Console.ReadLine(), out escolhaJogador)
         && escolhaJogador >= 0 && escolhaJogador <= palitosJogador;

// Depois
valido = int.TryParse(Console.ReadLine(), out escolhas[JOGADOR])
         && escolhas[JOGADOR] >= 0 && escolhas[JOGADOR] <= palitos[JOGADOR];
```

Apague as declarações `int escolhaJogador;`, `int palpiteJogador;` e `int palpiteComputador;`, porque essas informações agora estão nos vetores. Nas linhas que sorteiam a escolha e o palpite do Computador, tire o `int` da frente, porque agora você está atribuindo a uma posição do vetor.

O cálculo do máximo também muda. Em vez de somar duas variáveis, somamos todas as posições do vetor:

```csharp
// TODO 5: some todas as posições do vetor palitos em maximoPalpite
int maximoPalpite = 0;
for (int j = 0; j < palitos.Length; j++)
{
    /* TODO 5 */
}
```

### 5. Revelação

Substitua o cálculo do total e as mensagens de "Mãos abertas!" por:

```csharp
// TODO 6: some todas as escolhas em total, como foi feito com o maximoPalpite
int total = 0;
/* TODO 6 */

Console.WriteLine();
Console.WriteLine("Mãos abertas!");
for (int j = 0; j < nomes.Length; j++)
{
    // TODO 7: mostre "Ana escondeu 2." para cada jogador
    /* TODO 7 */
}
Console.WriteLine($"Total na mesa: {total}");
```

O seu `switch` com os comentários do Seu Zé continua igual.

### 6. Resultado

O `if` / `else if` / `else` que verificava quem acertou também vira um laço: **para cada jogador**, se o palpite dele é igual ao total, ele descarta. Como os palpites nunca são iguais (você garantiu isso no bloco 04), no máximo um jogador acerta. Substitua o bloco antigo por:

```csharp
bool alguemAcertou = false;
for (int j = 0; j < nomes.Length; j++)
{
    // TODO 8: o jogador j acertou?
    if (/* TODO 8 */)
    {
        Console.WriteLine($"{nomes[j]} acertou! Descarta um palito.");
        // TODO 9: tire um palito do jogador j
        /* TODO 9 */
        alguemAcertou = true;
    }
}
// TODO 10: se ninguém acertou, mostre "Ninguém acertou. Ninguém descarta."
```

### 7. Fim da partida

No anúncio do vencedor, troque `palitosJogador` por `palitos[JOGADOR]` e os nomes pelos do vetor `nomes`.

### Dicas

- **TODO 1:** os valores são separados por vírgula: a variável `nomeJogador` e o texto `"Computador"`.
- **TODO 2:** use a constante duas vezes.
- **TODO 3:** `palitos[JOGADOR] > 0` e o mesmo para o Computador.
- **TODO 4:** é o mesmo `for` do bloco 04, mas o limite agora é `palitos[j]`. Use outra letra no laço de dentro, como `i`, para não confundir com o `j` de fora.
- **TODO 5:** use o acumulador `maximoPalpite += palitos[j];`.
- **TODO 6:** mesma ideia, com o vetor `escolhas`.
- **TODO 7:** um `Console.WriteLine` com `nomes[j]` e `escolhas[j]`.
- **TODO 8:** compare `palpites[j]` com `total`.
- **TODO 9:** use o `--` na posição certa do vetor `palitos`.
- **TODO 10:** use o operador `!` com a variável `alguemAcertou`.

---

## Sua vez

1. Teste o que acontece se você escrever `palitos[2]`. Leia a mensagem de erro e depois desfaça a alteração.
2. Crie um vetor `acertos` com uma posição para cada jogador, conte quantas vezes cada um acertou e mostre esses números no fim da partida.
3. **Desafio com matriz:** crie uma constante `MAX_RODADAS = 50` e uma matriz `int[,] historico = new int[MAX_RODADAS, 2]`. Em cada rodada, guarde na matriz quantos palitos cada jogador escondeu (linha = rodada, coluna = jogador). No fim da partida, mostre uma tabela com o histórico:

```text
Rodada   Ana   Computador
1        2     1
2        0     3
...
```

---

## Checkpoint

O jogo funciona **exatamente igual** ao bloco 04. A única diferença visível é a revelação, que agora mostra uma linha para cada jogador:

```text
Mãos abertas!
Ana escondeu 2.
Computador escondeu 1.
Total na mesa: 3
```

Jogue uma partida inteira para conferir. Se algo mudou de comportamento, compare o trecho com a tabela de troca de nomes.

```bash
git add .
git commit -m "Bloco 05 concluído"
```

[← Bloco 04](04-repeticao.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 06 →](06-funcoes.md)
