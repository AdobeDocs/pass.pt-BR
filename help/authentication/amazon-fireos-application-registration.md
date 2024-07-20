---
title: Registro do aplicativo Amazon FireOS
description: Registro do aplicativo Amazon FireOS
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---

# Registro do aplicativo Amazon FireOS {#amazon-fireos-application-registration}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

## Introdução {#intro}

A partir da versão 3.0 do SDK FireOS AccessEnabler, estamos alterando o mecanismo de autenticação com servidores Adobe Access. Em vez de usar uma chave pública e um sistema secreto para assinar o requestorID, estamos introduzindo o conceito de uma string de Declaração de Software que pode ser usada para obter um token de acesso usado posteriormente para todas as chamadas que o SDK faz aos nossos servidores. Além de uma Declaração de Software, você também precisará criar um deep link para o seu aplicativo.

Para obter mais informações, consulte [Registro de Cliente Dinâmico](/help/authentication/dynamic-client-registration.md)

## O que é uma Declaração de Software? {#what}

Uma Declaração de Software é um token JWT que contém informações sobre seu aplicativo. Cada aplicativo deve ter uma Declaração de Software exclusiva, usada por nossos servidores para identificar o aplicativo no sistema Adobe. A Instrução de Software precisa ser passada ao inicializar o SDK do AccessEnabler e será usada para registrar o aplicativo no Adobe. Após o registro, o SDK receberá uma ID do cliente e um segredo do cliente que serão usados para obter um token de acesso. Qualquer chamada feita pelo SDK para nossos servidores exigirá um token de acesso válido. O SDK é responsável por registrar o aplicativo, obter e atualizar o token de acesso.

**Observação:** as Instruções de Software são específicas do aplicativo e não podem ser usadas para mais de um aplicativo. Observe que isso também se aplica a aplicativos que oferecem acesso a vários canais.

## Como obter uma Declaração de Software? {#how-to}

### Se você tiver acesso ao Painel TVE do Adobe:

1. Abra o navegador e navegue até `https://console.auth.adobe.com`.

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

Para obter mais informações, consulte [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)

### Se você não tiver acesso ao Painel do Adobe TVE:

Enviar um tíquete para [tve-support@adobe.com](mailto:tve-support@adobe.com). Inclua todas as informações necessárias, incluindo canal, nome do aplicativo, versão e plataformas, e alguém de nossa equipe de suporte criará uma declaração de software para você.

## Como usar a Declaração de Software {#use}

Depois de obter a Instrução de Software, você precisará passá-la como um parâmetro no construtor Access Enabler. A Adobe recomenda hospedar a Declaração de Software em um local remoto. Dessa forma, você pode revogar e alterar facilmente a Declaração de Software sem lançar uma nova versão do seu aplicativo.

## Como usar a Declaração de Software {#use-both}

No arquivo de recursos do aplicativo `strings.xml`, adicione o seguinte código:

```XML
<string name="software_statement">softwarestatement value</string>
```
