# Executando Tarefas Automatizadas com Lambda Function e S3

Este repositório documenta meu aprendizado  durante a **automação de tarefas** usando AWS Lambda e Amazon S3. O objetivo é consolidar o conhecimento em arquiteturas **Serverless** e **Orientadas a Eventos**.

##  Objetivo do Laboratório

O objetivo principal deste desafio é implementar um workflow automatizado onde uma **função Lambda é acionada (triggered) por um evento no S3**.

A prática envolveu a configuração de um *bucket* S3 para que, ao receber um novo arquivo (upload), ele automaticamente disparasse uma função Lambda para processar esse arquivo (por exemplo, ler seu conteúdo ou registrar seus metadados).

## O que é uma Arquitetura Orientada a Eventos?

Em vez de um sistema que *pergunta* ativamente "aconteceu alguma coisa?" (polling), uma arquitetura orientada a eventos *reage* a eventos a medida que eles ocorrem.

O **evento** é o `s3:ObjectCreated:*` (um novo objeto/arquivo sendo criado no bucket). A **reação** é a execução da função Lambda. O S3 (o produtor do evento) e o Lambda (o consumidor do evento) são desacoplados, tornando o sistema muito eficiente e escalável.

## Arquitetura da Solução

O fluxo de dados e execução é o seguinte:

1.  Um **usuário** (ou outro processo) faz o **upload** de um arquivo (ex: `relatorio.txt`) para o `Bucket S3 de Origem`.
2.  O S3 detecta este novo objeto e gera um **evento de notificação** (S3 Event Notification).
3.  O S3 envia este evento, que contém informações sobre o arquivo (nome, tamanho, etc.), para o serviço **AWS Lambda**.
4.  O AWS Lambda **invoca (executa)** nossa função.
5.  Nossa função Lambda (escrita em Python/Node.js) **recebe o evento**, extrai o nome do bucket e do arquivo, e executa sua lógica (ex: lê o arquivo do S3, processa os dados).
6.  A função Lambda registra sua atividade (logs) no **Amazon CloudWatch Logs** para depuração.

## Anotações 

O processo de implementação me ensinou sobre três componentes críticos que precisam trabalhar juntos:

### 1. A Função Lambda
* **O Código (Handler):** O ponto de entrada da função (ex: `lambda_handler`). Ele recebe dois argumentos principais: `event` e `context`.
* **O Objeto `event`:** Este é o JSON enviado pelo S3. Para entender o que fazer, o código precisa "navegar" dentro dele para encontrar o nome do bucket e a chave (nome do arquivo). Ex: `event['Records'][0]['s3']['