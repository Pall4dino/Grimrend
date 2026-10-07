# ADR 004 – Fases, checkpoint e progressão do jogador

- **Status:** Aceito
- **Data:** 06/10/2026

## Contexto

O jogo busca um ciclo viciante em que o jogador começa fraco, evolui, se sente forte e encontra um novo obstáculo. A morte não deve ser punitiva, e o MVP precisa ser simples de implementar. Era preciso decidir se a progressão vale só dentro de cada partida, se é persistente ou se é uma mistura, e o que acontece quando o jogador morre.

## Decisão

- O jogo é dividido em **fases**, com **checkpoint no início de cada uma**. Ao morrer, o jogador volta ao início da fase atual.
- Ao entrar em uma nova fase, o **equipamento volta ao padrão daquela fase**, e **nível e XP resetam**.
- **Habilidades temporárias** são obtidas ao subir de nível e valem apenas na fase atual.
- **Habilidades permanentes** são compradas com pontos permanentes, concedidos uma única vez por objetivo concluído (derrotar um boss, concluir uma fase). Os pontos podem ser gastos a qualquer momento no menu de habilidades e são mantidos ao morrer.
- No MVP, o jogo terá **salvamento automático** (periódico e ao concluir objetivos), que guarda o progresso geral: fase atual, pontos permanentes e objetivos concluídos. O **salvamento manual** de verdade (em qualquer momento) fica para depois do MVP.

## Alternativas consideradas

- **Sem checkpoint, com reset total ao morrer:** é mais tenso, mas punitivo e frustrante.
- **Progressão totalmente persistente (estilo RPG):** dificulta o balanceamento, porque jogadores muito fortes esmagam as fases e a tensão se perde.
- **Progressão apenas dentro da partida, sem nada permanente:** é simples, mas dá pouca sensação de avanço entre as tentativas.
- **Salvamento manual e estado completo da fase no meio da partida:** é complexo demais para o MVP e fica para uma versão futura.

## Consequências

- **Positivas:** cada fase funciona como uma mini-partida previsível, o que facilita o balanceamento; a morte não é punitiva; bosses e fases concluídas ganham peso por conceder pontos permanentes.
- **Negativas:** o MVP precisa de um sistema de save desde o início; o jogador perde os equipamentos ao mudar de fase, o que pode ser suavizado no futuro com a escolha do equipamento inicial entre os já desbloqueados; as regras podem mudar após os primeiros testes de jogo.
