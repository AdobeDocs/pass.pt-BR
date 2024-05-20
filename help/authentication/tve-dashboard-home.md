---
title: Painel
description: Saiba mais sobre a página inicial do Painel TVE.
source-git-commit: 06c2e1e54515a2ec47722ba1f360467dadd1f73b
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Painel {#dashboard}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A variável **Painel** no painel esquerdo serve como a página inicial do Painel TVE de autenticação do Adobe Pass.

Há duas seções disponíveis na página inicial:

* [Tela de boas-vindas](#welcome-screen)
* [Status da configuração](#configuration-status)

## Tela de boas-vindas {#welcome}

Nesta seção, você pode acessar a documentação pública diretamente da mensagem de boas-vindas e visualizar um instantâneo das configurações atuais.

* **Integrações ativas**: o número de integrações ativas no ambiente atual. Selecionar **Exibir mais na seção de integração** para acessar informações detalhadas na [Integrações](tve-dashboard-integrations.md) seção.
* **Canais ativos**: o número de canais ativos no ambiente atual. Selecionar **Exibir mais na seção Canais** para acessar informações detalhadas na [Canais](tve-dashboard-channels.md) seção.
* **Atualizações do banco de dados**: o número de alterações de configuração feitas no ambiente atual. Selecionar **Exibir mais na seção Log de alterações** para acessar informações detalhadas na [Registro de alterações](tve-dashboard-changes-log.md) seção.
* **Painel ESM**: fique atento ao próximo painel do ESM, que oferece métricas detalhadas sobre o uso de propriedades no ambiente atual. Essa funcionalidade estará acessível em atualizações futuras.

![Tela de boas-vindas](assets/welcome-screen.png)

*Tela de boas-vindas*

## Status da configuração {#conf-status}

Esta seção apresenta as 10 alterações de configuração mais recentes que incluem:

* **Descrição de alterações**: uma breve descrição da alteração selecionada pelo usuário.
* **Enviado por**: A conta responsável pela alteração.
* **Data de push**: a data em que a alteração foi feita.

![Status de configuração de um log de alterações](assets/configuration-status.png)

*Status de configuração de um log de alterações*

Para exibir a lista completa de alterações, selecione **Exibir mais no Log de Alterações** no canto inferior direito para exibir a [Registro de alterações](tve-dashboard-changes-log.md) seção.
