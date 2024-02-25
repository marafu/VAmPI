# Teste de Segurança para Aplicação
Este repositório contém os scripts e configurações necessários para realizar testes de segurança em uma aplicação utilizando o Horusec como ferramenta de Análise Estática de Segurança (SAST) e o Nuclei como ferramenta de Teste de Segurança Dinâmico (DAST).

## Configuração da Pipeline
Os testes de segurança são executados como parte de uma pipeline de integração contínua (CI) usando o GitHub Actions. O arquivo security-test.yml contém a configuração da pipeline, que pode ser ajustada conforme necessário para atender aos requisitos específicos do projeto.

## Ferramentas Utilizadas
- Horusec (SAST): O Horusec é utilizado para identificar vulnerabilidades no código-fonte da aplicação. Embora não seja um SAST tradicional, funciona como um orquestrador de SASTs, fornecendo uma análise abrangente da segurança do código.
- Nuclei (DAST): O Nuclei é empregado para realizar testes de segurança dinâmicos na aplicação. Apesar de certas considerações sobre o uso de um SAST como parte da pipeline, o Nuclei é escolhido pelas suas capacidades e adequação ao contexto.
Execução dos Testes
Os testes de segurança são executados de forma automatizada na pipeline de CI. Os resultados são armazenados e disponibilizados diretamente no GitHub para revisão e acompanhamento.

## Observações
Embora seja preferível realizar o teste DAST em um ambiente separado, devido a considerações práticas, optou-se por incluí-lo diretamente na pipeline.
Todas as integrações são realizadas por meio de contêineres Docker, utilizando as imagens oficiais das ferramentas.

## Contribuições
Contribuições são bem-vindas! Sinta-se à vontade para propor melhorias na configuração da pipeline, adicionar novas ferramentas de segurança ou sugerir ajustes no processo de testes.

