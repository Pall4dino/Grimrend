# ADR 004 – Fases, checkpoint e progressão do jogador

- **Status:** Aceito
- **Data:** 06/10/2026
- **Última revisão:** 10/10/2026 (o save passou a guardar também a progressão temporária)

## Contexto

O jogo busca um ciclo viciante em que o jogador começa fraco, evolui, se sente forte e encontra um novo obstáculo. A morte não deve ser punitiva, e o MVP precisa ser simples de implementar. Era preciso decidir se a progressão vale só dentro de cada partida, se é persistente ou se é uma mistura, o que acontece quando o jogador morre e o que acontece quando ele sai do jogo e volta depois.

## Decisão

- O jogo é dividido em **fases**, com **checkpoint no início de cada uma**. Ao morrer, o jogador volta ao início da fase atual, com o equipamento padrão da fase e com **nível, XP e habilidades temporárias zerados**.
- Ao entrar em uma nova fase, o **equipamento volta ao padrão daquela fase**, e **nível e XP resetam**.
- **Nível, XP e habilidades temporárias só são zerados ao morrer ou ao avançar de fase.** Sair do jogo e voltar não zera nada.
- **Habilidades temporárias** são compradas na árvore de habilidades com os pontos ganhos ao subir de nível e valem apenas na fase atual.
- **Habilidades permanentes** são compradas com pontos permanentes, concedidos uma única vez por objetivo concluído (derrotar um boss, concluir uma fase). Se o jogador refizer uma fase, os objetivos já concluídos não concedem o ponto de novo, mas precisam ser cumpridos outra vez para liberar a saída. Os pontos são gastos em uma área própria da árvore de habilidades e são mantidos ao morrer. A árvore é aberta pelo menu de pausa, durante a partida, e só existe dentro de uma partida com save válido (não é acessível pelo menu principal).
- No MVP, o jogo terá **salvamento automático**, feito periodicamente, ao concluir objetivos e ao voltar ao menu principal ou sair do jogo pelo menu. O save guarda **todo o progresso do jogador**: fase atual, nível, XP, habilidades temporárias e permanentes, pontos disponíveis, equipamento e objetivos concluídos. Ele **não guarda** a posição do jogador, o estado exato da fase (inimigos, projéteis etc.) nem a vida atual.
- Ao continuar, o jogador retoma do **início da fase atual** com todo o progresso salvo e com a **vida cheia**, de acordo com o máximo atual (que pode incluir bônus de habilidades temporárias, por exemplo, 115 em vez de 100).
- **Ao morrer, o save é atualizado** com o progresso temporário zerado, para que fechar o jogo na tela de morte não preserve o progresso anterior.
- O **salvamento manual** de verdade (em qualquer momento) e a retomada exata da fase ficam para depois do MVP.

## Alternativas consideradas

- **Sem checkpoint, com reset total ao morrer:** é mais tenso, mas punitivo e frustrante.
- **Progressão totalmente persistente (estilo RPG):** dificulta o balanceamento, porque jogadores muito fortes esmagam as fases e a tensão se perde.
- **Progressão apenas dentro da partida, sem nada permanente:** é simples, mas dá pouca sensação de avanço entre as tentativas.
- **Salvar só o progresso geral (fase, pontos permanentes e objetivos), zerando nível e XP ao sair do jogo:** descartada, porque faria o jogador perder progresso só por fechar o jogo, o que seria um problema sério de experiência, mesmo no MVP.
- **Salvamento manual e estado completo da fase no meio da partida:** é complexo demais para o MVP e fica para uma versão futura.
- **Árvore de habilidades acessível pelo menu principal:** descartada, porque as habilidades temporárias pertencem a uma partida e só fazem sentido com um save válido em andamento.

## Consequências

- **Positivas:** cada fase funciona como uma mini-partida previsível, o que facilita o balanceamento; a morte não é punitiva; o jogador não perde progresso ao sair do jogo; bosses e fases concluídas ganham peso por conceder pontos permanentes.
- **Negativas:** o MVP precisa de um sistema de save desde o início, e ele guarda mais dados (nível, XP, habilidades e equipamento), que devem ser validados e versionados; como a fase recomeça do início ao continuar, os inimigos reaparecem e o jogador pode, em tese, ganhar XP repetindo esse processo (o que já seria possível ficando na fase); o jogador perde os equipamentos ao mudar de fase, o que pode ser suavizado no futuro com a escolha do equipamento inicial entre os já desbloqueados; as regras podem mudar após os primeiros testes de jogo.
