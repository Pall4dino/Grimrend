# ADR 002 – Engine e linguagem de programação

- **Status:** Aceito
- **Data:** 06/10/2026

## Contexto

O jogo será desenvolvido por uma única pessoa, com orçamento zero ou próximo disso, e com a meta de ser lançado na Steam para Windows. A experiência do desenvolvedor é maior em programação e em jogar jogos (principalmente 3D) do que em criar jogos 2D. Por isso, a engine precisa ser gratuita, ter muito material de aprendizado e permitir publicar na Steam.

## Decisão

Usar o **Unity** (versão gratuita) com a linguagem **C#**.

## Alternativas consideradas

- **Godot:** gratuita, de código aberto, leve e muito boa para 2D. Não foi escolhida porque o Unity tem mais material de estudo, tutoriais e assets disponíveis, e o C# é uma linguagem útil também fora de jogos.
- **Unreal Engine:** voltada principalmente para jogos 3D e mais pesada, com C++ como linguagem principal. É exagerada para um jogo 2D feito por uma pessoa só.
- **GameMaker:** focada em 2D, mas usa linguagem própria (GML), menos reaproveitável fora da ferramenta.
- **Engine própria:** gastaria muito tempo com infraestrutura em vez de gastar com o jogo.

## Consequências

- **Positivas:** muito material de aprendizado; ampla oferta de assets com licença livre; boa integração com a publicação na Steam; C# aplicável em outras áreas da programação.
- **Negativas:** o projeto fica dependente da engine e das condições de licença dela, que devem ser verificadas antes de qualquer lançamento comercial; a ferramenta tem curva de aprendizado própria.
