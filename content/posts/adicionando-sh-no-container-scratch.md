---
draft :  false
title: "🇧🇷 Adicionando sh no container scratch"
date : 2021-01-21T21:15:05+02:00
description : ""
tags : [docker,tips]
tags : [docker,tips]
---

Primeiro post do ano, espero manter alguma frequência. Também quero testar esse formato
de dicas curtas e rápidas.  

Se você chegou aqui assumo que já criou imagens docker assim:(O binário é uma api em Go
só para termos algo para testar(o código pode ser visto
[aqui](https://play.golang.org/p/4v8HQ7pAdiO)).  

```
FROM scratch
COPY ./server
ENV PORT ":9000"
ENTRYPOINT ["./server"]
```

```
#docker run --name api -d -e PORT=":9090" my-post
b55a52ec13c5d1a282b28a9f1ff6ed3773d86624232dc2151631e869fb4081ce
```

Digamos que você tem o container rodando e não está funcionando por algum motivo e você
quer olhar a variável de ambiente, se ela está definida ou algum outro comando, talvez
ls para ver que o binário **server** está lá realmente (Estou fazendo um exemplo bem
simples mas tenho certeza que você pegou a idéia).  

```
 docker-scratch-post docker exec -it api env
OCI runtime exec failed: exec failed: container_linux.go:349: starting container process caused "exec: \"env\": executable file not found in $PATH": unknown
➜  docker-scratch-post docker exec -it api ls
OCI runtime exec failed: exec failed: container_linux.go:349: starting container process caused "exec: \"ls\": executable file not found in $PATH": unknown
```

Aqui vem o problema e pode se resolvido de 2 maneiras.  
A primeira poderia ser você trocar a image base e recriar o image e assim executar um
novo container. Essa solução funciona mas precisa desse novo build e se por algum motivo
não for uma opção para ti vamos para a segunda opção.  
```
FROM alpine
````

A segunda é rodar um container com busybox e copiar o binário e mandar para o outro
container que está em execução.  

```
# docker run -d --name busybox --rm busybox:latest sleep 100   
# docker cp busybox:/bin/busybox .  
```

Aqui se você olhar no diretório atual deverá ver que há um binário chamado **busybox**. 
A próxima fase é copiar o binário no outro container que temos server rodando.

```
# docker cp ./busybox api:/busybox
```

Pronto agora precisamos rodar esse comando para adicionar o busybox no **PATH** e
instalar 

```
# docker exec -it api /busybox sh -c '
export PATH="/busybin:$PATH"
/busybox mkdir /busybin
/busybox --install /busybin
sh'
```
Executando esse comando você já vai acessar o container e poderá executar os comandos
que precisar.   

```
/ # ls
busybin  busybox  dev      etc      proc     server   sys
/ # env
HOSTNAME=b55a52ec13c5
SHLVL=2
PORT=:9090
HOME=/
TERM=xterm
PATH=/busybin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/
/ #
```

Bom era isso que tinha para fazer hoje, espero que tenham gostado e que te ajude de alguma forma =)  
