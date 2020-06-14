---
draft : false
date : 2016-03-28T21:51:56+02:00
title : "🇧🇷  Conhecendo Tmux Part 2"
description : ""
tags : [tools]
categories : [tools]
---

> Antes de começar quero dizer duas coisas:

1. Infelizmente esqueci de falar uma coisa óbvia e muito importante, acessem a documentação do tmux, com certeza encontrará coisas legais que não falei nestes tutoriais.
2. leader ? te mostrará todos comandos disponíveis no tmux, com isso muitas vezes pode resolver sua dúvida sem precisar acessar a documentação;

---

### Recados dados, vamos começar!
Primeiro de tudo vamos mudar nossa tecla **leader**, se pesquisar sobre arquivos de configuração de **tmux** acho que verá todos se não 99,99% usando **ctrl a** como default. Dentro do **tmux** execute **leader** : (este é o atalho para executar comandos no tmux) , note que na barra de status apareceu uma linha para executar comandos, digite o comando a seguir:  

```shell
set prefix C-a 
#Onde C significa C significa ctrl e a a tecla para combinação do leader.
```
Com isso já poderíamos customizar nosso tmux, mas a infelicidade é que estes comandos não persistem, ou seja, se criar novas sessões ainda funcionarão. Mas no dia seguinte quando abrir seu tmux nada funcionará como deixou!  

>Usar o comando leader : é uma boa opção para testar comandos ou novos atalhos, apesar de não persistir é um comando muito útil.  

Vamos avançar com novos comandos, mas a partir de agora iremos criar o arquivo .tmux.conf e começar nossa configuração e customização deixando o tmux mais com a nossa cara e principalmente nos deixando mais produtivos!  
Primeiro devemos criar o arquivo para configuração, ele ficará na pasta raíz do usuário. Para criar o arquivo execute seguinte comando:  

```shell
touch ~/.tmux.conf
```
Pronto, agora podemos começar nossas configurações e alguns novos comandos.Vamos começar configurando a nossa **leader**. Com seu editor preferido(o meu é o *Vim* :D ) abra o arquivo que criamos anteriormente, dentro dele cole este código:  
```shell
#Change leader key
set -g prefix C-a
unbind C-b
```

Primeiro usamos *#* para criar comentários;  
Definimos nossa tecla *leader* nova;  
Desativamos a tecla leader default, sem esta linha ainda funcionará este comando, ou seja, teremos C-a e C-b como tecla leader.  
Para configuração entrar em vigor é necessário fechar as sessões do tmux e abrir novamente, mas antes vamos adicionar uma linha no final do arquivo **.tmux.conf** para tornar este processo de customização mais prático.  
```shell
#reload tmux.conf
bind r source-file ~/.tmux.conf \; display "Reloaded!"
```
Nesta linha fizemos duas coisas, primeiro fazemos o reload do arquivo *.tmux.conf* ou seja nossas atualizações no arquivo atualizaram também para sessões ativas. depois com o comando display "Reloaded!" exibimos esta linha para nos dar um feedback visual sobre o reload(a barra ficará amarela e exibirá o texto Reloaded!). O atalho para este comando é **leader r**.  
> Agora execute o comando source ~/.tmux.conf para atualizar a sessão do seu terminal. Desta forma as configurações que fizemos acima já funcionarão.  
Vamos adicionar este bloco de configurações, logo abaixo explico cada uma:  
> Numerei as linhas com comandos para conseguir explicar lá embaixo, ignorem os números.  
```shell
#Split window
1 bind | split-window -h
2 bind - split-window -v
#Mapping movements
3 bind h select-pane -L
4 bind j select-pane -D
5 bind k select-pane -U
6 bind l select-pane -R
#Resize panes
7 bind H resize-pane -L 5
8 bind J resize-pane -D 5
9 bind K resize-pane -U 5
10 bind L resize-pane -R 5
#Enable mouse, tmux version < 2.1
set -g mode-mouse on
set -g mouse-select-pane on
set -g mouse-resize-pane on
set -g mouse-select-window on
#Enable mouse, tmux version >= 2.1
set -g mouse on
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
bind -n WheelDownPane select-pane -t= \; send-keys -M
bind -n C-WheelUpPane select-pane -t= \; copy-mode -e \; send-keys -M
bind -t vi-copy    C-WheelUpPane   halfpage-up
bind -t vi-copy    C-WheelDownPane halfpage-down
bind -t emacs-copy C-WheelUpPane   halfpage-up
bind -t emacs-copy C-WheelDownPane halfpage-down
```

1 => Adicionamos o atalho **leader |** para dividir a tela verticalmente;  
2 => Adicionamos o atalho **leader -** para dividir a tela horizontalmente;  
3,4,5,6 => Adicionamos navegação like a *Vim* para mudar o pane ativo;*por padrão o *leader l* saltaria para o último window criado(como uso *Vim*, pra mim faz sentido, se preferir ter o last funcionando não faça este config).  
7,8,9,10 => Adicionamos as teclas em **maiúsculo** para fazer o resize do pane.Exemplo: **leader H**. Ainda por ser usuário do *Vim*, gosto desta configuração,o default é usar **ctrl setas**. Exemplo **leader ctrl up**(seta pra cima).   

> Enable mouse …. Esta configuração permite usar o scroll, selecionar um pane ou window, fazer resize do pane,etc…

Sobre o mouse, o tmux mudou e pesquisando no repositório vi esta sugestão e testando aqui vi que deu certo!  
Como fiz acima, vou adicionar várias linhas e explico abaixo.  
```shell
#Mode Vim
setw -g mode-keys vi
#Set index 1
set -g base-index 1
set -g pane-base-index 1
set-option -g status on
set-option -g status-utf8 on
#Define interval
set -g status-interval 60
setw -g monitor-activity on
set -g visual-activity on
#Define history-limit
set -g history-limit 30000
# improve colors                                
set -g default-terminal "screen-256color"
```

**mode-keys** também pode ser emacs,quando aciona a **leader [** pode navegar pelo contéudo, seja para olhar um log, copiar algum texto(mostrarei na próxima parte como copiar),etc… .Todos comandos estão disponíveis na documentação, mas sou um cara legal que extraiu este trecho e deixei disponível para você neste [link](https://gist.github.com/renatosuero/89e8042114f0185ce378) :D .  
**set -g base-index** quando criamos uma window ela começa com *0(zero)*, para navegarmos nas windows pelos números tendo um zero complica para as mãos, então começar por 1 pode ser uma escolha interessante.  
**set -g pane-base-index** idem ao base-index mas para pane.  
**set-option -g status** opção que força a status bar ficar ativa.  
**set-option -g status-utf8** definimos o encoding da status bar para utf-8.  
**set -g status-interval** o tmux precisa fazer um "refresh" para atualizar as informações na status bar, por exemplo se colocar um relógio com os segundos e não definir o intervalo como 1 os segundos não mudarão.  
**setw -g monitor-activity** quando estamos com várias windows criadas se alguma ter uma atividade esta configuração colocar um # nos avisando para olhar para window(exemplo você colocar um comando para executar, quando ele termina você será avisado).  
**set -g visual-activity** quando uma houver uma atividade ela usa a statusbar para notificar(a linha acima precisa estar ativa para esta funcionar) . 
**set -g history-limit** já vi pessoas reclamando por lentidão e recomendavam usar um limit no histórico, coloco este valor pq mesmo para ler um log acho que tem um tamanho razoável.  
**set -g default-terminal** esta opção serve para melhorar as cores do tmux;  

---

Copiar um texto no *tmux* pode ser um pouco estranho no começo, mas vamos lá :D .  
Primeiro vamos entrar no modo de navegação executando **leader [**, aqui que entra a configuração *mode-keys vi* (desculpe aos amigos de emacs,mas vou falar como fazer no *Vim*).Usamos as mesmas teclas do vim para navegar($ fim da linha, 0 ínicio da linha,etc…).    
Para copiar o texto fazemos o seguinte:    
**leader [**, pare no primeiro carácter que quer copiar e aperte a tecla space, com isso quando andar pelo texto verá que ele tem um background diferente. Quando terminar de selecionar o texto tecle **enter**. Pronto você tem o texto copiado, agora para colar você faz **leader ]**.  
Aqui termino minhas configurações, agora já temos ele configurado com alguns atalhos e configurações úteis. Vamos começar a diversão onde vamos deixar ele com a nossa cara, abaixo mostrarei como deixar sua status bar com seu estilo.  

---
```shell
#Info status bar
set -g status-left " #S "
#set -g status-left " #S W:#{session_windows} P:#{windows_pane}"
set -g status-right "%d %b %R "
set -g status-justify left
#Status bar Colors
set -g status-fg black
set -g status-bg green
#Window Status bar Colors
setw -g window-status-fg white
setw -g window-status-bg black
setw -g window-status-attr dim
#Current Window Status bar Colors
set -g window-status-current-fg red
setw -g window-status-current-format "[#I:#W]"
#set -g window-status-current-bg red
set -g window-status-current-attr bold
#Example formating like a boss
#setw -g window-status-current-format " #[fg=green,bg=white] #I #[bg=cyan]#[fg=red] #W "
```
**set -g status-left**, **set -g status-right** definimos as informações que queremos ter na nossa status bar, por exemplo no lado esquerdo mostro o nome da sessão e no lado direito mostro a data e hora. Podemos colocar variáveis e ter outras informações como o exemplo na linha comentada. Novamente extrai este trecho com as variáveis e deixei disponível neste [link](https://gist.github.com/renatosuero/82b5b57092b80217677c).  
**set -g status-justify** definimos onde queremos nossas windows, temos 3 opções sendo elas: [left | centre | right].  

Pra tentar deixar mais dinâmico e menos repetitivo fiz o seguitne, coloquei toda configuração da barra, notem que tem vários lugares com -fg,-bg,-attr.   
Primeiro vou explicar os 3 últimos blocos que adicionei, ficará mais fácil falar sobre as propriedades.  
1. #Status bar Colors, configuração default para toda barra . 
2. #Window Status bar Colors, configuração para windows criada . 
3. #Current Window Status bar Colors, configuração para windows ativa, ou seja a window que está vendo.  

Pronto agora já sabe o que cada bloco faz, vamos falar sobre as configurações para customizá-los.  

**Opções com -fg** definimos a *cor do texto.  
**Opções com -bg** definimos a *cor do fundo.  
**Opções com -format** podemos usar aquelas variáveis da status bar(fiz um exemplo mostrando o índice e o nome da window). Também pode formatar o conteúdo de uma forma mais divertida (acho que isto é útil se quiser ter mais de uma cor no item, a última linha é um exemplo de como poderia fazer).  
**Opções com -attr** podemos adicionar algum atributo como: bright (or bold), dim, underscore, blink, reverse, hidden, or italics(Note que na janela ativa eu deixo o texto em negrito).  
>Podemos passar valores Hexadecimais,RGB, cores por nome(em inglês claro!).

---
Poxa esse texto ficou mais longo do que imaginava. Espero que te ajude nesta introdução sobre esta maravilhosa ferramenta que é o *tmux*. Uso todos os dias e por acreditar no ganho que tive que decidi escrever estes textos. Torço para que tenha a mesma experiência e se descobrir algo legal deixe um comentário, me ajude a tornar este material mais completo.  

#### Antes de finalizar gostaria de falar sobre algumas coisas:
1. Como sugerido pelo meu amigo Anibal Fernandes ,criei um CheatSheet com os comandos básicos e uns avançados bem legais, assim pode imprimir ou mesmo ter uma forma diferente do **leader ?** de ver os comandos,Também adicionei algumas informações interessantes.Ele está disponível neste [link](https://cheatography.com/renatosuero/cheat-sheets/my-tmux-conf/).  
2. Recomendo olhar o [tmuxinator](https://github.com/tmuxinator/tmuxinator), pretendia falar sobre ele, mas o [André Luís](https://medium.com/@ndrluis) já fez um excelente trabalho que poderá ler neste [link](https://medium.com/@ndrluis/agilizando-o-desenvolvimento-com-tmuxinator-6a49bf80f296).  
3. Não esqueci da status bar lindona, decidi escrever um texto separado(este está muito longo).  
