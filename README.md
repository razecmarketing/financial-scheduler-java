# Financial Scheduler - Sistema Completo

Sistema de agendamento de transferências financeiras com API backend em Java/Spring Boot e interface de usuário em Angular.

## Visão Geral

Este projeto consiste em duas partes principais:

1.  **Backend API (`financial-scheduler`):** Uma API RESTful construída com Java 17 e Spring Boot, responsável pela lógica de negócios, cálculo de taxas e persistência de dados.
2.  **Frontend UI (`financial-scheduler-ui`):** Uma aplicação Single Page Application (SPA) desenvolvida com Angular, que consome a API do backend para fornecer a interface do usuário.

---

## Backend API (Java/Spring Boot)

Sistema de agendamento de transferências financeiras.

### Decisões Arquiteturais

*   **Linguagem e Framework:** Java 17 e Spring Boot 3.3.11 para a construção da API REST.
*   **Persistência de Dados:** Banco de dados em memória H2, configurado para ser reiniciado a cada execução da aplicação. Isso simplifica o desenvolvimento e os testes, sem a necessidade de um setup de banco de dados externo.
*   **API Design:** RESTful, utilizando JSON para as requisições e respostas.
*   **Gerenciamento de Dependências e Build:** Apache Maven.

### Versões e Ferramentas

*   **Java:** JDK 17
*   **Spring Boot:** 3.3.11 (ou a versão especificada no `pom.xml`)
*   **Maven:** (Versão empacotada com o `mvnw`)
*   **Banco de Dados:** H2 (em memória)
*   **Testes da API:** PowerShell com `Invoke-WebRequest`. Para outros sistemas operacionais ou terminais (como Git Bash, Linux, macOS), `curl` pode ser usado com sintaxe similar.

### Pré-requisitos do Backend

*   JDK 17 instalado e configurado (variável de ambiente `JAVA_HOME` apontando para o diretório do JDK).
*   Apache Maven (opcional, pois o projeto inclui o Maven Wrapper `mvnw`).

### Instruções para Subida do Backend API

1.  **Navegar para o Diretório do Backend:**
    ```bash
    cd financial-scheduler-java # Ou o nome da pasta raiz do backend
    ```

2.  **Compilar e Empacotar o Projeto Backend:**
    No diretório raiz do backend, execute o comando apropriado para seu sistema operacional:

    *   **Windows (PowerShell ou CMD):**
        ```powershell
        .\\mvnw clean package
        ```
    *   **Linux/macOS (Terminal Bash/Zsh):**
        ```bash
        ./mvnw clean package
        ```
    Este comando irá limpar builds anteriores, compilar o código-fonte, executar os testes unitários Java existentes e criar um arquivo `.jar` executável em `target/financial-scheduler-0.0.1-SNAPSHOT.jar`.

3.  **Executar a Aplicação Backend:**
    Após o build bem-sucedido, execute o seguinte comando no terminal:
    ```bash
    java -jar target/financial-scheduler-0.0.1-SNAPSHOT.jar
    ```
    A API estará disponível em `http://localhost:8080`.
    O console do banco de dados H2 (em memória) estará acessível em `http://localhost:8080/h2-console`.
    *   **JDBC URL:** `jdbc:h2:mem:financialschedulerdb`
    *   **User Name:** `sa`
    *   **Password:** (deixe em branco)

---

## Frontend UI (Angular)

Interface de usuário desenvolvida com Angular para interagir com a API de agendamento financeiro.

### Pré-requisitos do Frontend

*   **Node.js:** Versão 18.x ou superior recomendada. Você pode baixar em [nodejs.org](https://nodejs.org/).
*   **Angular CLI:** Instalado globalmente. Após instalar o Node.js, instale o Angular CLI com o comando:
    ```bash
    npm install -g @angular/cli
    ```

### Instruções para Subida do Frontend UI

1.  **Navegar para o Diretório do Frontend:**
    A partir da raiz do projeto completo, navegue para a pasta do frontend:
    ```bash
    cd financial-scheduler-ui
    ```

2.  **Instalar Dependências do Frontend:**
    Execute o seguinte comando para instalar todas as dependências do projeto Angular:
    ```bash
    npm install
    ```

3.  **Executar a Aplicação Frontend:**
    Após a instalação das dependências, inicie o servidor de desenvolvimento do Angular:
    ```bash
    npm start
    ```
    ou
    ```bash
    ng serve
    ```
    A aplicação frontend estará disponível em `http://localhost:4200/`. O servidor de desenvolvimento recarregará automaticamente a aplicação se você alterar os arquivos de origem.

---

## Executando o Sistema Completo Localmente

1.  **Inicie o Backend API:** Siga as instruções na seção "Instruções para Subida do Backend API". Certifique-se de que o backend esteja rodando (geralmente em `http://localhost:8080`).
2.  **Inicie o Frontend UI:** Siga as instruções na seção "Instruções para Subida do Frontend UI". Certifique-se de que o frontend esteja rodando (geralmente em `http://localhost:4200`).
3.  **Acesse a Aplicação:** Abra seu navegador e acesse `http://localhost:4200/` para usar o sistema de agendamento financeiro.

---

## Testando a API Manualmente

Os seguintes comandos podem ser executados em um terminal PowerShell para testar os endpoints da API. Certifique-se de que a aplicação Java (`financial-scheduler-0.0.1-SNAPSHOT.jar`) esteja em execução.

**Nota Importante sobre Datas:** Para os exemplos abaixo, a "data de agendamento" (data em que o teste é realizado e a transferência é registrada no sistema) é considerada **10 de maio de 2025**. Se você estiver testando em uma data diferente, ajuste os valores de `transferDate` nos exemplos para manter a mesma diferença de dias e testar as faixas de taxas corretas.

**1. Agendar uma Transferência (Sucesso - Taxa para 1-10 dias)**
   *   Data da Transferência: 15 de maio de 2025 (5 dias de diferença da data de agendamento 10/05/2025)
   *   Valor: 250.75
   *   Taxa Esperada: R$ 3,00 + 3% de 250.75 = 3.00 + 7.5225 = R$ 10.52

   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/transfers -Method POST -Headers @{"Content-Type"="application/json"} -Body '''{"originAccount":"1111222233", "destinationAccount":"4444555566", "amount":250.75, "transferDate":"2025-05-15"}'''
   ```
   **Resultado Esperado:** `StatusCode : 201 Created` e um corpo JSON contendo o ID da transferência, os dados da transferência e `"fee":10.52`.

**2. Listar Todas as Transferências Agendadas**
   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/transfers -Method GET
   ```
   **Resultado Esperado:** `StatusCode : 200 OK` e um corpo JSON contendo um array com todas as transferências agendadas (incluindo a do passo 1).

**3. Buscar Transferência por ID**
   (Assumindo que a primeira transferência criada tem ID 1. Ajuste o ID se necessário.)
   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/transfers/1 -Method GET
   ```
   **Resultado Esperado:** `StatusCode : 200 OK` e um corpo JSON com os detalhes da transferência com ID 1.

**4. Agendar uma Segunda Transferência (Sucesso - Taxa para 11-20 dias)**
   *   Data da Transferência: 25 de maio de 2025 (15 dias de diferença da data de agendamento 10/05/2025)
   *   Valor: 300.00
   *   Taxa Esperada: R$ 12,00 + 2.5% de 300.00 = 12.00 + 7.50 = R$ 19.50

   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/transfers -Method POST -Headers @{"Content-Type"="application/json"} -Body '''{"originAccount":"2222333344", "destinationAccount":"5555666677", "amount":300.00, "transferDate":"2025-05-25"}'''
   ```
   **Resultado Esperado:** `StatusCode : 201 Created` e um corpo JSON contendo o ID da nova transferência (ex: 2) e `"fee":19.50`.

**5. Testar Erro: Contas de Origem e Destino Iguais**
   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/transfers -Method POST -Headers @{"Content-Type"="application/json"} -Body '''{"originAccount":"3333333333", "destinationAccount":"3333333333", "amount":50.00, "transferDate":"2025-05-20"}'''
   ```
   **Resultado Esperado:** Erro. O `Invoke-WebRequest` mostrará uma mensagem como `Invoke-WebRequest : A conta de origem não pode ser igual à conta de destino.` (A API retorna HTTP 400).

**6. Testar Erro: Data de Transferência no Passado**
   *   Data da Transferência: 09 de maio de 2025 (anterior à data de agendamento 10/05/2025)

   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/transfers -Method POST -Headers @{"Content-Type"="application/json"} -Body '''{"originAccount":"4444444444", "destinationAccount":"5555555555", "amount":75.00, "transferDate":"2025-05-09"}'''
   ```
   **Resultado Esperado:** Erro. O `Invoke-WebRequest` mostrará uma mensagem como `Invoke-WebRequest : O servidor remoto retornou um erro: (400) Solicitação Incorreta.` A mensagem específica da API ("A data de transferência não pode ser anterior à data de hoje.") estará no corpo da resposta HTTP.

**7. Testar Erro: Taxa Não Aplicável (Transferência no Mesmo Dia)**
   *   Data da Transferência: 10 de maio de 2025 (0 dias de diferença da data de agendamento 10/05/2025)

   ```powershell
   Invoke-WebRequest -Uri http://localhost:8080/api/transfers -Method POST -Headers @{"Content-Type"="application/json"} -Body '''{"originAccount":"6666666666", "destinationAccount":"7777777777", "amount":100.00, "transferDate":"2025-05-10"}'''
   ```
   **Resultado Esperado:** Erro. O `Invoke-WebRequest` mostrará uma mensagem como `Invoke-WebRequest : Não há taxa aplicável para a data de transferência especificada.` (A API retorna HTTP 400).

## Executando Testes Unitários Java (Existentes)

Para executar os testes unitários automatizados que fazem parte do projeto Java (como o teste de contexto do Spring Boot em `FinancialSchedulerApplicationTests.java`), use o seguinte comando Maven no diretório raiz do projeto:

*   **Windows (PowerShell ou CMD):**
    ```powershell
    .\mvnw test
    ```
*   **Linux/macOS (Terminal Bash/Zsh):**
    ```bash
    ./mvnw test
    ```
Os resultados dos testes serão exibidos no console.

---

**Nota sobre Testes Manuais da API com `curl` (Linux/macOS/Git Bash):**

Os exemplos de `Invoke-WebRequest` para PowerShell podem ser adaptados para `curl`. Por exemplo, o primeiro teste de agendamento seria:

```bash
curl -X POST -H \"Content-Type: application/json\" -d '{\"originAccount\":\"1111222233\", \"destinationAccount\":\"4444555566\", \"amount\":250.75, \"transferDate\":\"2025-05-15\"}' http://localhost:8080/api/transfers
```
Lembre-se de ajustar as datas conforme a data atual do seu teste, se necessário, para validar as regras de taxa corretamente.

---
