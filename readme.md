# 🚀 Automação Serverless na Prática: S3 + AWS Lambda com Java!

E aí, dev! 👋 

Este repositório é a entrega do meu desafio de projeto no bootcamp da **DIO**. O objetivo aqui foi montar uma automação *serverless* completa na AWS: no momento em que um arquivo faz *land* num bucket do **Amazon S3**, um evento é disparado automaticamente para executar uma **Lambda em Java**, processando as informações e registrando tudo nos logs do **CloudWatch**.

Tudo isso rodando de forma 100% automatizada e sem precisar gerenciar servidores! 😎

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

* **Java 21**: A linguagem utilizada para escrever o manipulador de eventos da Lambda.
* **Apache Maven**: Responsável pelo gerenciamento de dependências e build do pacote executável (`.jar`).
* **AWS CloudShell**: O terminal direto do navegador da AWS usado para compilar o código sem complicação local.
* **Amazon S3**: Nosso armazenamento de objetos e gatilho inicial do fluxo.
* **AWS Lambda**: O cérebro *serverless* que roda o código Java sob demanda.
* **AWS IAM**: Gerenciamento de papéis (*roles*) e políticas de segurança.
* **Amazon CloudWatch**: Onde acompanhamos os logs da execução e validamos a automação.

---

## 🔍 Passo a Passo: Como o Projeto foi Construído

Todo o processo foi feito direto no ecossistema da AWS! Dá uma olhada em como as peças foram se encaixando:

### 1️⃣ Preparando o Código Java (via AWS CloudShell)
* Abri o **AWS CloudShell** e instalei o Maven (`sudo dnf install -y maven`).
* Criei o arquivo `pom.xml` com as dependências do SDK da AWS (`aws-lambda-java-core` e `aws-lambda-java-events`).
* Escrevi a classe Java `S3EventHandler.java` que lê os metadados do arquivo enviado (nome do bucket, nome do arquivo, tamanho) e grava no log.
* Rodei o `mvn clean package` e gerei o artefato `.jar` pronto para o deploy!

### 2️⃣ Configurando o Bucket no Amazon S3
* No console do **Amazon S3**, criei um bucket com configurações padrão de segurança para ser a nossa "origem" de dados.

### 3️⃣ Ajustando a Segurança no AWS IAM
* Criei uma *Role* de execução para a Lambda do tipo **AWS Service** selecionando o caso de uso **Lambda**.
* Anexei as políticas essenciais de segurança (Princípio do Menor Privilégio):
  * `AWSLambdaBasicExecutionRole` (para escrever logs no CloudWatch).
  * `AmazonS3ReadOnlyAccess` (para conseguir ler os arquivos que chegam no S3).

### 4️⃣ Criando e Subindo a Lambda Function
* No serviço **AWS Lambda**, criei uma nova função do zero usando a runtime do **Java 21** e associando a *Role* que criei no IAM.
* Atualizei o código enviando o arquivo `.jar` gerado na compilação.
* Ajustei o **Handler** nas *Runtime settings* para apontar exatamente para o caminho da classe Java:  
  `com.dio.lambda.S3EventHandler::handleRequest`

### 5️⃣ Conectando o Gatilho (Trigger S3 ➡️ Lambda)
* Nas propriedades da Lambda (ou nas *Event Notifications* do bucket S3), adicionei o gatilho apontando para o bucket criado.
* Defini o tipo de evento como **All object create events** (`s3:ObjectCreated:*`).

### 6️⃣ O Teste Final (A Mágica Acontece! 🪄)
* Subi um arquivo de teste no bucket S3.
* Fui no **CloudWatch Logs** da Lambda e... *BOOM!* 💥 Os logs estavam lá confirmando que a função capturou o evento e processou as informações do arquivo com sucesso!

---

## 📸 Evidências do Projeto

* `images/01-bucket-s3.png`: Bucket S3 pronto para receber arquivos.
* `images/02-iam-role.png`: Permissões da Role no IAM.
* `images/03-lambda-trigger.png`: Diagrama da Lambda conectada ao S3.
* `images/04-cloudwatch-logs.png`: Logs de execução confirmando a automação rodando!

---

💡 *Projeto desenvolvido durante o bootcamp da Digital Innovation One (DIO).*
💡 *Após a conclusão e registrado as evidências das atividades todos os recursos foram excluídos para evitar custos desnecessários*