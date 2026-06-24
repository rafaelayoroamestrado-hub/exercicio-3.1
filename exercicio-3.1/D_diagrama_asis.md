"# Diagrama da Jornada"



\# D\_diagrama\_asis.md



\# Diagrama Visual do Service Blueprint (AS-IS)

Serviço: Emissão de Passaporte - Polícia Federal



\[CAMADA 1: EVIDÊNCIAS FÍSICAS]

&#x20; ├── Site DPF (F: Instabilidade)

&#x20; ├── GRU (G: Confusão de guias)

&#x20; ├── Posto PF (F: Filas)

&#x20; └── Passaporte Físico (F: Defeitos/Extravio)



\[CAMADA 2: AÇÕES DO CIDADÃO]

&#x20; ├── Preenchimento formulário (F: Erros de digitação)

&#x20; ├── Pagamento GRU (G: Atraso conciliação)

&#x20; ├── Agendamento (F: Escassez de vagas)

&#x20; ├── Coleta Biométrica (F: Falha leitura)

&#x20; └── Retirada (G: Falha comunicação)



\[CAMADA 3: FRONTSTAGE (LINHA DE FRENTE)]

&#x20; ├── Sistema Web (F: Erros genéricos)

&#x20; └── Atendente do Posto (G: Dependência equipamentos)



\[CAMADA 4: BACKSTAGE (BASTIDORES)]

&#x20; ├── Validação TSE/Receita (F: Indisponibilidade)

&#x20; ├── Conciliação Financeira (G: Prazo compensação)

&#x20; ├── Envio Casa da Moeda (F: Falha transmissão)

&#x20; └── Gestão de Status (G: Canais limitados)



\[CAMADA 5: PROCESSOS DE SUPORTE]

&#x20; ├── Infraestrutura SERPRO (F: Incidentes datacenter)

&#x20; ├── Backup Geográfico (G: Failover não transparente)

&#x20; ├── ICP-Brasil (F: Expiração certificados)

&#x20; └── Logística de Transporte (G: Greves/Atrasos)



\--------------------------------------------------

Legenda: (F) Fail Point | (G) Gargalo

