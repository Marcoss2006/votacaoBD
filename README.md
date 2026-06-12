# Projeto Banco de Dados Eleição

## Sobre o projeto

<p> Projeto desenvolvido com base em exercícios acadêmicos com foco em Banco de Dados, ministradas pelo professor Gabriel M. de Carvalho. </p>

## Cenário e Negócios

- Neste modelo apresenta, eleitores, candidatos, partidos, e eleições
- Cada candidato pertence a um único partido
- Uma eleição possui vários candidatos
- O eleitor pode votar apenas uma vez por eleição

### Relacionamentos e cardinalidades
- Um partido possui vários candidatos (1:N).
- Um candidato pertence a um partido (1:1).
- Um eleitor realiza um voto (1:1).
- Uma eleição possui vários votos (1:N).
- Um candidato recebe vários votos (1:N).

Em seguida o DER modelo conceitual.

<img width="1021" height="780" alt="image" src="https://github.com/Marcoss2006/votacaoBD/blob/main/src/der.png?raw=true">

## 🛠️ Tecnologias Usadas

* Banco de Dados de Origem: MariaDB
* Banco de Dados Adaptado: Supabase (PostgreSQL)

## Manipulção do banaco de dados pelo Terminal

```sql

-- Eleições ADS    CREATE TABLE ();

Este é um exercício acadêmico baseado nas eleições para treinar e testar as
técnicas apresentadas durante a matéria de banco de dados. 

# Criando o banco de dados.

CREATE TABLE eleicoes;

# Criando a tabela partido
# obs: As chaves primárias ja tem como padrão 'not null'
# e não precisaria colocar na linha, porém por motivos
# acadêmicos decedi colocar mesmo assim.

CREATE TABLE partido (
id_partido int primary key auto_increment not null,
nome_ptd varchar(100) not null,
sigla varchar(10)
);

# Criando a tabela candidato
# Fazendo o relacionamento entre as duas tabelas
# usando a chave estrangeira

CREATE TABLE candidato (
id_candidato int primary key auto_increment not null,
nome_cdt varchar(100) not null,
numero int not null,
cargo varchar(50),
id_partido int,
foreign key (id_partido) references partido (id_partido)
);

# Criando a tabela eleiçao

CREATE TABLE eleicao (
id_eleicao int primary key auto_increment not null,
descricao varchar(100),
data_eleicao date
);

# Criando a tabela eleitor

CREATE TABLE eleitor (
id_eleitor int primary key auto_increment not null,
nome_ele varchar(100) not null,
cpf varchar(14) unique not null,
titulo_eleitor varchar(20) unique not null
);

# Criando a tabela voto, nela possui todas as
# chaves estrangeiras das outras tabelas
# então aqui também vai conter os relacionamentos
# e as chaves estrangeiras

CREATE TABLE voto (
id_voto int primary key auto_increment not null,
id_eleitor int,
id_candidato int,
id_eleicao int,
data_hora timestamp,
foreign key (id_eleitor) references eleitor (id_eleitor),
foreign key (id_candidato) references candidato (id_candidato),
foreign key (id_eleicao) references eleicao (id_eleicao)
);


- Inserindo Dados - 

# Inserindo dados reais para ficar mai parecido com um projeto grande, 
e ser um pouco mais dificultoso, já que terei que relacionar cada candidato
a seu devido partido.
#

# Inserindo dados na tabela partido.

insert into partido values (default, 'Partido dos Trabalhadores', 'PT');
insert into partido values (default, 'Partido Socialista Brasileiro', 'PSB'),
(default, 'Movimento Democrático Brasileirto', 'MDB'),
(default, 'Partido Comunista do Brasil', 'PCdoB'),
(default, 'Partido Liberal', 'PL');


# Inserindo dados na tabela candidato.

insert into candidato values (default, 'Luiz Inácio', 13, 'Presidente da República', 1);
---------------
insert into candidato values
(default, 'Flávio Bolsonaro', 22, 'Presidente da República', 5), 
(default, 'Edmilson Costa', 21, 'Presidente da República', 3),
(default, 'Hertz Dias', 16, 'Presidente da República', 4),
(default, 'Ciro Gomes', 45, 'Presidente da República', 2);


# Inserindo dados na tabela eleitor
# (obs: Aqui será com nomes e numeros de documentos por questõees de seurança) 

- Nome, cpf, Titulo

insert into eleitor values
(default, 'João Ferraz Silva', '134.189.209-54', '9374-2394-2325'),
(default, 'Joana Telles Pereira', '162.738.460-23', '9783-8934-0834'),
(default, 'Henrique Luiz Andrade', '193.495.730-42', '0837-7847-8384'),
(default, 'Sarah Diaz Albuquerq', '197.523.892-87', '8641-7402-2845'),
(default, 'Mona Lisa Martins', '139.934.822-98', '8303-2743-2831'),
(default, 'Leonardo Silva Monteiro', '123.720.029-91', '7673-6749-2376'),
(default, 'Michael dos Santos', '186.827.343-10', '9575-7821-4732'),
(default, 'Amanda da Silva Texeira', '102.465.390-09', '0274-1674-7320'),
(default, 'Felipe Anderson de Almeida', '171.323.289-73', '3830-3826-3743'),
(default, 'Rodrigo Lombard Reis', '125.348.645-95', '1903-3731-2831');


# Inserindo dados na tabela eleicao

insert into eleicao values (default, 'Eleição de 2026 para o cargo de presidente da República', '2026-05-30');

## Inserindo registros de votos.

insert into voto values
(default, 11, 1, 1, now());

insert into voto values 
(default, 12, 5, 1, now()),
(default, 13, 2, 1, now()),
(default, 14, 4, 1, now()),
(default, 15, 1, 1, now()),
(default, 16, 1, 1, now()),
(default, 17, 1, 1, now()),
(default, 18, 3, 1, now()),
(default, 19, 5, 1, now()),
(default, 20, 5, 1, now());

```

## Joins Realizados para Consultas

```sql
# Usando o inner Join para poder realizar consulta
# em duas tabelas simultaneamente.
# Mostrando os Candidatos e seus respectivos partidos,

select
	candidato.nome_cdt,
	candidato.numero,
	candidato.cargo,
	candidato.id_candidato,
	partido.id_partido,
	partido.nome_ptd,
	partido.sigla
from candidato
inner join partido on candidato.id_partido = partido.id_partido;
```
<img width="1021" height="780" alt="image" src="https://github.com/Marcoss2006/votacaoBD/blob/main/src/select.com.join.png?raw=true">

```sql
# Select com Join com três tabelas simultâneas
# Consultando como um registro de votos

select 

	v.id_voto,
	v.data_hora,
	e.nome_ele,
	e.cpf,
	c.nome_cdt,
	c.numero,
	el.descricao,
	el.data_eleicao
	
FROM voto AS v

INNER JOIN eleitor AS e
	ON v.id_eleitor = e.id_eleitor
INNER JOIN candidato AS c
	ON v.id_candidato = c.id_candidato
INNER JOIN eleicao AS el
	ON v.id_eleicao = el.id_eleicao;

```
<img width="1021" height="780" alt="image" src="https://github.com/Marcoss2006/votacaoBD/blob/main/src/select.join3tables.png?raw=true">


