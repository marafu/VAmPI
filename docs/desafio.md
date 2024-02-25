#

## Parte 1: Conceitos e práticas
### 1. Descreva um processo de revisão de código seguro e por que é essencial para a Segurança de Aplicações.

Normalmente a primeira camada de revisão de código se dá por meio de uma ferramenta automatizada dentro do processo de desenvolvimento seguro SSDLC, porém podemos revisar manualmente de acordo com uma vulnerabilidade encontrada por um SAST, isso garante legitmidade além de conseguir identificar e remover falsos-positivos encontrados pela ferramenta, também podemos analisar manualmente a ausência de controles de segurança ou analisar o código para identificar algumas vulnerabilidade no dominio do projeto (algum use case inseguro que pode possibilitar um ataque).

Caso haja alguma vulnerabilidade no código-fonte encontrado pelo SAST, nós podemos validar a logica de programação, realizando simulação teorica e aplicação de prova de conceito caso haja disponibilidade. Dependendo da parte do código podemos realizar até mesmo um teste unitário que valide a existencia da vulnerabilidade, podendo ser utilizado em um teste regressivo juntamente com QA.

Como a questão pede para eu descrever um processo vou focar dominio do projeto (use cases e entities), porque são as partes mais dificeis de ser identificado alguma vulnerabilidade utilizando um SAST.

#### Processo de code review
1. Validação da Lógica de Negócios: Verificar pontos de segurança como o isolamento do lógico das entidades em relação aos casos de uso, por exemplo, o use case não pode ter visão de como é criado o calculo de juros da entidade emprestimo, isso é uma violação grave de segurança pois caso um atacante consiga ter acesso à esse método, o mesmo poderá modificar o calculo causando prejuizo. Então eu procuraria se o método de calcular o juros está privado dentro da classe emprestimo. Podendo ser criado testes unitários que consiga identificar esse tipo de vulnerabilidade e sendo utilizado em um teste regressivo.

2. Validação de controle de segurança: Podemos verificar se o código contém controles de segurança como validação de input, utilização de logs entre outros processos. Podemos usar tanto a modelagem de ameaças como o OWASP Proactive Controls.

3. Documentação de Descobertas: Documentar todas as vulnerabilidades descobertas relacionadas à lógica de negócios na ferramenta de gestão de vulnerabilidade, incluindo quaisquer melhorias propostas ou correções necessárias relacionadas a segurança como recomendação de correção, seguindo o SLA do fluxo de gestão de vulnerabilidades. (Deixando claro que o foco é no código-fonte ao invés do processo, pois a responsabilidade de segurança do processo é do time de fraudes, por exemplo)

4. Apoio técnico para o time de desenvolvimento: Caso necessite do apoio técnico para correção dessas vulnerabilidades. Esse apoio deve incluir explicações claras sobre os riscos potenciais e sugestões para mitigação do risco de segurança relacionado ao código-fonte.

#### Importância da Revisão de Código Seguro
A importância da revisão de código seguro (code review), é crucial para identificar vulnerabilidades que normalmente os outros processos não identificam, por exemplo, pentest e ferramentas SASTs. Isso permite melhorar a qualidade do código, garantir conformidade com padrões de segurança, fortalecer a cultura de segurança, reduzir riscos e custos a longo prazo, trazendo confiança para usuário final. 


### 2. Quais são os princípios básicos de uma arquitetura segura para um aplicativo web?
Depende de que arquitetura estamos falando, se tivermos falando sobre arquitetura de software, posso citar:
1. Segregação de componentes
2. Independencia de modulos externos (utilização de contratos/interfaces)
3. Validação de input e output (utilização de DTOs)
4. Segregação de camadas (Segregar infra, app e domain)
5. Implementação de logs nos use cases

Porém se falarmos de arquitetura de infra, posso citar:
1. Utilização de load balancer para o isolamento do servidor de aplicação na internet
2. Utilização de WAF para bloqueio de tentativas de ataque
3. Integração com OAUTH segregando a credencial do usuário do aplicativo web 
4. Integração com SIEM e SOAR para resposta a incidente
5. Integração com APM para observabilidade

### 3. Quais são as melhores práticas para autenticação e controle de acesso em um aplicativo web?
#### Autenticação:
1. Senhas Fortes
2. Autenticação de Dois Fatores
3. Proteção contra Força Bruta
4. Bloqueio da conta por tentativas erradas
5. Notificação para o usuário da tentativa de login 

#### Controle de Acesso:
1. Single Sign-On (SSO)
3. OAuth para Delegação de Acesso
4. JWT para Tokens de Acesso (Stateless)
5. Monitoramento e Logging
6. Politicas de autorização (OPA)


### 4. Quais são os riscos associados ao uso de bibliotecas e componentes de terceiros em um aplicativo web? Como você mitigaria esses riscos?
Bibliotecas podem conter vulnerabilidades que podem comprometer completamente a aplicação que utiliza ela. Para mitigar o risco é necessário que o código seja feito com baixo acoplamento e dependencia de bibliotecas, para caso seja necessário a troca não impossibilite a mudança. Existe algumas ferramentas que identificam as vulnerabilidades como, Jfrog Xray ou Github Dependabot, mas a mitigação seria ou trocar a versão da lib vulnerável ou implementar desenvolver a lib como módulo próprio. Por isso é necessário design patterns como adapter para ajudar o baixo acoplamento dessas libs.  

## Parte 2: Segurança de Código
### 5. Quais os principais projetos da OWASP para ajudar desenvolvedores a escrever código seguro?
 - OWASP Top Ten
 - OWASP Cheat Sheet Series
 - OWASP Proactive Controls

### 6. Como você integraria as práticas do SSDLC em um ambiente de desenvolvimento ágil? Descreva os principais desafios e como você os superaria.

Eu integraria as praticas do SSDLC conforme a imagem abaixo:

[Fluxo do SSDLC](./img/SSDLC.png)

Detalharei todos os pontos relevantes para o processo de desenvolvimento ágil.

####  Concepção do projeto:
- SD Elements para Modelagem de Ameaças
- OWASP Risk Rating Calculator para classificação de risco do projeto
#### Desenvolvimento seguro:
- Code Review manual
- Extensão do SAST para verificação em tempo de desenvolvimento via IDE

#### Versionamento seguro:
- Git Hooks para commit seguro (evitar principalmente Hard-Coded Credentials)
- Segregação de branchs utilizando git flow (facilita a remoção da branch vulnerável no SVC)

#### Pipeline seguro:
- Checkov como IAC Scan
- Fortify ou Vera Code como SAST Scan
- OWASP Dependency Check como SCA Scan
- Projeto cross com o time de QA para realização de testes regressivos

#### Artifactory seguro:
- Trivy para escanear os containers
- Cosign para assinar as imagens

#### Ambiente de Homologação:
- Realização de Pentest no ambiente 
- Realização de testes dinamicos DAST 

#### Gerenciamento de vulnerabilidade:
- Defect Dojo para centralizar todos os insumos (vulnerabilidades, testes, relatórios)
- Grafana para gerar KPIs da área
- Integração com sistema de gerenciamento de projeto   

Os principais desafios para incluir todo esse processo, além do custo financeiro das aquisições das ferramentas, seria a automatizar todo o processo e realizar a sustentação de todo o processo. Dependendo da demanda o Defect Dojo perderia desempenho, então poderia ter que mudar a arquitetura do Defect Dojo para alcançar o objetivo. 

Para superar esses problemas precisaria de desenvolver automação para a inclusão dos resultados para o defectdojo principalmente das ferramentas open sources, pois as mesmas não enviariam o resultado dos scan de forma nativa então por exemplo, teria que ser criado uma imagem docker personalizada juntamente com o script de automação que enviasse o resultado para o engagement do projeto relacionado.

Também outro desafio seria o tamanho do time que realizaria a sustentação, seria um time muito grande, talvez 15 à 20 profissionais para sustentar todo o processo.

## Parte 3: Teste de Segurança
### 7. Como você lidaria com a descoberta e correção de vulnerabilidades de segurança em um aplicativo legado? Quais seriam os passos envolvidos nesse processo?

Incluiria a vulnerabilidade no sistema de gerenciamento de vulnerabilidades para seguir o fluxo de vulnerabilidade, classificaria a vulnerabilidade para definir o SLA de correção, procuraria o tech lead do projeto (caso exista) e explicaria a vulnerabilidade e correção. Caso não existisse um dono levaria para o head da área esse ponto para definir um dono. Caso persista sem dono, escalaria o assunto pedindo a permissão para a realização da correção, caso fosse aprovado, realizaria uma análise de impacto para avaliar como as correções afetarão o aplicativo. Caso o impacto da vulnerabilidade fosse maior que o impacto de correção, realizaria a correção da vulnerabilidaade e acionaria o time de QA para incluir um teste regressivo de acordo com a vulnerabilidade e contexto encontrado. Realizaria diversos testes em ambiente não produtivo até que estivesse claro que a correção não afetaria o aplicativo em ambiente produtivo. Após isso fecharia a vulnerabilidade finalizando o processo de gestão de vulnerabilidade.

### 8. Descreva as principais ameaças que as aplicações podem enfrentar num contexto web/api.
1. **IDOR (Insecure Direct Object References):**
   - *Descrição:* Um atacante tenta acessar ou modificar recursos diretamente, explorando falhas na implementação do controle de acesso, sem a devida autorização.

2. **SQL Injection:**
   - *Descrição:* Injeção de código SQL malicioso através de inputs da aplicação, permitindo que um atacante manipule o banco de dados, execute consultas não autorizadas ou exponha dados sensíveis.

3. **DDoS (Distributed Denial of Service):**
   - *Descrição:* Um ataque que visa sobrecarregar um serviço, aplicação ou rede com tráfego excessivo, tornando-o inacessível para usuários legítimos.

4. **Deserialização de Objeto:**
   - *Descrição:* Ataques que exploram falhas na deserialização de objetos, permitindo que um invasor manipule objetos serializados para executar código arbitrário.

5. **SSRF (Server Side Request Forgery):**
   - *Descrição:* Um ataque no qual um invasor induz o servidor a fazer requisições a recursos internos, podendo explorar vulnerabilidades de rede ou acessar informações sensíveis.

6. **Vazamento de Dados:**
   - *Descrição:* Divulgação não autorizada ou acidental de informações sensíveis, seja através de exposição inadequada de APIs, configurações incorretas ou outras falhas de segurança.

7. **Terceirização das Funcionalidades (Third-party Component):**
   - *Descrição:* Utilização de componentes ou serviços de terceiros sem a devida verificação de segurança, podendo resultar em vulnerabilidades ou dependência de serviços externos não confiáveis.

8. **XSS (Cross-Site Scripting):**
   - *Descrição:* Inserção de scripts maliciosos em páginas web, que são então executados no navegador dos usuários, permitindo a coleta de informações, roubo de sessões ou execução de ações indesejadas em nome do usuário.


## Parte 4: Segurança em DevOps
<<<<<<< HEAD
9. Como a segurança pode ser integrada em um pipeline de CI/CD? Descreva algumas práticas e ferramentas que podem ser usadas para garantir a segurança em cada estágio do ciclo de vida do desenvolvimento de software.

    Conforme detalhei na questão 6, podemos utilizar SAST para analizar o código-fonte em tempo de esteira, por exemplo o Fortify que analisa e identifica vulnerabilidades no código-fonte, porém, temos o Vera Code que realiza scans de SAST e SCA no seu projeto, identificando as vulnerabilidades do código-fonte e bibliotecas utilizadas. Temos também o trivy que analisa códigos IAC, podemos usar também o Clair (que recentemente foi incluido no docker hub) para analisar os containers, podemos assinar as nossas imagens docker com o COSIGN antes de enviar para o repositorio de imagens de container. 

10. Contêineres e Orquestração: Quais são as preocupações de segurança comuns ao usar contêineres (por exemplo, Docker) e orquestração (por exemplo, Kubernetes)?
Como você mitigaria essas preocupações?
 
    As preocupações de seguranças mais comuns de docker são as vulnerabilidades das imagens, que podem ser exploradas nos containers, injeção de container maliciosos nos namespaces das empresas o que permite realizar deploy da imagem vulnerável e ser passível de exploração (como o que aconteceu com a SolarWinds) e falta de hardenização e gestão de permissionamento do container (normalmente tudo é rodado como root).

    O trivy analisa tanto Dockerfile buscando vulnerabilidades e falta de configurações de segurança em arquivo Dockerfile e manifesto k8s, apesar da análise de k8s de ser uma feature experimental. Além disso, podemos assinar os containers com o cosign trazendo legitmidade nas nossas imagens, também conseguimos realizar o controle de imagens não assinadas no cluster kubernetes com algum policy engine como por exemplo o Kyverno que é um dos mais famosos policy engines de kubernetes, ele permite baixar somente as imagens que são assinadas no container registry utilizado. O Kubernetes é mais complexos e é preciso configurar muita coisa por exemplo Network Policy, controle de acesso com RBAC,Security Content.  
=======
### 9. Como a segurança pode ser integrada em um pipeline de CI/CD? Descreva algumas práticas e ferramentas que podem ser usadas para garantir a segurança em cada estágio do ciclo de vida do desenvolvimento de software.

Conforme detalhei na questão 6, podemos utilizar SAST para analizar o código-fonte em tempo de esteira, por exemplo o Fortify que analisa e identifica vulnerabilidades no código-fonte, porém, temos o Vera Code que realiza scans de SAST e SCA no seu projeto, identificando as vulnerabilidades do código-fonte e bibliotecas utilizadas. Temos também o trivy que analisa códigos IAC, podemos usar também o Clair (que recentemente foi incluido no docker hub) para analisar os containers, podemos assinar as nossas imagens docker com o COSIGN antes de enviar para o repositorio de imagens de container. 

### 10. Contêineres e Orquestração: Quais são as preocupações de segurança comuns ao usar contêineres (por exemplo, Docker) e orquestração (por exemplo, Kubernetes)?Como você mitigaria essas preocupações?
 
As preocupações de seguranças mais comuns de docker são as vulnerabilidades das imagens, que podem ser exploradas nos containers, injeção de container maliciosos nos namespaces das empresas o que permite realizar deploy da imagem vulnerável e ser passível de exploração (como o que aconteceu com a SolarWinds) e falta de hardenização e gestão de permissionamento do container (normalmente tudo é rodado como root).

O trivy analisa tanto Dockerfile buscando vulnerabilidades e falta de configurações de segurança em arquivo Dockerfile e manifesto k8s, apesar da análise de k8s de ser uma feature experimental. Além disso, podemos assinar os containers com o cosign trazendo legitmidade nas nossas imagens, também conseguimos realizar o controle de imagens não assinadas no cluster kubernetes com algum policy engine como por exemplo o Kyverno que é um dos mais famosos policy engines de kubernetes, ele permite baixar somente as imagens que são assinadas no container registry utilizado. O Kubernetes é mais complexos e é preciso configurar muita coisa por exemplo Network Policy, controle de acesso com RBAC,Security Content.  

### 11. Cenário de Ataque: Analise o cenário hipotético abaixo, onde a empresa foi comprometida através de um acesso inicial via phishing. Os candidatos devem explicar o fluxo do ataque e sugerir medidas de detecção, mitigação e prevenção.

O ataque foi iniciado por meio de um phishing personalizado, porém não dá para determinar como foi feito esse phishing, o fluxo do ataque segue com o download de um arquivo malicioso, execução do arquivo, a infecção da maquina, exclusão de arquivo, uma tentativa de acesso externo, listagem ou acesso das contas da maquina, execução da interface do usuário, execução de firmware da maquina provavelmente via kernel, acesso ao sistema de desligamente e reinicialização do sistema, limitou o registrição de acesso da máquina, além de acessar a politica de senha da maquina, por fim houve um ataque de negação de serviço.

Como medida de detecção pode ser usado um EDR para a detecção em tempo real do ataque, como mitigação um antivirus poderia ser usado para analisar o arquivo malicioso, caso o EDR não bloqueasse o email e como medida de prevenção um programa de conscientização do usuário pode ser realizado é o mais dificil porém o mais eficaz. 

>>>>>>> 5a55ad5 (add doc)
