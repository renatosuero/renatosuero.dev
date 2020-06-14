---
draft: false
title: "🇧🇷  Sendo produtivo com vim-go"
date: 2016-11-15T07:53:41+02:00
description: ""
tags : [tools,vim]
categories : [tools, vim]
---

![image alt text](/vim-go/vim-go.png)

Minha proposta para este post é mostrar como e o que uso para ter produtividade escrevendo códigos GO no vim. Presumo que você tenha GO instalado e configurado na sua máquina e conhecimentos no vim, não é necessário um conhecimento avançado, mas pelo menos precisa conhecer os modos,navegação e instalação de plugins.  
Espero que isso te ajude e estimule sua curiosidade para aprender mais (No fim do post deixarei algumas opções para se aprofundar no assunto). Também não pretendo mostrar todos plugins que uso,pois o texto ficaria extenso e perderia o foco. Vou focar no plugin vim-go e mostrar outros 2 (Tagbar e splitjoin.vim).  
Chega de papo furado e vamos botar a mão na massa.  

---
Para começar você precisa instalar o plugin:  
```shell
Pathogen
> git clone https://github.com/fatih/vim-go.git ~/.vim/bundle/vim-go
vim-plug
> Plug 'fatih/vim-go'
NeoBundle
> NeoBundle 'fatih/vim-go'
Vundle
> Plugin 'fatih/vim-go'
Vim packages (since Vim 7.4.1528)
> git clone https://github.com/fatih/vim-go.git ~/.vim/pack/plugins/start/vim-go
```
Também é necessário instalar os binários(gocode,godef,goimports,etc…).  
Dentro do vim execute **:GoInstallBinaries**.  
Para começarmos e confirmar que tudo está bem, abra o vim e execute o comando **:GoPath**, na barra de status do vim você deverá ver o diretório que definiu como GOPATH.  

Criei um projeto para me ajudar a mostrar as features do plugin. Se você quiser usar ele.  
```shell
git clone git@github.com:renatosuero/tutorial_vim-go.git  
```

![Este é o projeto no github](/vim-go/show-example.png)

Se digitar **:Go** e apertar o **<tab>** verá muitas opções, vou falar sobre algumas,executando desta forma. Você pode configurar teclas de atalho como neste exemplo retirado do repositório.  

```shell
au FileType go nmap <leader>r <Plug>(go-run)
au FileType go nmap <leader>b <Plug>(go-build)
au FileType go nmap <leader>t <Plug>(go-test)
au FileType go nmap <leader>c <Plug>(go-coverage)
```

**GoRun** => Executará seu código;  
**GoBuild** => Compila o pacote, mas não gera o binário (retorno na barra de status);  
**GoAlternate** => Abrirá o arquivo teste (procura pelo arquivoAberto_test.go), se usar o sinal **!** ele criará o arquivo se não existir;  
**GoTest** => Executa sua suíte de teste (retorno na barra de status);  
**GoTestFunc**=> Executa apenas o teste onde o cursor está (retorno na barra de status);  
**GoTestCompile** => Testa se não há erros para compilar o projeto (retorno na barra de status);  
**GoCoverage** => Este acho bem legal ele bota um highlight no teu código mostrando sua cobertura (Se está usando meu projeto, note a linha 28);  
**GoCoverageClear**=> Remove o highlight criado pelo comando acima;  
**GoCoverageToogle** => Alterna entre exibir ou remover o highlight do código;  
**GoCoverageBrowser** => Como o GoCoverage, mas abre no seu navegador;  
**GoImport** => Sabe quando está lá no fim do arquivo e quer usar o Print.., ai tu vai rodar o código e o compilador diz que não importou o pacote fmt, com este comando você faz isso, execute por ex. **:GoImport fmt**;  
**GoDrop** => Este faz o inverso do anterior, sabe quando tu remove o print e esquece de remover o pacote e ele reclama, execute por ex. **:GoDrop fmt**;  
**GoImportAs** => Este serve quando tu precisa importar o pacote com um álias, execute por ex. **:GoImportAs f fmt**;  
**GoImports** => Este "auto importa" os pacotes necessários.Se tu usar o Print por ex. e rodar este comando ele adicionará o **fmt** (Confesso que acho útil, mas meu TOC não me deixa usar ele =) );  
**GoMetaLinter** => Executa as ferramentas **go vet, golint e errcheck** (se está com meu projeto note que ele vai falar sobre o comentário das funções, isso me ajuda a manter um código padronizado);  
**GoDef** => Vai para definição onde o cursor está;  
**GoDefPop** => Volta para onde estava antes de executar o GoDef;  
**GoDecls** => * Mostrará todas func e types criadas no arquivo;  
**GoDeclsDir** => * Mostrará todas funcs e types criadas no diretório;  
**GoDoc** => Faz um split no topo da tela e mostra a documentação da função onde o cursor está(vale dizer que funciona tanto para o teu código como a standard library);  
**GoInfo** => Mostrará na barra de status a assinatura da função;  
**GoSameIds** => Em uma variável se usar este comando ele irá colocar um highlight em todos lugares onde a chamou (apenas na variável e não na linha);  
**GoSameIdsClear** => Remove o highlight criado pelo comando acima;  
**GoSameIdsToggle** => Alterna entre exibir ou remover o highlight do código;  
**GoReferrers** => Em uma declaração execute este comando para procurar referências em todos pacotes do seu workspace;  
**GoDescribe** => Parecido com o GoInfo, um pouco mais avançado (te dará mais informações,onde está definida e onde está sendo usado);  
**GoRename** => Este é bem legal, ele vai pegar a palavra onde o cursor está e na barra de status permitirá que a mude. Ele vai procurar em todos pacotes do teu workspace (ajuda no refactoring, mas deve tomar cuidado);  
**GoPlay** => Confesso que este não usei ainda, mas achei legal mostrar. Ele irá compartilhar o código na internet(play.golang.org).Exibirá o link na barra de status e abrirá a url no seu navegador;  


Digamos que ao executar o GoPlay você não deseja que o browser abra com o link criado, para desativá-lo insira o seguinte código no seu .vimrc.
```shell
let g:go_play_open_browser = 0
```

O plugin vem com tudo que tem disponível habilitado por padrão. O post vai ficar gigante então vou deixar esta parte para sua curiosidade.  
Alguns comandos citados são do [guru](https://godoc.org/golang.org/x/tools/cmd/guru), para mais informações recomendo olharem este [link](https://docs.google.com/document/d/1_Y9xCEMj5S-7rv2ooHpZNH15JgRT5iM742gJkw5LtmQ/edit#heading=h.m302pxb7w2w7).  

* É necessário ter o plugin [ctrlp](https://github.com/kien/ctrlp.vim). Caso não saiba ele faz um split no rodapé da tela e mostra as funções.   
Você pode digitar alguns caracteres para diminuir as opções. Para navegar entre as opções use **ctrl j** (descer) e **ctrl k** (subir).    

---

Antes de Tagbar esse era um dos métodos que julgava ser produtivo,ainda acho fantástico,mas para arquivo com muitas funcs o Tagbar dá mais jeito.  
```shell
]] Apontará o cursor na próxima func.
[[ Apontará o cursor na func anterior.
//Aproveitando o assunto sobre func
if seleciona todo conteúdo/instrução da função.
af seleciona toda função(declaração e instruções).
```
Tá mas qual a vantagem nisso, você que usa vim já deve ter pensando nas combinações =). Por ex. se quer remover uma função poderá fazer digitando **daf**, para selecionar **vaf** ou se quiser copiar **yaf**, e por ai vai. O legal neles é que você pode estar em qualquer ponto da função que deseja fazer uma ação.  

---
Já vi alguns programadores falando sobre o autocomplete em seus editores, no vim também é possível usando as * teclas <C-x><C-o>.   
Para snippets, ele suporta 2 plugins populares [ultisnips](https://github.com/SirVer/ultisnips) e [neosnippet](https://github.com/Shougo/neosnippet.vim).  
[Aqui](https://github.com/fatih/vim-go/tree/master/gosnippets) encontrará os snippets disponíveis. Vamos testar se tudo está OK.  
Em um arquivo .go digite **ff** e aperte a tecla **tab**, ele deverá completar com o código abaixo e o cursor estará antes do sinal de **=**.  
```go
fmt.Printf(" = %+v\n", )
```
> Só para reforçar caso não esteja confortável com o Vim, você vai digitar um trecho da palavra e depois digitar **ctrl x ctrl o** (este último é a vogal o e não o número zero). Se houver apenas uma opção ele completará o texto, se não abrirá uma janela com opções. (para navegar entre elas use **ctrl n** para o próximo e **ctrl p** para o anterior).

---

O plugin [Tagbar](https://github.com/majutsushi/tagbar) era um cara que não usava e quando comecei a escrever este post resolvi testar,gostei e não poderia deixar de falar dele.  
Se quiser ver como funciona execute **:TagbarToggle**. E para mapear uma tecla, neste caso copiei do README do projeto e ele será executado quando apertar F8.  
```shell
nmap <F8> :TagbarToggle<CR
```

Quando abrir o Tagbar verá suas variáveis,métodos,funcs,etc.., basicamente você para em cima do que quer ver e digita **p** ** ele irá colocar a declaração selecionada no meio da tela, se digitar **P** ele abrirá um preview(um split no topo da tua janela).  
* O [CTags](http://ctags.sourceforge.net/) é uma dependência para usar o TagBar.     
** Se você usa o mouse (deve aprender a navegar sem ele =) ) quando clicar na tela do código ele vai parar onde o mouse clicou. Para navegar like a boss você faz o seguinte **ctrl w** e a direção que desejar (**hjkl** ou setas,também recomendo abandonar as setas se ainda não o fez =) ).  

Outro plugin que é interessante é o [splitjoin](https://github.com/AndrewRadev/splitjoin.vim), resumindo você para o cursor na linha onde quer fazer uma das ações e digitar **gS** para split e **gJ** para join.    
Em GO ele fará nas structs, para outras linguagens você deve olhar o projeto.    
```go
//gJ para botar tudo na mesma linha
foo := Foo{Name: "gopher", Ports: []int{80, 443}, Enabled: true}
//gS para quebrar a struct em linhas
foo := Foo{                                                      
       Name: "gopher",
       Ports: []int{80, 443},
       Enabled: true,                                                                  }
```
---

Enfim acabamos, espero que este post seja útil. Pra mim escrever é sempre uma atividade prazerosa, sempre aprendo algo novo enquanto estou escrevendo(neste caso usar o TagBar e alguns comandos que não estava acostumado).  
Esta semana houve uma conversa bem legal sobre editores no canal Brasil do Slack. Pelo que vi dos colegas que usam outros editores acredito que eles tenham os mesmos recursos,claro uns (vim/emacs) podem ser necessário fazer alguma configuração. Acho que não existe o melhor, existe o que se sente confortável e feliz usando, aquele que se sente em casa.  

![image Mas com o Vim posso fazer isso, e o seu ?:P](/vim-go/vi-gang.jpeg)

Você leitor que usa outro editor, te convido a falar sobre o qual usa, não para uma comparação, mas para ajudar os novos programadores na escolha do editor. Se fizer por favor me avise para que eu possa colocar o seu link aqui no fim do post.  
Estas são as opções que disse no começo do post.  
[Apresentação do próprio criador do plugin](https://www.youtube.com/watch?v=7BqJ8dzygtU).  
[Tutorial também criado pelo próprio criador do plugin](https://github.com/fatih/vim-go-tutorial) (Foi minha bíblia para escrever este post =) ).  
**:help vim-go** ou apenas usando **:h vim-go**, a documentação do plugin disponível no editor.    
