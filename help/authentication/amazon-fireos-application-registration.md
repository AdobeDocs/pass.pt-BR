---
title: Registro do aplicativo Amazon FireOS
description: Registro do aplicativo Amazon FireOS
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Registro do aplicativo Amazon FireOS {#amazon-fireos-application-registration}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

## Introdução {#intro}

A partir da versão 3.0 do SDK FireOS AccessEnabler, estamos alterando o mecanismo de autenticação com servidores Adobe Access. Em vez de usar uma chave pública e um sistema secreto para assinar o requestorID, estamos introduzindo o conceito de uma string de Declaração de Software que pode ser usada para obter um token de acesso usado posteriormente para todas as chamadas que o SDK faz aos nossos servidores. Além de uma Declaração de Software, você também precisará criar um deep link para o seu aplicativo.

Mais informações, consulte [Registro de cliente dinâmico](/help/authentication/dynamic-client-registration.md)

## O que é uma Declaração de Software? {#what}

Uma Declaração de Software é um token JWT que contém informações sobre seu aplicativo. Cada aplicativo deve ter uma Declaração de Software exclusiva, usada por nossos servidores para identificar o aplicativo no sistema Adobe. A Instrução de Software precisa ser passada ao inicializar o SDK do AccessEnabler e será usada para registrar o aplicativo no Adobe. Após o registro, o SDK receberá uma ID do cliente e um segredo do cliente que serão usados para obter um token de acesso. Qualquer chamada feita pelo SDK para nossos servidores exigirá um token de acesso válido. O SDK é responsável por registrar o aplicativo, obter e atualizar o token de acesso.

**Nota:** As instruções de software são específicas do aplicativo e uma instrução de software individual não pode ser usada para mais de um aplicativo. Observe que isso também se aplica a aplicativos que oferecem acesso a vários canais.

## Como obter uma Declaração de Software? {#how-to}

### Se você tiver acesso ao Painel TVE do Adobe:

- Abra o navegador e acesse <https://console.auth.adobe.com>
- Navegue até `Channels` e selecione seu canal.
- Navegue até `Registered Applications` Guia.
- Clique em `Add new application`.
- Forneça um nome e uma versão para o aplicativo e selecione as plataformas em que ele estará disponível (por exemplo, Android no nosso caso).
- Forneça um Nome de domínio escolhendo em uma lista de domínios já configurados para seu Programador.
- Envie suas alterações ao servidor e navegue de volta para a guia Aplicativos registrados do canal.
- Você deve ver uma lista com todos os aplicativos registrados. Clique em `Download` no aplicativo recém-criado. Observação: Talvez seja necessário aguardar alguns minutos para que a Declaração de Software esteja pronta para download.
- Um arquivo de texto será baixado. Use seu conteúdo como sua Declaração de Software.

Mais informações, consulte [Gerenciamento dinâmico de registro de clientes](/help/authentication/dynamic-client-registration-management.md)

### Se você não tiver acesso ao Painel do Adobe TVE:

Enviar um tíquete para <tve-support@adobe.com>. Inclua todas as informações necessárias, incluindo canal, nome do aplicativo, versão e plataformas, e alguém de nossa equipe de suporte criará uma declaração de software para você.

## Como usar a Declaração de Software? {#use}

Depois de obter a Instrução de Software, você precisará passá-la como um parâmetro no construtor Access Enabler. Recomendamos hospedar a Declaração de Software em um local remoto. Dessa forma, você pode revogar e alterar facilmente a Declaração de Software sem lançar uma nova versão do seu aplicativo.

## Como usar a Declaração de Software {#use-both}

No arquivo de recursos do aplicativo `strings.xml` adicione o seguinte código:

```XML
<string name="software_statement">softwarestatement value</string>
```
