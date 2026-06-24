"# Relat¢rio Assistente v2"



\# Relatório Técnico - Emissão de Passaporte (Polícia Federal) v2



\## 1. Resumo Executivo



O presente relatório consolida a correção dos apontamentos identificados na auditoria técnica v1 do processo de Emissão de Passaporte conduzido pela Polícia Federal (PF). O objetivo é apresentar a arquitetura atualizada do fluxo, descrever os pontos de integração com órgãos externos, detalhar a arrecadação via Guia de Recolhimento da União (GRU) por intermédio do Tesouro Nacional e assegurar o alinhamento com a Lei Geral de Proteção de Dados Pessoais (LGPD) e as diretrizes de acessibilidade do ICOM. O documento contempla também os ajustes realizados para mitigar falhas de validação, inconsistências cadastrais e fragilidades no rastreamento de pagamentos verificadas na versão anterior.



\## 2. Correções da Auditoria v1



A auditoria v1 identificou quatro grupos principais de falhas: (i) inconsistência na validação do CPF e do título de eleitor; (ii) ausência de controle de duplicidade de protocolos; (iii) geração de guias de pagamento sem vinculação automática ao Orçamento Fiscal; e (iv) falta de rastreabilidade de logs de acesso. Para a v2, foram adotadas as seguintes correções: implementação de validações síncronas e assíncronas junto à Receita Federal e ao TSE; criação de índice único composto no banco de protocolos; alteração do serviço de geração de GRU para consumir a API do Tesouro Nacional e retornar o número de documento de arrecadação (NDA); e habilitação de logs estruturados com identificação do usuário, timestamp, hash de integridade e classificação do evento.



\## 3. Integrações Sistêmicas



\### 3.1 TSE (Tribunal Superior Eleitoral)

A integração com o TSE permite verificar a regularidade do título de eleitor do solicitante e impedir a emissão de passaporte em situações de pendências eleitorais. A consulta ocorre por meio de serviço federado do PJe, com resposta padronizada em JSON e cache temporário de 30 minutos para reduzir carga nos sistemas externos.



\### 3.2 Receita Federal

A validação do CPF é realizada diretamente contra a base da Receita Federal, verificando situação cadastral, existência de restrições e impedimentos legais. A chamada é feita por API segura, com autenticação mútua (mTLS) e criptografia TLS 1.3. Eventuais situações de "não informada" ou "pendente de regularização" são encaminhadas para análise manual do atendente.



\### 3.3 Exército Brasileiro

Para candidatos em idade de serviço militar, o sistema consulta a situação de reservista junto ao Exército. A integração utiliza web service específico do Sistema de Gestão de Pessoas Militar, garantindo que a emissão do passaporte respeite as normas de alistamento e reservista.



\### 3.4 Interpol

O sistema realiza conferência de alertas e notificações difusas de vermelho e outras restrições de natureza internacional por meio da interface da Interpol disponível para órgãos policiais. A consulta é restrita a usuários credenciados e registrada em log de segurança.



\### 3.5 Casa da Moeda

Após aprovação e confirmação do pagamento, os dados biométricos e fotográficos são enviados à Casa da Moeda para produção do documento físico. A transferência utiliza canal criptografado e acompanhamento de status por etapas: impressão, laminação, personalização e expedição.



\## 4. Fluxos Financeiros da GRU via Tesouro Nacional



A arrecadação do passaporte utiliza a Guia de Recolhimento da União (GRU) gerada e quitada por meio da Plataforma de Gerenciamento de Arrecadação do Tesouro Nacional (PGA). O fluxo v2 compreende:



1\. Cálculo do valor da taxa conforme portaria vigente;

2\. Requisição à API do Tesouro Nacional para emissão da GRU, informando o código de unidade gestora (UG), gestão, identificador do contribuinte e o código de receita correspondente;

3\. Retorno do Número de Documento de Arrecadação (NDA) e linha digitável;

4\. Recebimento automático do arquivo de conciliação bancária (extrato do Tesouro);

5\. Atualização do status do protocolo para "pago" após compensação financeira;

6\. Envio do comprovante de arrecadação para o solicitante por e-mail e área logada;

7\. Estorno automatizado em caso de solicitação de cancelamento dentro do prazo legal.



A conciliação ocorre diariamente, em horário noturno, e os registros são armazenados por cinco anos para fins de auditoria e transparência fiscal.



\## 5. Conformidade com a LGPD



A v2 incorpora medidas de governança de dados pessoais em conformidade com a Lei nº 13.709/2018 (LGPD):



\- \*\*Base legal\*\*: o tratamento é realizado com fundamento no cumprimento de obrigação legal e execução de política pública (art. 7º, incisos I e II).

\- \*\*Consentimento\*\*: o solicitante é informado sobre as finalidades de coleta, compartilhamento e prazo de retenção.

\- \*\*Minimização\*\*: somente dados estritamente necessários são coletados, conforme princípio da necessidade.

\- \*\*Segurança\*\*: criptografia em trânsito e em repouso, controle de acesso baseado em perfis (RBAC), autenticação multifator para servidores e auditoria contínua.

\- \*\*Direitos do titular\*\*: canal específico para acesso, correção, anonimização e eliminação de dados, com resposta no prazo legal.

\- \*\*Registro de operações\*\*: manutenção de Relatório de Impacto à Proteção de Dados Pessoais (RIPD) e Inventário de Dados Pessoais atualizado semestralmente.

\- \*\*Encarregado (DPO)\*\*: designação do responsável por atender titulares e autoridades de fiscalização.



\## 6. Acessibilidade (ICOM)



O sistema de emissão de passaporte adota as diretrizes do ICOM - Instrumento de Conformidade para Acessibilidade Digital do governo federal. As melhorias implementadas na v2 incluem:



\- Contraste adequado entre texto e fundo, conforme nível AA das WCAG 2.1;

\- Navegação total por teclado;

\- Leitores de tela compatíveis com WAI-ARIA;

\- Textos alternativos em imagens e ícones;

\- Mensagens de erro claras e orientativas;

\- Responsividade para dispositivos móveis;

\- Libras e facilidades de compreensão leitura para pessoas com deficiência intelectual.



A avaliação de acessibilidade é realizada a cada release por meio de testes automatizados e inspeção manual, com registro de não conformidades e plano de correção.



\## 7. Considerações Finais



A versão 2 do fluxo de Emissão de Passaporte apresenta ganhos significativos em robustez, rastreabilidade, conformidade legal e experiência do usuário. As integrações com TSE, Receita Federal, Exército, Interpol e Casa da Moeda estão operando de forma orquestrada, com controles de idempotência e tolerância a falhas. A arrecadação via GRU/Tesouro Nacional foi padronizada, permitindo conciliação automática e auditoria financeira. As práticas de proteção de dados e acessibilidade foram reforçadas, alinhando a solução às políticas de governo digital e ao atendimento cidadão de qualidade. Recomenda-se a continuidade de monitoramento, testes de segurança periódicos e atualização das integrações conforme evolução dos sistemas parceiros.

