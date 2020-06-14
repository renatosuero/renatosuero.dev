---
draft: false
title: "🇧🇷 Vimux - integrando o tmux no vim"
date: 2017-02-20T07:53:41+02:00
description: ""
tags : [tools,vim]
categories : [tools, vim]
---

Hoje estou sem internet(obrigado MEO) então decidi falar sobre um plugin do vim que integra com minha tool preferida, o Tmux. O plugin se chama [Vimux](https://github.com/benmills/vimux).  

> Adicione o plugin no seu arquivo .vimrc e usando seu gerenciador de pacote execute o comando para fazer a instalação.  
> O vimux recomenda que use o tmux na versão superior a 1.4(por favor atualiza isso, no momento que escrevo esse post a versão atual é a 2.3).  

Chega de enrolação e vamos ao assunto!  

Este plugin envia comandos ao tmux, por exemplo, posso abrir um pane ou window direto usando um comando no vim. É exatamente o que vou mostrar. Para esquentar e começarmos amizade com o plugin vamos abrir o vim(isso deve ser feito em uma sessão do tmux), em qualquer diretório e executar o seguinte comando:  

Para esquentar e começarmos amizade com o plugin vamos abrir o vim, em qualquer diretório e executar o seguinte comando:  

```shell
VimuxRunCommand "ls"
```
![image Resultado do comando acima](/vimux/example.png)

Ele deverá abrir uma pane na parte inferior do seu terminal e executar o comando ls, assim mostrando os arquivos do diretório. Bacana não é?  
Separei o post em partes, abaixo falarei sobre os comandos, na sequência mostro algumas configurações e por último mostro alguns exemplos para mapear os comandos no vim.  

## Comandos
**VimuxRunCommand**
Já somos amigos deste comando,quando chamado ele executa o comando definido entre os parenteses, abrindo uma pane/window com o resultado do comando.  

**VimuxCloseRunner**
Este comando irá fechar a pane/window criada/aberta pelo Vimux.  

**VimuxLastCommand**
Este irá executar o último comando executado pelo Vimux.  

**VimuxPromptCommand**
Acho este bem legal =), quando executado ele espera que digitemos o comando desejado para correr no terminal. É possível deixar um valor default passando como parâmetro na chamada da função. Por exemplo VimuxPromptCommand(“git status”).  

**VimuxInspectRunner**
Este irá para o pane/window aberto e estará com o modo copy do tmux ativado.  

**VimuxInterruptRunner**
Este cancela o comando enviado. Imagina que mandou correr o servidor, mas desistiu e quer cancelar, use este comando.  

**VimuxZoomRunner**
Este dará zoom no pane criado pelo Vimux, para desfazer é só executar o comando do tmux <bind-key>z.  

### Configurações
Se você é como eu já deve estar querendo saber onde mudar algumas coisas, vou te mostrar algumas coisas bacana.
1. Porcentagem que o pane ocupará. O default é 20.  
> let g:VimuxHeight = "20"  
2. Orientação que o pane abrirá, as opções são h ou v. O default é v (vertical).  
> let g:VimuxOrientation = "v"  
3. Tipo de view Object que o Vimux usará para executar a ação, as opções são pane ou window . O default é pane.  
> let g:VimuxRunnerType = "window"  

### Mapeando teclas no Vim
Esta parte vai de gosto, vou dar uns exemplos de como faço meus mapeamentos. Primeiro passo o <Leader>(no meu caso a tecla space), depois v (apenas pelo nome do plugin, assim não corro risco de conflitos com outros comandos) e uma letra para o comando. No meu caso uso r para run, b para build, t para test, c para close,etc…  

```shell
map <Leader>vr :call VimuxRunCommand("go run main.go")<CR>  
map <Leader>vt :call VimuxRunCommand("go test ./… -v — v")<CR>  
map <Leader>vc :call VimuxCloseRunner()<CR>  
map <Leader>vp :call VimuxPromptCommand("go run")<CR>  
```

Bom chegamos ao fim, espero que tenham gostado e que isso seja útil te ajudando na tua rotina de trabalho.  
Gosto muito de ferramentas que rodam ou consigo integrar no meu terminal, se conhece/usa alguma me dá um fala, terei o prazer de dar uma olhada, quem sabe não vira um post.  
