# Desafio-CodeGirl-EC2
> Consolidar seus conhecimentos em gerenciamento de instâncias EC2 na AWS
---

![1codegirlEC2](https://github.com/user-attachments/assets/1c54e021-1bd8-4013-9543-6ab5027c1dd0)


### Uma arquitetura AWS onde um usuário envia o arquivo para o servidor EC2, que o processa localmente no volume EBS, salvando metadados no RDS. 
***Fluxo do Envio de Arquivos***
1. Upload do Usuário:
O usuário envia um arquivo através da interface web da sua aplicação.

3. Recebimento pelo EC2:
A instância EC2, que hospeda o servidor web/aplicação, recebe o arquivo do usuário.

5. Processamento e Armazenamento:
Armazenamento em EBS (Alternativa): O arquivo é salvo em um volume EBS anexado à instância EC2.

7. Salvar Metadados no RDS:
A aplicação salva informações sobre o arquivo no banco de dados RDS.

9. Acesso e Visualização:
Quando o usuário precisa acessar o arquivo, a aplicação busca os metadados do RDS, recupera o arquivo do EBS e o disponibiliza para download ou visualização.

***Componentes da Arquitetura***

* Amazon EC2: Uma máquina virtual na nuvem que executa o aplicativo. O servidor web (ou aplicação) roda aqui.
 
* Amazon EBS:Volumes de armazenamento em nível de bloco que funcionam como disco rígido para suas instâncias EC2. Armazena os dados que precisam de acesso persistente, como logs da aplicação, arquivos de configuração ou, em alguns casos, arquivos temporários do upload, garantindo que os dados não se percam quando a instância EC2 for reiniciada.

* Amazon RDS (Relational Database Service): Serviço de banco de dados relacional gerenciado. Fácil de configurar, operar e escalar bancos de dados como MySQL, PostgreSQL, etc. Armazena os dados estruturados do seu aplicativo, como informações sobre os usuários, o conteúdo dos arquivos enviados (metadados) e a relação entre eles. 
---

![2codegirlS3](https://github.com/user-attachments/assets/efbe28f9-27e6-45d9-b343-b60305d46d89)


### Uma arquitetura AWS com o Amazon S3 como um serviço de armazenamento de arquivos onde o AWS Lambda é acionado por um evento do S3, como o upload de um novo arquivo. 
O Lambda, por sua vez, processa esse arquivo, extrai metadados e os grava em uma tabela do Amazon DynamoDB que é banco de dados NoSQL de alta performance, para persistência e consulta. 

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
