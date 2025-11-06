---
title: Política de retenção de dados
description: Política de retenção de dados
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# Política de retenção de dados {#data-retention-policy}

>[!WARNING]
>
>**Aviso:** o conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.


## Introdução {#introduction}

A Adobe, na sua função de processador de dados, deve tomar as medidas apropriadas para ajudar seus clientes a cumprir o acesso, a exclusão e outras solicitações individuais. A aplicação de políticas de exclusão oportunas, seguras e apropriadas é uma parte importante do cumprimento dessa obrigação.

## Definições {#definitions}

Uma política de retenção de dados determina por quanto tempo a Adobe armazena os dados do cliente. A Política de Retenção de Dados padrão para Monitoramento de Simultaneidade é **25 meses**.

| Período de retenção de dados | O período de retenção de dados é o período padrão (25 meses). |
|---|---|
| **Janela de retenção de dados** | A janela de retenção de dados define os parâmetros para os quais os dados concluídos podem ser visualizados e relatados. A janela de retenção de dados é determinada da seguinte maneira:<br/> *Data de início* = data atual - período de retenção de dados <br/>*Data de término* = data atual |

## Coleção de dados {#data-collection}

*Dados de sequência de cliques* representam dados compartilhados por clientes nas pulsações da sessão (por exemplo, subjectID, mvpdName e metadados). Todos os campos de metadados personalizados são mencionados nos [Atributos de metadados padrão](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Tipos de cliente {#customer-types}

### Clientes atuais {#current-customers}

A menos que o cliente adquira extensões de retenção de dados, o Monitoramento de simultaneidade cumprirá os seguintes requisitos de retenção de dados do cliente:

* Os *dados de sequência de cliques* coletados pelo Monitoramento de Simultaneidade devem ser excluídos até **25 meses** da data de coleta.

### Clientes Encerrados {#terminated-customers}

Um cliente encerrado é um cliente que encerrou o relacionamento com a Adobe e não está mais usando o Monitoramento de simultaneidade.

* Os *dados de sequência de cliques* coletados pelo Monitoramento de Simultaneidade devem ser excluídos dentro de **6 meses** a partir da data de término do contrato do cliente.

## Exclusão de dados {#data-deletion}

A Adobe mantém o direito de excluir dados para datas além do período de retenção de dados sem opção de recuperação. Os dados de clientes atuais devem ser excluídos mensalmente.
