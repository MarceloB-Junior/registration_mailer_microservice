# Registration Mailer Microservice

## Visão Geral

**Registration Mailer Microservice** é um conjunto de microserviços composto por dois componentes principais: o microserviço de Usuário e o microserviço de E-mail. Esses microserviços trabalham em conjunto para fornecer uma solução eficiente para o registro de usuários e o envio de notificações por e-mail.

O **microserviço de Usuário** é uma aplicação desenvolvida com Spring Boot que fornece uma interface para o registro de novos usuários. Ele permite que as empresas criem novos registros de usuários através de um endpoint específico (POST), garantindo que os dados sejam validados e que conflitos, como e-mails duplicados, sejam evitados.

O **microserviço de E-mail** se integra ao microserviço de Usuário para enviar notificações por e-mail. Ele é responsável pelo envio automático de e-mails de boas-vindas sempre que um novo usuário é registrado. Utilizando **RabbitMQ** para comunicação assíncrona, o microserviço garante que as mensagens sejam enviadas de forma eficiente e desacoplada.

Juntos, esses microserviços oferecem uma solução robusta e escalável para empresas que precisam gerenciar seus usuários e suas comunicações de forma eficiente. Com uma arquitetura bem estruturada, eles podem ser facilmente integrados a outras aplicações e serviços.

### Principais Funcionalidades

- **Validação de Dados**: Utiliza a validação Bean do Jakarta para garantir que os dados inseridos estejam corretos, como campos obrigatórios e formatos válidos.
- **Verificação de Conflitos**: Implementa lógica para verificar se um usuário com o mesmo e-mail já está em uso antes da criação, evitando duplicações indesejadas.
- **Envio Automático de E-mails**: O microserviço de E-mail envia automaticamente um e-mail de boas-vindas após o registro de um novo usuário.
- **Comunicação Assíncrona**: Utiliza RabbitMQ para enviar mensagens entre os microserviços, garantindo uma arquitetura desacoplada.

## Tabela de Conteúdos

- [Recursos](#recursos)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Como Começar](#como-começar)
  - [Pré-requisitos](#pré-requisitos)
  - [Instalação](#instalação)
  - [Executando o Microserviço](#executando-o-microserviço)
- [Endpoints](#endpoints)
- [Estrutura do Projeto](#estrutura-do-projeto)

## Recursos

- Registro e gerenciamento de usuários.
- Envio de e-mails de notificação após o registro do usuário.
- Comunicação assíncrona entre microserviços usando RabbitMQ.
- Persistência dos dados em um banco de dados MySQL.

## Tecnologias Utilizadas

- Java
- Spring Boot
- Spring Data JPA
- RabbitMQ
- MySQL
- Jackson
- Maven

## Como Começar

### Pré-requisitos

Antes de começar, você precisará ter instalado em sua máquina:

- Java 23 ou superior
- Maven
- MySQL
- RabbitMQ

### Instalação

1. Clone este repositório:
   ```bash
   git clone https://github.com/MarceloB-Junior/registration_mailer_microservice.git
   cd registration_mailer_microservice
   ```

2. Configure o banco de dados MySQL:
   - Crie dois bancos de dados: `ms_user` e `ms_email`.

3. Configure as credenciais do banco de dados e do RabbitMQ no arquivo `application.properties` de cada microserviço:
    
    **Microserviço de Usuário**
    ```application.properties
    spring.application.name=user
    server.port=8081
    
    spring.datasource.url=jdbc:mysql://localhost:3306/ms_user
    spring.datasource.username=seu_usuario
    spring.datasource.password=sua_senha
    spring.jpa.hibernate.ddl-auto=update
    
    spring.rabbitmq.addresses=amqps://usuario:senha@host/virtual_host
    broker.queue.email.name=default.email
    ```
    
    **Microserviço de E-mail**
    ```application.properties
    spring.application.name=email
    server.port=8082
    
    spring.datasource.url=jdbc:mysql://localhost:3306/ms_email
    spring.datasource.username=seu_usuario
    spring.datasource.password=sua_senha
    spring.jpa.hibernate.ddl-auto=update
    
    spring.rabbitmq.addresses=amqps://usuario:senha@host/virtual_host
    broker.queue.email.name=default.email
    
    spring.mail.host=smtp.gmail.com
    spring.mail.port=587
    spring.mail.username=seu_email
    spring.mail.password=sua_senha_app_do_email
    spring.mail.properties.mail.smtp.auth=true
    spring.mail.properties.mail.smtp.starttls.enable=true
    ```

### Executando o Microserviço

1. Navegue até a pasta do microserviço de Usuário:
   ```bash
   cd user
   ```

2. Execute o microserviço:
   ```bash
   mvn spring-boot:run
   ```

3. Em seguida, navegue até a pasta do microserviço de E-mail:
   ```bash
   cd ../email
   ```

4. Execute o microserviço:
   ```bash
   mvn spring-boot:run
   ```

## Endpoints

### Microserviço de Usuário

- **Registrar Usuário**
  - **Método**: POST
  - **URL**: `/users`
  - **Corpo**:
    ```json
    {
      "name": "Nome do Usuário",
      "email": "email@exemplo.com"
    }
    ```

### Microserviço de E-mail

Os endpoints para o microserviço de E-mail são internos e são acionados automaticamente quando um novo usuário é registrado.


## Estrutura do Projeto
```
registration_mailer_microservice/
│
├── user/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/
│   │   │   │       └── ms/
│   │   │   │           └── user/
│   │   │   │               ├── configs/
│   │   │   │               ├── controllers/
│   │   │   │               ├── dtos/
│   │   │   │               ├── models/
│   │   │   │               ├── producers/
│   │   │   │               ├── repositories/
│   │   │   │               └── services/
│   │   │   └── resources/
│   │   │       └── application.properties
│   │   └── test/
│   └── pom.xml
│
└── email/
    ├── src/
    │   ├── main/
    │   │   ├── java/
    │   │   │   └── com/
    │   │   │       └── ms/
    │   │   │           └── email/
    │   │   │               ├── configs/
    │   │   │               ├── consumers/
    │   │   │               ├── dtos/
    │   │   │               ├── enums/
    │   │   │               ├── models/
    │   │   │               ├── repositories/
    │   │   │               └── services/
    │   │   └── resources/
    │   │       └── application.properties
    │   └── test/
    └── pom.xml
```

## Agradecimentos

Meus agradecimentos à Michelli Brito, arquiteta de software e desenvolvedora Fullstack, por sua significativa contribuição ao meu entendimento sobre Spring Boot e microserviços. Michelli é uma referência na comunidade de desenvolvimento Java, reconhecida por suas palestras e conteúdos educacionais.

Michelli foi premiada como Microsoft MVP na categoria Developer Technologies em quatro anos consecutivos: 2020, 2021, 2022 e 2023. Este prêmio destaca sua influência e expertise na área, especialmente em Java e Spring Boot. Além disso, Michelli é co-autora do livro *Spring Boot: Da API REST aos Microservices*, que tem sido uma valiosa fonte de aprendizado para muitos profissionais.