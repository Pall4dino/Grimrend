# Diagrama de sequência: fluxo principal do MVP

Este diagrama mostra como o Grimrend deve se comportar, do menu principal até o fim de uma fase. Ele cobre o escopo do MVP descrito no [README](../../README.md) e as decisões registradas em [`docs/decisoes/`](../decisoes/).

## Participantes

| Participante | O que representa |
|---|---|
| Jogador | Pessoa que joga |
| Menus e HUD | Menu principal, menu de pausa, árvore de habilidades e informações na tela |
| Controle do jogo | Lógica central: carrega fases, controla objetivos e checkpoints |
| Personagem | Personagem controlado pelo jogador (movimento, mira, ataque e vida) |
| Inimigos | Inimigos, mini bosses e boss da fase |
| Progressão (XP e habilidades) | XP, níveis, pontos de habilidade e habilidades temporárias e permanentes |
| Sistema de save | Salvamento automático do progresso do jogador (fase, nível, XP, habilidades, equipamento e objetivos) |

## Diagrama

```mermaid
sequenceDiagram
    autonumber
    actor J as Jogador
    participant UI as Menus e HUD
    participant GM as Controle do jogo
    participant P as Personagem
    participant E as Inimigos
    participant PR as Progressão (XP e habilidades)
    participant S as Sistema de save

    Note over J,S: 1. Início do jogo (menu principal)
    J->>UI: Escolhe Novo jogo ou Continuar
    alt Novo jogo
        UI->>S: Verifica se já existe um save
        opt Já existe um save
            UI->>J: Pede confirmação para sobrescrever
            J->>UI: Confirma
        end
        UI->>GM: Inicia novo jogo na fase 1
        GM->>S: Cria o save inicial
    else Continuar
        UI->>S: Carrega o último save
        S-->>GM: Progresso completo (fase, nível, XP, habilidades, equipamento e objetivos)
    end

    Note over J,S: 2. Início da fase (checkpoint)
    alt Novo jogo, morte ou nova fase
        GM->>PR: Zera nível e XP e aplica as habilidades permanentes
        GM->>P: Cria o personagem com o equipamento padrão da fase e vida cheia
    else Continuar
        GM->>PR: Restaura nível, XP e habilidades salvos
        GM->>P: Cria o personagem com o equipamento salvo e vida cheia (máximo atual)
    end
    GM->>E: Gera os inimigos da fase
    GM->>UI: Exibe a HUD (vida, XP e munição)

    Note over J,S: 3. Partida (repete até o jogador morrer ou chegar à saída)
    loop Durante a fase
        J->>P: Move (teclado) e mira (mouse)
        J->>P: Ataca
        P->>E: Causa dano (normal ou crítico, físico ou fogo)
        E-->>UI: Exibe o número de dano flutuante
        opt O inimigo morre
            E->>PR: Concede XP
            opt XP suficiente para subir de nível
                PR-->>UI: Sobe de nível e concede ponto de habilidade
            end
        end
        E->>P: Ataca o personagem
        P-->>UI: Atualiza a barra de vida
        opt O jogador encontra uma arma
            J->>P: Coleta a arma
            P-->>UI: Atualiza a arma equipada na HUD
        end
        opt Um objetivo da fase é cumprido
            GM->>PR: Concede ponto permanente (uma única vez por objetivo)
            GM->>S: Salva automaticamente
            GM-->>UI: Libera o ponto de saída da fase
        end
        opt Passou o intervalo do save periódico
            GM->>S: Salva automaticamente
        end
        opt O jogador pausa o jogo
            J->>UI: Pausa
            UI->>GM: Congela a partida
            alt Abre a árvore de habilidades
                J->>UI: Escolhe uma habilidade (temporária ou permanente)
                UI->>PR: Gasta o ponto e aplica a habilidade
                PR-->>P: Atualiza os atributos
                J->>UI: Retoma a partida
            else Ajusta o volume
                J->>UI: Altera o volume geral
                J->>UI: Retoma a partida
            else Volta ao menu principal ou sai do jogo
                UI->>J: Avisa que, ao continuar, a fase recomeça do início
                J->>UI: Confirma
                UI->>S: Salva o progresso (fase, nível, XP, habilidades, equipamento e objetivos)
                UI->>GM: Encerra a partida
                Note over GM,S: Ao continuar, o jogador retoma do início da fase com todo o progresso salvo
            end
        end
    end

    Note over J,S: 4. Fim da fase
    alt O jogador morre
        P-->>GM: Vida chega a zero
        GM->>S: Atualiza o save com o progresso temporário zerado
        GM->>UI: Exibe a tela de morte
        J->>UI: Escolhe reiniciar a fase
        UI->>GM: Reinicia a fase atual
        Note over GM,PR: Volta ao checkpoint com nível, XP e habilidades temporárias zerados, mantendo os pontos permanentes
    else O jogador chega à saída liberada
        J->>P: Alcança o ponto de saída
        GM->>PR: Zera nível e XP
        GM->>P: Restaura o equipamento padrão da próxima fase
        GM->>S: Salva automaticamente (fase concluída e início da próxima)
        Note over GM,PR: Começa a próxima fase com novo checkpoint
    end
```

## Observações

- O salvamento automático ocorre periodicamente, ao concluir objetivos, ao voltar ao menu principal ou sair e ao morrer. Ele guarda todo o progresso do jogador, mas não a posição, o estado exato da fase nem a vida atual.
- Ao continuar, o jogador retoma do início da fase com o progresso salvo e com a vida cheia, de acordo com o máximo atual.
- Ao morrer, nível, XP e habilidades temporárias são zerados, e os pontos permanentes são mantidos.
- Ao concluir uma fase, o equipamento volta ao padrão da próxima fase, e nível e XP são zerados.
- Itens fora do MVP (base, moedas, traders, salvamento manual, configurações, seleção de fase) não aparecem neste diagrama.
