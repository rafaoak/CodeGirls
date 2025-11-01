#  Explorando Workflows Automatizados com AWS Step Functions

Este repositório documenta meu aprendizado e os insights adquiridos durante o laboratório sobre AWS Step Functions. O objetivo é consolidar o conhecimento em workflows automatizados e servir como material de consulta futura.

##  Objetivo 

O objetivo principal deste desafio é aplicar os conceitos de orquestração de serviços da AWS usando o Step Functions.

## O que é o AWS Step Functions?

Minha principal anotação sobre o serviço é:

O AWS Step Functions é um serviço de orquestração visual *serverless*. Ele permite coordenar múltiplos serviços da AWS em fluxos de trabalho (workflows) fáceis de entender e depurar.

O conceito central é a **Máquina de Estados (State Machine)**. Cada passo no seu workflow é um "Estado", e você define as transições entre eles usando uma linguagem chamada Amazon States Language (ASL), que é baseada em JSON.

---

### Serviços Utilizados

* **AWS Step Functions:** Para orquestrar todo o fluxo.
* **AWS Lambda:** Para executar a lógica de negócio principal. (Ex: processar um pedido, validar dados de um formulário, etc.).
* **Amazon S3:** Para servir como gatilho (trigger). (Ex: o workflow começa quando um novo arquivo é carregado no S3).

---

## Anotações e Conceitos-Chave

Esta seção reúne os principais insights e conceitos que aprendi durante as aulas.

### 1. Máquina de Estados (State Machine)
* É a definição do seu workflow. Representa todo o processo do início ao fim.
* Pode ser de dois tipos: **Standard (Padrão)** e **Express (Expresso)**.

### 2. Diferença: Standard vs. Express Workflows
* **Standard (Padrão):**
    * **Duração:** Longa duração (até 1 ano).
    * **Execução:** Exatamente uma vez (exactly-once).
    * **Caso de uso:** Ideal para processos que precisam de auditoria, são demorados e não podem ter duplicidade (ex: processar um pedido, orquestrar um fluxo de BI).
* **Express (Expresso):**
    * **Duração:** Curta duração (até 5 minutos).
    * **Execução:** Pelo menos uma vez (at-least-once).
    * **Caso de uso:** Ideal para alto volume e alta velocidade (ex: processar dados de streaming de IoT, validar microserviços).

### 3. Tipos de Estados (Passos)
O workflow é composto por diferentes tipos de estados. Os que mais se destacaram foram:

* **`Task` (Tarefa):** O estado mais comum. É usado para "fazer um trabalho", como invocar uma função Lambda, enviar um item para o SQS ou publicar em um tópico SNS.
* **`Choice` (Escolha):** Adiciona lógica condicional (um "if/else"). Ele permite que o workflow siga caminhos diferentes com base nos dados de entrada.
* **`Parallel` (Paralelo):** Permite executar múltiplos ramos do seu workflow ao mesmo tempo, economizando tempo.
* **`Wait` (Espera):** Pausa o workflow por um tempo determinado.
* **`Succeed` / `Fail` (Sucesso / Falha):** Terminam o workflow com um status específico.

### 4. Tratamento de Erros
Uma das partes mais poderosas. Em vez de escrever código complexo para tratar falhas no Lambda, o Step Functions pode fazer isso de forma declarativa.

* **`Retry` (Tentar Novamente):** Permite que você configure novas tentativas automáticas caso uma `Task` (como um Lambda) falhe. Você pode definir quantas vezes tentar e o intervalo entre as tentativas.
* **`Catch` (Capturar):** Funciona como um "try/catch". Se um estado falhar (mesmo após as retentativas), você pode "capturar" esse erro específico e enviar o workflow para um caminho de tratamento de falha (ex: enviar um e-mail de notificação).

