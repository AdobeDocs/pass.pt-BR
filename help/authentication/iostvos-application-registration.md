---
title: Registro de aplicativo iOS/tvOS
description: Registro de aplicativo iOS/tvOS
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 0%

---

# Registro de aplicativo iOS/tvOS {#iostvos-application-registration}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#Intro}

A partir da versão 3.0 do SDK AccessEnabler do iOS/tvOS, estamos alterando o mecanismo de autenticação com servidores Adobe. Em vez de usar uma chave pública e um sistema secreto para assinar o requestorID, estamos introduzindo o conceito de uma string de instrução de software que pode ser usada para obter um token de acesso usado posteriormente para todas as chamadas que o SDK faz aos nossos servidores. Além de uma declaração de software, você também precisará de um esquema de URL personalizado para seu aplicativo.

Para obter mais informações, consulte [Registro de Cliente Dinâmico](/help/authentication/dynamic-client-registration.md)

## O que é uma Declaração de Software? {#Soft_state}

Uma Declaração de Software é um token JWT que contém informações sobre seu aplicativo. Cada aplicativo deve ter uma instrução de software exclusiva, usada por nossos servidores para identificar o aplicativo no sistema Adobe. A Instrução de Software precisa ser passada ao inicializar o SDK do AccessEnabler e será usada para registrar o aplicativo no Adobe. Após o registro, o SDK receberá uma ID do cliente e um segredo do cliente que serão usados para obter um token de acesso. Qualquer chamada feita pelo SDK para nossos servidores exigirá um token de acesso válido. O SDK é responsável por registrar o aplicativo, obter e atualizar o token de acesso.

**Observação:** uma Instrução de Software é específica para o aplicativo e a mesma instrução de software não pode ser usada em mais de um aplicativo. Observe que as instruções de software de nível de programador também seguem o mesmo, ou seja, elas só podem ser usadas para um único aplicativo - seja de canal único ou de vários canais. Esta limitação também se aplica ao regime personalizado.

## Como obter uma Declaração de Software? {#obtain}

### Se você tiver acesso ao Painel TVE do Adobe:

- Abra seu navegador e navegue até <https://console.auth.adobe.com>
- Navegue até a seção `Channels` e selecione seu canal.
- Navegue até a guia `Registered Applications`.
- Clique em `Add new application`.
- Forneça um nome e uma versão para o aplicativo e selecione o   plataformas em que estará disponível. iOS/tvOS no nosso caso.
- Envie suas alterações ao servidor e navegue de volta para a guia Aplicativos registrados do canal.
- Você deve ver uma lista com todos os aplicativos registrados. Clique em   Botão `Download` no aplicativo recém-criado. Talvez seja necessário aguardar alguns minutos antes que a Declaração de software esteja pronta para download.
- Um arquivo de texto será baixado. Use seu conteúdo como sua Declaração de Software.

Para obter mais informações, consulte [Gerenciamento dinâmico de registros de clientes](/help/authentication/dynamic-client-registration-management.md).

### Se você não tiver acesso ao Painel do Adobe TVE:

Enviar tíquete para <tve-support@adobe.com>. Inclua todas as informações necessárias, como canal, nome do aplicativo, versão e plataformas. Uma pessoa da nossa equipe de suporte criará uma declaração de software para você.

## Como usar a Declaração de Software? {#use}

Depois de obter a Instrução de Software, você precisará passá-la como um parâmetro no construtor Access Enabler. Recomendamos hospedar a Declaração de Software em um local remoto. Dessa forma, você pode revogar e alterar facilmente a Declaração de Software sem lançar uma nova versão do aplicativo.

## Geração de um esquema de URL personalizado para seu aplicativo {#generating}

### Se você tiver acesso ao Painel TVE do Adobe:

- Abra seu navegador e navegue até <https://console.auth.adobe.com>
- Navegue até a seção `Channels` e selecione seu canal.
- Navegue até a guia `Custom Schemes`.
- Clique em `Generate a new custom scheme`.
- Um novo esquema personalizado será gerado para seu aplicativo. Exemplo: `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Enviar as alterações para o servidor.

### Se você não tiver acesso ao Painel do Adobe TVE:

Enviar tíquete para <tve-support@adobe.com>. Inclua a ID do canal e alguém de nossa equipe de suporte criará um esquema personalizado para você.

## Como usar o esquema personalizado {#use_custom}

No arquivo `info.plist` do aplicativo, adicione o seguinte código:

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
