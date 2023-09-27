---
title: Política de retenção de dados
description: Política de retenção de dados
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Política de retenção de dados {#data-retention-policy}

>[!WARNING]
>
>**Aviso:** O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


## Introdução {#introduction}

O Adobe, na sua função de processador de dados, deve tomar as medidas apropriadas para ajudar seus clientes a cumprir o acesso, a exclusão e outras solicitações dos indivíduos. A aplicação de políticas de exclusão oportunas, seguras e apropriadas é uma parte importante do cumprimento dessa obrigação.

## Definições {#definitions}

Uma política de retenção de dados determina por quanto tempo o Adobe armazena os dados do cliente. A política de retenção de dados padrão para monitoramento de simultaneidade é **25 meses**.

| Período de retenção de dados | O período de retenção de dados é o período padrão (25 meses). |
|---|---|
| **Janela de retenção de dados** | A janela de retenção de dados define os parâmetros para os quais os dados concluídos podem ser visualizados e relatados. A janela de retenção de dados é determinada da seguinte maneira:<br/> *Data inicial* = data atual - período de retenção de dados <br/>*Data final* = data atual |

## Coleta de dados {#data-collection}

*Dados de sequência de cliques* representa dados compartilhados por clientes nas pulsações da sessão (por exemplo, subjectID, mvpdName e metadados). Todos os campos de metadados personalizados são referenciados no [Atributos de metadados padrão](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Tipos de cliente {#customer-types}

### Clientes atuais {#current-customers}

A menos que o cliente adquira extensões de retenção de dados, o Monitoramento de simultaneidade cumprirá os seguintes requisitos de retenção de dados do cliente:

* *Dados de sequência de cliques* coletado pelo Monitoramento de simultaneidade deve ser excluído até **25 meses** a partir da data de recolha.

### Clientes Encerrados {#terminated-customers}

Um cliente encerrado é um cliente que encerrou o relacionamento com o Adobe e não está mais usando o Monitoramento de simultaneidade.

* *Dados de sequência de cliques* coletado pelo Monitoramento de simultaneidade deve ser excluído em **6 meses** a partir da data de término do contrato do cliente.

## Exclusão de dados {#data-deletion}

O Adobe retém o direito de excluir dados para datas além do período de retenção de dados sem opção de recuperação. Os dados de clientes atuais devem ser excluídos mensalmente.

