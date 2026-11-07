# Bloco 08 – A cara do bar

[← Bloco 07](07-procedimentos.md) · [Voltar ao início](../README.md)

> O jogo está pronto e o Computador já joga quase como o Seu Zé. Mas ele ainda reclama: "Tá tudo muito sem graça! Cadê as cores, cadê o letreiro?" Neste último bloco, o jogo ganha a cara do bar.

## Objetivo

Neste bloco você vai aprender a usar uma **biblioteca externa**, a **Spectre.Console**, para deixar a interface do jogo mais bonita: título em letras grandes, cores, placar em tabela e menu com as setas do teclado.

Este bloco é **totalmente guiado**. Cada alteração aparece no formato **antes e depois**, com a explicação de cada comando novo. E repare em uma coisa importante: **a lógica do jogo não muda**. As regras, os vetores, as funções `Somar` e `PalpiteComputador`, o `DescartarPalito`... tudo continua igual. Só muda a forma de mostrar e de perguntar. Isso é **manutenção de software**: melhorar uma parte do sistema sem quebrar o resto.

---

## Conceitos

### O que é uma biblioteca

Uma **biblioteca** é um conjunto de código pronto, escrito por outras pessoas, que você pode usar no seu programa. Em vez de programar do zero uma tabela com bordas no console, você usa a de uma biblioteca que já faz isso bem.

No mundo .NET, as bibliotecas são distribuídas como **pacotes NuGet**, baixados do site [nuget.org](https://www.nuget.org). A Spectre.Console é um pacote gratuito e de código aberto: [spectreconsole.net](https://spectreconsole.net).

### Um aviso sobre o `new`

A Spectre.Console usa uma forma de escrever que você ainda não viu:

```csharp
AnsiConsole.Write(new FigletText("Porrinha").Centered().Color(Color.Yellow));
```

- O `new FigletText("Porrinha")` **cria um elemento visual**, neste caso um texto em letras grandes. É assim que a biblioteca pede para criar os elementos dela.
- Cada `.Centered()`, `.Color(...)` depois dele **acrescenta uma configuração** ao elemento. Dá para encadear quantas quiser.
- O `AnsiConsole.Write(...)` **desenha** o elemento na tela.

Você não precisa entender como isso funciona por dentro para usar. Pense assim: **crie o elemento, configure, desenhe**.

---

## Passo 1 – Instalando a biblioteca

### Se você roda com `dotnet run Program.cs` (.NET 10)

Coloque esta linha como **a primeira linha** do `Program.cs`, antes de qualquer comentário ou código:

```csharp
#:package Spectre.Console@0.57.2
```

Essa diretiva diz ao .NET: "este programa precisa do pacote Spectre.Console, na versão 0.57.2". Na primeira vez que você rodar, o .NET baixa o pacote sozinho.

> Os números depois do `@` são a versão do pacote. Fixar a versão garante que o tutorial funcione do mesmo jeito para todo mundo. Você pode conferir as versões disponíveis em [nuget.org/packages/Spectre.Console](https://www.nuget.org/packages/Spectre.Console).

### Se você usa um projeto do Visual Studio

**Não** coloque a linha `#:package`: ela só funciona quando o arquivo é rodado sozinho e dá erro dentro de um projeto. No lugar dela, instale o pacote no projeto. No terminal, na pasta do projeto:

```bash
dotnet add package Spectre.Console
```

Ou, no Visual Studio, clique com o botão direito no projeto, escolha **Gerenciar Pacotes do NuGet**, procure **Spectre.Console** e clique em **Instalar**.

### Nos dois casos

Logo depois do comentário do topo, adicione:

```csharp
using Spectre.Console;
```

O `using` avisa ao C# que você vai usar os comandos que estão dentro da biblioteca. Sem ele, o C# não reconhece `AnsiConsole`, `Table` e os outros.

### Onde rodar

O menu com as setas do teclado precisa de um **terminal de verdade**. Rode o jogo no terminal do sistema ou no terminal integrado do VS Code. A janela de saída de depuração de alguns editores não aceita as setas.

---

## Passo 2 – Cores com *markup*

O comando principal da Spectre.Console é o `AnsiConsole.MarkupLine`. Ele funciona como o `Console.WriteLine`, mas entende **etiquetas** entre colchetes que mudam a aparência do texto:

```csharp
AnsiConsole.MarkupLine("Pastel de [yellow]queijo[/] saindo!");
AnsiConsole.MarkupLine("[bold red]Acabou o pastel![/]");
```

- `[yellow]` abre a etiqueta e `[/]` fecha. Só o texto entre elas fica amarelo.
- Dá para combinar estilos em uma etiqueta: `[bold red]` é negrito e vermelho.

| Etiqueta | Efeito |
| --- | --- |
| `[red]`, `[green]`, `[yellow]`, `[blue]`, `[grey]` | Cores do texto |
| `[orange1]`, `[purple]`, `[aqua]` | Mais cores |
| `[bold]` | Negrito |
| `[italic]` | Itálico |
| `[underline]` | Sublinhado |

### Cuidado com colchetes

Como os colchetes têm um significado especial, um texto com `[` ou `]` pode confundir a biblioteca. Isso é perigoso com textos **digitados pelo usuário**: se alguém disser que se chama `[Ana]`, o programa quebra.

Para evitar isso, todo texto que vem do usuário passa pelo `Markup.Escape(...)`, que deixa os colchetes "inofensivos":

```csharp
AnsiConsole.MarkupLine($"Olá, [green]{Markup.Escape(nome)}[/]!");
```

Neste bloco, **todo nome de jogador** mostrado com *markup* vai passar pelo `Markup.Escape`.

---

## Passo 3 – O título

**Antes:**

```csharp
static void ExibirTitulo()
{
    Console.WriteLine("==========================================");
    Console.WriteLine("        BEM-VINDO AO BAR DO SEU ZÉ        ");
    Console.WriteLine("==========================================");
    Console.WriteLine();
}
```

**Depois:**

```csharp
static void ExibirTitulo()
{
    AnsiConsole.Write(new FigletText("Porrinha").Centered().Color(Color.Yellow));
    AnsiConsole.Write(new Rule("[bold]Bar do Seu Zé[/]"));
    AnsiConsole.WriteLine();
}
```

O que cada comando faz:

- `new FigletText("Porrinha")` cria um texto desenhado com letras grandes, feitas de caracteres. A fonte padrão não tem acentos, por isso usamos uma palavra sem acento.
- `.Centered()` centraliza o texto na largura do terminal.
- `.Color(Color.Yellow)` pinta o texto de amarelo. As cores ficam em `Color`: `Color.Red`, `Color.Green`, `Color.Grey`...
- `new Rule("...")` cria uma **linha horizontal** de ponta a ponta do terminal, com um título no meio. O título aceita *markup*.
- `AnsiConsole.WriteLine()` pula uma linha, como o `Console.WriteLine()`.

---

## Passo 4 – Perguntando o nome

**Antes:**

```csharp
Console.Write("Qual é o seu nome? ");
string nomeJogador = Console.ReadLine() ?? "";
if (nomeJogador == "")
{
    nomeJogador = "Jogador";
}
```

**Depois:**

```csharp
string nomeJogador = AnsiConsole.Ask<string>("Qual é o seu [green]nome[/]?");
```

- `AnsiConsole.Ask<string>(...)` mostra a pergunta e lê a resposta. O `<string>` diz o tipo da resposta que queremos.
- A pergunta aceita *markup*, por isso a palavra "nome" aparece em verde.
- O `Ask` **não aceita resposta vazia**: se a pessoa só apertar Enter, ele pergunta de novo. Por isso o `if` do nome vazio não é mais necessário.

---

## Passo 5 – O menu com setas

**Antes:**

```csharp
int opcao;
do
{
    ExibirMenu();
    opcao = LerInteiro("Escolha uma opção: ", 1, 3);

    switch (opcao)
    {
        case 1:
            JogarPartida(nomeJogador);
            break;
        case 2:
            ExibirRegras();
            break;
        case 3:
            Console.WriteLine("Até sexta que vem no Bar do Seu Zé!");
            break;
    }
} while (opcao != 3);
```

**Depois:**

```csharp
string opcao;
do
{
    AnsiConsole.WriteLine();
    opcao = AnsiConsole.Prompt(
        new SelectionPrompt<string>()
            .Title("[yellow]O que você quer fazer?[/]")
            .AddChoices("Jogar uma partida", "Ver as regras", "Sair"));

    switch (opcao)
    {
        case "Jogar uma partida":
            JogarPartida(nomeJogador);
            break;
        case "Ver as regras":
            ExibirRegras();
            break;
        case "Sair":
            AnsiConsole.MarkupLine("[yellow]Até sexta que vem no Bar do Seu Zé![/]");
            break;
    }
} while (opcao != "Sair");
```

O que mudou:

- `new SelectionPrompt<string>()` cria um **menu de seleção**: o jogador escolhe a opção com as **setas** e confirma com **Enter**. O `<string>` diz que cada opção é um texto.
- `.Title(...)` define a pergunta que aparece em cima das opções.
- `.AddChoices(...)` define as opções, separadas por vírgula.
- `AnsiConsole.Prompt(...)` mostra o menu, espera a escolha e **devolve o texto da opção escolhida**.
- Como a resposta agora é um texto, a variável `opcao` virou `string`, e o `switch` compara textos: `case "Jogar uma partida":`. O `switch` funciona com textos do mesmo jeito que funciona com números.

Com isso, o procedimento `ExibirMenu` não é mais usado. **Apague** esse procedimento.

Repare: não existe mais a possibilidade de digitar uma opção inválida, porque o jogador só consegue escolher entre as opções da lista.

---

## Passo 6 – As regras em um painel

**Antes:**

```csharp
static void ExibirRegras()
{
    Console.WriteLine();
    Console.WriteLine("REGRAS DA PORRINHA");
    Console.WriteLine($"- Cada jogador começa com {PALITOS_INICIAIS} palitos.");
    // ... as outras regras
}
```

**Depois:**

```csharp
static void ExibirRegras()
{
    string regras =
        $"Cada jogador começa com [bold]{PALITOS_INICIAIS}[/] palitos.\n" +
        "A cada rodada, cada um esconde na mão de 0 até todos os palitos que tem.\n" +
        "Cada um dá um palpite do [bold]total[/] de palitos nas duas mãos.\n" +
        "O segundo a chutar [red]não pode repetir[/] o palpite do primeiro.\n" +
        "Quem acerta descarta um palito.\n" +
        "Vence quem ficar [green]sem palitos[/] primeiro.";

    Panel painel = new Panel(regras)
        .Header("[yellow] Regras da Porrinha [/]")
        .BorderColor(Color.Yellow);

    AnsiConsole.Write(painel);
}
```

Duas coisas novas aqui:

- **Montando um texto grande:** o `+` entre textos **junta** os textos em um só. O `\n` dentro de um texto significa **quebra de linha**. Assim, a variável `regras` guarda as seis regras, uma por linha.
- **O painel:** `new Panel(regras)` cria uma **caixa com borda** em volta do texto, que aceita *markup*. O `.Header(...)` coloca um título na borda de cima, e o `.BorderColor(...)` muda a cor da borda.

Desta vez o painel foi guardado em uma variável do tipo `Panel` antes de ser desenhado. Funciona igual a criar e desenhar na mesma linha, e deixa o código mais fácil de ler quando há muitas configurações.

---

## Passo 7 – O placar em tabela

**Antes:**

```csharp
static void ExibirPlacar(string[] nomes, int[] palitos)
{
    for (int j = 0; j < nomes.Length; j++)
    {
        Console.Write($"{nomes[j]}: ");
        for (int i = 0; i < palitos[j]; i++)
        {
            Console.Write("| ");
        }
        Console.WriteLine();
    }
    Console.WriteLine();
}
```

**Depois:**

```csharp
static void ExibirPlacar(string[] nomes, int[] palitos)
{
    Table tabela = new Table()
        .Border(TableBorder.Rounded)
        .BorderColor(Color.Grey);

    tabela.AddColumn("Jogador");
    tabela.AddColumn("Palitos");

    for (int j = 0; j < nomes.Length; j++)
    {
        string desenho = "";
        for (int i = 0; i < palitos[j]; i++)
        {
            desenho += "| ";
        }
        tabela.AddRow(Markup.Escape(nomes[j]), $"[yellow]{desenho}[/]");
    }

    AnsiConsole.Write(tabela);
}
```

O que cada comando faz:

- `new Table()` cria uma tabela vazia.
- `.Border(TableBorder.Rounded)` escolhe uma borda com cantos arredondados. Experimente também `TableBorder.Double` ou `TableBorder.Simple`.
- `tabela.AddColumn("Jogador")` acrescenta uma coluna com esse cabeçalho.
- `tabela.AddRow(...)` acrescenta uma linha. Passamos um texto para cada coluna, na mesma ordem das colunas. Os textos aceitam *markup*.
- `AnsiConsole.Write(tabela)` desenha a tabela pronta.

Repare na lógica do desenho: antes, cada `|` era escrito direto na tela. Agora, os `|` são **juntados** na variável `desenho` com o `+=`, porque a tabela precisa receber o texto inteiro de cada célula de uma vez. O `for` continua o mesmo.

---

## Passo 8 – O início da rodada

Dentro de `JogarPartida`, **antes:**

```csharp
Console.WriteLine();
Console.WriteLine($"============== RODADA {rodada} ==============");
ExibirPlacar(nomes, palitos);
```

**Depois:**

```csharp
AnsiConsole.WriteLine();
AnsiConsole.Write(new Rule($"[yellow]Rodada {rodada}[/]"));
ExibirPlacar(nomes, palitos);
```

É a mesma linha horizontal do título, agora com o número da rodada. Repare que o *markup* também funciona com interpolação: o `$` e as chaves continuam funcionando normalmente.

---

## Passo 9 – Perguntando números: `TextPrompt` × `LerInteiro`

Este é o passo mais interessante do bloco. Lembra do trabalho que deu, nos blocos 04 e 06, fazer o `LerInteiro`? Um `do-while`, o `int.TryParse`, a verificação dos limites, a mensagem de erro...

A Spectre.Console tem um comando que faz **tudo isso sozinho**: o `TextPrompt`.

**Antes:**

```csharp
static int LerInteiro(string mensagem, int minimo, int maximo)
{
    int valor;
    bool valido;
    do
    {
        Console.Write(mensagem);
        valido = int.TryParse(Console.ReadLine(), out valor)
                 && valor >= minimo && valor <= maximo;
        if (!valido)
        {
            Console.WriteLine($"Valor inválido! Digite um número de {minimo} a {maximo}.");
        }
    } while (!valido);
    return valor;
}
```

**Depois:**

```csharp
static int LerInteiro(string mensagem, int minimo, int maximo)
{
    TextPrompt<int> pergunta = new TextPrompt<int>(mensagem)
        .ValidationErrorMessage("[red]Isso não é um número inteiro.[/]")
        .Validate(valor => valor >= minimo && valor <= maximo,
                  $"[red]Digite um número de {minimo} a {maximo}.[/]");

    return AnsiConsole.Prompt(pergunta);
}
```

Compare as duas versões, parte por parte:

| O que precisa ser feito | Antes (o seu código) | Depois (Spectre.Console) |
| --- | --- | --- |
| Mostrar a pergunta | `Console.Write(mensagem)` | `new TextPrompt<int>(mensagem)` |
| Converter o texto em número | `int.TryParse(...)` | o `<int>` faz a conversão sozinho |
| Erro quando não é número | `if (!valido)` com mensagem | `.ValidationErrorMessage(...)` |
| Verificar os limites | `valor >= minimo && valor <= maximo` | `.Validate(...)` com a mesma condição |
| Perguntar de novo se for inválido | o `do-while` | o `AnsiConsole.Prompt` repete sozinho |
| Devolver o valor | `return valor` | `return AnsiConsole.Prompt(pergunta)` |

Tem uma novidade no `.Validate`: o trecho `valor => valor >= minimo && valor <= maximo`. Ele é uma **mini função escrita no meio do código**, chamada de **expressão lambda**. Leia assim: "dado um `valor`, ele é válido se `valor >= minimo && valor <= maximo`". A biblioteca chama essa mini função para cada número digitado, e pergunta de novo enquanto o resultado for `false`.

Repare em outro detalhe: **o resto do programa não precisou mudar nada**. Todas as chamadas de `LerInteiro` continuam iguais, porque a função tem o mesmo nome, os mesmos parâmetros e devolve o mesmo tipo. Só o "miolo" mudou. Essa é uma das grandes vantagens de organizar o código em funções.

Só um ajuste: o `TextPrompt` já deixa um espaço depois da pergunta. Por isso, tire o espaço do fim das mensagens que você passa para o `LerInteiro`. Por exemplo, `"... (0 a {palitos[JOGADOR]})? "` vira `"... (0 a {palitos[JOGADOR]})?"`. Nas mensagens dos palpites, troque também o `:` final por `?`, para combinar com as outras perguntas.

**Então eu fiz o `LerInteiro` à toa?** De jeito nenhum. Agora você sabe exatamente o que o `TextPrompt` está fazendo por você. Quem nunca escreveu essa lógica usa a biblioteca como uma caixa-preta. Você sabe o que tem dentro dela.

---

## Passo 10 – A revelação em um painel

**Antes:**

```csharp
static void ExibirRevelacao(string[] nomes, int[] escolhas, int total)
{
    Console.WriteLine();
    Console.WriteLine("Mãos abertas!");
    for (int j = 0; j < nomes.Length; j++)
    {
        Console.WriteLine($"{nomes[j]} escondeu {escolhas[j]}.");
    }
    Console.WriteLine($"Total na mesa: {total}");

    switch (total)
    {
        case 0:
            Console.WriteLine("Seu Zé: \"Mão vazia geral! Ninguém arriscou nada.\"");
            break;
        // ...
    }
}
```

**Depois:**

```csharp
static void ExibirRevelacao(string[] nomes, int[] escolhas, int total)
{
    string texto = "";
    for (int j = 0; j < nomes.Length; j++)
    {
        texto += $"{Markup.Escape(nomes[j])} escondeu [bold]{escolhas[j]}[/]\n";
    }
    texto += $"Total na mesa: [bold yellow]{total}[/]";

    Panel painel = new Panel(texto)
        .Header("[bold] Mãos abertas! [/]")
        .BorderColor(Color.Orange1);

    AnsiConsole.Write(painel);

    switch (total)
    {
        case 0:
            AnsiConsole.MarkupLine("[grey]Seu Zé: \"Mão vazia geral! Ninguém arriscou nada.\"[/]");
            break;
        case 1:
            AnsiConsole.MarkupLine("[grey]Seu Zé: \"Um palitinho solitário na mesa!\"[/]");
            break;
        default:
            AnsiConsole.MarkupLine("[grey]Seu Zé: \"Olha a mesa cheia de palito!\"[/]");
            break;
    }
}
```

É a mesma técnica do placar: em vez de escrever cada linha direto na tela, o `for` **junta** as linhas na variável `texto` (com o `\n` no fim de cada uma), e o texto completo vai para dentro de um painel. Os comentários do Seu Zé ficam em cinza, como quem fala lá do balcão.

---

## Passo 11 – Mensagens coloridas

Agora troque os `Console.WriteLine` que sobraram dentro de `JogarPartida` por `AnsiConsole.MarkupLine`, com cores que ajudam a entender o jogo:

```csharp
// Palpite do Computador
AnsiConsole.MarkupLine($"O {Markup.Escape(nomes[COMPUTADOR])} chutou [bold]{palpites[COMPUTADOR]}[/].");

// Quando o Computador chuta primeiro
AnsiConsole.MarkupLine($"O {Markup.Escape(nomes[COMPUTADOR])} chuta primeiro: [bold]{palpites[COMPUTADOR]}[/].");

// Palpite repetido
AnsiConsole.MarkupLine("[red]Esse palpite já foi dado! Escolha outro.[/]");

// Alguém acertou
AnsiConsole.MarkupLine($"[green]{Markup.Escape(nomes[j])} acertou![/] Descarta um palito.");

// Ninguém acertou
AnsiConsole.MarkupLine("[grey]Ninguém acertou. Ninguém descarta.[/]");
```

A ideia é usar as cores com **significado**: verde para coisa boa, vermelho para erro, cinza para informação de menor importância e negrito para os números que importam.

---

## Passo 12 – O anúncio do vencedor

**Antes:**

```csharp
static void AnunciarVencedor(string[] nomes, int[] palitos, int rodadas)
{
    Console.WriteLine();
    Console.WriteLine("==========================================");
    if (palitos[JOGADOR] == 0)
    {
        Console.WriteLine($"PARABÉNS, {nomes[JOGADOR]}! Você venceu a porrinha!");
    }
    else
    {
        Console.WriteLine($"O {nomes[COMPUTADOR]} venceu desta vez. Bora treinar mais!");
    }
    Console.WriteLine($"A partida durou {rodadas} rodadas.");
    Console.WriteLine("==========================================");
}
```

**Depois:**

```csharp
static void AnunciarVencedor(string[] nomes, int[] palitos, int rodadas)
{
    AnsiConsole.WriteLine();
    if (palitos[JOGADOR] == 0)
    {
        AnsiConsole.Write(new FigletText("Venceu!").Centered().Color(Color.Green));
        AnsiConsole.MarkupLine($"[green]Parabéns, {Markup.Escape(nomes[JOGADOR])}! Nem o Seu Zé jogaria melhor.[/]");
    }
    else
    {
        AnsiConsole.MarkupLine($"[red]O {Markup.Escape(nomes[COMPUTADOR])} venceu desta vez. Bora treinar mais![/]");
    }
    AnsiConsole.MarkupLine($"A partida durou [bold]{rodadas}[/] rodadas.");
}
```

A vitória merece letreiro: o mesmo `FigletText` do título, agora em verde.

---

## O que não mudou

Antes de terminar, confira o que ficou **exatamente igual** ao bloco 07:

- As constantes `PALITOS_INICIAIS`, `JOGADOR` e `COMPUTADOR`.
- Os vetores `nomes`, `palitos`, `escolhas` e `palpites`.
- O `while` da partida, a alternância com `rodada % 2` e a verificação de quem acertou.
- As funções `Somar` e `PalpiteComputador`.
- O procedimento `DescartarPalito`, com o `ref`.
- Todas as chamadas de `LerInteiro`, que continua com o mesmo nome, os mesmos parâmetros e o mesmo retorno.

Toda a **lógica** do jogo ficou intacta. Só a **interface** mudou. Separar bem essas duas coisas é o que permite trocar a "cara" de um programa sem medo de quebrar as regras dele.

---

## Código completo

Para conferir, este é o `Program.cs` completo do bloco 08. Se você usa um projeto do Visual Studio, lembre-se de **não** incluir a primeira linha (`#:package`).

```csharp
#:package Spectre.Console@0.57.2

// =====================================================
//  PORRINHA – Bar do Seu Zé
// =====================================================

using Spectre.Console;

const int PALITOS_INICIAIS = 3;
const int JOGADOR = 0;
const int COMPUTADOR = 1;

ExibirTitulo();

string nomeJogador = AnsiConsole.Ask<string>("Qual é o seu [green]nome[/]?");

string opcao;
do
{
    AnsiConsole.WriteLine();
    opcao = AnsiConsole.Prompt(
        new SelectionPrompt<string>()
            .Title("[yellow]O que você quer fazer?[/]")
            .AddChoices("Jogar uma partida", "Ver as regras", "Sair"));

    switch (opcao)
    {
        case "Jogar uma partida":
            JogarPartida(nomeJogador);
            break;
        case "Ver as regras":
            ExibirRegras();
            break;
        case "Sair":
            AnsiConsole.MarkupLine("[yellow]Até sexta que vem no Bar do Seu Zé![/]");
            break;
    }
} while (opcao != "Sair");


// =====================================================
//  PROCEDIMENTOS
// =====================================================

static void ExibirTitulo()
{
    AnsiConsole.Write(new FigletText("Porrinha").Centered().Color(Color.Yellow));
    AnsiConsole.Write(new Rule("[bold]Bar do Seu Zé[/]"));
    AnsiConsole.WriteLine();
}

static void ExibirRegras()
{
    string regras =
        $"Cada jogador começa com [bold]{PALITOS_INICIAIS}[/] palitos.\n" +
        "A cada rodada, cada um esconde na mão de 0 até todos os palitos que tem.\n" +
        "Cada um dá um palpite do [bold]total[/] de palitos nas duas mãos.\n" +
        "O segundo a chutar [red]não pode repetir[/] o palpite do primeiro.\n" +
        "Quem acerta descarta um palito.\n" +
        "Vence quem ficar [green]sem palitos[/] primeiro.";

    Panel painel = new Panel(regras)
        .Header("[yellow] Regras da Porrinha [/]")
        .BorderColor(Color.Yellow);

    AnsiConsole.Write(painel);
}

static void ExibirPlacar(string[] nomes, int[] palitos)
{
    Table tabela = new Table()
        .Border(TableBorder.Rounded)
        .BorderColor(Color.Grey);

    tabela.AddColumn("Jogador");
    tabela.AddColumn("Palitos");

    for (int j = 0; j < nomes.Length; j++)
    {
        string desenho = "";
        for (int i = 0; i < palitos[j]; i++)
        {
            desenho += "| ";
        }
        tabela.AddRow(Markup.Escape(nomes[j]), $"[yellow]{desenho}[/]");
    }

    AnsiConsole.Write(tabela);
}

static void ExibirRevelacao(string[] nomes, int[] escolhas, int total)
{
    string texto = "";
    for (int j = 0; j < nomes.Length; j++)
    {
        texto += $"{Markup.Escape(nomes[j])} escondeu [bold]{escolhas[j]}[/]\n";
    }
    texto += $"Total na mesa: [bold yellow]{total}[/]";

    Panel painel = new Panel(texto)
        .Header("[bold] Mãos abertas! [/]")
        .BorderColor(Color.Orange1);

    AnsiConsole.Write(painel);

    switch (total)
    {
        case 0:
            AnsiConsole.MarkupLine("[grey]Seu Zé: \"Mão vazia geral! Ninguém arriscou nada.\"[/]");
            break;
        case 1:
            AnsiConsole.MarkupLine("[grey]Seu Zé: \"Um palitinho solitário na mesa!\"[/]");
            break;
        default:
            AnsiConsole.MarkupLine("[grey]Seu Zé: \"Olha a mesa cheia de palito!\"[/]");
            break;
    }
}

static void DescartarPalito(ref int palitosDoJogador)
{
    palitosDoJogador--;
}

static void AnunciarVencedor(string[] nomes, int[] palitos, int rodadas)
{
    AnsiConsole.WriteLine();
    if (palitos[JOGADOR] == 0)
    {
        AnsiConsole.Write(new FigletText("Venceu!").Centered().Color(Color.Green));
        AnsiConsole.MarkupLine($"[green]Parabéns, {Markup.Escape(nomes[JOGADOR])}! Nem o Seu Zé jogaria melhor.[/]");
    }
    else
    {
        AnsiConsole.MarkupLine($"[red]O {Markup.Escape(nomes[COMPUTADOR])} venceu desta vez. Bora treinar mais![/]");
    }
    AnsiConsole.MarkupLine($"A partida durou [bold]{rodadas}[/] rodadas.");
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
        AnsiConsole.WriteLine();
        AnsiConsole.Write(new Rule($"[yellow]Rodada {rodada}[/]"));
        ExibirPlacar(nomes, palitos);

        // ---------- Escolhas ----------
        escolhas[JOGADOR] = LerInteiro($"Quantos palitos você vai esconder na mão (0 a {palitos[JOGADOR]})?", 0, palitos[JOGADOR]);
        escolhas[COMPUTADOR] = Random.Shared.Next(0, palitos[COMPUTADOR] + 1);

        int maximoPalpite = Somar(palitos);

        // ---------- Palpites ----------
        if (rodada % 2 == 1)
        {
            palpites[JOGADOR] = LerInteiro($"Seu palpite para o total (0 a {maximoPalpite})?", 0, maximoPalpite);
            palpites[COMPUTADOR] = PalpiteComputador(escolhas[COMPUTADOR], palitos[JOGADOR], palpites[JOGADOR]);
            AnsiConsole.MarkupLine($"O {Markup.Escape(nomes[COMPUTADOR])} chutou [bold]{palpites[COMPUTADOR]}[/].");
        }
        else
        {
            palpites[COMPUTADOR] = PalpiteComputador(escolhas[COMPUTADOR], palitos[JOGADOR], -1);
            AnsiConsole.MarkupLine($"O {Markup.Escape(nomes[COMPUTADOR])} chuta primeiro: [bold]{palpites[COMPUTADOR]}[/].");

            palpites[JOGADOR] = LerInteiro($"Seu palpite para o total (0 a {maximoPalpite})?", 0, maximoPalpite);
            while (palpites[JOGADOR] == palpites[COMPUTADOR])
            {
                AnsiConsole.MarkupLine("[red]Esse palpite já foi dado! Escolha outro.[/]");
                palpites[JOGADOR] = LerInteiro($"Seu palpite para o total (0 a {maximoPalpite})?", 0, maximoPalpite);
            }
        }

        // ---------- Revelação e resultado ----------
        int total = Somar(escolhas);
        ExibirRevelacao(nomes, escolhas, total);

        bool alguemAcertou = false;
        for (int j = 0; j < nomes.Length; j++)
        {
            if (palpites[j] == total)
            {
                AnsiConsole.MarkupLine($"[green]{Markup.Escape(nomes[j])} acertou![/] Descarta um palito.");
                DescartarPalito(ref palitos[j]);
                alguemAcertou = true;
            }
        }
        if (!alguemAcertou)
        {
            AnsiConsole.MarkupLine("[grey]Ninguém acertou. Ninguém descarta.[/]");
        }

        rodada++;
    }

    AnunciarVencedor(nomes, palitos, rodada - 1);
}


// =====================================================
//  FUNÇÕES
// =====================================================

static int LerInteiro(string mensagem, int minimo, int maximo)
{
    TextPrompt<int> pergunta = new TextPrompt<int>(mensagem)
        .ValidationErrorMessage("[red]Isso não é um número inteiro.[/]")
        .Validate(valor => valor >= minimo && valor <= maximo,
                  $"[red]Digite um número de {minimo} a {maximo}.[/]");

    return AnsiConsole.Prompt(pergunta);
}

static int Somar(int[] valores)
{
    int soma = 0;
    for (int i = 0; i < valores.Length; i++)
    {
        soma += valores[i];
    }
    return soma;
}

static int PalpiteComputador(int escolhaPropria, int palitosAdversario, int palpiteProibido)
{
    int palpite;
    do
    {
        palpite = escolhaPropria + Random.Shared.Next(0, palitosAdversario + 1);
    } while (palpite == palpiteProibido);
    return palpite;
}
```

---

## Sua vez

1. Troque as cores e as bordas a seu gosto. Experimente `TableBorder.Double`, `Color.Aqua` e o estilo `[italic]`.
2. Acrescente uma terceira coluna no placar com o número de acertos de cada jogador (se você fez o vetor `acertos` no bloco 05).
3. Na documentação da Spectre.Console, em [spectreconsole.net](https://spectreconsole.net), procure o componente **Status** e use-o para mostrar uma animação de "O Computador está pensando..." antes do palpite dele.

---

## Checkpoint

O jogo deve abrir com o letreiro **PORRINHA** em amarelo, o menu deve funcionar com as setas do teclado e cada rodada deve mostrar o placar em uma tabela e a revelação em um painel colorido.

```bash
git add .
git commit -m "Bloco 08 concluído"
git push
```

**Parabéns!** Você construiu um jogo completo, do primeiro `Console.WriteLine` até uma interface caprichada, usando variáveis, decisões, repetições, vetores, funções e procedimentos. Agora é só aparecer na sexta-feira no Bar do Seu Zé e mostrar que você treinou.

[← Bloco 07](07-procedimentos.md) · [Voltar ao início](../README.md)
