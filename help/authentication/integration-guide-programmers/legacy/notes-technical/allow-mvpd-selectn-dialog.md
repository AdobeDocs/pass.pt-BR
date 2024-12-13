---
title: Permitir MVPDs na Caixa de Diálogo de Seleção
description: Permitir MVPDs na Caixa de Diálogo de Seleção
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# (Herdado) Permitir MVPDs na Caixa de Diálogo de Seleção {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Problema {#issue}

O programador pode testar ou verificar a experiência do usuário de novas integrações do MVPD antes de tornar públicas para os usuários finais.

## Solução {#solution}

No retorno de chamada `displayProviderDialog()`, a Autenticação do Adobe Pass retorna todos os MVPDs integrados ao Programador selecionado (ID do Solicitante). Mas o Programador pode aplicar um filtro na matriz de retorno de MVPDs e exibir apenas aqueles que estão em ambas as listas.

## Exemplo {#example}

Este exemplo demonstra como exibir somente CableCompany_1 e CableCompany_2 na caixa de diálogo do seletor do MVPD e não mostrar CableCompany_NewIntegration.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
