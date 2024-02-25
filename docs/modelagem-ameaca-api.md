#  Modelagem de Ameaças

## Identificação dos ativos
- AuthN User
- AuthZ Transaction
- Perfom Transaction
- Audit Log
- Account
- Police


## Entendimento de cada processo
- **AuthN User**: processo de autenticação de usuário, no contexto de api, pode ser um sistema de login stateless.

- **AuthZ Transaction**: Este processo parece ser o processo de autorização de uma transação, e como está sendo integrado com o **Policies** e **Bad Account**, esse processo também serve como um motor de fraudes. 

- **Perform Transaction**: Este é o ato de realizar a transação em si, após a autenticação e autorização serem bem-sucedidas.

- **Audit Log**: Isso envolve registrar a transação ou evento no log de auditoria para monitoramento futuro e análise de segurança.

- **Police**: Este processo sugere uma possível notificação ou interação com as autoridades financeiras no caso de detecção de atividades suspeitas ou fraudulentas.

- **Account**: Isso indica a conta do usuário.

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

### Escopo de Autenticação e Autorização
- **AuthN User**: Sistema de verificação da identidade do cliente.
- **AuthZ Transaction**: Sistema de autorização de transações bancárias.

### Escopo de Processamento de Transações
- **Perform Transaction**: Mecanismo central para processamento de transações.
- **New Balance**: Atualização do saldo da conta pós-transação.

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

## Identificação de Ameaças por escopo
### Escopo de Autenticação e Autorização
- **Roubo de Credenciais**: Captura não autorizada de credenciais de autenticação, como nomes de usuário e senhas.
- **Ataques de Força Bruta**: Tentativas repetidas e automáticas de adivinhar credenciais de autenticação através de tentativas sistemáticas ou uso de listas de palavras comuns para tentar adivinhar senhas de usuários.
- **Roubo de Token de Autenticação**: Interceptação ou obtenção indevida de tokens de autenticação válidos para acessar recursos protegidos.
- **Vulnerabilidades de Autenticação Multifator**: Exploração de falhas em sistemas de autenticação multifator para comprometer a segurança.
- **Quebra de Protocolos de Autenticação**: Exploração de vulnerabilidades em protocolos de autenticação para contornar mecanismos de segurança.
- **Politica de senha fraca**: Utilização de senhas com baixa entropia, possibilitando a identificação rapida da senha.
- **Processo de reset de senha inseguro**: Exploração de falhas no processo de reset de senha para o acesso não autorizado de terceiros. 
- **Vazamento de Credenciais**: Exposição não autorizada de credenciais de autenticação devido a falhas de segurança ou má configuração.
- **Invasão sistêmica do processo de autenticação ou autorização**: Possibilidade de invasão e posse da maquina provisionada para o sistema de autenticação ou autorização. Por falta de validação de input ou configuração de segurança. 

### Escopo de Processamento de Transações
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

### Escopo de Integração com Entidades Externas
- **Falta de Autenticação de Entidades Externas**: Falha na verificação adequada da identidade e autorização das entidades externas antes de permitir o acesso aos sistemas internos.
- **Ataques de Negação de Serviço**: Sobrecarga intencional dos sistemas por entidades externas para interromper o funcionamento normal ou negar o serviço aos usuários legítimos.
- **Vazamento de Dados Sensíveis**: Exposição não autorizada de informações sensíveis ou confidenciais devido a falhas na proteção de dados durante a integração com entidades externas.
- **Falta de Monitoramento de Segurança**: Ausência de monitoramento contínuo das atividades das entidades externas para detectar e responder a atividades maliciosas ou suspeitas.
- **Violação de Acordos de Nível de Serviço (SLA)**: Não conformidade com os acordos estabelecidos com entidades externas em relação à disponibilidade, desempenho e segurança dos serviços prestados.
