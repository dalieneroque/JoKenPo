# Jogo JoKenPo (Pedra, Papel e Tesoura)

Este projeto implementa um jogo de Pedra, Papel e Tesoura utilizando C# com Windows Forms. Abaixo está a explicação detalhada do código principal, seguindo o passo a passo.

---

## Estrutura Geral do Projeto

O projeto é composto por duas classes principais:
1. **Form1**: Gerencia a interface gráfica e as interações do jogador.
2. **Game**: Contém a lógica principal do jogo.

---

<h2>Como funciona o jogo?</h2>

<ol>
  <li>O jogador escolhe <strong>Pedra</strong>, <strong>Papel</strong> ou <strong>Tesoura</strong> clicando em um botão.</li>
  <li>A função <code>StartGame</code> é acionada com o número correspondente à escolha do jogador.</li>
  <li>A lógica do jogo é processada pela classe <code>Game</code>, que determina o resultado:
    <ul>
      <li><strong>Vitória:</strong> Quando o jogador vence o computador.</li>
      <li><strong>Derrota:</strong> Quando o computador vence o jogador.</li>
      <li><strong>Empate:</strong> Quando ambos fazem a mesma escolha.</li>
    </ul>
  </li>
  <li>O resultado é exibido de forma clara e intuitiva:
    <ul>
      <li>Uma imagem representando o resultado.</li>
      <li>As escolhas feitas pelo jogador e pelo computador.</li>
    </ul>
  </li>
</ol>

---

<h2>Estrutura do Código</h2>

<ul>
  <li>
    Define o <code>namespace JoKenPo</code>, que agrupa o código em um escopo lógico.
  </li>
  <li>
    A classe <code>Form1</code> herda de <code>Form</code>, representando a janela principal da aplicação.
  </li>
  <li>
    O método <code>Form1()</code> é o construtor da classe. Ele chama o método <code>InitializeComponent</code>, que configura a interface gráfica.
  </li>
</ul>


```
namespace JoKenPo
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
    }
}
```

---

<h2>Eventos de Clique para os Botões do Jogo</h2>

<ul>
  <li>
    <code>btnPedra_Click</code>: Chamado ao clicar no botão <strong>"Pedra"</strong>.
  </li>
  <li>
    <code>btnPapel_Click</code>: Chamado ao clicar no botão <strong>"Papel"</strong>.
  </li>
  <li>
    <code>btnTesoura_Click</code>: Chamado ao clicar no botão <strong>"Tesoura"</strong>.
  </li>
</ul>

<p>
  Cada botão chama o método <code>StartGame</code>, passando um número que representa a escolha:
</p>

<ul>
  <li><code>0</code> = Pedra</li>
  <li><code>1</code> = Papel</li>
  <li><code>2</code> = Tesoura</li>
</ul>


```
private void btnPedra_Click(object sender, EventArgs e)
{
    StartGame(0);
}
private void btnPapel_Click(object sender, EventArgs e)
{
    StartGame(1);
}
private void btnTesoura_Click(object sender, EventArgs e)
{
    StartGame(2);
}
```
---

<h2>Método</h2>

O método `StartGame` inicia uma rodada de um jogo e atualiza a interface com base no resultado. Ele recebe como parâmetro um número inteiro chamado `opcao`, que representa a escolha do jogador.

Primeiramente, ele oculta o rótulo que exibe o resultado (`labelResultado`) ao definir sua visibilidade como falsa, preparando a interface para a nova rodada. Em seguida, uma nova instância da classe `Game` é criada, que provavelmente gerencia as regras do jogo.

O método `Jogar` da classe `Game` é chamado, utilizando a escolha do jogador como entrada. Esse método retorna um resultado, indicando se o jogador ganhou, perdeu ou empatou. A decisão é tratada dentro de uma estrutura `switch`.

- Se o jogador ganhou, perdeu ou empatou, o programa altera a imagem de fundo de um componente visual (`picResultado`) para indicar o respectivo resultado. As imagens são carregadas de arquivos armazenados localmente.
- Independentemente do resultado, as imagens de dois outros componentes (`pictureBox1` e `pictureBox2`) são atualizadas para mostrar as escolhas feitas pelo jogador e pelo computador.

O código também utiliza a instrução `goto` para simplificar o tratamento do fluxo no `switch`, redirecionando todos os casos ao bloco `default` para exibir as escolhas feitas.

Assim, o método gerencia tanto a lógica do jogo quanto a atualização visual da interface com base no resultado da partida.
  
```
private void StartGame(int opcao)
{
    labelResultado.Visible = false;
    Game jogo = new Game();

    switch (jogo.Jogar(opcao))
    {
        case Game.Resultado.Ganhar:
            picResultado.BackgroundImage = Image.FromFile("imagens/Ganhar.png");
            goto default;
        case Game.Resultado.Perder:
            picResultado.BackgroundImage = Image.FromFile("imagens/Perder.png");
            goto default;
        case Game.Resultado.Empatar:
            picResultado.BackgroundImage = Image.FromFile("imagens/Empatar.png");
            goto default;
        default:
            pictureBox1.Image = jogo.ImgJogador;
            pictureBox2.Image = jogo.ImgPC;
            break;
    }
}

```

---

<h2>Classe Game</h2>


***1. Enumeração*** `Resultado` 
Define três possíveis resultados para o jogo: **Ganhar**, **Perder** e **Empatar**.

***2. Imagens***
A variável estática `imagens` contém as imagens correspondentes a cada jogada (Pedra, Tesoura, Papel). As imagens são carregadas de arquivos localizados no diretório `imagens`.

***3. Propriedades*** `ImgPC` e `ImgJogador`
Elas representam as imagens da jogada do computador (`ImgPC`) e do jogador (`ImgJogador`), sendo acessíveis para visualização após o jogo.

***4. Método*** `Jogar`
Este método recebe o número correspondente à jogada do jogador (0 = Pedra, 1 = Tesoura, 2 = Papel) e calcula o resultado do jogo:
- A jogada do computador é gerada pelo método `JogadaPC()`.
- O método compara a jogada do jogador com a do computador e retorna o **Resultado** (Ganhar, Perder ou Empatar).

***5. Método*** `JogadaPC`
Gera a jogada do computador com base no milissegundo atual. O computador escolhe:
- **Pedra** (0) se o milissegundo for menor que 333.
- **Tesoura** (1) se o milissegundo estiver entre 333 e 667.
- **Papel** (2) se o milissegundo for maior que 667.

```
namespace JoKenPo
{
    internal class Game
    {
        public enum Resultado
        {
            Ganhar, Perder, Empatar
        }

        public static Image[] imagens =
        {
            Image.FromFile("imagens/Pedra.png"),
            Image.FromFile("imagens/Tesoura.png"),
            Image.FromFile("imagens/Papel.png")
        };

        public Image ImgPC { get; private set; }
        public Image ImgJogador { get; private set; }
        
        public Resultado Jogar(int Jogador)
        {
            int pc = JogadaPC();

            ImgJogador = imagens[Jogador];
            ImgPC = imagens[pc];

            if (Jogador == pc)
            {
                return Resultado.Empatar;
            }
            else if ((Jogador == 0 && pc == 1) || (Jogador == 1 && pc == 2) || (Jogador == 2 && pc == 0))
            {
                return Resultado.Ganhar;
            }
            else
            {
                return Resultado.Perder;
            }
            
        }

        private int JogadaPC()
        {
            int mil = DateTime.Now.Millisecond;

            if (mil < 333)
            {
                return 0;
            }
            else if (mil >= 33 && mil < 667)
            {
                return 1;
            }
            else
            {
                return 2;
            }
        }
    }
}
```


