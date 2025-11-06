---
title: Perguntas e respostas sobre certificados
description: Perguntas e respostas sobre certificados
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Perguntas e respostas sobre certificados (herdados) {#certificates-q}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

**T1:** É possível registrar certificados no iOS e no Android?

**A:** O certificado do iOS e do Android é o mesmo na configuração atual. O certificado nativo é usado para ambas as plataformas.

</br>

**T2:** os mesmos certificados do iOS podem ser usados como certificados Primários e de Backup nos ambientes de Produção e Preparo? Se isso não for recomendado, você pode oferecer uma explicação?

**A:** Não há motivo para configurar o mesmo certificado como certificado Primário e de Backup. Temos o conceito de certificados primários e de backup para que possamos configurar vários certificados para um programador caso o certificado primário expire ou seja revogado. Ter um certificado de backup dará aos programadores tempo para alterar o principal sem afetar o ambiente da versão. Mas você pode usar o mesmo conjunto de certificados primários e de backup para os perfis de produção e de preparo.

</br>

**Q3:** É necessário um novo certificado para as páginas da Web que usarão o novo TempPass flexível?

**A:** O certificado (e qualquer certificado) está configurado no nível Empresa de Mídia e Programador. O FlexibleTempPass é um MVPD, não é necessário configurar nenhum certificado para ele, portanto, se você estiver integrando um Programador existente com o TempPass flexível, o certificado já configurado no nível Programador/Empresa de Mídia será usado.
