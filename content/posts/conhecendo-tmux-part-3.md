---
draft : false
date : 2016-04-09T21:51:56+02:00
title : "🇧🇷  Conhecendo Tmux Part 3"
description : ""
tags : [tools]
categories : [tools]
---

Não é que chegamos ao fim desta jornada, e aqui estou para cumprir o prometido e mostrar como deixar a status bar lindona :D.Gostaria de ter feito antes, mas semana passada foi aniversário da esposa, ai ela tem prioridade :D.  
Primeiro de tudo precisamos clonar este projeto.  
```shell
git clone https://github.com/erikw/tmux-powerline.git tre ~/.tmux-powerline
```
Agora precisamos colocar duas linhas no nosso **.tmux.conf** ou substituir se está seguindo esta sequência de tutoriais.  
```shell
set-option -g status-left "#(~/.tmux-powerline/powerline.sh left)"
set-option -g status-right "#(~/.tmux-powerline/powerline.sh right)"
##Adicionais para ajustar melhor as coisas(estas são as minhas)
set -g status-justify centre
set-option -g status-interval 2
set-option -g status-justify "centre"
set-option -g status-left-length 60
set-option -g status-right-length 140
```
Agora precisamos configurar nossa status bar com os itens que irão aparecer.   
Não removi itens deixei tudo comentado, acredito que seja fácil a configuração. Recomendo olharem o github do projeto.  
No meu caso, preciso de outro [projeto](https://github.com/thewtex/tmux-mem-cpu-load), este é responsável por mostrar cpu, memória ram e load. No projeto já explica como configurar, apesar de inglês está bem simples.   
Infelizmente o Yahoo mudou a api de previsão do tempo e não está mais funcionando esta feature(se olharem no print que fiz do meu tmux exibia) . 

---
Duas últimas coisas que faço:
Instalar as [fontes do powerline](https://github.com/powerline/fonts), com isso configuro meu terminal para usar a fonte "DejaVu Sans Mono for Powerline Oblique" com tamanho 12.
Adicionar o [base16Shell](https://github.com/chriskempson/base16-shell), precisamos apenas clonar o projeto, e adicionar o tema preferido(ocean-dark é o meu) no arquivo .bashrc
```shell
git clone https://github.com/chriskempson/base16-shell.git ~/.config/base16-shell
```
No arquivo **~/.bashrc** precisamos adicionar esta linha.
```shell
# Base16 Shell
BASE16_SHELL="$HOME/.config/base16-shell/base16-ocean.dark.sh"
[[ -s $BASE16_SHELL ]] && source $BASE16_SHELL
```
---
E chegamos ao fim desse tema, espero que tenham gostado e que principalmente o *tmux* esteja ajudando no seu trabalho.
