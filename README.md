# Sistema de Votação em Banco de Dados

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
