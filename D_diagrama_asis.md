\# Diagrama Visual do Service Blueprint (AS-IS)



Serviço: Emissão de Passaporte — Polícia Federal



\---



\## Tabela RACI — Matriz de Responsabilidades



| Atividade | Cidadão | Sistema PF | Atendente PF | TSE/Receita | Casa da Moeda | SERPRO |

|-----------|---------|------------|--------------|-------------|---------------|--------|

| Preencher formulário online | R, A | R | I | - | - | C |

| Gerar GRU | I | R | - | - | - | C |

| Pagar GRU | R, A | I | - | - | - | - |

| Agendar atendimento | R | R | I | - | - | C |

| Conferir documentos | I | - | R, A | - | - | - |

| Validar CPF/Título | I | R | I | R | - | C |

| Coletar biometria | C | R | R, A | - | - | - |

| Produzir passaporte | I | R | - | - | R, A | C |

| Notificar retirada | I | R | - | - | - | C |

| Retirar passaporte | R, A | I | R | - | - | - |



\*\*Legenda:\*\* R = Responsável pela execução, A = Aprovador (Accountable), C = Consultado, I = Informado



\---



\## Handoffs entre Atores



| Handoff | De | Para | O que é transferido | Risco |

|---------|----|------|---------------------|-------|

| H1 | Cidadão | Sistema PF | Dados pessoais preenchidos | F4: Erros de digitação |

| H2 | Sistema PF | TSE/Receita | Consulta de CPF e título | F8: Indisponibilidade |

| H3 | TSE/Receita | Sistema PF | Status de regularidade | F8: Timeout |

| H4 | Cidadão | Banco | Pagamento da GRU | G2: Atraso conciliação |

| H5 | Banco | Sistema PF | Confirmação de pagamento | G5: Prazo variável |

| H6 | Cidadão | Atendente PF | Documentos físicos | F6: Divergência |

| H7 | Atendente PF | Sistema PF | Dados biométricos coletados | F9: Falha transmissão |

| H8 | Sistema PF | Casa da Moeda | Pacote de dados do cidadão | F9: Inconsistência |

| H9 | Casa da Moeda | Posto PF | Passaporte físico | F3: Extravio/avaria |

| H10 | Sistema PF | Cidadão | Notificação de prontidão | G3: Falha comunicação |

| H11 | Atendente PF | Cidadão | Passaporte entregue | F3: Defeito |



\---



\## Diagrama de Swim Lanes┌─────────────────────────────────────────────────────────────────────────────┐

│ EVIDÊNCIAS FÍSICAS                                                          │

│  \[Site PF] → \[GRU] → \[Comprovante] → \[Posto PF] → \[Passaporte Físico]      │

├─────────────────────────────────────────────────────────────────────────────┤

│ AÇÕES DO CIDADÃO                                                            │

│  1. Preencher formulário ──► 2. Pagar GRU ──► 3. Agendar                    │

│       │                          │                │                          │

│       ▼                          ▼                ▼                          │

│  4. Comparecer ao posto ──► 5. Coleta biométrica ──► 6. Retirar             │

├─────────────────────────────────────────────────────────────────────────────┤

│ FRONTSTAGE (Linha de Frente)                                                │

│  ┌──────────────────────┐    ┌──────────────────────────┐                   │

│  │   Sistema Web PF     │    │   Atendente do Posto     │                   │

│  │  (autoatendimento)   │    │  (recepção + biometria)  │                   │

│  └──────────┬───────────┘    └─────────────┬────────────┘                   │

│             │                              │                                │

│             ▼                              ▼                                │

├─────────────────────────────────────────────────────────────────────────────┤

│ BACKSTAGE (Bastidores)                                                      │

│  ┌──────────────────┐  ┌────────────────┐  ┌──────────────────────┐        │

│  │ Validação TSE/   │  │  Conciliação   │  │  Envio Casa da       │        │

│  │ Receita Federal  │  │  de Pagamento  │  │  Moeda               │        │

│  └────────┬─────────┘  └───────┬────────┘  └──────────┬───────────┘        │

│           │                    │                      │                     │

│           ▼                    ▼                      ▼                     │

│  ┌──────────────────────────────────────────────────────────────┐          │

│  │              Gestão de Status e Notificações                 │          │

│  │              (E-mail / SMS)                                  │          │

│  └──────────────────────────────────────────────────────────────┘          │

├─────────────────────────────────────────────────────────────────────────────┤

│ PROCESSOS DE SUPORTE                                                        │

│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │

│  │ Infraestrutura│  │    Backup    │  │  ICP-Brasil  │  │  Logística   │    │

│  │   SERPRO     │  │  Geográfico  │  │ Certificados │  │  Transporte  │    │

│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘    │

└─────────────────────────────────────────────────────────────────────────────┘markdown

\---



\## Fluxo da Jornada com Handoffs\[Cidadão] ──H1──► \[Sistema PF] ──H2──► \[TSE/Receita]

│                      │

│◄───────H3────────────┘

│

│  GRU gerada

│

\[Cidadão] ──H4──► \[Banco] ──H5──► \[Sistema PF]

│

│  Pagamento confirmado → Agendamento liberado

│

\[Cidadão] ──H6──► \[Atendente PF] ──H7──► \[Sistema PF]

│

│  Dados biométricos enviados

│

\[Sistema PF] ──H8──► \[Casa da Moeda]

│

│  Passaporte produzido

│

\[Casa da Moeda] ──H9──► \[Posto PF]

│

\[Sistema PF] ──H10──► \[Cidadão] (notificação)

│

\[Atendente PF] ──H11──► \[Cidadão] (entrega)markdown

\---



\## Legenda



| Símbolo | Significado |

|---------|-------------|

| F1 a F11 | Fail Points (pontos de falha) |

| G1 a G8 | Gargalos (pontos de estrangulamento) |

| H1 a H11 | Handoffs (transferências entre atores) |

| R | Responsável pela execução |

| A | Aprovador (Accountable) |

| C | Consultado |

| I | Informado |

