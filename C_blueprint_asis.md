"# Service Blueprint AS-IS"



\# Service Blueprint: Emissão de Passaporte — Polícia Federal (As-Is)



\## 1. Evidências Físicas



| Evidência | Descrição | Observações |

|-----------|-----------|-------------|

| Site do Departamento de Polícia Federal (DPF) | Canal digital principal para solicitação, preenchimento e agendamento. | \*\*Fail Point (F1):\*\* instabilidade durante picos de demanda (feriados, férias escolares) e lentidão em conexões de baixa velocidade. |

| Guia de Recolhimento da União (GRU) | Documento de arrecadação gerado pelo sistema para pagamento da taxa de emissão. | \*\*Gargalo (G1):\*\* cidadãos confundem GRU simples com GRU de outro órgão, gerando pagamentos inválidos. |

| Posto de Atendimento da PF | Local presencial para coleta biométrica, entrevista e retirada do documento. | \*\*Fail Point (F2):\*\* filas, falta de sinalização e deslocamento até postos em cidades sem unidade própria. |

| Passaporte (documento físico) | Documento de viagem produzido pela Casa da Moeda e entregue ao solicitante. | \*\*Fail Point (F3):\*\* defeitos de impressão, avaria no transporte ou extravio logístico. |



\---



\## 2. Ações do Cidadão



1\. \*\*Preenchimento do formulário eletrônico\*\* — O cidadão acessa o site da PF, insere dados pessoais, informações de filiação e endereço, e declara o motivo da viagem.  

&#x20;  \*F4:\* erros de digitação, inconsistência entre o nome social e o registro civil, ou dados desatualizados na Receita Federal podem bloquear o prosseguimento.



2\. \*\*Geração e pagamento da GRU\*\* — O sistema emite a guia com valor vigente; o cidadão paga em banco, casa lotérica, internet banking ou via Pix.  

&#x20;  \*G2:\* atraso na conciliação bancária pode impedir o agendamento mesmo após o pagamento confirmado.



3\. \*\*Agendamento de atendimento presencial\*\* — Após compensação do pagamento, o cidadão escolhe data, horário e posto de atendimento.  

&#x20;  \*F5:\* escassez de vagas em grandes centros urbanos; cancelamentos em massa por problemas climáticos ou greves.



4\. \*\*Coleta biométrica e entrevista\*\* — No dia agendado, o cidadão comparece ao posto, apresenta documentos originais, realiza a coleta de fotografia, digitais e assinatura.  

&#x20;  \*F6:\* divergência entre documentos; falha na leitura biométrica de idosos ou pessoas com dificuldades dermatológicas.



5\. \*\*Retirada do passaporte\*\* — O cidadão é notificado e retira o documento no posto, mediante apresentação de protocolo e identificação.  

&#x20;  \*G3:\* prazos de produção variam e a comunicação por e-mail/SMS pode falhar (caixa de spam, troca de número).\*



\---



\## 3. Linha de Frente / Frontstage



\### 3.1 Sistema Web da PF

\- Interface pública responsável por guiar o cidadão no preenchimento, geração da GRU, acompanhamento de status e agendamento.

\- Regras de negócio executadas no front-end (máscaras, validações simples de CPF e e-mail) e validações síncronas no back-end.

\- \*\*Fail Point (F7):\*\* mensagens de erro genéricas dificultam a autosserviço e aumentam o volume de chamados no atendimento humano.



\### 3.2 Atendente do Posto

\- Recepciona o cidadão, confere a documentação, inicia o atendimento no sistema interno, coleta biometria e finaliza o protocolo.

\- Atua como ponto de resolução de problemas quando o cidadão não consegue concluir sozinho.

\- \*\*Gargalo (G4):\*\* dependência de capacitação do atendente e de equipamentos específicos (câmera, leitor de digitais, impressora) que, quando falham, interrompem o atendimento presencial.



\---



\## 4. Bastidores / Backstage



\### 4.1 Validação Síncrona com TSE e Receita Federal

\- O sistema da PF consulta, em tempo real, a base do Tribunal Superior Eleitoral (TSE) e a Receita Federal para conferir regularidade eleitoral e situação cadastral do CPF.

\- \*\*Fail Point (F8):\*\* indisponibilidade dos serviços externos gera mensagem de "tente novamente mais tarde", frustrando o cidadão e aumentando o abandono do serviço.



\### 4.2 Conciliação de Pagamento

\- A GRU paga é reconciliada junto às instituições financeiras e ao sistema de arrecadação da União. O status "pago" libera o agendamento.

\- \*\*Gargalo (G5):\*\* o prazo de conciliação pode variar de instantâneo a até 48 horas úteis, gerando incerteza e chamados.



\### 4.3 Envio para a Casa da Moeda

\- Após a coleta biométrica e aprovação do protocolo, os dados e a imagem do cidadão são transmitidos de forma segura à Casa da Moeda para produção física do passaporte.

\- \*\*Fail Point (F9):\*\* falhas de transmissão ou inconsistência no pacote de dados podem atrasar a produção ou exigir nova coleta.



\### 4.4 Gestão de Status e Notificações

\- O sistema atualiza o andamento do pedido e dispara notificações por e-mail e SMS sobre mudanças de status (recebido, em produção, pronto para retirada).

\- \*\*Gargalo (G6):\*\* falta de integração multicanal (WhatsApp oficial, aplicativo) limita o alcance das comunicações.



\---



\## 5. Processos de Suporte



\### 5.1 Infraestrutura SERPRO

\- Hospeda parte dos sistemas e serviços de conectividade da Polícia Federal, garantindo processamento, redes e segurança perimetral.

\- \*\*Fail Point (F10):\*\* incidentes em datacenters ou links de comunicação podem derrubar o site e o atendimento presencial simultaneamente.



\### 5.2 Backup Geográfico e Continuidade de Negócio

\- Replicação de dados críticos entre diferentes regiões para recuperação em caso de desastres.

\- \*\*Gargalo (G7):\*\* failover nem sempre é transparente, causando indisponibilidade parcial durante manutenções ou incidentes.



\### 5.3 ICP-Brasil (Infraestrutura de Chaves Públicas Brasileira)

\- Fornece certificados digitais e assinaturas que garantem autenticidade e não repúdio nas transações e documentos eletrônicos do serviço.

\- \*\*Fail Point (F11):\*\* expiração ou revogação de certificados pode invalidar comunicações seguras com parceiros (TSE, Receita, Casa da Moeda).



\### 5.4 Logística de Transporte e Entrega

\- Responsável pelo deslocamento físico dos passaportes produzidos até os postos de retirada, além de eventual logística reversa de documentos com defeito.

\- \*\*Gargalo (G8):\*\* greves dos Correios, atrasos rodoviários ou avarias no transporte impactam diretamente o prazo de retirada prometido ao cidadão.



\---



\## Resumo de Riscos Críticos



| Código | Tipo | Descrição do Risco | Camada |

|--------|------|-------------------|--------|

| F1 | Fail Point | Indisponibilidade/lentidão do site em picos de demanda | Evidências Físicas |

| F2 | Fail Point | Deslocamento e filas nos postos presenciais | Evidências Físicas |

| F3 | Fail Point | Defeitos ou extravio do passaporte físico | Evidências Físicas |

| F4 | Fail Point | Erros de preenchimento e inconsistência cadastral | Ações do Cidadão |

| F5 | Fail Point | Escassez de vagas e cancelamentos de agendamento | Ações do Cidadão |

| F6 | Fail Point | Falhas na coleta biométrica | Ações do Cidadão |

| F7 | Fail Point | Mensagens de erro genéricas no sistema web | Frontstage |

| F8 | Fail Point | Indisponibilidade das bases do TSE/Receita | Backstage |

| F9 | Fail Point | Falhas na transmissão de dados à Casa da Moeda | Backstage |

| F10 | Fail Point | Incidentes na infraestrutura SERPRO | Suporte |

| F11 | Fail Point | Problemas com certificados ICP-Brasil | Suporte |

| G1 | Gargalo | Confusão na GRU | Ações do Cidadão |

| G2 | Gargalo | Demora na conciliação de pagamento | Backstage |

| G3 | Gargalo | Falha na comunicação de prontos para retirada | Ações do Cidadão |

| G4 | Gargalo | Dependência de equipamentos e capacitação | Frontstage |

| G5 | Gargalo | Variabilidade de prazos de compensação | Backstage |

| G6 | Gargalo | Canais de notificação limitados | Backstage |

| G7 | Gargalo | Failover não transparente | Suporte |

| G8 | Gargalo | Instabilidade logística de transporte | Suporte |



\---



\## Considerações Finais



O Service Blueprint da emissão de passaporte evidencia um serviço híbrido, no qual a experiência do cidadão depende da integração entre canais digitais, atendimento presencial, parceiros governamentais (TSE, Receita Federal, Casa da Moeda) e fornecedores de infraestrutura (SERPRO, ICP-Brasil, transportadoras). A melhoria da jornada exige não apenas investimentos em tecnologia, mas também padronização de mensagens de erro, ampliação de canais de comunicação, capacitação contínua dos atendentes e redundância nos pontos críticos de integração.

