# PsychoDiggers
Projeto para a aula de Técnica de programação para games

Changelog:

Versão 6.1.0

changes:
- novos prefabs de plataformas
- organização dos objetos na scene hierarchy
- alteração no tamanho da altura da tela
- pequena mudança de posicionamento da camera
- aumento de velocidade do prefab de tiro "Pá"
- alterado o script do "Trigger Enemy" - IMPORTANTE - Os inimigos foram separados por grupos. Cada grupo de inimigo deve ser adicionado ao "filho" do objeto Trigger Enemy. Isso é necessário pois agora o script procura apenas os inimigos que são filhos desse objeto. Ao contrário do modelo anterior, onde o script buscava todos os inimigos da tela.
- Reposicionado vários objetos na tela e adicionado algumas plataformas móveis na tela

____________________________________________________________________________________________________

Versão 6.08 

Inclusão de Tiro do Inimigo:
Um inimigo atirando e andando e outro atirando parado;
Criação de um PreFab SkullShot;
A Tag de SkullShot foi marcada como Hazard;

OBS:Foi alterado o nome do antigo Script ControllerNecro para EnemyController para ficar mais genérico;

____________________________________________________________________________________________________
Versão 6.07 - Prefabs

- Criação de vários prefabs de Plataformas fixas, com vários tamanhos

- Criação de uma plataforma que gira. Velocidade, Distancia do raio e sentido podem ser alterados em variáveis públicas no objeto

- Criação de uma plataforma que se movimenta para cima e para baixo. Velocidade e distancia podem ser alterados em variáveis públicas no objeto

- Criação de uma plataforma que cai no momento que o player pula em cima dela.

- Criação de um objeto "Spike" que se comporta como a plataforma, mas pode ferir o player.

- Criação deprefabs de itens obrigatórios em todas as cenas:
	- Camera + backuground
	- Player Shadow (Obrigatório para a troca de personagens ocorrer sem "quebrar" o jogo)
	- Trigger Enemy (Obrigatório para os inimigos seguirem o player)

- Bug/Melhoria identificado. O trigger enemy talvez tenha que ser alterado em uma futura versão. Da forma que está ele ativa todos os inimigos na tela. Talvez o correto fosse que ele ativasse apenas um pequeno grupo de inimigos.

____________________________________________________________________________________________________

Alterado - Versão 6.05

- Adicionado um efeito de explosão na troca dos personagens

- Alterado a forma que o pulo do personagem 2 afeta os inimigos. Agora o personagem 2 só mata os inimigos quando estiver caindo

- Inclusão de danos nos personagens 1 e 2:
	-Quando eles tocam um inimigo, uma animação de dano é iniciada, 
	-O personagem é "empurrado" para trás por um pouco menos de 1 segundo e o jogador não consegue movimentar o personagem durante esse período.
	-Após receber o dano, o player fica invencível por 2 segundos. Visualmente o player fica "piscando" na tela para representar a invencibilidade
	-Durante a invencibilidade, o player não colide com os inimigos, justamente para permitir que o jogador fuja do local.
	-Ao receber dano, uma caveira é reduzida do contador.

- Adicionado animações de tiro para o player 1 e player 2

- Ao cair fora da tela, o jogador perde as 3 caveiras de uma vez e morre. Um box colider "Destruidor" foi adicionado embaixo das plataformas que inicia essa ação ao tocar com  algum dos players.

- Após matar um inimigo, o box colider dele é desativado, para que ele não colida mais com o player ou com os tiros durante a animação da morte do inimigo.
	- Essa ação causou uma animação estranha durante a morte dos inimigos, podemos reve-la.


______________________________________________________________________________________________________

Versão 6.03 Barra de Energia

Implementações:

1) Pontos do Player:

Um HUD para o Player com Script ScoreManager.
Cada inimigo tem um número de pontuação, ao matá-lo o player recebe os pontos do inimigo.

2) Vidas

Implementação de um GameObject Image com as vidas no HUD do Player;
OBS: Somente para efeito de teste, ao trocar de player perde-se uma vida. Quando as vidas acabam, volta-se ao ínicio da fase.

3) Troca da imagem do player ao lado dos pontos a cada troca de player.

Foi colocada a seguinte linha de código no script controller de cada player na função de troca:

PlayerFaceManager.iFace = 1;

______________________________________________________________________________________________________

versão 6.03

1) Corrigido o bug do inimigo necro que travava o jogo ao trocar de personagens.
No script do inimigo Necro, o código que buscava o player estava fixa no Digger1. 
Além disso ele buscava uma variável "direction" dentro do script do Digger1, que varia de -1 até 1 de acordo com a movimentação do jogador. 
Como o Digger 1 era destruído ao mudar de personagem, criei um gameObject vazio na tela que sempre recebe dos Digger1 ou 2 qual deles está ativo na tela no momento e então assume a posição do eixo X e também o LocalScale do Digger Ativo. 
Dessa forma O script do Necro, ou qualquer outro inimigo na tela deve buscar esse GameObject chamado PlayerSombra no lugar de especificar o Digger1. 
Outra alteração, no lugar de buscar a variável "direction" o necro agora busca o localScale do novo GameObject, que também varia de -1 a 1.

2) Alteração na forma que o tiro da pá faz danos aos inimigos.
Ao invez de fazer o tiro buscar os componentes dos inimigos para causar hit, o tiro agora apenas manda a mensagem de que colidiu. O proprio objeto que recebeu o hit vai iniciar os proprios componentes como por exemplo receber o dano do tiro, iniciar a animação de dano, barra de energia, etc. 
Isso facilitará no script do tiro que não precisará de nenuma alteração caso novos inimigos sejam adicionados com componentes diferentes.

3) Limitação no número máximo de tiros (pás) visiveis na tela para apenas 3
Ao atirar uma pá, uma variável de contador é somada. Ao passar de 3 um novo tiro não é mais permitido.
A variável de contador é diminuida quando uma pá é destruida.

4) Destruir o tiro da pá quando ela sair da tela.
Ao sumir da tela, a pá é destruida

5) Digger 2 agora pode pular em cima dos inimigos para mata-los
Adicionado um objeto vazio com um trigger collider nos pés do Digger2. Esse collider tem um script semelhante ao tiro da pá. Ou seja, ao colidir com um inimigo, ele recebe o dano do objeto Stomp. Além disso o collider aciona um método do script do Digger2, fazendo com que se o botão de pulo estiver sendo pressionado, o Digger 2 é impulsionado ao pular sobre o inimigo.
