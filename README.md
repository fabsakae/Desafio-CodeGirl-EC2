# Desafio-CodeGirl-EC2
Consolidar seus conhecimentos em gerenciamento de instâncias EC2 na AWS
---

![desafioec2](https://github.com/user-attachments/assets/5c088f23-4378-4901-a5da-98da5ad7e9e4)

---
![codegirlS3](https://github.com/user-attachments/assets/b27dc294-7335-4c65-b8e6-cd8468ea4a7f)
### Uma arquitetura AWS com o Amazon S3 como um serviço de armazenamento de arquivos onde o AWS Lambda é acionado por um evento do S3, como o upload de um novo arquivo. O Lambda, por sua vez, processa esse arquivo, extrai metadados e os grava em uma tabela do Amazon DynamoDB que é banco de dados NoSQL de alta performance, para persistência e consulta. 
***Como Funciona a arquitetura da atividade - Desafio de projeto:***

1. Armazenamento de Arquivos (S3):
Os objetos, como imagens ou documentos, são carregados para um bucket do Amazon S3. 

2. Acionamento do Lambda (S3 -> Lambda):
Um evento de "upload de objeto" é gerado no bucket S3, configurado para acionar uma função do AWS Lambda.

3. Processamento do Arquivo (Lambda):
A função Lambda é executada e, dependendo da aplicação, pode realizar diversas tarefas:
* Extrair metadados: Obter informações relevantes do arquivo, como formato, tamanho ou conteúdo. 
* Transformação: Converter o arquivo para outro formato ou realizar validações. 
* Auditoria: Registrar detalhes sobre a operação de upload.

4. Persistência de Dados (Lambda -> DynamoDB):
Os metadados extraídos são, então, armazenados na tabela do Amazon DynamoDB.

### Benefícios desta Arquitetura
* Sem Servidor (Serverless):
Você não precisa gerenciar infraestrutura, a AWS cuida da execução das funções e da infraestrutura do S3 e DynamoDB. 
* Escalabilidade:
O Lambda escala automaticamente com base na demanda, e o DynamoDB é otimizado para alta performance e escalabilidade. 
* Orientada a Eventos:
A arquitetura reage a eventos do S3, tornando-a eficiente para processar dados em tempo real à medida que são carregados. 
* Flexibilidade:
O Lambda permite que você escreva código para realizar diversas tarefas complexas em resposta ao upload de arquivos. 
* Custo-efetividade:
Paga-se apenas pelo tempo de computação utilizado pelo Lambda e pelo armazenamento no S3 e DynamoDB, sem custo por capacidade ociosa. 
