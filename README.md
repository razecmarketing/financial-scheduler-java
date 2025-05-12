# Financial Scheduler - Sistema Completo

Sistema de agendamento de transferências financeiras com API backend em Java/Spring Boot e interface de usuário em Angular.

## Visão Geral

Este projeto consiste em duas partes principais:

* **Backend API (financial-scheduler):** Uma API RESTful construída com Java 17 e Spring Boot, responsável pela lógica de negócios, cálculo de taxas e persistência de dados.
* **Frontend UI (financial-scheduler-ui):** Uma aplicação Single Page Application (SPA) desenvolvida com Angular, que consome a API do backend para fornecer a interface do usuário.

---

## Backend API (Java/Spring Boot)

### Decisões Arquiteturais

* **Linguagem e Framework:** Java 17 e Spring Boot 3.3.11 para a construção da API REST.
* **Persistência de Dados:** Banco de dados em memória H2, configurado para ser reiniciado a cada execução da aplicação. Isso simplifica o desenvolvimento e os testes, sem a necessidade de um setup de banco de dados externo.
* **API Design:** RESTful, utilizando JSON para as requisições e respostas.
* **Gerenciamento de Dependências e Build:** Apache Maven.

### Versões e Ferramentas

* **Java:** JDK 17
* **Spring Boot:** 3.3.11 (ou a versão especificada no `pom.xml`)
* **Maven:** (Versão empacotada com o `mvnw`)
* **Banco de Dados:** H2 (em memória)
* **Testes da API:** PowerShell com `Invoke-WebRequest`. Para outros sistemas operacionais ou terminais (como Git Bash, Linux, macOS), `curl` pode ser usado com sintaxe similar.

### Pré-requisitos do Backend

* JDK 17 instalado e configurado (variável de ambiente `JAVA_HOME` apontando para o diretório do JDK).
* Apache Maven (opcional, pois o projeto inclui o Maven Wrapper `mvnw`).

### Comando para executar o Backend no terminal VSCode

```bash
cd financial-scheduler-java
./mvnw spring-boot:run
```

---

## Frontend UI (Angular)

### Pré-requisitos do Frontend

* **Node.js:** Versão 18.x ou superior recomendada. Você pode baixar em [nodejs.org](https://nodejs.org).
* **Angular CLI:** Instalado globalmente:

```bash
npm install -g @angular/cli
```

### Instruções para Subida do Frontend UI

```bash
cd financial-scheduler-ui
npm install
ng serve
```

A aplicação frontend estará disponível em `http://localhost:4200`.

---

## Executando o Sistema Completo Localmente

1. **Inicie o Backend API:**

   ```bash
   cd financial-scheduler-java
   ./mvnw spring-boot:run
   ```

2. **Inicie o Frontend UI:**

   ```bash
   cd financial-scheduler-ui
   ng serve
   ```

3. **Acesse a Aplicação:**

   * Frontend: `http://localhost:4200`
   * Backend (API): `http://localhost:8080`
   * Console H2: `http://localhost:8080/h2-console`

---

## Testes e Comandos da API

(Incluir seções existentes para `Invoke-WebRequest`, `curl`, testes de erro, etc. conforme o restante do conteúdo já existente)

---

## Executando Testes Unitários Java (Existentes)

```bash
cd financial-scheduler-java
./mvnw test
```

Os resultados dos testes serão exibidos no console.

---

Este projeto está preparado para ser executado de forma simples e local, com um comando para cada camada, permitindo rápida experimentação e validação de funcionalidades.
