# Bloco 06 – Pensando como o Seu Zé

[← Bloco 05](05-vetores.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 07 →](07-procedimentos.md)

> Sabe por que o Seu Zé nunca perde? Ele não chuta no escuro. Antes de dar o palpite, ele conta os palitos que tem na própria mão. Se escondeu 2, o total é pelo menos 2. Hoje você vai ensinar o Computador a pensar assim.

## Objetivo

Neste bloco você vai aprender a criar **funções**: trechos de código com nome, que recebem valores, fazem um trabalho e **devolvem um resultado**.

É mais um bloco de **refatoração**. O código repetido do jogo vai virar funções, e o programa principal vai ficar mais curto e fácil de ler. De quebra, o Computador vai ficar mais esperto.

---

## Conceitos

### Por que funções?

Olhe para o seu código. O `do-while` que lê um número e verifica se ele é válido aparece **três vezes**, quase igual. Se você quiser mudar a mensagem de erro, precisa mudar em três lugares. E se esquecer um?

Uma **função** resolve isso: escrevemos o código **uma vez**, damos um nome a ele e o **chamamos** quantas vezes precisarmos.

### Anatomia de uma função

```csharp
static int CalcularTroco(int valorPago, int valorConta)
{
    int troco = valorPago - valorConta;
    return troco;
}
```

| Parte | No exemplo | O que é |
| --- | --- | --- |
| Tipo de retorno | `int` | O tipo do valor que a função devolve |
| Nome | `CalcularTroco` | Como a função é chamada. Usamos *PascalCase*: cada palavra começa com maiúscula |
| Parâmetros | `int valorPago, int valorConta` | Os valores que a função recebe para trabalhar |
| Corpo | o que está entre `{ }` | O código da função |
| `return` | `return troco;` | Encerra a função e devolve o valor |

### Chamando uma função

Para usar a função, escrevemos o nome dela e passamos os valores entre parênteses. O valor devolvido pode ser guardado em uma variável ou usado direto:

```csharp
int troco = CalcularTroco(20, 15);
Console.WriteLine($"Troco: {troco}");                       // Troco: 5
Console.WriteLine($"Troco: {CalcularTroco(50, 32)}");       // Troco: 18
```

Os valores passados na chamada (`20` e `15`) são chamados de **argumentos**. Eles são copiados para os **parâmetros** (`valorPago` e `valorConta`), na mesma ordem.

### Escopo

Uma variável criada **dentro** de uma função só existe dentro dela. É o que chamamos de **escopo**. No exemplo, a variável `troco` de dentro de `CalcularTroco` e a variável `troco` de fora são **duas variáveis diferentes**, que só têm o mesmo nome.

### O que significa o `static`?

Neste tutorial, todas as funções são declaradas com `static`. Aqui, ele quer dizer que **a função não enxerga as variáveis do programa principal**: tudo de que ela precisa tem que chegar pelos parâmetros. As constantes (`const`) continuam visíveis.

Isso é uma vantagem: olhando só para a assinatura da função, você sabe tudo o que ela usa.

### Onde escrever as funções

Escreva todas as funções **no final do `Program.cs`**, depois de todo o código do jogo. Mesmo ficando no final, elas podem ser chamadas de qualquer ponto do programa.

---

## No jogo

Primeiro você vai escrever as três funções no final do arquivo. Depois, vai trocar o código repetido por chamadas a elas.

### 1. Escrevendo as funções

Vá até o **final do `Program.cs`** e adicione:

```csharp
// =====================================================
//  FUNÇÕES
// =====================================================

// Lê um número inteiro do teclado e só devolve quando ele
// for válido e estiver entre minimo e maximo.
static int LerInteiro(string mensagem, int minimo, int maximo)
{
    int valor;
    bool valido;
    do
    {
        // TODO 1: escreva o corpo do do-while. Ele é igual ao que você já usou
        //         para ler a escolha do jogador, mas usando os parâmetros:
        //         mostra a mensagem, tenta converter para valor, verifica se
        //         está entre minimo e maximo e mostra um erro se não estiver.
    } while (!valido);
    // TODO 2: devolva o valor lido
}

// Soma todos os valores de um vetor de inteiros.
static int Somar(int[] valores)
{
    // TODO 3: crie um acumulador, percorra o vetor somando cada posição
    //         e devolva a soma
}

// O palpite "à moda do Seu Zé": o Computador sabe quantos palitos
// escondeu e só chuta quantos o adversário pode ter na mão.
// Nunca repete o palpiteProibido (use -1 quando não houver).
static int PalpiteComputador(int escolhaPropria, int palitosAdversario, int palpiteProibido)
{
    int palpite;
    do
    {
        // TODO 4: o palpite é a escolha do próprio Computador mais um sorteio
        //         de 0 até palitosAdversario
        palpite = /* TODO 4 */;
    } while (/* TODO 5 */);
    return palpite;
}
```

Entenda a estratégia da função `PalpiteComputador`. Se o Computador escondeu 2 e o adversário tem 3 palitos, o total só pode ser 2, 3, 4 ou 5, nunca 0, 1 ou 6. Sorteando só entre os totais possíveis, ele acerta muito mais.

### 2. Usando as funções

Agora volte ao código do jogo e substitua os trechos repetidos.

**Escolha do jogador.** O `do-while` inteiro vira uma linha:

```csharp
escolhas[JOGADOR] = LerInteiro($"Quantos palitos você vai esconder na mão (0 a {palitos[JOGADOR]})? ", 0, palitos[JOGADOR]);
```

**Máximo do palpite.** O `for` que somava os palitos vira:

```csharp
// TODO 6: use a função Somar com o vetor palitos
int maximoPalpite = /* TODO 6 */;
```

**Palpites.** O `if` / `else` dos palpites fica assim:

```csharp
if (rodada % 2 == 1)
{
    // TODO 7: leia o palpite do jogador com LerInteiro (de 0 a maximoPalpite)
    palpites[JOGADOR] = /* TODO 7 */;
    palpites[COMPUTADOR] = PalpiteComputador(escolhas[COMPUTADOR], palitos[JOGADOR], palpites[JOGADOR]);
    Console.WriteLine($"O {nomes[COMPUTADOR]} chutou {palpites[COMPUTADOR]}.");
}
else
{
    // TODO 8: o Computador chuta primeiro, então não há palpite proibido
    palpites[COMPUTADOR] = /* TODO 8 */;
    Console.WriteLine($"O {nomes[COMPUTADOR]} chuta primeiro: {palpites[COMPUTADOR]}.");

    palpites[JOGADOR] = LerInteiro($"Seu palpite para o total (0 a {maximoPalpite}): ", 0, maximoPalpite);
    while (palpites[JOGADOR] == palpites[COMPUTADOR])
    {
        Console.WriteLine("Esse palpite já foi dado! Escolha outro.");
        palpites[JOGADOR] = LerInteiro($"Seu palpite para o total (0 a {maximoPalpite}): ", 0, maximoPalpite);
    }
}
```

Repare no `else`: o `LerInteiro` garante que o número é válido, e o `while` garante que ele é diferente do palpite do Computador. Cada parte cuida de uma regra.

**Total.** O `for` que somava as escolhas vira:

```csharp
// TODO 9: use a função Somar com o vetor escolhas
int total = /* TODO 9 */;
```

Por fim, apague a declaração `bool valido;` do programa principal. Ela agora só existe dentro de `LerInteiro`.

### Dicas

- **TODO 1:** copie o corpo do `do-while` da escolha do bloco 05 e troque `escolhas[JOGADOR]` por `valor`, `0` por `minimo` e `palitos[JOGADOR]` por `maximo`. Use `Console.Write(mensagem);` para fazer a pergunta.
- **TODO 2:** `return` seguido da variável.
- **TODO 3:** é o mesmo acumulador do bloco 05: `int soma = 0;`, um `for` até `valores.Length` e `return soma;`.
- **TODO 4:** `escolhaPropria + Random.Shared.Next(0, palitosAdversario + 1)`.
- **TODO 5:** sorteie de novo enquanto o palpite for igual ao proibido.
- **TODO 6 e 9:** passe o vetor como argumento, sem colchetes: `Somar(palitos)`.
- **TODO 7:** veja como a escolha do jogador foi lida com `LerInteiro` e adapte a mensagem e os limites.
- **TODO 8:** chame `PalpiteComputador` passando `-1` como palpite proibido. Como nenhum palpite é negativo, o `-1` nunca vai ser sorteado.

---

## Sua vez

1. Jogue algumas partidas. O Computador ficou mais difícil de vencer?
2. Crie uma função `static string DesenharPalitos(int quantidade)` que devolve um texto como `"| | | "` e use essa função no desenho dos palitos.
3. Crie uma função `static bool Venceu(int palitosRestantes)` que devolve `true` quando o jogador ficou sem palitos, e use essa função no anúncio do vencedor.
4. **Desafio com recursividade:** uma função **recursiva** é uma função que chama a si mesma. Escreva uma versão de `LerInteiro` **sem o `do-while`**: se o valor for inválido, ela mostra o erro e **chama ela mesma de novo**, devolvendo o que essa nova chamada devolver.

```csharp
static int LerInteiroRecursivo(string mensagem, int minimo, int maximo)
{
    Console.Write(mensagem);
    if (int.TryParse(Console.ReadLine(), out int valor) && valor >= minimo && valor <= maximo)
    {
        return valor;
    }
    Console.WriteLine($"Valor inválido! Digite um número de {minimo} a {maximo}.");
    // complete: chame a própria função e devolva o resultado
}
```

Funciona igual? Qual das duas versões você acha mais fácil de entender?

---

## Checkpoint

O jogo funciona igual ao bloco 05, mas o programa principal ficou bem mais curto, e o Computador agora chuta com estratégia. Conte as linhas: quantas você economizou?

```bash
git add .
git commit -m "Bloco 06 concluído"
```

[← Bloco 05](05-vetores.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 07 →](07-procedimentos.md)
