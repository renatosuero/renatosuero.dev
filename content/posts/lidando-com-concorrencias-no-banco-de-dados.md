---
draft :  false
title : "🇧🇷 Lidando com concorrências no banco de dados"
date : 2020-10-24T13:34:05+02:00
description : ""
tags : [go,postgres]
---

Você já ouviu essa pergunta ? "O que acontece se 2 pessoas executarem a ação ao mesmo tempo?"(no contexto de um serviço web com o banco de dados). Algumas respostas possíveis(se conhece outras me diz ai).

1. Transações estão ai para resolver isso.
2. Não sei (simples,direto e principalmente honesto).

Beleza, vamos criar um cenário para que possamos ter o contexto em comum. Podemos olhar para esse problema pensando em reserva de assentos/quartos, estoque de produtos,qualquer problema onde 2 pessoas podem alterar o mesmo registro, etc...
Vamos trabalhar com um cinema. Temos uma tabela com os assentos 1-5(sim é um cinema muito pequeno, mas garanto que a qualidade é muito boa).

Vamos ter uma tabela assentos com os registros para cada assento e o nome de quem reservou. A tabela será simplesmente isso e eu farei as inserções dos assentos com cliente vazio oq significa disponível.

```sql
create database cinema;
create table assentos ( 
   id serial PRIMARY KEY, 
   cliente varchar NOT NULL DEFAULT '' 
);
insert into assentos (cliente) values(''),(''),(''),(''),('');
```

```bash
postgres@localhost:cinema> select * from assentos;                                              
+------+-----------+
| id   | cliente   |
|------+-----------|
| 1    |           |
| 2    |           |
| 3    |           |
| 4    |           |
| 5    |           |
+------+-----------+
SELECT 5
Time: 0.018s
```

---

o fluxo da reserva vai ser:

Checar se o assento está disponível

1. Caso negativo: "Retornar um erro avisando que o assento não está disponível";
2. Caso positivo: Reservar o assento e retornar uma mensagem avisando que foi reservado;

No sql seria algo assim

```sql
begin;
select id from assentos where id = 5 and cliente='';
/// caso retorne 0
update assentos set cliente = 'Renato' where id = 5;
Commit;
```

Criei um código para mostrar isso, não vou mostrar aqui porque o objetivo é executar as instruções acima, mas você pode ver o código nesse [link](https://gist.github.com/renatosuero/1cc0a031c485542d41276cd718382eaa).(você pode ver isso funcionando, mas não recomendo esse código, eu fiz ele rápido só para mostrar meu ponto porque só com o sql não seria tão divertido).

---

Legal temos nossa API para reservar assentos no cinema :), massss o que acontece se durante o processo de reserva tivermos alguma lentidão no servidor ou mesmo se tivermos algum processo ali no meio entre a consulta e reserva(por ex. enviar a informação sobre a reserva para outro serviço)?

Vou Adicionar um sleep antes do update para simular o cenário, caso o cliente seja Henrique(por que Henrique?, porque ele é um vacilão 🙂) assim podemos responder à pergunta anterior. 

Vou mandar duas requisições usando o curl para que fique tudo na mesma imagem. E ai quer dar um palpite? quem leva o quarto Henrique ou Renato ?

[![asciicast](https://asciinema.org/a/367353.svg)](https://asciinema.org/a/367353)

Brincadeiras de lado, o que rolou foi que quando o Henrique enviou o pedido a API fez o fluxo que combinamos, porém ela deu uma pausa para que desse tempo de fazermos outro pedido.
O outro pedido fez o mesmo processo e na primeira parte confirmou que o assento estava livre então poderia avançar e assim fez executando o update para o Renato, quando o sleep terminou o update do Henrique foi feito, nesse caso como foi o último ele levou o assento(apesar de ambos terem o retorno de sucesso). Agora se Renato também tivesse uma lentidão e fosse maior que Henrique(executasse o update por último) ele/eu levaria o assento.  

Legal agora podemos começar o post sobre esse assunto. Existe algumas opções de lock que podemos bloquear o dados seja uma tabela ou registro(nosso caso).

A solução é bem simples, mas infelizmente não muito conhecida (motivo pelo qual eu escrevo este post).

Na nossa query , vamos adicionar mais 2 palavras, **FOR UPDATE** Ficando assim:

```sql
SELECT id FROM assentos WHERE id = 5 AND cliente='' FOR UPDATE;
```

 Essa instrução nova vai dar um lock nesse registro não permitindo outras consultas no registro. Assim quando a próxima query tentar consultar o assento 5 nesse caso, vai ficar esperando até que quem deu o bloqueio(a transação que fez o pedido de reserva) execute o Commit ou Rollback.
 Vamos ver novamente o resutlado agora com o essa alteração.

[![asciicast](https://asciinema.org/a/367354.svg)](https://asciinema.org/a/367354)

Legal né? com duas palavras resolvemos um problema de reservar o mesmo assento para para mais pessoas.

Uma boa notícia para quem usa o GORM é que ele tem suporte para essa [feature](https://gorm.io/docs/advanced_query.html#Locking-FOR-UPDATE)
Beleza se vc não quis clicar tá aqui o exemplo :) 

```sql
DB.Clauses(clause.Locking{Strength: "UPDATE"}).Find(&assentos,"id = $1 and cliente =''",id)
```

Uma forma que você poderia resolver sem precisar do "lock" seria fazer algo assim(obrigado Henrique)

```sql
select id from assentos where id = 5 and cliente='';
update assentos set cliente ='Henrique' where id=5 and cliente='';
```

Para o cenário simples que criei sim é uma solução que funcionaria bem, a única coisa diferente é que a API precisaria ver o retorno **RowsAffected** para saber se houve a reserva(update aconteceu e retornaria 1 RowsAffected,caso 0 significa que não encontrou). Para esse cenário de um campo fica fácil validar se o campo ainda está no estado anterior(cliente is null) mas para a vida real ou algo menos simples que um campo para atualizar isso te obrigaria a checar todos campos que vai fazer o update. Acho que usando o **FOR UPDATE** você resolve isso de uma maneira mais "limpa/simples".

Bom era isso que tinha para fazer hoje, espero que tenham gostado e que te ajude de alguma forma =)
