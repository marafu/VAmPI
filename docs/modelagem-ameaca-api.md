#  Modelagem de Ameaças
Essa modelagem de API se baseou no entendimento da arquitetura do item 12, se basou no modelo STRIDE porém com uma modelagem livre.


## Identificação dos ativos
- AuthN User
- AuthZ Transaction
- Perfom Transaction
- Audit Log
- Account
- Police

## Entendimento de cada processo
- **AuthN User**: processo de autenticação de usuário, no contexto de api, pode ser um sistema de login stateless.
- **AuthZ Transaction**: Este processo parece ser o processo de autorização de uma transação, e como está sendo integrado com o **Policies** e **Bad Account**, esse processo também serve como um Open Policy Agent (OPA), definindo de forma centralizada se uma determinada solicitação de acesso deve ser permitida ou negada com base nas políticas definidas.
- **Perform Transaction**: É o processo transação em si, após a autenticação e autorização serem bem-sucedidas.
- **Audit Log**: Processe de registrar a transação ou evento no log de auditoria para monitoramento futuro.
- **Police**: Este processo sugere uma possível notificação ou interação com as autoridades financeiras no caso de detecção de atividades suspeitas ou fraudulentas.
- **Account**: Indica a conta do usuário.
- **Policies**: Indica ser um repositório de políticas de transação.
- **Bad accounts**: Indica ser um repositórios de contas fraudulentas.

## Entendimento das fronteiras externas
- **Tellers Counter**: Essa fronteira aparenta ser o ponto de acesso externo  de comunicação com um caixa eletronico, portanto, fora da rede corporativa da empresa. Porém, em um contexto de API pode ser qualquer cliente que integre com a API. 
- **Police**: Essa fronteira aparenta ser um ponto de comunicação externo entre o sistema bancário e as autoridades externas. Em um contexto brasileiro pode ser entendido como o BACEN.

## Entendimento das linhas de conexão
- **Linha verde**: processos sistemicos esperados
- **Linha vermelha**: processo criticos, esperado atenção.
- **Linha amarela**: processo de auditoria.

## Divisão de escopos
### Escopo de Interface com o Cliente
- **Customer Interaction**: Interações diretas com os clientes. Caixa Eletronico e Internet Banking (Web/Mobile).

- **Withdrawal Request / Identification**: Processamento de solicitações de retirada e identificação do cliente.

### Escopo de Autenticação de usuário
- **AuthN User**: Sistema de verificação da identidade do cliente.

### Escopo de Autorização de transação
- **AuthZ Transaction**: Sistema de autorização de transações bancárias.
- **Withdrawal request / Identity**: Conjunto de dados da conta e transação para ser enviado para o **Perform Transaction**.
- **Withdrawal request / Identity / AuthZ Result**: Conjunto de dados que serão registrados no **Audit Log**.

### Escopo de Processamento de Transações
- **Perform Transaction**: Mecanismo central para processamento de transações.
- **Account balance**: Saldo da conta pré-transação.
- **New Balance**: Atualização do saldo da conta pós-transação.
- **Transaction details**: Conjunto de dados que serão registrados no **Audit Log**.

### Escopo de Registro e Auditoria
- **Audit Log**: Registro de transações e atividades para auditoria e conformidade. 
- **Withdrawal request / Identity / AuthZ result**: Conjunto de dados que serão registrados no **Audit Log**.

### Escopo de Gestão de Políticas e Conformidade
- **Withdrawal Policy**: Regras específicas para gestão de retiradas.
- **Policies**: Conjunto de políticas de operação do sistema bancário.

### Escopo de Integração com Entidades Externas
- **Police**: Comunicação com autoridades externas e órgãos reguladores.
- **Identity / Location / etc**: Conjunto de dados que serão repassados para a entidade externa após o processo de transação

### Escopo de Segurança e Monitoramento
- **Bad Account Numbers / Bad Accounts**: Detecção e gestão de contas problemáticas.
------------
# Identificação de Ameaças por escopo
### Escopo de Autenticação (AuthN User)
- **Roubo de Credenciais**: Captura não autorizada de credenciais de autenticação, como nomes de usuário e senhas.
- **Ataques de Força Bruta**: Tentativas repetidas e automáticas de adivinhar credenciais de autenticação através de tentativas sistemáticas ou uso de listas de palavras comuns para tentar adivinhar senhas de usuários.
- **Roubo de Token de Autenticação**: Interceptação ou obtenção indevida de tokens de autenticação válidos para acessar recursos protegidos.
- **Vulnerabilidades de Autenticação Multifator**: Exploração de falhas em sistemas de autenticação multifator para comprometer a segurança.
- **Quebra de Protocolos de Autenticação**: Exploração de vulnerabilidades em protocolos de autenticação para contornar mecanismos de segurança.
- **Politica de senha fraca**: Utilização de senhas com baixa entropia, possibilitando a identificação rapida da senha.
- **Processo de reset de senha inseguro**: Exploração de falhas no processo de reset de senha para o acesso não autorizado de terceiros. 
- **Vazamento de Credenciais**: Exposição não autorizada de credenciais de autenticação devido a falhas de segurança ou má configuração.
- **Invasão sistêmica do processo de autenticação ou autorização**: Possibilidade de invasão e posse da maquina provisionada para o sistema de autenticação ou autorização. Por falta de validação de input ou configuração de segurança. 

### Escopo de autorização de transação (AuthZ Transaction)
- **Roubo de Credenciais de Acesso**: Atacantes podem obter acesso não autorizado às credenciais de usuários ou sistemas privilegiados, permitindo que eles realizem transações fraudulentas.

- **Alteração de Dados em Trânsito**: Atacantes podem interceptar e modificar dados durante a transmissão entre o cliente e o servidor, alterando o conteúdo da transação para benefício próprio.

- **Ataques de Injeção de Dados**: Atacantes podem injetar comandos maliciosos ou dados falsificados nas solicitações de transação, explorando vulnerabilidades como injeção (SQL, NoSQL, etc) para manipular o comportamento do sistema.

- **Acesso Não Autorizado a Recursos Sensíveis**: Atacantes podem explorar falhas na autorização para acessar recursos ou funcionalidades sensíveis que não deveriam estar disponíveis para eles, permitindo manipulação indevida de dados ou execução de transações não autorizadas.

- **Elevação de Privilégios**: Atacantes podem tentar elevar seus privilégios dentro do sistema, obtendo acesso a funcionalidades ou recursos que normalmente estão além de suas permissões autorizadas.

- **Fraude de Identidade**: Atacantes podem usar informações pessoais roubadas ou falsificadas para se passar por usuários legítimos e realizar transações fraudulentas em seus nomes.

- **Negligência de Controle de Acesso**: Falhas na implementação de controles de acesso adequados podem resultar em acesso não autorizado a funcionalidades ou recursos do sistema, permitindo a realização de transações não autorizadas.

- **Ataques de Negação de Serviço (DoS)**: Atacantes podem sobrecarregar o sistema com solicitações maliciosas, tornando-o inacessível para usuários legítimos ou interrompendo o processamento de transações normais.

- **Manipulação de Transações em Trânsito**: Atacantes podem manipular dados de transações durante a transmissão para modificar os detalhes da transação, como valor ou destinatário bloqueadas (bad accounts), para benefício próprio.

### Escopo de Processamento de Transações (Perform Transaction)
- **Manipulação de Dados de Transação**: Alteração indevida dos dados de transação para beneficiar o atacante.
- **Ataques de Replay**: Repetição de transações legítimas capturadas anteriormente para obter ganhos financeiros indevidos.
- **Injeção de Dados Maliciosos**: Inserção de dados falsos ou maliciosos no fluxo de transações para comprometer a integridade do sistema.
- **Ataques de Intercepção de Transações**: Captura não autorizada de transações em trânsito para roubo de informações sensíveis.
- **Negligência de Validação de Dados**: Falha na validação adequada dos dados de entrada das transações, permitindo a exploração de vulnerabilidades.
- **Ataques de Repudiação**: Negação indevida de uma transação legítima por parte do usuário ou do sistema.
- **Manipulação de Fluxo de Fundos**: Desvio ilegal de fundos durante o processamento de transações para contas controladas pelo atacante.
- **Ataques de Exaustão de Recursos**: Sobrecarga intencional do sistema de processamento de transações para interromper o serviço ou causar indisponibilidade.
- **Falta de Controles de Autenticação e Autorização**: Ausência ou insuficiência de mecanismos de autenticação e autorização para proteger o processamento de transações.
- **Ataques de Injeção de SQL ou NoSQL**: Exploração de vulnerabilidades de injeção de SQL ou NoSQL para manipular transações e acessar dados sensíveis.
- **Ataques de Enumeração de Transações**: Identificação de transações legítimas através de tentativas sistemáticas de enumeração.

### Escopo de Registro e Auditoria
- **Manipulação de Logs**: Alteração indevida dos registros de auditoria para encobrir atividades maliciosas.
- **Negligência de Log**: Falha na geração ou retenção adequada de logs, resultando na perda de evidências de segurança.
- **Vazamento de Informações Sensíveis nos Logs**: Inclusão não intencional de informações confidenciais nos registros de auditoria.
- **Falsificação de Identidade nos Logs**: Impersonação de usuários legítimos nos registros de auditoria para realizar ações não autorizadas. Caso algum dos processos que integram com o registro de log sejam invadido.
- **Falta de Integridade dos Logs**: Alteração não autorizada dos registros de auditoria, comprometendo a integridade das evidências. Caso algum dos processos que integram com o registro de log sejam invadido.
- **Ataques de Escalação de Privilégios nos Logs**: Manipulação dos registros de auditoria para obter acesso a níveis mais elevados de privilégios. Caso algum dos processos que integram com o registro de log sejam invadido.
- **Falta de Monitoramento de Log em Tempo Real**: Falha na detecção imediata de eventos de segurança significativos nos logs de auditoria.

### Escopo de Gestão de Políticas e Conformidade
- **Violação de Políticas de Segurança**: Descumprimento das políticas e diretrizes de segurança estabelecidas pela organização.
- **Falta de Conformidade Regulatória**: Não atendimento aos requisitos legais e regulamentares aplicáveis ​​ao setor financeiro.
- **Ataques de Engenharia Social contra Funcionários**: Manipulação de funcionários para contornar políticas de segurança ou divulgar informações sensíveis.
- **Falta de Auditoria de Conformidade**: Ausência de verificações regulares para garantir que as políticas de segurança estejam sendo seguidas conforme exigido.
- **Roubo de Informações de Conformidade**: Apropriação não autorizada de dados relacionados à conformidade regulatória para uso indevido.
- **Ataques de Falsificação de Documentos**: Criação ou alteração de documentos falsos para aparentar conformidade falsa com regulamentos.
- **Insider Threats relacionados à Conformidade**: Abuso de privilégios por parte de funcionários para contornar controles de conformidade.
- **Falta de Revisão de Políticas de Segurança**: Falha em revisar e atualizar regularmente as políticas de segurança para garantir sua eficácia contínua.
- **Falha na Manutenção de Registros de Conformidade**: Ausência de documentação adequada para comprovar a conformidade com as políticas e regulamentos.
- **Ataques de Phishing visando a Conformidade**: Envio de e-mails de phishing disfarçados como comunicações relacionadas à conformidade para enganar os usuários.
- **Violação de Acordos de Confidencialidade**: Divulgação não autorizada de informações confidenciais relacionadas à conformidade a partes não autorizadas.

### Escopo de Integração com Entidades Externas (Police)
- **Falta de Autenticação de Entidades Externas**: Falha na verificação adequada da identidade e autorização das entidades externas antes de permitir o acesso aos sistemas internos.
- **Ataques de Negação de Serviço**: Sobrecarga intencional dos sistemas por entidades externas para interromper o funcionamento normal ou negar o serviço aos usuários legítimos.
- **Vazamento de Dados Sensíveis**: Exposição não autorizada de informações sensíveis ou confidenciais devido a falhas na proteção de dados durante a integração com entidades externas.
- **Falta de Monitoramento de Segurança**: Ausência de monitoramento contínuo das atividades das entidades externas para detectar e responder a atividades maliciosas ou suspeitas.
- **Violação de Acordos de Nível de Serviço (SLA)**: Não conformidade com os acordos estabelecidos com entidades externas em relação à disponibilidade, desempenho e segurança dos serviços prestados.

--------
# Recomendações de mitigação

### Mitigações para Autenticação de Usuário (AuthN User):
- Utilizar um serviço de IAM para centralizar o armazenamento e a gestão de identidades de usuários, simplificando a administração e o controle de acesso em todo o ambiente de TI.
- Implementar autenticação multifator (MFA) por meio do serviço de IAM, exigindo que os usuários forneçam mais de um método de autenticação para verificar suas identidades.
- Criar políticas de senha robustas no serviço de IAM, incluindo requisitos de comprimento mínimo, uso de caracteres especiais e expiração regular de senhas, para mitigar ameaças relacionadas à fraqueza das senhas.
- Utilizar recursos de gerenciamento de sessão e single sign-on (SSO) do serviço de IAM para garantir que os usuários tenham acesso adequado aos recursos necessários após a autenticação inicial, reduzindo a sobrecarga de senhas e logins repetidos.
- Aproveite os recursos de auditoria e registro de eventos do serviço de IAM para monitorar e rastrear atividades de autenticação de usuários, facilitando a detecção e investigação de incidentes de segurança.
- Integrar o serviço de IAM com tecnologias de autenticação modernas, como biometria, autenticação baseada em token e autenticação sem senha, para oferecer opções de autenticação mais seguras e convenientes para os usuários.

### Mitigações para Autorização de Transações (AuthZ Transaction):
- Implementar validação rigorosa dos dados de entrada para prevenir ataques de injeção de código, como injeção de SQL.
- Implementar controles de acesso baseados em RBAC para garantir que os usuários tenham apenas as permissões necessárias para suas funções.
- Utilizar tokens de autorização de curta duração para reduzir a janela de oportunidade para ataques de interceptação.
- Criptografar dados sensíveis durante o transporte e armazenamento para protegê-los contra acesso não autorizado.
- Implementar mecanismos de revisão e aprovação para transações de alto valor ou sensíveis.
- Monitorar e auditar atividades de autorização para detectar e responder a comportamentos incomuns ou maliciosos.
- Integrar o AuthZ em um motor de fraude, para a identificação de comportamentos anormais em contas.
- Criar politicas de integração bem definidas.

### Mitigações para Processamento de Transações (Perform Transaction):
- Implementar validação rigorosa dos dados de entrada para prevenir ataques de injeção de código, como injeção de SQL.
- Utilizar tokenização com expiração para a transação bancária. 
- Implementar um motor de fraude para a identificação de comportamento anormal nas contas, como por exemplo transações com quantias altas de dinheiro para contas terceiros, transações em horários anormais, etc.

### Mitigações para Registro e Auditoria (Audit Log):
- Implementar validação rigorosa dos dados de entrada para prevenir ataques de injeção de log.
- Definir políticas claras para retenção de logs e certifique-se de que os registros de auditoria sejam mantidos por um período adequado de tempo para fins de conformidade e investigação de incidentes.
- Implementar regras nos logs para o sistemas de monitoramento em tempo real para analisar e alertar sobre atividades incomuns ou suspeitas registradas nos logs de auditoria, verificar a possibilidade de integração com SIEM.
- Criptografar os registros de auditoria durante o transporte e armazenamento para protegê-los contra acesso não autorizado ou adulteração.
- Implementar controles de acesso rigorosos para garantir que apenas usuários autorizados possam acessar e modificar os registros de auditoria.
- Registrar eventos críticos, como tentativas de acesso não autorizado, modificações de configuração e atividades privilegiadas, sejam registrados e monitorados adequadamente.

### Mitigações para Integração com Entidades Externas (Police):
- Implementar mTLS para estabelecer uma conexão segura entre a API interna e as entidades externas de forma mútua, garantindo que ambas as partes se autentiquem e comuniquem de forma criptografada. O mTLS pode ser usado como uma camada a mais de autenticação.
- Implementar um serviço de mensageria como Apache Kafka para a que não perca nenhum dado de transação caso o sistema da entidade externa esteja indisponível. 
- Criptografe mensagens sensíveis antes de enviá-las para as entidades externas por meio da sua API interna para proteger o conteúdo contra acesso não autorizado durante a transmissão.
- Implementar políticas de rotação regular de chaves (API Keys) para manter a segurança contínua das comunicações entre a API interna e a entidade externa.