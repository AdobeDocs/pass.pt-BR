---
title: Redefinir Temp Pass no iOS
description: Redefinir Temp Pass no iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Redefinir Temp Pass no iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

O aplicativo de demonstração do iOS inclui uma tela dedicada para redefinir o TTL de passagem temporária. As seguintes informações são necessárias para a operação de redefinição:

- **Ambiente:** especifica o ponto de extremidade do servidor de passagem de TV por Adobe que receberá a chamada de rede de passagem temporária de redefinição. Valores possíveis: **Pré-Igual** (*mgmt-prequal.auth-staging.adobe.com*), **Versão** (*mgmt.auth.adobe.com* ou **Personalizado** (reservado para testes internos de Adobe).
- **Token de portador OAuth2:** o token OAuth2 é necessário para autorizar o Programador para a autenticação de TV por Adobe. Esse token pode ser obtido do endpoint OAuth2 de autenticação de TV por assinatura dedicada (por exemplo, *curl -u &quot;\&lt;consumer _key=&quot;&quot;>:\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **ID do Solicitante:** o identificador exclusivo do programador atual. Esse valor é lido na tela principal do aplicativo de demonstração (o campo do solicitante).
- **ID de passagem temporária:** o identificador exclusivo para o MVPD de passagem temporária.
- **ID do dispositivo:** ID de dispositivo com hash computada pelo aplicativo de demonstração.
- **Chave genérica:** alguns MVPDs de Temp Pass (ou seja, a próxima funcionalidade extensível de Temp Pass) oferecem suporte a uma chave genérica para redefinir Temp Pass (junto com a ID do Dispositivo).

Todos os parâmetros acima (exceto o *Chave genérica*) são obrigatórios. Este é um exemplo de parâmetros e a chamada de rede associada que será executada pelo aplicativo de demonstração (o exemplo está no formato de um comando *curl *):

- **Ambiente:** Versão (*mgmt.auth.adobe.com*)
- **Token de portador OAuth2:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **ID do programador:** REF
- **ID de passagem temporária:** TempPassREF
- **ID do dispositivo:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **Chave genérica:** null (nenhum valor fornecido)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

uma solicitação HTTP DELETE será feita para o **/reset** endpoint, transmitindo o *Token de portador OAuth2* no cabeçalho Autorização e no campo *ID do dispositivo*, *ID do Solicitante* e *ID de Aprovação Temporária (ID do MVPD)* como parâmetros.

Se o Programador fornecer um valor para a variável *Chave genérica*, outra chamada HTTP será executada (desta vez para o **/reset/generic** ponto de extremidade), transmitindo o *Chave genérica* dentro do *key* parâmetro de solicitação.

Por exemplo, definir a variável *Chave genérica* para um hash de endereço de email (para MVPDs Temp Pass que oferecem suporte a esse tipo de funcionalidade) gerará a seguinte chamada HTTP (o email é `user@domain.com` seu hash SHA-256 é `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

É importante enfatizar que a redefinição do Temp Pass no aplicativo de demonstração pode não ter o mesmo efeito para um aplicativo programador no mesmo dispositivo. Isso se deve ao fato de que a ID do dispositivo (conforme calculada pelo aplicativo de demonstração e pelo AccessEnabler) pode não ser a mesma para todos os aplicativos no dispositivo:

- iOS 6 e inferior: a ID do dispositivo é calculada usando o endereço do MAC (que é exclusivo para todos os aplicativos), portanto, redefinir o Temp Pass no aplicativo de demonstração a redefinirá em todos os outros aplicativos do programador no dispositivo.

- iOS 7 e superior: a ID do dispositivo é calculada com base no valor IDFV (ID para fornecedor), que é exclusivo para todos os aplicativos que têm o mesmo prefixo de ID do pacote (ou seja, todos os componentes, exceto o último). Como o aplicativo de demonstração e um aplicativo de programador têm IDs de pacote diferentes, redefinir o Temp Pass no aplicativo de demonstração não terá efeito em um aplicativo de programador.

O último caso de uso (iOS 7 e superior) é o mais comum, então vamos ver como os programadores podem redefinir o Temp Pass para seus aplicativos nessa situação. Há várias opções:

1. Porte o código do aplicativo de demonstração para o aplicativo programador. A variável *TempPassResetViewController* e *DeviceIdDemoApp* As classes contêm a lógica principal para redefinir o Temp Pass e podem ser facilmente modificadas e incluídas no aplicativo Programador.

1. Execute a solicitação HTTP para redefinir a Aprovação Temporária com *curl*. O parâmetro device\_Id pode ser obtido calculando o IDFV do aplicativo Programmer e aplicando um hash SHA-256 sobre ele (código de amostra no *DeviceIdDemoApp* classe).

1. Basta executar a redefinição do aplicativo de demonstração especificando o IDFV com hash do aplicativo do programador como o *Chave genérica*. Isso resultará em duas chamadas de rede: uma para redefinir o Temp Pass para o aplicativo Demo (irrelevante para o Programador) e outra para redefinir o Temp Pass para o aplicativo Programador.

Todas as opções acima são semelhantes, depende do Programador escolher uma dependendo da facilidade de implementação.
