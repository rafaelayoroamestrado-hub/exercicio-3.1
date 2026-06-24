"# Relat¢rio Assistente v3"



\# Relatório Consolidado — Emissão de Passaporte (Polícia Federal)



\## 1. Introdução



A emissão de passaporte brasileiro constitui um serviço público crítico de soberania e identificação civil, coordenado pela Polícia Federal (PF) e apoiado por uma cadeia complexa de parceiros institucionais, fornecedores de tecnologia e entidades de segurança. O presente relatório consolida as diretrizes finais para a operação do sistema de emissão de passaportes, abordando planos de contingência contra falhas de sistemas externos, a logística de segurança e custódia da Casa da Moeda, a gestão de exceções no atendimento presencial, a matriz de responsabilidade (RACI) e os requisitos de segurança da informação baseados na ICP-Brasil.



\## 2. Planos de Contingência para Falhas de Sistemas Externos (TSE/Receita)



A autenticidade e a integridade dos dados do solicitante dependem de consultas em tempo real a sistemas externos, notadamente ao Tribunal Superior Eleitoral (TSE), para conferência de título de eleitor e situação eleitoral, e à Receita Federal, para validação do Cadastro de Pessoa Física (CPF) e regularidade fiscal. A indisponibilidade dessas fontes pode causar paralisação parcial ou total do atendimento, gerando filas, insatisfação e risco de concessão indevida.



Para mitigar esses riscos, recomenda-se a implementação dos seguintes planos de contingência:



\- \*\*Modo degradado de consulta\*\*: quando o sistema do TSE ou da Receita estiver fora do ar, a plataforma de passaportes deve permitir o registro do atendimento em modo offline, desde que os dados obrigatórios sejam posteriormente validados antes da finalização do documento. O atendimento prossegue, mas a emissão efetiva fica condicionada à reconciliação dos dados.

\- \*\*Cache controlado de dados cadastrais\*\*: manter, em ambiente seguro, um cache criptografado de consultas recentes e bem-sucedidas, com prazo de validade curto (até 24 horas), para evitar reprocesssamento desnecessário durante indisponibilidade transitória.

\- \*\*Fila de reconciliação assíncrona\*\*: toda consulta que não puder ser concluída em tempo real deve ser enfileirada e reprocessada automaticamente assim que o sistema externo retornar. Alertas devem ser enviados à equipe de operações e à área de segurança.

\- \*\*Protocolo de comunicação com as áreas responsáveis\*\*: estabelecer canais diretos com as equipes técnicas do TSE e da Receita Federal, inclusive em horários de pico e finais de semana, com SLAs claros de restabelecimento.

\- \*\*Comunicação ao cidadão\*\*: em caso de indisponibilidade que impossibilite a conclusão, deve ser gerado automaticamente um protocolo de atendimento com orientações sobre retorno, evitando que o cidadão permaneça sem informação.



\## 3. Logística de Segurança e Custódia da Casa da Moeda



A Casa da Moeda do Brasil (CMB) desempenha papel estratégico na produção física do passaporte, incluindo a impressão do livreto, personalização dos dados biométricos e custódia dos documentos em trânsito. Qualquer falha nessa cadeia pode comprometer a soberania do documento e a segurança nacional.



A logística deve seguir as seguintes diretrizes:



\- \*\*Cadeia de custódia rastreável\*\*: cada lote de passaportes deve possuir identificador único, lacres invioláveis e registro de todos os responsáveis pelas etapas de produção, armazenamento, transporte e entrega.

\- \*\*Transporte escoltado e sigiloso\*\*: o trânsito entre a CMB e as unidades da Polícia Federal deve ser realizado por transporte blindado ou escoltado, com roteiros predefinidos, horários alternativos e comunicação constante com a central de operações.

\- \*\*Área restrita e controle de acesso\*\*: as dependências de impressão e personalização devem ser classificadas como áreas restritas, com acesso biométrico, registro de visitantes, vigilância por circuito fechado de TV e separação de funções críticas.

\- \*\*Inventário cíclico e auditoria\*\*: realização de inventários periódicos dos materiais em branco, documentos em produção e prontos para entrega, com auditorias independentes para detectar discrepâncias ou desvios.

\- \*\*Destruição controlada de resíduos\*\*: materiais defeituosos, descartados ou excedentes devem ser destruídos de forma controlada, com certificação e registro fotográfico, impedindo reuso ou vazamento de documentos em branco.



\## 4. Gestão de Exceções no Frontstage (Falhas Biométricas)



O atendimento presencial ao cidadão (frontstage) é o ponto mais sensível da experiência de serviço e também o mais exposto a falhas operacionais. Falhas na coleta biométrica — como impossibilidade de captura de digitais por desgaste, lesões ou problemas técnicos nos equipamentos — exigem procedimentos claros e flexíveis.



As diretrizes para gestão de exceções são:



\- \*\*Tentativas múltiplas e troca de equipamento\*\*: o atendente deve realizar até três tentativas de captura por dispositivo e, persistindo a falha, utilizar equipamento alternativo ou outro sensor biométrico disponível na unidade.

\- \*\*Registro documental da exceção\*\*: toda falha biométrica deve ser registrada no sistema, com motivo, número de tentativas, identificação do atendente e do equipamento, e decisão adotada.

\- \*\*Autenticação alternativa\*\*: quando a biometria for inviável, deve ser acionado procedimento de autenticação alternativa, baseado em documentos originais, comprovação de identidade por meio de declaração e validação por supervisor.

\- \*\*Escalonamento imediato\*\*: casos não resolvidos em até dez minutos devem ser encaminhados a um supervisor técnico, que poderá autorizar a continuidade do atendimento, o reagendamento ou a abertura de chamado técnico.

\- \*\*Treinamento e simulação\*\*: os atendentes devem receber treinamento periódico sobre falhas biométricas, incluindo simulações e análise de casos reais, para reduzir o tempo de resolução e aumentar a confiança do cidadão.



\## 5. Matriz de Responsabilidade (RACI)



A matriz RACI abaixo define os papéis e responsabilidades sobre os principais processos de emissão de passaporte:



| Atividade / Processo | Polícia Federal (Direção) | Casa da Moeda | Área de TI | Atendente | Cidadão |

|---|---|---|---|---|---|

| Definição de política de emissão | R (Responsável) | C (Consultado) | C | I (Informado) | I |

| Desenvolvimento e manutenção do sistema | A (Aprovador) | I | R | I | - |

| Produção e personalização do passaporte | A | R | C | I | I |

| Coleta de dados biométricos no posto | I | I | C | R | A |

| Validação de dados com TSE/Receita | A | - | R | C | I |

| Logística de transporte e custódia | A | R | C | I | - |

| Gestão de exceções no atendimento | A | I | C | R | C |

| Segurança da informação e certificação digital | A | C | R | I | - |

| Comunicação e atendimento ao público | A | I | C | R | C |



Legenda: R = Responsável pela execução; A = Aprovador/Accountable; C = Consultado; I = Informado.



\## 6. Segurança da Informação (ICP-Brasil)



A Infraestrutura de Chaves Públicas Brasileira (ICP-Brasil) é a base para garantir a autenticidade, integridade, confidencialidade e validade jurídica das informações trocadas no processo de emissão de passaporte. Todos os certificados digitais, assinaturas eletrônicas e canais de comunicação devem estar em conformidade com a ICP-Brasil.



As diretrizes de segurança são:



\- \*\*Uso obrigatório de certificados ICP-Brasil\*\*: servidores, sistemas e usuários que realizam operações críticas devem utilizar certificados digitais de pessoa física, pessoa jurídica ou aplicação emitidos por Autoridades Certificadoras credenciadas.

\- \*\*Assinatura digital de documentos e registros\*\*: todos os registros de atendimento, laudos de exceção, relatórios de transporte e relatórios de auditoria devem ser assinados digitalmente, garantindo rastreabilidade e validade jurídica.

\- \*\*Criptografia em trânsito e em repouso\*\*: os dados pessoais e biométricos devem ser criptografados durante a transmissão (TLS 1.2 ou superior) e no armazenamento (AES-256), com gestão segura de chaves.

\- \*\*Controle de acesso baseado em função (RBAC)\*\*: as permissões nos sistemas devem ser atribuídas conforme o papel do usuário, com registro de auditoria, revisão periódica e revogação automática em caso de mudança de função ou desligamento.

\- \*\*Monitoramento e resposta a incidentes\*\*: deve haver central de monitoramento contínuo, com detecção de intrusões, análise de logs e plano de resposta a incidentes de segurança da informação alinhado à Política de Segurança da Informação do governo federal.

\- \*\*Conformidade legal e revisão periódica\*\*: a segurança da informação deve ser revisada anualmente, considerando a LGPD, o Decreto nº 7.724/2012 e as normas do Gabinete de Segurança Institucional (GSI) e da Autoridade Nacional de Proteção de Dados (ANPD).



\## 7. Considerações Finais



A emissão de passaporte exige coordenação robusta entre a Polícia Federal, a Casa da Moeda, órgãos de informação e cidadãos. Os planos de contingência, a logística segura de custódia, a gestão de exceções no frontstage, a clara distribuição de responsabilidades e a observância à ICP-Brasil formam um conjunto integrado de salvaguardas para garantir a continuidade, a confiabilidade e a segurança do serviço. A revisão periódica deste relatório e a realização de exercícios simulados são essenciais para manter a maturidade operacional e a resiliência do processo.

