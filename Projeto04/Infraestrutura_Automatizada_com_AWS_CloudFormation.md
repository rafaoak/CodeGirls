# Implementando Infraestrutura Automatizada com AWS CloudFormation

Este repositório documenta meu aprendizado e os insights adquiridos durante o laboratório de **automação** com AWS CloudFormation. O objetivo é consolidar o conhecimento em Infraestrutura como Código (IaC) para provisionar **stacks de múltiplos recursos**.

##  Objetivo 

O objetivo principal deste desafio é aplicar os conceitos de IaC para provisionar (criar) uma **infraestrutura de aplicação web funcional** de forma automatizada.

A prática envolveu a criação de um *template* CloudFormation para construir uma **instância EC2 (servidor web) que é acessível ao público através de um Security Group (firewall)**, tudo em uma única execução.


##  O que é Infraestrutura como Código (IaC)?

O AWS CloudFormation é o serviço de Infraestrutura como Código (IaC) da AWS. Em vez de criar recursos manualmente pelo console (clicando em botões), nós escrevemos um **arquivo de texto (template)**, em formato YAML ou JSON, que descreve *quais* recursos queremos e *como* eles devem ser configurados.

A "automação" vem do fato de que o CloudFormation entende as **dependências** entre os recursos (ex: ele sabe que precisa criar o Security Group *antes* de criar a instância EC2 que o utiliza) e provisiona tudo na ordem correta.

## Anotações 

O processo de criação da Stack (upload, eventos, exclusão) é o mesmo, mas os conceitos aprendidos foram mais avançados:

### Conceitos-Chave da Automação:

* **Gerenciamento de Dependências:** Esta é a "mágica" da automação. Eu não precisei me preocupar em "criar o firewall primeiro". Eu apenas declarei no EC2 que ele *usava* o Security Group, e o CloudFormation automaticamente entendeu a ordem correta de criação.
* **Parâmetros (Reusabilidade):** Um template "hard-coded" (com valores fixos) não é automatizado. O uso de `Parameters` (como `KeyName`) é o que permite que o mesmo template seja usado por diferentes pessoas ou em diferentes ambientes (produção, testes) apenas mudando as entradas.
* **Scripts `UserData`:** Ver a instância EC2 não apenas "ser criada", mas também "se configurar sozinha" (instalando o Apache) é o verdadeiro poder da automação. A infraestrutura já nasce pronta para o uso.
* **Gerenciamento de Atualizações (Changesets):** Aprendi que, se eu quiser mudar algo (ex: trocar `t2.micro` para `t2.small`), eu não preciso destruir tudo. Eu posso enviar uma **atualização (update)** para a Stack, e o CloudFormation aplica apenas aquela mudança.

