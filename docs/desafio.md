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

### 3. Quais são as melhores práticas para autenticação e controle de acesso em um
aplicativo web?

### 4. Quais são os riscos associados ao uso de bibliotecas e componentes de terceiros
em um aplicativo web? Como você mitigaria esses riscos?

## Parte 2: Segurança de Código
5. Quais os principais projetos da OWASP para ajudar desenvolvedores a escrever
código seguro?
6. Como você integraria as práticas do SSDLC em um ambiente de desenvolvimentoTeste de Fluxo de Dados:
ágil? Descreva os principais desafios e como você os superaria.

## Parte 3: Teste de Segurança
7. Como você lidaria com a descoberta e correção de vulnerabilidades de segurança
em um aplicativo legado? Quais seriam os passos envolvidos nesse processo?
8. Descreva as principais ameaças que as aplicações podem enfrentar num contexto
web/api.

## Parte 4: Segurança em DevOps
9. Como a segurança pode ser integrada em um pipeline de CI/CD? Descreva algumas
práticas e ferramentas que podem ser usadas para garantir a segurança em cada
estágio do ciclo de vida do desenvolvimento de software.
10. Contêineres e Orquestração: Quais são as preocupações de segurança comuns ao
usar contêineres (por exemplo, Docker) e orquestração (por exemplo, Kubernetes)?
Como você mitigaria essas preocupações?