---
title: Implementação da API sem cliente - Erro códigos / Mensagens com motivo provável / causa
description: Implementação da API sem cliente - Erro códigos / Mensagens com motivo provável / causa
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# Implementação da API sem cliente - Erro códigos / Mensagens com motivo provável / causa {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>O conteúdo neste página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe Systems. Não é permitida nenhuma utilização não autorizada.

</br>


## Erro: Não autorizado

### Causas:

1. Cabeçalho de autorização ausente no POST
1. Problema com cabeçalho de autorização - verifique se solicitação hora está em milissegundos.

## Erro: SC 400 durante a autenticação

### Causas:

1. O servidor não encontrou o código de registro, que foi criado para um solicitante e um ambiente específicos.
1. Você pode estar entrando em problemas de script entre domínios
1. A falsificação adequada deve ser adicionada ao arquivo /etc/hosts

## Erro: 400 Solicitação inválida

### Causas:

1. URL mal-formado para POST/GET
1. SAMLAssertionParserException : a asserção SAML criptografada não pôde ser descriptografada no final de Adobe Systems

## Erro: 403 Proibido

### Causas:

1. Muitas solicitações rápidas - uma característica do gerenciamento de API para evitar ataques do DoS.
2. Se estiver usando ambiente anteriores, adicione a falsificação, caso contrário, verifique se a falsificação foi removida do arquivo /etc/hosts

## Erro: não foi possível fazer logon no MVPD página

### Causas:

1. O nome de usuário e o senha não correspondem
2. O logon pode ter sido desativado
3. Verificar se o logon é para produção ou preparo


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
