"# Transcript do Grill-me"



\# C\_grill\_transcript.md



\# Registro da Sabatina (Grill-Me) — Service Blueprint de Emissão de Passaporte



\*\*Data:\*\* \[data da sessão]  

\*\*Participantes:\*\* \[arquitetos, representantes de negócio, segurança, infraestrutura, parceiros SERPRO/Casa da Moeda]  

\*\*Objeto:\*\* validação técnica e registro de consenso do Service Blueprint de Emissão de Passaporte  

\*\*Formato:\*\* 6 rodadas de perguntas diretas, com decisão consolidada ao final de cada rodada.



\---



\## Rodada 1 — Falhas de API (Circuit Breaker)



\### Perguntas levantadas

1\. Qual o comportamento esperado quando a API de consulta de antecedentes (polícia federal / outros órgãos) fica indisível ou apresenta latência excessiva?

2\. O sistema deve tentar novamente a requisição indefinidamente, ou adotar um mecanismo de Circuit Breaker para evitar cascata de falhas?

3\. Como o cidadão e a unidade de atendimento serão notificados quando a análise automática for suspensa por falha de API?

4\. Qual a estratégia de fallback para casos em que o Circuit Breaker abre: fila de retorno, análise manual, ou rejeição automática?



\### Decisão consensuada

\- Adotar \*\*Circuit Breaker\*\* nos pontos de integração críticos externos (antecedentes, identificação civil nacional, Receita Federal, etc.).

\- Configurar thresholds: após \*\*3 falhas consecutivas\*\* ou \*\*tempo de resposta > 5 s por 30 s\*\*, o circuito abre por \*\*60 s\*\*, com incremento exponencial até o máximo de \*\*5 min\*\*.

\- Em caso de abertura, a solicitação é direcionada para uma \*\*fila de retorno (DLQ)\*\* e sinalizada ao operador para análise humana.

\- O cidadão recebe notificação push/SMS indicando que o pedido está em "análise especializada" e não em erro.

\- Logs e métricas de Circuit Breaker devem ser expostos via health endpoint e monitorados pelo time de SRE.



\---



\## Rodada 2 — Autenticação Alternativa por Falha Biométrica



\### Perguntas levantadas

1\. O que acontece se o leitor biométrico falhar ou se a digital do cidadão não for reconhecida no posto?

2\. O cadastro pode prosseguir apenas com documento apresentado e senha presencial, ou exige-se um segundo fator biométrico?

3\. Haverá limites de tentativas biométricas antes de acionar o fluxo alternativo?

4\. Como garantir que a autenticação alternativa preserve o mesmo nível de confiança da via biométrica?



\### Decisão consensuada

\- Estabelecer \*\*até 3 tentativas de captura biométrica\*\* por sessão de atendimento. Após isso, aciona-se o fluxo alternativo.

\- O fluxo alternativo será composto por: \*\*validação documental presencial (dois documentos oficiais com foto)\*\* + \*\*código OTP enviado ao celular cadastrado na Receita Federal / gov.br\*\* + \*\*comprovação de vínculo com o CPF\*\*.

\- A autenticação alternativa gera um \*\*registro de exceção auditável\*\*, com hash da transação, motivo da falha e identificação do operador responsável.

\- A aprovação pelo caminho alternativo mantém o pedido no mesmo nível de confiança, porém exige \*\*validação adicional da equipe de atendimento\*\* (supervisor) antes de seguir para impressão.

\- A face (foto) capturada no posto continua sendo comparada à base de rosto, mesmo quando a digital falha, reforçando a prova de presença.



\---



\## Rodada 3 — Liberação em Tempo Real após Pagamento da GRU



\### Perguntas levantadas

1\. O pagamento da GRU (Guia de Recolhimento da União) deve liberar automaticamente a continuidade do fluxo, ou há etapa manual de conferência?

2\. Qual a integração com a arrecadação: consulta por polling, webhook ou evento via fila?

3\. Como lidar com divergências entre pagamento identificado e valor/quantidade de passaportes solicitados?

4\. Qual o SLA máximo entre a liquidação financeira e a liberação do pedido para a Casa da Moeda?



\### Decisão consensuada

\- A liberação será \*\*automática e em tempo real\*\*, baseada em \*\*evento de pagamento da GRU\*\* propagado pelo sistema de arrecadação.

\- Implementar integração via \*\*webhook com idempotência\*\* (identificador único da GRU) e fila de eventos como camada resiliência.

\- O polling será mantido apenas como \*\*mecanismo de contingência\*\*, com intervalo máximo de 15 min.

\- Divergências de valor ou duplicidade são tratadas por regras de negócio: se pagamento inferior, solicita complementação; se superior, gera crédito a ressarcir; se GRU já utilizada, bloqueia e notifica.

\- \*\*SLA:\*\* após liquidação financeira, o pedido deve estar liberado para impressão em até \*\*5 minutos\*\* em operação normal e \*\*30 minutos\*\* em contingência.



\---



\## Rodada 4 — Uso de ICP-Brasil na Integração com a Casa da Moeda



\### Perguntas levantadas

1\. Quais transações com a Casa da Moeda exigem assinatura digital ou certificação ICP-Brasil?

2\. O certificado a ser utilizado é de pessoa jurídica (aplicação) ou de pessoa física (operador)?

3\. Como será feita a gestão de ciclo de vida (emissão, renovação, revogação) dos certificados?

4\. A cadeia ICP-Brasil precisa ser validada em cada requisição ou há cache de confiança?



\### Decisão consensuada

\- Toda transação que envolva \*\*ordem de produção/entrega de passaportes\*\*, \*\*envio de dados pessoais sensíveis\*\* e \*\*recebimento de lote de passaportes prontos\*\* deverá ser \*\*assinada e/ou cifrada com certificado ICP-Brasil\*\*.

\- Será utilizado \*\*certificado de pessoa jurídica (e-CNPJ)\*\* vinculado à aplicação do órgão emissor, com \*\*HSM para armazenamento seguro da chave privada\*\*.

\- A renovação de certificados deve ser programada com \*\*antecedência mínima de 90 dias\*\*, com processo de transição sem indisponibilidade (sobreposição de certificados válidos).

\- A validação da cadeia de confiança ICP-Brasil será feita em \*\*cada requisição crítica\*\*, sem cache de status de revogação (CRL/OCSP em tempo real).

\- Logs de assinatura devem ser arquivados para fins de auditoria e prova legal, com retenção mínima de \*\*5 anos\*\*.



\---



\## Rodada 5 — Validação Biométrica na Entrega



\### Perguntas levantadas

1\. A entrega do passaporte ao cidadão exige nova coleta biométrica ou basta apresentar documento e comprovante de retirada?

2\. Qual a comparação realizada: digital capturada na entrega vs. digital coletada no ato do pedido, ou contra base nacional?

3\. Quem pode retirar o passaporte em nome do titular? Quais regras de procuração/autorização são aplicadas?

4\. O que ocorre se a validação biométrica na entrega falhar?



\### Decisão consensuada

\- A retirada do passaporte exige \*\*identificação biométrica do titular\*\* quando o posto dispuser de equipamento compatível.

\- A coleta na entrega é comparada \*\*primeiramente contra o template biométrico gerado no ato da solicitação\*\* (1:1); em caso de falha, pode-se recorrer à base nacional (1:N) como segunda validação, desde que haja autorização legal e consentimento.

\- A retirada por terceiros será permitida mediante \*\*procuração com firma reconhecida\*\*, \*\*documento do procurador\*\* e \*\*validação biométrica do procurador\*\*, com registro auditável.

\- Se a validação biométrica na entrega falhar, o cidadão/terceiro é encaminhado para \*\*atendimento especializado\*\*, onde ocorre análise humana e, se necessário, nova tentativa de coleta ou regularização do documento.

\- A entrega só é concluída após registro digital do receptor, com foto no momento da retirada e hash da transação de entrega.



\---



\## Rodada 6 — Papel do SERPRO e Backup Geográfico



\### Perguntas levantadas

1\. Quais componentes da arquitetura são operados diretamente pelo SERPRO e quais permanecem sob responsabilidade do órgão emissor?

2\. O SERPRO atua apenas como infraestrutura/hospedagem, ou também como integrador de alguns serviços compartilhados (identidade, pagamento, notificações)?

3\. Qual a estratégia de backup geográfico: hot-site, warm-site, cold-site? Qual o RPO/RTO acordado?

4\. Como é garantida a soberania dos dados pessoais em caso de operação multicloud ou datacenters do SERPRO?



\### Decisão consensuada

\- O \*\*SERPRO\*\* é responsável pelo \*\*hospedagem em ambiente governamental\*\*, \*\*conectividade com redes de comunicação seguras (Gov.br, RPV, etc.)\*\*, \*\*serviços de computação e armazenamento\*\*, além de integrar serviços compartilhados como \*\*identidade digital (gov.br)\*\* e \*\*notificações\*\* quando contratados.

\- O órgão emissor mantém a \*\*responsabilidade final pelo processo de negócio\*\*, regras de concessão, integração com a Casa da Moeda e governança dos dados pessoais.

\- A estratégia de backup geográfico será \*\*warm-site\*\* em região distinta do Brasil, com replicação assíncrona de dados críticos e automação de failover.

\- \*\*RPO:\*\* máximo \*\*15 minutos\*\* para dados de pedidos em andamento; \*\*RTO:\*\* máximo \*\*4 horas\*\* para recuperação do serviço essencial de emissão.

\- Dados biométricos e pessoais sensíveis \*\*não podem sair do território nacional\*\*; a replicação geográfica ocorre apenas entre datacenters do SERPRO localizados no Brasil.

\- Criptografia em trânsito (TLS 1.3) e em repouso (AES-256) é obrigatória, com gestão de chaves em HSM e segregação por ambiente.



\---



\## Registro de Consenso Final



As decisões acima foram validadas pelos participantes e compõem a \*\*baseline técnica\*\* do Service Blueprint de Emissão de Passaporte. Eventuais revisões deverão passar por nova sabatina e atualização deste registro.



\*\*Responsáveis pela implementação:\*\*  

\- Circuit Breaker e resiliência de APIs: Time de Arquitetura \& SRE  

\- Autenticação alternativa e regras biométricas: Time de Segurança \& Identidade  

\- Integração com GRU e liberação em tempo real: Time de Negócio \& Integração Financeira  

\- ICP-Brasil e Casa da Moeda: Time de Segurança da Informação \& Parceiros  

\- Validação biométrica na entrega: Time de Operações de Postos \& Identidade  

\- SERPRO e backup geográfico: Time de Infraestrutura \& Governança de Dados  



\*\*Próximos passos:\*\* elaboração dos diagramas de sequência detalhados e checklists de conformidade para cada decisão.

