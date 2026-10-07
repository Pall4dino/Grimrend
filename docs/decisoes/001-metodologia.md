# ADR 001 – Metodologia de desenvolvimento

- **Status:** Aceito
- **Data:** 06/10/2026

## Contexto

Projeto individual de um jogo digital 2D, desenvolvido por uma única pessoa e com a ideia ainda em fase inicial. O que é divertido em um jogo só se descobre jogando, então os requisitos e o design vão mudar ao longo do desenvolvimento. É preciso uma metodologia que aceite mudanças e que funcione sem uma equipe.

## Decisão

A metodologia do projeto será **ágil, em formato Kanban**, com ciclos curtos inspirados no Scrum e adaptados para uma pessoa só. O trabalho será organizado em um quadro de tarefas (GitHub Projects) com as colunas *Backlog → Em andamento → Em teste → Concluído*, e o roadmap será dividido em versões pequenas (v0.1, v0.2, v0.3...).

## Alternativas consideradas

- **Cascata:** exige requisitos fechados antes de começar e dificulta voltar atrás. Não combina com um jogo cujo design ainda está sendo descoberto.
- **Espiral:** é focada em gestão de riscos em projetos grandes e caros. É pesada demais para um projeto individual.
- **Scrum completo:** é pensado para equipes, com papéis (Product Owner, Scrum Master) e reuniões que não fazem sentido para uma pessoa só. Apenas algumas ideias foram aproveitadas, como ciclos curtos e backlog.

## Consequências

- **Positivas:** flexibilidade para ajustar o jogo conforme o protótipo evolui; entregas pequenas e frequentes; fácil de aplicar sozinho.
- **Negativas:** prazos menos previsíveis; exige disciplina própria para manter o quadro e a documentação atualizados.
