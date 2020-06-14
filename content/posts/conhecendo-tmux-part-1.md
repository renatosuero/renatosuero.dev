---
draft: false
title: "🇧🇷  Conhecendo Tmux Part 1"
date: 2016-03-22T07:53:41+02:00
description: ""
tags : [tools]
categories : [tools]
---

Volta e meia converso sobre ferramentas de trabalho com as pessoas e falo do [Tmux](https://github.com/tmux/tmux/wiki), para alguns é novidade e sempre tento dar uma resumida em como usar, mas mostro muito pouco, a idéia deste texto é mostrar mais coisas legais, garanto que no final terá uma boa base para trabalhar com o tmux e não saberá como viver sem ele!
Para não ficar longo e até ter um feedback dos amigos, decidi dividir o tutorial, acredito que ele terá mais 1 ou 2 partes.

## O que esperar destes tutoriais

* Entenderá o que significa window, pane, session,etc..
* Aprenderá a usar os atalhos;
* Aprenderá a configurar os atalhos, deixando o tmux com sua cara;

### Extras

* Aprenderá a instalar e configurar uma [statusbar lindona](https://github.com/erikw/tmux-powerline);
* Aprenderá outros comandos que podem facilitar seu trabalho, mas que só vale conhecer depois de estar acostumado a usar os comandos básicos.Este te dará poderes extras!

> Antes de começar quero avisar que muita coisa inicialmente pode não fazer sentido ou parecer difícil aplicar, mas garanto que quanto mais usar o tmux mais entenderá e usará estes comandos.

Você pode instalar o tmux com brew,apt,yum,etc.., o problema é que o pacote pode não ser o mais recente. Sugiro instalar o tmux desta forma:
No momento que escrevo este tutorial a versão mais recente é a 2.1, para conferir a versão acesse o [repositório oficial](https://github.com/tmux/tmux/releases) , e mude os três primeiros comandos para versão que fez o download.

```shell
wget https://github.com/tmux/tmux/releases/download/2.1/tmux-2.1.tar.gz
tar -zxvf tmux-2.1.tar.gz
cd tmux-2.1
./configure && make
sudo make install
```

Quando abrir seu terminal e escrever **tmux** irá ver uma barra na parte inferior do seu terminal.Como na imagem Abaixo:

![Tmux com configurações default](/tmux/tmux-welcome.jpeg)

> O Atalho "base" do tmux é **ctrl b**, para evitar mostrar a tecla e facilitar, sempre que citar a tecla **leader** estarei falando sobre esta combinação.

## Sessão

Quando você executa **tmux** você cria uma sessão, isso quer dizer que se deixar algum comando executando e fechar seu terminal este comando continuará executando, claro que é possível acessar novamente a sessão criada.  
Primeiro vamos olhar as sessões criadas, execute **tmux ls** , este comando te mostrará as sessões criadas, este é um exemplo:  

> 0: 2 windows (created Sun Mar 20 13:48:19 2016) [80x23]

O **0** é o nome da sessão, **2 windows** significa que tem 2 windows abertas na sessão, o timestamp da criação e o tamanho.  
Primeiro vamos aprender a renomear a sessão, dentro do **tmux** execute **leader $**, na barra de status aparecerá um **0** que é o nome atual da sessão, apague e escreva um nome para ela, no meu caso escrevi tmux, agora com o comando **tmux ls** nos retornará:

> tmux: 1 windows (created Sun Mar 20 13:50:38 2016) [80x23]

Se digitarmos no terminal **tmux** criaremos outra sessão, mas se quisermos acessar a sessão já criada usamos o comando **tmux attach -t nome_da_sessão** onde nome_da_sessão é o nome exibido antes de : no nosso caso seria tmux.   
Se estivermos dentro de uma sessão e queremos acessar outra podemos usar o **leader s**. Este comando exibirá as sessões criadas, onde a que estiver (attached) é a sessão que está acessando. Para navegar pode usar as setas ou para os fãs de *Vim* pode usar as teclas **j** e **k** , para selecionar a sessão basta apertar a tecla enter.  

## Básico sobre window

> O tmux permite criarmos windows, ou seja seria novas janelas de terminal dentro da mesma sessão.

Para criar nova janela no tmux digitar **leader c**, com isso notará que criou uma nova window no seu terminal.  
Para navegar entre as janelas basta digitar **leader** e algum destes comandos, sendo:  
**n** => Next, ou seja para próxima janela;  
**p** => Previous, ou seja para janela anterior;  
**0..9** => Acessar uma janela específica, quando temos várias windows abertas digitar o número faz mais sentido(por exemplo temos 3 janelas abertas, estou na 0 e quero acessar a 2 basta digitar **leader 2**);  
**,** => para renomar a janela, isso é muito útil para saber o que está executando na janela;  
**&** => para matar esta janela, também pode digitar exit que funcionará da mesma forma;  

## Básico sobre pane

> O tmux permite dividir a window ou seja podemos dividir a tela na posição horizonal ou vertical.
Para criar um pane primeiro precisamos conhecer as 2 opções para isso,sendo:

**%** => Divide a janela verticalmente;  
**"** => Divide a janela horizontalmente;  

Para navegar entre os panes use **leader seta**, agora te apresento alguns comandos úteis:  
**!** => Converte um pane em window;  
**x** => Para matar o panel, também podemos usar o exit ou o x para matar uma window;  
**z** => Se quiser dar zoom em um pane, imagine o cenário onde tem 4 pane criada na tela e um deles está exibindo um log, em algum momento você precisa de mais espaço para ver esta informação, com o z você verá este pane como uma window, mas se repetir o comando voltará como estava, ou seja 4 panes criados;  

Na parte 2 vou falar sobre a configuração do arquivo *tmux.conf*, que é onde a brincadeira vira coisa séria.Se o texto não ficar muito longo(dúvido) eu mostro como configurar a statusbar e deixar como a essa,senão ficará para parte 3.

![image alt text](/tmux/tmux-example.jpeg)
