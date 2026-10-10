# Diagrama de atividades: fluxo do jogo (MVP)

Estes diagramas mostram a sequência de atividades do Grimrend, do menu principal até o fim de uma fase, dentro do escopo do MVP descrito no [README](../../README.md). Para ficarem legíveis, o fluxo foi dividido em três diagramas: o principal e duas subatividades.

## Como ler

O Mermaid não tem um diagrama de atividades nativo, então eles foram desenhados como fluxogramas, usando a notação da UML:

| Símbolo | Significado |
|---|---|
| Círculo preto (primeiro do diagrama) | Início |
| Círculo preto com anel | Fim |
| Retângulo azul de cantos arredondados | Atividade (algo que o jogo ou o jogador faz) |
| Losango amarelo | Decisão, com as condições escritas nas setas |
| Retângulo verde com bordas duplas | Subatividade, detalhada em outro diagrama |
| Retângulo branco de borda grossa | Resultado, ou seja, como a subatividade termina |

## 1. Fluxo geral

Do menu principal até o resultado de cada fase. A atividade "Jogar a fase" está detalhada no diagrama 2.

```mermaid
flowchart TD
    Inicio(("&nbsp;")) --> M0("Exibir o menu principal")
    M0 --> M1{"Opção escolhida"}
    M1 -->|"Novo jogo"| M2{"Já existe um save?"}
    M1 -->|"Continuar"| M5{"Existe um save?"}
    M1 -->|"Sair do jogo"| Fim1((("&nbsp;")))
    M2 -->|"Sim"| M3{"Jogador confirma sobrescrever?"}
    M2 -->|"Não"| M4("Criar novo save na fase 1")
    M3 -->|"Sim"| M4
    M3 -->|"Não"| M0
    M5 -->|"Sim"| M6("Carregar o último save")
    M5 -->|"Não (opção indisponível)"| M0
    M4 --> F0
    M6 --> F0

    F0("Zerar nível e XP e aplicar as habilidades permanentes") --> F1("Criar o personagem com o equipamento padrão da fase e gerar os inimigos")
    F1 --> F2("Exibir a HUD")
    F2 --> J[["Jogar a fase (diagrama 2)"]]
    J --> R{"Resultado da fase"}

    R -->|"Jogador morreu"| D0("Exibir a tela de morte")
    D0 --> D1("Reiniciar a fase atual (volta ao checkpoint)")
    D1 --> F0

    R -->|"Fase concluída"| E0("Salvar automaticamente a conclusão da fase")
    E0 --> E1{"Existe uma próxima fase?"}
    E1 -->|"Sim"| F0
    E1 -->|"Não"| E2("Exibir a tela final da campanha (a definir)")
    E2 --> M0

    R -->|"Voltou ao menu principal"| M0
    R -->|"Saiu do jogo"| Fim2((("&nbsp;")))

    classDef decisao fill:#fff4d6,stroke:#b8860b,color:#222
    classDef acao fill:#e8f0fe,stroke:#4a6fa5,color:#222
    classDef sub fill:#e6f4ea,stroke:#2e7d32,stroke-width:2px,color:#222
    classDef no fill:#000,stroke:#000,color:#000
    class M1,M2,M3,M5,R,E1 decisao
    class M0,M4,M6,F0,F1,F2,D0,D1,E0,E2 acao
    class J sub
    class Inicio,Fim1,Fim2 no
```

## 2. Jogar a fase

O ciclo que se repete durante a partida. Ele termina quando o jogador morre, conclui a fase, volta ao menu principal ou sai do jogo. A atividade "Menu de pausa" está detalhada no diagrama 3.

```mermaid
flowchart TD
    Inicio(("&nbsp;")) --> P0("Mover, mirar e atacar")
    P0 --> P1("Causar dano e exibir os números de dano")
    P1 --> P2{"O inimigo morreu?"}
    P2 -->|"Sim"| P3("Conceder XP")
    P3 --> P4{"XP suficiente para subir?"}
    P4 -->|"Sim"| P5("Subir de nível e ganhar ponto de habilidade")
    P4 -->|"Não"| P6
    P5 --> P6
    P2 -->|"Não"| P6("Inimigos atacam e a barra de vida é atualizada")
    P6 --> P7{"Vida chegou a zero?"}
    P7 -->|"Sim"| X1(["Resultado: jogador morreu"])
    P7 -->|"Não"| P8{"Encontrou uma arma?"}
    P8 -->|"Sim"| P9("Equipar a arma")
    P8 -->|"Não"| P10
    P9 --> P10{"Objetivo da fase cumprido?"}
    P10 -->|"Sim"| P11("Conceder ponto permanente, salvar automaticamente e liberar a saída")
    P10 -->|"Não"| P12
    P11 --> P12{"Hora do save periódico?"}
    P12 -->|"Sim"| P13("Salvar automaticamente")
    P12 -->|"Não"| P14
    P13 --> P14{"Jogador pausou?"}
    P14 -->|"Sim"| Z[["Menu de pausa (diagrama 3)"]]
    P14 -->|"Não"| P15{"Chegou à saída liberada?"}
    P15 -->|"Não"| P0
    P15 -->|"Sim"| X2(["Resultado: fase concluída"])
    Z --> ZR{"Resultado do menu de pausa"}
    ZR -->|"Retomar a partida"| P0
    ZR -->|"Voltou ao menu principal"| X3(["Resultado: voltou ao menu principal"])
    ZR -->|"Saiu do jogo"| X4(["Resultado: saiu do jogo"])

    classDef decisao fill:#fff4d6,stroke:#b8860b,color:#222
    classDef acao fill:#e8f0fe,stroke:#4a6fa5,color:#222
    classDef sub fill:#e6f4ea,stroke:#2e7d32,stroke-width:2px,color:#222
    classDef res fill:#fff,stroke:#000,stroke-width:3px,color:#222
    classDef no fill:#000,stroke:#000,color:#000
    class P2,P4,P7,P8,P10,P12,P14,P15,ZR decisao
    class P0,P1,P3,P5,P6,P9,P11,P13 acao
    class Z sub
    class X1,X2,X3,X4 res
    class Inicio no
```

## 3. Menu de pausa

O que o jogador pode fazer ao pausar a partida. Ajustar o volume e usar a árvore de habilidades não saem do menu, e só retomar, voltar ao menu principal ou sair o encerram.

```mermaid
flowchart TD
    Inicio(("&nbsp;")) --> Z0("Congelar a partida e abrir o menu de pausa")
    Z0 --> Z1{"Opção escolhida"}
    Z1 -->|"Árvore de habilidades"| Z2("Gastar pontos e aplicar habilidades temporárias e permanentes")
    Z2 --> Z1
    Z1 -->|"Volume"| Z3("Alterar o volume geral")
    Z3 --> Z1
    Z1 -->|"Retomar"| X1(["Resultado: retomar a partida"])
    Z1 -->|"Voltar ao menu principal"| W1("Avisar que o progresso da fase será perdido")
    W1 --> W2{"Jogador confirma?"}
    W2 -->|"Sim"| X2(["Resultado: voltou ao menu principal"])
    W2 -->|"Não"| Z1
    Z1 -->|"Sair do jogo"| W3("Avisar que o progresso da fase será perdido")
    W3 --> W4{"Jogador confirma?"}
    W4 -->|"Sim"| X3(["Resultado: saiu do jogo"])
    W4 -->|"Não"| Z1

    classDef decisao fill:#fff4d6,stroke:#b8860b,color:#222
    classDef acao fill:#e8f0fe,stroke:#4a6fa5,color:#222
    classDef res fill:#fff,stroke:#000,stroke-width:3px,color:#222
    classDef no fill:#000,stroke:#000,color:#000
    class Z1,W2,W4 decisao
    class Z0,Z2,Z3,W1,W3 acao
    class X1,X2,X3 res
    class Inicio no
```

## Observações

- O salvamento periódico acontece em segundo plano no jogo real. Aqui ele aparece como uma verificação dentro do ciclo, para simplificar o desenho.
- O comportamento ao concluir a última fase ainda não foi definido, e por isso aparece como "Exibir a tela final da campanha (a definir)".
- Voltar ao menu principal ou sair durante uma fase faz o jogador perder o progresso daquela fase, porque o estado no meio da fase não é salvo no MVP.
- Itens fora do MVP (base, moedas, traders, salvamento manual, configurações, seleção de fase) não aparecem nestes diagramas.
