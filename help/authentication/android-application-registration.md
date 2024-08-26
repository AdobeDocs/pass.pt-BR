---
title: Registro do aplicativo Android
description: Registro do aplicativo Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Registro do aplicativo Android {#android-application-registration}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

A partir da versão 3.0 do SDK do Android AccessEnabler, estamos alterando o mecanismo de autenticação com servidores Adobe. Em vez de usar uma chave pública e um sistema secreto para assinar o requestorID, estamos introduzindo o conceito de uma string de Declaração de Software que pode ser usada para obter um token de acesso usado posteriormente para todas as chamadas que o SDK faz aos nossos servidores. Além de uma Declaração de Software, você também precisará criar um deep link para o seu aplicativo.

Para obter mais informações, consulte [Visão Geral do Registro de Cliente Dinâmico](./dcr-api/dynamic-client-registration-overview.md).

## O que é uma Declaração de Software? {#what}

Uma Declaração de Software é um token JWT que contém informações sobre seu aplicativo. Cada aplicativo deve ter uma instrução de software exclusiva usada pelos nossos servidores para identificar o aplicativo no sistema Adobe.

A Instrução de Software precisa ser passada quando você inicializa o SDK `AccessEnabler`. É usado para registrar o aplicativo com o Adobe. Após o registro, o SDK recebe uma ID do cliente e um segredo do cliente, que é usado para obter um token de acesso. Qualquer chamada feita pelo SDK para servidores Adobe requer um token de acesso válido. O SDK é responsável por registrar o aplicativo, obter e atualizar o token de acesso.

>[!NOTE]
>
>As instruções de software são específicas do aplicativo e uma instrução de software individual não pode ser usada para mais de um aplicativo. Observe que as instruções de software de nível de programador têm a mesma restrição; elas só podem ser usadas para um único aplicativo, seja para um único canal ou para vários canais.

## Como obter uma Declaração de Software {#how-to-get-ss}

Estas são as maneiras de obter uma Declaração de software.

### Se você tiver acesso ao Painel TVE do Adobe

1. Abra o navegador e navegue até o [Painel do Adobe Pass TVE](https://console.auth.adobe.com).

1. Navegue até a seção **[!UICONTROL Channels]** e selecione seu canal.

1. Navegue até a guia **[!UICONTROL Registered Applications]**.

1. Clique em **[!UICONTROL Add new application]**.

1. Nomeie o aplicativo e especifique uma versão.

1. Selecione as plataformas em que o aplicativo estará disponível (Android, neste caso).

1. Forneça um **[!UICONTROL Domain Name]** escolhendo em uma lista de domínios já configurados para seu Programador.

1. Envie suas alterações por push ao servidor e navegue de volta para a guia **[!UICONTROL Registered Applications]** do Canal.

   Você deve ver uma lista com todos os aplicativos registrados. Selecione **[!UICONTROL Download]** no aplicativo criado. Talvez seja necessário aguardar alguns minutos antes que a Declaração de software esteja pronta para download.

   Um arquivo de texto é baixado. Use seu conteúdo como a Declaração de Software.

Para obter mais informações, consulte [Dynamic Client Registration Management](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Se você não tiver acesso ao Painel Adobe TVE

Enviar tíquete para `tve-support@adobe.com`. Inclua as informações necessárias, como canal, nome do aplicativo, versão e plataformas. Alguém da nossa equipe de suporte criará uma declaração de software para você.

## Como usar a Declaração de Software {#how-to-use-ss}

Depois de obter a Instrução de Software, você precisará passá-la como um parâmetro no construtor Access Enabler. Recomendamos hospedar a Declaração de Software em um local remoto. Dessa forma, você pode revogar e alterar facilmente a Declaração de Software sem lançar uma nova versão do seu aplicativo.

## Criar e usar um deep link para seu aplicativo {#create}

No Android, use como valor de deep link o inverso do nome de domínio selecionado quando você criou a Declaração de software

O deep link criado deve ter um valor exclusivo no dispositivo Android. Quando vários aplicativos usam o mesmo valor de deep link, os fluxos de autenticação e logout interferem.

## Como usar a Declaração de software e o deep link {#use-both}

No arquivo de recursos do aplicativo `strings.xml`, adicione o seguinte código:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
