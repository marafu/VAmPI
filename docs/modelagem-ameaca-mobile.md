# Modelagem de Ameaça
Essa modelagem de ameaça foi baseada no modelo STRIDE, com o foco maior em mobile.

## Levantamento dos ativos
### Câmeras de Segurança
- **Câmeras físicas**
- **Firmware/Software das câmeras**
- **Dados de vídeo e configurações das câmeras**

### DVR (Gravador de Vídeo Digital)
- **Dispositivo DVR**
- **Software e sistema operacional do DVR**
- **Vídeos e configurações armazenados no DVR**
- **Configurações de rede do DVR**

### Aplicativos Móveis (iOS e Android)
- **Código-fonte e binários dos aplicativos**
- **Dados armazenados localmente nos dispositivos móveis**

### Usuário
- **Informações pessoais e credenciais do usuário**
- **Acesso e controle do sistema pelo usuário**

### Conexões de Rede
- **Transmissão de dados RTSP das câmeras para o DVR**
- **Comunicação segura entre o DVR e o servidor STUN**
- **Comunicação entre os aplicativos móveis e o ambiente de nuvem**

### Ambiente de Nuvem do Fornecedor
- **Servidores e serviços do ambiente de nuvem**
- **Software de aplicação SaaS e ambiente de execução**
- **Dados armazenados na nuvem**

### Servidor STUN
- **Serviço STUN**
- **Informações de configuração e estado do servidor STUN**

## Ameaças por Ativo
### Câmeras de Segurança
- **Acesso público as câmeras**
- **Interceptação de vídeo (sniffing)**
- **Adulteração de firmware/software**
- **Desabilitação física ou remota das câmeras**

### DVR (Gravador de Vídeo Digital)
- **Invasão do sistema operacional do DVR**
- **Roubo ou exclusão de vídeos armazenados**
- **Comprometimento das configurações de rede**

### Aplicativos Móveis (iOS e Android)
- **Vazamento de credenciais armazenadas**
- **Interceptação de comunicações não seguras (bypass SSL Pinning)**
- **Injeção de código malicioso através de vulnerabilidades**
- **Engenharia reversa dos aplicativos**
- **Vazamento de informações sensíveis no log do aplicativo**

### Usuário
- **Phishing para obter credenciais do usuário**
- **Engenharia social para manipular usuários a realizar ações não seguras**
- **Perda ou roubo de dispositivos com acesso ao sistema**

### Conexões de Rede
- **Ataques Man-in-the-Middle (MitM)**
- **Ataques de negação de serviço (DoS ou DDoS)**
- **Comprometimento da integridade dos dados transmitidos**

### Servidor STUN
- **Ataques que exploram vulnerabilidades no protocolo STUN**
- **Uso indevido do servidor STUN para realizar ataques de reflexão**

### Rede e Infraestrutura de Comunicação
- **Serviço exposto na internet sem configuração de segurança**

## Recomendações de Controles de Segurança 
### Câmeras de Segurança
- **Implementação de autenticação com MFA**
- **Transmissão de vídeo criptografada**

### DVR (Gravador de Vídeo Digital)
- **Backup regular dos vídeos armazenados**

### Aplicativos Móveis (iOS e Android)
- **Armazenamento seguro de credenciais utilizando mecanismos nativos do sistema operacional**
- **Comunicação segura através de HTTPS e pinagem de certificados**
- **Validação de entrada para prevenir injeção de código**
- **Implementação de detecção de jailbreak/rooting/virtualização**
- **Implementação de serviço de Mobile Device Fingerprint (tokenização do dispositivel móvel)**
- **Token de sessão expirável**
- **Ofuscador de código (DexGuard)**
- **Desabilitar debug mode**
-  **Implementação de antibot**
- **Sanitização de log**

### Conexões de Rede
- **Implementação de TLS para proteger a integridade e confidencialidade dos dados**
- **Redundância de rede e mitigação de DDoS**

### Servidor STUN
- **Monitoramento e análise de logs para atividades suspeitas**
- **Implementação de rate limiting para prevenir abusos**
