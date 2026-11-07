# Bloco 04 – A partida completa

[← Bloco 03](03-decisao.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 05 →](05-vetores.md)

> Uma rodada só não é porrinha. No Bar do Seu Zé, a partida segue rodada após rodada até alguém se livrar de todos os palitos. E ninguém ali aceita palpite repetido.

## Objetivo

Neste bloco você vai aprender a **repetir** trechos de código: enquanto uma condição for verdadeira, até uma entrada ser válida ou um número certo de vezes.

Ao final, o programa vai jogar **uma partida completa**, alternar quem chuta primeiro, impedir palpites repetidos e não vai mais quebrar quando alguém digitar uma letra.

---

## Conceitos

### Repetição com `while`

O `while` (**enquanto**) repete um bloco de código **enquanto** a condição for verdadeira. A condição é testada **antes** de cada repetição. Se ela já começar falsa, o bloco não é executado nenhuma vez.

```csharp
int pasteis = 3;
while (pasteis > 0)
{
    Console.WriteLine($"Saindo um pastel! Restam {pasteis - 1}.");
    pasteis = pasteis - 1;
}
Console.WriteLine("Acabou o pastel.");
```

**Cuidado com o laço infinito:** algo dentro do laço precisa, em algum momento, tornar a condição falsa. Se no exemplo acima você esquecer a linha `pasteis = pasteis - 1;`, o programa nunca para. Se isso acontecer, aperte `Ctrl + C` no terminal para interromper.

### Repetição com `do-while`

O `do-while` (**faça... enquanto**) testa a condição **depois** de cada repetição. Por isso, o bloco é executado **pelo menos uma vez**. Ele é perfeito para validar uma entrada: primeiro pergunta, depois verifica se precisa perguntar de novo.

```csharp
int mesa;
do
{
    Console.Write("Qual mesa (1 a 8)? ");
    mesa = int.Parse(Console.ReadLine() ?? "");
} while (mesa < 1 || mesa > 8);
```

Repare no `;` depois do `while (...)`. No `do-while` ele é obrigatório.

### Repetição com `for`

O `for` (**para**) é usado quando sabemos **quantas vezes** queremos repetir. Ele reúne três partes em uma linha:

```csharp
for (int i = 0; i < 3; i++)
{
    Console.WriteLine($"Pastel número {i + 1}");
}
```

1. `int i = 0`: a **inicialização**, feita uma vez, antes de começar.
2. `i < 3`: a **condição**, testada antes de cada repetição.
3. `i++`: o **incremento**, feito depois de cada repetição. O `i++` é um atalho para `i = i + 1`, e o `i--` é um atalho para `i = i - 1`.

### Contadores e o operador `%`

Uma variável que aumenta de 1 em 1 a cada repetição é chamada de **contador**. No jogo, o contador `rodada` vai dizer em que rodada estamos.

Para alternar quem chuta primeiro, usamos o **resto da divisão por 2**, que você viu no bloco 02:

| `rodada` | `rodada % 2` | Quem chuta primeiro |
| --- | --- | --- |
| 1 | 1 | Jogador |
| 2 | 0 | Computador |
| 3 | 1 | Jogador |
| 4 | 0 | Computador |

Se `rodada % 2 == 1`, a rodada é ímpar.

### Convertendo texto sem quebrar o programa: `int.TryParse`

No bloco 02 você viu que o `int.Parse` quebra o programa quando a pessoa digita `abc`. O `int.TryParse` **tenta** converter e devolve um `bool` dizendo se conseguiu:

```csharp
Console.Write("Quantos pastéis? ");
int pasteis;
bool conseguiu = int.TryParse(Console.ReadLine(), out pasteis);

if (conseguiu)
{
    Console.WriteLine($"Ok, {pasteis} pastéis!");
}
else
{
    Console.WriteLine("Isso não é um número.");
}
```

A palavra `out` indica que o `TryParse` vai **escrever o número convertido** na variável `pasteis`. Assim ele consegue entregar duas coisas de uma vez: o `bool` (deu certo ou não) e o número. Você vai entender melhor esse mecanismo no bloco 07.

---

## No jogo

Agora a estrutura do programa muda: **a rodada inteira vai para dentro de um `while`**. Por isso, abaixo está o `Program.cs` completo do bloco. Use o seu código atual como base, reorganize-o neste formato e complete os `// TODO`.

```csharp
// =====================================================
//  PORRINHA – Bar do Seu Zé
// =====================================================

const int PALITOS_INICIAIS = 3;

Console.WriteLine("==========================================");
Console.WriteLine("        BEM-VINDO AO BAR DO SEU ZÉ        ");
Console.WriteLine("==========================================");
Console.WriteLine();

Console.Write("Qual é o seu nome? ");
string nomeJogador = Console.ReadLine() ?? "";
if (nomeJogador == "")
{
    nomeJogador = "Jogador";
}

int palitosJogador = PALITOS_INICIAIS;
int palitosComputador = PALITOS_INICIAIS;
// TODO 1: crie o contador de rodadas, começando em 1
int rodada = /* TODO 1 */;
bool valido;

Console.WriteLine();
Console.WriteLine($"Olá, {nomeJogador}! Hoje você vai enfrentar o Computador.");
Console.WriteLine($"Cada jogador começa com {PALITOS_INICIAIS} palitos.");

// TODO 2: a partida continua enquanto OS DOIS jogadores ainda tiverem palitos
while (/* TODO 2 */)
{
    Console.WriteLine();
    Console.WriteLine($"============== RODADA {rodada} ==============");

    // Desenha um "|" para cada palito do jogador
    Console.Write($"{nomeJogador}: ");
    for (int i = 0; i < palitosJogador; i++)
    {
        Console.Write("| ");
    }
    Console.WriteLine();

    // TODO 3: faça o mesmo desenho para os palitos do Computador
    Console.Write("Computador: ");
    /* TODO 3 */
    Console.WriteLine();
    Console.WriteLine();

    // ---------- Escolha do jogador (com validação) ----------
    int escolhaJogador;
    do
    {
        Console.Write($"Quantos palitos você vai esconder na mão (0 a {palitosJogador})? ");
        valido = int.TryParse(Console.ReadLine(), out escolhaJogador)
                 && escolhaJogador >= 0 && escolhaJogador <= palitosJogador;
        if (!valido)
        {
            Console.WriteLine($"Valor inválido! Digite um número de 0 a {palitosJogador}.");
        }
    } while (!valido);

    int escolhaComputador = Random.Shared.Next(0, palitosComputador + 1);

    int maximoPalpite = palitosJogador + palitosComputador;
    int palpiteJogador;
    int palpiteComputador;

    // ---------- Palpites: quem começa alterna a cada rodada ----------
    // TODO 4: o jogador chuta primeiro nas rodadas ímpares
    if (/* TODO 4 */)
    {
        // TODO 5: leia o palpite do jogador com validação, como foi feito
        //         com a escolha lá em cima (o palpite vai de 0 a maximoPalpite)
        /* TODO 5 */

        // O Computador sorteia de novo enquanto o palpite dele for igual ao do jogador
        do
        {
            palpiteComputador = Random.Shared.Next(0, maximoPalpite + 1);
        } while (/* TODO 6 */);

        Console.WriteLine($"O Computador chutou {palpiteComputador}.");
    }
    else
    {
        palpiteComputador = Random.Shared.Next(0, maximoPalpite + 1);
        Console.WriteLine($"O Computador chuta primeiro: {palpiteComputador}.");

        do
        {
            Console.Write($"Seu palpite para o total (0 a {maximoPalpite}): ");
            valido = int.TryParse(Console.ReadLine(), out palpiteJogador)
                     && palpiteJogador >= 0 && palpiteJogador <= maximoPalpite;
            if (!valido)
            {
                Console.WriteLine($"Palpite inválido! Digite um número de 0 a {maximoPalpite}.");
            }
            else if (/* TODO 7: o palpite é igual ao do Computador? */)
            {
                Console.WriteLine("Esse palpite já foi dado! Escolha outro.");
                valido = false;
            }
        } while (!valido);
    }

    int total = escolhaJogador + escolhaComputador;

    Console.WriteLine();
    Console.WriteLine("Mãos abertas!");
    Console.WriteLine($"{nomeJogador} escondeu {escolhaJogador} e o Computador escondeu {escolhaComputador}.");
    Console.WriteLine($"Total na mesa: {total}");

    // Cole aqui o seu switch com os comentários do Seu Zé (do bloco 03)

    if (palpiteJogador == total)
    {
        Console.WriteLine($"{nomeJogador} acertou! Descarta um palito.");
        palitosJogador--;
    }
    else if (palpiteComputador == total)
    {
        Console.WriteLine("O Computador acertou! Descarta um palito.");
        palitosComputador--;
    }
    else
    {
        Console.WriteLine("Ninguém acertou. Ninguém descarta.");
    }

    // TODO 8: passe para a próxima rodada
    /* TODO 8 */
}

// ---------- Fim da partida ----------
Console.WriteLine();
Console.WriteLine("==========================================");
// TODO 9: se o jogador ficou sem palitos, ele venceu; senão, o Computador venceu
if (/* TODO 9 */)
{
    Console.WriteLine($"PARABÉNS, {nomeJogador}! Você venceu a porrinha!");
}
else
{
    Console.WriteLine("O Computador venceu desta vez. Bora treinar mais!");
}
Console.WriteLine($"A partida durou {rodada - 1} rodadas.");
Console.WriteLine("==========================================");
```

Algumas observações sobre o que mudou:

- Os `if` de validação do bloco 03 **saíram**. No lugar, o `do-while` pergunta de novo até o valor ser válido.
- A linha do placar do bloco 03 também saiu, porque agora o desenho dos palitos aparece no início de cada rodada.
- A variável `valido` é declarada **antes** do `while`, porque ela é usada em vários laços diferentes.
- A condição do `do-while` de validação usa `&&` para juntar três testes: "conseguiu converter **e** é maior ou igual a 0 **e** é menor ou igual ao máximo".

### Dicas

- **TODO 1:** é um `int` comum.
- **TODO 2:** use `palitosJogador > 0` e `palitosComputador > 0`. Os dois precisam ser verdadeiros ao mesmo tempo. Qual operador lógico faz isso?
- **TODO 3:** copie o `for` do jogador e troque a variável de palitos.
- **TODO 4:** veja a tabela do operador `%` nos conceitos.
- **TODO 5:** é um `do-while` quase igual ao da escolha. Troque `escolhaJogador` por `palpiteJogador`, `palitosJogador` por `maximoPalpite` e ajuste as mensagens.
- **TODO 6:** o Computador precisa sortear de novo **enquanto** o palpite dele for igual ao do jogador.
- **TODO 7:** compare `palpiteJogador` com `palpiteComputador`.
- **TODO 8:** aumente o contador em 1. Use o atalho que você viu no `for`.
- **TODO 9:** quem chega a 0 palitos vence.
- **Por que `rodada - 1` na última linha?** O contador é incrementado no fim de cada rodada, inclusive da última. Quando o laço termina, ele está uma unidade à frente.

---

## Sua vez

1. Crie um **contador de acertos** do jogador e mostre, no fim da partida, quantas vezes ele acertou o total.
2. No fim da partida, pergunte `Jogar de novo? (s/n)`. Se a resposta for `s`, a partida recomeça. Dica: envolva a partida em um `do-while` e lembre-se de voltar os palitos e a rodada aos valores iniciais.
3. **Para pensar:** por que foi usado `do-while` para validar a entrada e `while` para a partida? O que aconteceria se trocássemos um pelo outro?

---

## Checkpoint

O programa joga até alguém vencer e não quebra com entradas inválidas:

```text
============== RODADA 2 ==============
Ana: | |
Computador: | | |

Quantos palitos você vai esconder na mão (0 a 2)? abc
Valor inválido! Digite um número de 0 a 2.
Quantos palitos você vai esconder na mão (0 a 2)? 1
O Computador chuta primeiro: 3.
Seu palpite para o total (0 a 5): 3
Esse palpite já foi dado! Escolha outro.
Seu palpite para o total (0 a 5): 2
...

==========================================
PARABÉNS, Ana! Você venceu a porrinha!
A partida durou 7 rodadas.
==========================================
```

Teste bem: digite letras, números negativos, números grandes demais e palpites repetidos.

```bash
git add .
git commit -m "Bloco 04 concluído"
```

[← Bloco 03](03-decisao.md) · [Voltar ao início](../README.md) · [Próximo: Bloco 05 →](05-vetores.md)
