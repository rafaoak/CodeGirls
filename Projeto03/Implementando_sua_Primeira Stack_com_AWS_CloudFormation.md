# Implementando sua Primeira Stack com AWS CloudFormation

Este repositório documenta meu aprendizado e os insights adquiridos durante a implementação da minha primeira *Stack* com AWS CloudFormation. O objetivo é consolidar o conhecimento em **Infraestrutura como Código (IaC)** e servir como material de consulta futura.

##  Objetivo 

O objetivo principal deste desafio é aplicar os conceitos de IaC para provisionar (criar) recursos na AWS de forma automatizada. A prática envolveu a criação de um *template* CloudFormation para construir [**Aqui você descreve em 1-2 frases qual foi a arquitetura. Ex: "uma instância EC2 simples" ou "um bucket S3 com acesso público"**].

##  O que é AWS CloudFormation?

O CloudFormation é o serviço de **Infraestrutura como Código (IaC)** da AWS. Em vez de criar recursos manualmente pelo console (clicando em botões), nós escrevemos um **arquivo de texto (template)**, em formato YAML ou JSON, que descreve *quais* recursos queremos e *como* eles devem ser configurados.

O CloudFormation então lê esse "mapa" (o template) e constrói tudo na ordem correta. O conjunto de recursos criados a partir de um template é chamado de **Stack**.

### O Template (O Código-Fonte da Infraestrutura)

O coração do CloudFormation é o arquivo de template (em YAML ou JSON). Ele é dividido em seções que descrevem a infraestrutura.


##  Principais Anotações e Descobertas

O processo de criação de uma Stack envolve alguns passos-chave que observei:

1.  **Criação da Stack:** Fazemos o upload do template e preenchemos os parâmetros definidos.
2.  **Acompanhamento (Eventos):** A aba "Events" (Eventos) é crucial. Ela mostra o log em tempo real de tudo o que o CloudFormation está fazendo, desde `CREATE_IN_PROGRESS` até `CREATE_COMPLETE`.
3.  **Resultados (Outputs):** Ao final, a aba "Outputs" (Saídas) exibe os valores que definimos como importantes (ex: o IP público do servidor criado).
4.  **Exclusão da Stack:** Tão importante quanto criar é destruir. Ao final do laboratório, deletei a Stack. O CloudFormation removeu **todos** os recursos que ele criou, na ordem inversa, garantindo que nada ficasse para trás (evitando custos).

### Conceitos-Chave que Aprendi:

* **O Poder da IaC:** A maior vantagem é a **reprodutibilidade**. Posso usar esse mesmo template para criar 100 cópias idênticas da minha infraestrutura em diferentes regiões, com garantia de consistência.
* **Gerenciamento de Estado:** O CloudFormation sabe exatamente o que ele criou. Se eu **atualizar** o template (ex: mudar o tipo da instância de `t2.micro` para `t2.small`), ele não destrói tudo, ele apenas aplica a mudança necessária.
* **Rollback Automático:** Se algo der errado durante a criação (ex: um parâmetro inválido ou falta de permissão), o CloudFormation para e **desfaz (rollback) tudo** o que ele criou até aquele ponto. Isso evita que a infraestrutura fique em um estado "quebrado" ou inconsistente.

