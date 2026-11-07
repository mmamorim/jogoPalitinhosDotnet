# Bloco 07 – O jogo pronto

[← Bloco 06](06-funcoes.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 08 →](08-spectre.md)

> O Seu Zé passou pela sua casa e viu o jogo rodando. "Tá bom, mas cadê o cardápio? Ninguém senta na mesa sem saber o que tem!" Ele quer um menu na entrada: jogar, ver as regras ou ir embora.

## Objetivo

Neste bloco você vai aprender a criar **procedimentos**, que são funções que executam uma tarefa sem devolver valor, e a diferença entre passar parâmetros **por valor** e **por referência**.

Ao final, o jogo vai ter um **menu** e todo o código vai estar organizado em módulos. O programa principal vai caber em poucas linhas.

---

## Conceitos

### Procedimentos

Um **procedimento** é uma função que **não devolve valor**: ela apenas executa uma tarefa, como mostrar algo na tela. Em C#, o tipo de retorno de um procedimento é `void` (vazio), e ele não precisa de `return`:

```csharp
static void MostrarCardapio()
{
    Console.WriteLine("1 - Pastel de queijo");
    Console.WriteLine("2 - Pastel de carne");
}
```

Para chamar, basta escrever o nome com os parênteses:

```csharp
MostrarCardapio();
```

A diferença prática é esta:

| | Função | Procedimento |
| --- | --- | --- |
| Tipo de retorno | `int`, `string`, `bool`... | `void` |
| Tem `return` com valor? | Sim | Não |
| Como é usado | `int x = Somar(v);` | `MostrarCardapio();` |
| Serve para | **calcular** e devolver um resultado | **fazer** alguma coisa |

### Passagem por valor

Por padrão, quando você passa uma variável para uma função, a função recebe **uma cópia do valor**. Se ela alterar o parâmetro, a variável original **não muda**:

```csharp
static void ComerPastel(int pasteis)
{
    pasteis--;
    Console.WriteLine($"Dentro: {pasteis}");
}

int meusPasteis = 3;
ComerPastel(meusPasteis);
Console.WriteLine($"Fora: {meusPasteis}");
```

```text
Dentro: 2
Fora: 3
```

A função comeu um pastel da **cópia**. Os pastéis de verdade continuam lá.

### Passagem por referência: `ref`

Com a palavra `ref`, a função recebe **a própria variável**, e não uma cópia. Toda alteração feita no parâmetro vale para a variável original. O `ref` precisa aparecer **na declaração e na chamada**:

```csharp
static void ComerPastel(ref int pasteis)
{
    pasteis--;
    Console.WriteLine($"Dentro: {pasteis}");
}

int meusPasteis = 3;
ComerPastel(ref meusPasteis);
Console.WriteLine($"Fora: {meusPasteis}");
```

```text
Dentro: 2
Fora: 2
```

Lembra do `out` do `int.TryParse`, no bloco 04? Ele é parecido com o `ref`: também deixa a função escrever na variável de quem chamou. A diferença é que, com `out`, a variável não precisa ter um valor antes da chamada.

### E os vetores?

Quando você passa um **vetor** para uma função, ela recebe acesso **ao mesmo vetor**, e não a uma cópia dele. Por isso, se a função alterar uma posição do vetor, a alteração aparece fora dela, mesmo sem `ref`. Nos procedimentos deste bloco, os vetores são usados só para **ler** os dados.

---

## No jogo

Agora o `Program.cs` vai ser reorganizado em três partes: o **programa principal** (com o menu), os **procedimentos** e as **funções** (que você já tem do bloco 06).

### 1. O programa principal

Substitua **todo o código do jogo** (do título até o anúncio do vencedor) por este programa principal. Calma: o código que sai daqui não vai ser jogado fora, ele vai ser movido para os procedimentos no passo 2.

```csharp
// =====================================================
//  PORRINHA – Bar do Seu Zé
// =====================================================

const int PALITOS_INICIAIS = 3;
const int JOGADOR = 0;
const int COMPUTADOR = 1;

ExibirTitulo();

Console.Write("Qual é o seu nome? ");
string nomeJogador = Console.ReadLine() ?? "";
if (nomeJogador == "")
{
    nomeJogador = "Jogador";
}

int opcao;
do
{
    ExibirMenu();
    // TODO 1: leia a opção do menu com LerInteiro (de 1 a 3)
    opcao = /* TODO 1 */;

    switch (opcao)
    {
        case 1:
            JogarPartida(nomeJogador);
            break;
        case 2:
            // TODO 2: mostre as regras
            break;
        case 3:
            Console.WriteLine("Até sexta que vem no Bar do Seu Zé!");
            break;
    }
} while (/* TODO 3 */);
```

### 2. Os procedimentos

Logo abaixo do programa principal, **antes** das funções do bloco 06, crie os procedimentos:

```csharp
// =====================================================
//  PROCEDIMENTOS
// =====================================================

static void ExibirTitulo()
{
    // TODO 4: mova para cá as quatro linhas que mostram o título do bar
}

static void ExibirMenu()
{
    Console.WriteLine();
    Console.WriteLine("---------------- MENU ----------------");
    Console.WriteLine("1 - Jogar uma partida");
    Console.WriteLine("2 - Ver as regras");
    Console.WriteLine("3 - Sair");
}

static void ExibirRegras()
{
    Console.WriteLine();
    Console.WriteLine("REGRAS DA PORRINHA");
    Console.WriteLine($"- Cada jogador começa com {PALITOS_INICIAIS} palitos.");
    // TODO 5: complete com as outras regras do jogo (veja o README)
}

static void ExibirPlacar(string[] nomes, int[] palitos)
{
    // TODO 6: mova para cá o desenho dos palitos de cada jogador
    //         (os dois "for", um dentro do outro, e o Console.WriteLine() final)
}

static void ExibirRevelacao(string[] nomes, int[] escolhas, int total)
{
    // TODO 7: mova para cá a revelação: "Mãos abertas!", o for que mostra
    //         quanto cada jogador escondeu, o total e o switch do Seu Zé
}

// Passagem por REFERÊNCIA: a alteração vale para a variável original.
static void DescartarPalito(ref int palitosDoJogador)
{
    // TODO 8: tire um palito
}

static void AnunciarVencedor(string[] nomes, int[] palitos, int rodadas)
{
    // TODO 9: mova para cá o anúncio do vencedor, usando o parâmetro rodadas
    //         no lugar de "rodada - 1"
}

static void JogarPartida(string nomeJogador)
{
    string[] nomes = { nomeJogador, "Computador" };
    int[] palitos = { PALITOS_INICIAIS, PALITOS_INICIAIS };
    int[] escolhas = new int[2];
    int[] palpites = new int[2];
    int rodada = 1;

    while (palitos[JOGADOR] > 0 && palitos[COMPUTADOR] > 0)
    {
        Console.WriteLine();
        Console.WriteLine($"============== RODADA {rodada} ==============");
        // TODO 10: chame o procedimento que mostra o placar
        /* TODO 10 */

        // TODO 11: mova para cá as escolhas e os palpites (do bloco 06)

        int total = Somar(escolhas);
        // TODO 12: chame o procedimento da revelação
        /* TODO 12 */

        bool alguemAcertou = false;
        for (int j = 0; j < nomes.Length; j++)
        {
            if (palpites[j] == total)
            {
                Console.WriteLine($"{nomes[j]} acertou! Descarta um palito.");
                // TODO 13: use DescartarPalito para tirar um palito do jogador j
                /* TODO 13 */
                alguemAcertou = true;
            }
        }
        if (!alguemAcertou)
        {
            Console.WriteLine("Ninguém acertou. Ninguém descarta.");
        }

        rodada++;
    }

    // TODO 14: anuncie o vencedor
    /* TODO 14 */
}
```

Repare que as variáveis da partida (`nomes`, `palitos`, `escolhas`, `palpites` e `rodada`) agora são criadas **dentro** de `JogarPartida`. Assim, cada vez que o jogador escolhe "Jogar" no menu, a partida começa do zero, com 3 palitos para cada um.

### Dicas

- **TODO 1:** `LerInteiro("Escolha uma opção: ", 1, 3)`.
- **TODO 2:** chame o procedimento `ExibirRegras`.
- **TODO 3:** o menu se repete enquanto a opção for diferente de 3.
- **TODO 4, 6, 7, 9 e 11:** é só **recortar** o código do bloco 06 e **colar** dentro do procedimento. Os parâmetros têm os mesmos nomes das variáveis que o código já usava, então quase nada precisa mudar.
- **TODO 5:** um `Console.WriteLine` para cada regra.
- **TODO 8:** um `--` no parâmetro.
- **TODO 10:** passe os vetores `nomes` e `palitos`.
- **TODO 12:** passe `nomes`, `escolhas` e `total`.
- **TODO 13:** não esqueça do `ref` na chamada: `DescartarPalito(ref palitos[j]);`.
- **TODO 14:** passe `nomes`, `palitos` e `rodada - 1`.

---

## Sua vez

1. **Experimento:** tire o `ref` de `DescartarPalito` (na declaração e na chamada) e jogue uma partida. O que acontece? Por quê? (Se o programa não parar, aperte `Ctrl + C`.) Depois, coloque o `ref` de volta.
2. Acrescente uma opção `4 - Sobre o Bar do Seu Zé` no menu, com um procedimento que mostra a história do jogo. Lembre-se de ajustar o `LerInteiro` e a condição do `do-while`.
3. Crie um placar geral: conte, no programa principal, quantas partidas o jogador venceu e quantas perdeu, e mostre esse placar ao sair. Dica: `JogarPartida` pode virar uma **função** que devolve `true` quando o jogador vence.

---

## Checkpoint

O jogo está pronto:

```text
==========================================
        BEM-VINDO AO BAR DO SEU ZÉ
==========================================

Qual é o seu nome? Ana

---------------- MENU ----------------
1 - Jogar uma partida
2 - Ver as regras
3 - Sair
Escolha uma opção: 2

REGRAS DA PORRINHA
- Cada jogador começa com 3 palitos.
...
```

Teste todas as opções do menu e jogue duas partidas seguidas, para conferir se a segunda começa do zero.

```bash
git add .
git commit -m "Bloco 07 concluído"
```

[← Bloco 06](06-funcoes.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 08 →](08-spectre.md)
