---
title: Caminho de atualização do AccessEnabler iOS/tvOS 3.7.0
description: Caminho de atualização do AccessEnabler iOS/tvOS 3.7.0
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Caminho de atualização do AccessEnabler iOS/tvOS 3.7.0 (herdado) {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

As alterações de armazenamento de chaveiros da [nova versão do AccessEnabler 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md) são incompatíveis com a implementação de armazenamento de chaveiros da versão do AccessEnabler anterior à 3.7.0.

O caminho de atualização de um aplicativo que adota a nova versão 3.7.0 do AccessEnabler migrará todos os tokens de versões anteriores do armazenamento de Chaves. Portanto, os usuários finais **não devem sofrer perda de sessões de autenticação/autorização** durante o processo de atualização da estrutura AccessEnabler.

## Limitações conhecidas

Algumas limitações, descritas abaixo, podem ser encontradas pelos implementadores.


1. O SSO regular (Adobe) não funcionará entre um aplicativo usando o AccessEnabler versão 3.7.0 e um aplicativo usando o AccessEnabler versão/s inferior a 3.7.0, mesmo para aplicativos desenvolvidos pelo mesmo fornecedor.

   >[!IMPORTANT]
   >
   >* O SSO de nível de sistema (Apple) não será afetado!
   >
   >* O SSO regular (Adobe) continuará a funcionar se ambos os aplicativos forem desenvolvidos pelo mesmo fornecedor e usarem versões do AccessEnabler inferiores a 3.7.0!
   >
   >* O SSO regular (Adobe) funcionará se ambos os aplicativos forem desenvolvidos pelo mesmo fornecedor e usarem o AccessEnabler versão 3.7.0!


1. No caso de fazer downgrade de um aplicativo usando o AccessEnabler versão 3.7.0 para uma versão inferior do AccessEnabler, os novos tokens gerados não serão migrados. Portanto, os usuários finais podem enfrentar a perda de sessões de autenticação/autorização, sem esperar essa perda.

   >[!IMPORTANT]
   >
   >* Os usuários finais autenticados por SSO de nível de sistema (Apple) não serão afetados.
   >* Os usuários finais que já foram autenticados antes da atualização para o novo aplicativo usando o AccessEnabler versão 3.7.0 não serão afetados!

1. Na situação de fazer downgrade de um aplicativo usando o AccessEnabler versão 3.7.0 para uma versão inferior do AccessEnabler, os tokens excluídos não serão reconhecidos. Portanto, os usuários finais podem enfrentar a presença de sessões de autenticação/autorização, sem esperar essa presença.
