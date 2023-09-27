---
title: Registro do aplicativo Android
description: Registro do aplicativo Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Registro do aplicativo Android {#android-application-registration}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

A partir da versão 3.0 do SDK AccessEnabler do Android, estamos alterando o mecanismo de autenticação com servidores Adobe. Em vez de usar uma chave pública e um sistema secreto para assinar o requestorID, estamos introduzindo o conceito de uma string de instrução de software que pode ser usada para obter um token de acesso usado posteriormente para todas as chamadas que o SDK faz aos nossos servidores. Além de uma instrução de software, também será necessário criar um deep link para o seu aplicativo.

Para obter mais informações, consulte [Registro de cliente dinâmico](/help/authentication/dynamic-client-registration.md)

## O que é uma Declaração de Software? {#what}

Uma Declaração de Software é um token JWT que contém informações sobre seu aplicativo. Cada aplicativo deve ter uma instrução de software exclusiva, usada por nossos servidores para identificar o aplicativo no sistema Adobe. A Instrução de Software precisa ser passada ao inicializar o SDK do AccessEnabler e será usada para registrar o aplicativo no Adobe. Após o registro, o SDK receberá uma ID do cliente e um segredo do cliente que serão usados para obter um token de acesso. Qualquer chamada feita pelo SDK para nossos servidores exigirá um token de acesso válido. O SDK é responsável por registrar o aplicativo, obter e atualizar o token de acesso.

>[!NOTE]
>
>As instruções de software são específicas do aplicativo e uma instrução de software individual não pode ser usada para mais de um aplicativo. Observe que as instruções de software de nível de programador têm a mesma restrição; elas só podem ser usadas para um único aplicativo, seja para um único canal ou para vários canais.

## Como obter uma Declaração de Software? {#how-to-get-ss}

### Se você tiver acesso ao Painel TVE do Adobe:

* Abra o navegador e acesse [Painel Adobe Pass TVE](https://console.auth.adobe.com).
* Navegue até `Channels` e selecione seu canal.
* Navegue até `Registered Applications` Guia.
* Clique em `Add new application`.
* Forneça um nome e uma versão para o aplicativo e selecione as plataformas em que ele estará disponível. Android no nosso caso.
* Forneça um Nome de domínio escolhendo em uma lista de domínios já configurados para seu Programador.
* Envie suas alterações ao servidor e navegue de volta para a guia Aplicativos registrados do canal.
* Você deve ver uma lista com todos os aplicativos registrados. Selecione o **Baixar** no aplicativo recém-criado. Talvez seja necessário aguardar alguns minutos antes que a Declaração de software esteja pronta para download.
* Um arquivo de texto será baixado. Use seu conteúdo como a Declaração de Software.

Para obter mais informações, consulte [Gerenciamento dinâmico de registros de clientes](/help/authentication/dynamic-client-registration-management.md)

### Se você não tiver acesso ao Painel do Adobe TVE:

Enviar um tíquete para `tve-support@adobe.com`. Inclua todas as informações necessárias, como canal, nome do aplicativo, versão e plataformas. Uma pessoa da nossa equipe de suporte criará uma declaração de software para você.

## Como usar a Declaração de Software? {#how-to-use-ss}

Depois de obter a Instrução de Software, você precisará passá-la como um parâmetro no construtor Access Enabler. Recomendamos hospedar a Declaração de Software em um local remoto. Dessa forma, você pode revogar e alterar facilmente a Declaração de Software sem lançar uma nova versão do aplicativo.

## Criar e usar um deep link para seu aplicativo {#create}

No Android, use como valor de deep link o reverso do nome de domínio selecionado quando você criou a Declaração de software

O deep link criado deve ter um valor exclusivo no dispositivo Android. Quando vários aplicativos usam o mesmo valor de deep link, os fluxos de autenticação e logout interferem.

## Como usar a Declaração de software e o deep link {#use-both}

No arquivo de recursos do aplicativo `strings.xml` adicione o seguinte código:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
