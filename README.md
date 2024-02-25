# VAmPI

Eu utilizei o Horusec como SAST pelo motivo de ser é opensource, apesar dele tecnicamente não ser um SAST mas um orquestrador de SASTs ele cumpre o papel de identificar as vulnerabilidades no código.

Eu utilizei o Nuclei como DAST apesar de não gostar da ideia de utilizar o SAST como step na pipeline, o motivo de utilizar ele é o mesmo do SAST. 

Eu realizei o teste completo do DAST na própria Pipeline, apesar de não ser recomendável, porém eu fiz dessa forma para armazenar o resultado e apresentado no próprio github já que é uma aplicação de teste e opensource. 

Todas as integrações foram feitas via docker pelas imagens oficiais das ferramentas. Para rodar o DAST na pipeline eu tive que rodar o container da aplicação via docker compose. 

Todos os steps da pipeline estão localizados no arquivo security-test.yml