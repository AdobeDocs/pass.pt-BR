---
title: Registro do aplicativo Amazon FireOS
description: Registro do aplicativo Amazon FireOS
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# Registro de aplicativo Amazon FireOS (herdado) {#amazon-fireos-application-registration}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

## Introdução {#intro}

A partir da versão 3.0 do FireOS AccessEnabler SDK, estamos alterando o mecanismo de autenticação com servidores Adobe. Em vez de usar uma chave pública e um sistema secreto para assinar o requestorID, estamos introduzindo o conceito de uma string de Declaração de Software que pode ser usada para obter um token de acesso usado posteriormente para todas as chamadas que o SDK faz aos nossos servidores. Além de uma Declaração de Software, você também precisará criar um deep link para o seu aplicativo.

Para obter mais informações, consulte [Visão Geral do Registro de Cliente Dinâmico](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## O que é uma Declaração de Software? {#what}

Uma Declaração de Software é um token JWT que contém informações sobre seu aplicativo. Cada aplicativo deve ter uma Declaração de Software exclusiva, usada por nossos servidores para identificar o aplicativo no sistema Adobe. A Instrução de Software precisa ser passada quando você inicializar o AccessEnabler SDK e será usada para registrar o aplicativo com o Adobe. Após o registro, a SDK receberá uma ID do cliente e um segredo do cliente que será usado para obter um token de acesso. Qualquer chamada feita pela SDK para nossos servidores exigirá um token de acesso válido. A SDK é responsável por registrar o aplicativo, obter e atualizar o token de acesso.

**Observação:** as Instruções de Software são específicas do aplicativo e não podem ser usadas para mais de um aplicativo. Observe que isso também se aplica a aplicativos que oferecem acesso a vários canais.

## Como obter uma Declaração de Software? {#how-to}

### Se você tiver acesso ao Painel TVE do Adobe:

1. Abra o navegador e navegue até `https://experience.adobe.com/#/pass/authentication`.

1. Navegue até a seção **[!UICONTROL Channels]** e selecione seu canal.

1. Navegue até a guia **[!UICONTROL Registered Applications]**.

1. Clique em **[!UICONTROL Add new application]**.

1. Forneça um nome e uma versão para o aplicativo e selecione as plataformas em que ele estará disponível (como Android).

1. Forneça um **[!UICONTROL Domain Name]** escolhendo em uma lista de domínios já configurados para seu Programador.

1. Envie suas alterações por push ao servidor e navegue de volta para a guia **[!UICONTROL Registered Applications]** do canal.

   Você deve ver uma lista com todos os aplicativos registrados.

1. Clique em **[!UICONTROL Download]** no aplicativo recém-criado.

   Talvez seja necessário aguardar alguns minutos antes que a Declaração de software esteja pronta para download.

   Um arquivo de texto é baixado. Use seu conteúdo como a Declaração de Software.

Para obter mais informações, consulte [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Se você não tiver acesso ao Painel do Adobe TVE:

Enviar um tíquete para [tve-support@adobe.com](mailto:tve-support@adobe.com). Inclua todas as informações necessárias, incluindo canal, nome do aplicativo, versão e plataformas, e alguém de nossa equipe de suporte criará uma declaração de software para você.

## Como usar a Declaração de Software {#use}

Depois de obter a Instrução de Software, você precisará passá-la como um parâmetro no construtor Access Enabler. A Adobe recomenda hospedar a Declaração de Software em um local remoto. Dessa forma, você pode revogar e alterar facilmente a Declaração de Software sem lançar uma nova versão do seu aplicativo.

## Como usar a Declaração de Software {#use-both}

No arquivo de recursos do aplicativo `strings.xml`, adicione o seguinte código:

```XML
<string name="software_statement">softwarestatement value</string>
```
