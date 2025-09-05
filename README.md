# Desafio-CodeGirl-EC2
> Consolidar seus conhecimentos em gerenciamento de instâncias EC2 na AWS

---

![3codegirlEC2](https://github.com/user-attachments/assets/31fac287-9c06-4db5-a960-8219cb5452a7)


### Uma arquitetura AWS onde um usuário envia o arquivo para o servidor EC2, que o processa localmente no volume EBS, salvando metadados no RDS. 
***Fluxo do Envio de Arquivos***
1. Upload do Usuário:
O usuário envia um arquivo através da interface web da sua aplicação.

2. Recebimento pelo EC2:
A instância EC2, que hospeda o servidor web/aplicação, recebe o arquivo do usuário.

3. Processamento e Armazenamento:
O arquivo é salvo em um volume EBS anexado à instância EC2.

4. Salvar Metadados no RDS:
A aplicação salva informações/metadados sobre o arquivo no banco de dados RDS.

Acesso e Visualização:
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
---
> Diagrama da Arquitetura
---
![Dessafio CodeGirl EC2](https://github.com/user-attachments/assets/bf7d5175-292c-4161-82ed-9b13e8315084)

# O diagrama de duas arquiteturas para o Desafio CodeGirl EC2, cada uma em Zonas de Disponibilidade diferente. Ambas as arquiteturas utilizam uma VPC com um bloco de endereços IP 10.0.0.0/16.

## Zona de Disponibilidade A:

* EC2 A1 Instance: Essa instância está na sub-rede privada para segurança.

* Amazon RDS: Está na sub-rede privada para evitar acesso direto da internet e aumentar a segurança. A instância EC2 pode acessar e se comunicar com o banco de dados.

* EBS: É um volume de armazenamento em bloco que é obrigatoriamente anexado a uma instância EC2. Estão conectados na mesma sub-rede privada.

## Zona de Disponibilidade B:
***É um exemplo de uma arquitetura "serverless" ou orientada a eventos***
* EC2 A1 Instance: A instância está na sub-rede privada.

* Amazon S3 (Simple Storage Service): Mostra a comunicação entre a instância EC2 e o S3. O S3 é um serviço de armazenamento de objetos que é global e não está vinculado a uma VPC ou sub-rede específica, mas sim acessado por meio de endpoints.

* AWS Lambda: O Lambda é um serviço de computação serverless. A sua conexão com o S3 e o DynamoDB indica que o S3 pode acionar uma função Lambda, que por sua vez pode interagir com o DynamoDB.

* Amazon DynamoDB: É um banco de dados NoSQL totalmente gerenciado. É acessado através de endpoints.
***Aqui representa um fluxo onde o upload de um objeto para o S3 pode disparar (trigger) uma função Lambda, que por sua vez pode ler ou escrever dados no banco de dados NoSQL DynamoDB.***
