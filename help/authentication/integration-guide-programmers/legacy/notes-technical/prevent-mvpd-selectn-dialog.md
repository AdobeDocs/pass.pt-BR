---
title: Impedir que os MVPDs apareçam na Caixa de Diálogo de Seleção
description: Impedir que os MVPDs apareçam na Caixa de Diálogo de Seleção
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# (Herdado) Impedir que os MVPDs apareçam na Caixa de Diálogo de Seleção

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Problema {#issue-prevent-mvpd-sel-dialog}

Você precisa impedir que MVPDs específicos (&quot;lista de bloqueios&quot;) sejam exibidos no seletor do MVPD.


## Solução {#solution-prevent-mvpd-sel-dialog}

A solução é fazer uma lista de bloqueios quando `displayProviderDialog()` é chamado.

Por exemplo, se você deseja que CableCompany_1 e CableCompany_2 não sejam exibidos no seletor de MVPD, é possível fazer algo semelhante ao mostrado no exemplo a seguir.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```
