---
title: Compreensão das IDs de usuário
description: Compreensão das IDs de usuário
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---

# Compreensão das IDs de usuário {#understanding-user-ids}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Conceitualmente, cada usuário que inicia um fluxo de direitos é associado a uma única ID de usuário. No entanto, durante um fluxo de direitos, essa ID de usuário pode ser apresentada de diferentes maneiras, dependendo da API da qual você obtém a ID.

O `sessionGUID` no Token de Mídia Curta é a forma segura da ID de Usuário, que está disponível por meio da chamada `sendTrackingData()`. Em todas as integrações atuais, esse é um GUID persistente para o usuário no tempo e nos dispositivos. A origem da GUID começa com a ID de usuário da resposta de autenticação proveniente do MVPD. No entanto, alguns MVPDs podem mudar de ideia no futuro e começar a enviar um GUID transitório. Se um programador quiser garantir que a ID de usuário de origem MVPD na resposta AuthN seja persistente, ele deverá providenciar isso em seus contratos com os MVPDs.

Estas são as diferentes maneiras de representar a ID de usuário nas APIs de autenticação da Adobe Pass:

| Propriedade | Finalidade | Com hash | Assinado digitalmente | Descrição |
| --- | --- | --- | --- | --- |
| propriedade de GUID sendTrackingData() | Rastreamento/análise | Sim | Não | - A ID de usuário MVPD, com hash por Adobe. A ID de Usuário não é rastreável de volta à origem para o MVPD. </br> </br> - Este formulário da ID não é assinado digitalmente, portanto, não é seguro para prevenção de fraudes. No entanto, é bom o suficiente para análises.  </br> </br> - Este formulário da ID de Usuário é fornecido no lado do cliente em todos os eventos gerados pela Autenticação Adobe Pass no fluxo AuthN/AuthZ. |
| Propriedade sessionGUID do token de mídia curta | Rastreamento de fraude de uso simultâneo | Sim | Sim | - É o mesmo que a ID de usuário via sendTrackingData(), no entanto, este é assinado digitalmente para proteger sua integridade e é bom o suficiente para ser usado para o rastreamento de fraudes. </br> </br> - Ele deve ser processado no lado do servidor depois de usar nossa biblioteca de validação e pode ser analisado em busca de padrões de fraude antes de liberar o fluxo de vídeo para o cliente.  Fazer qualquer uma dessas tarefas depende do Programador. |
| propriedade getMetadata() userID | Vinculação de contas, investigação de fraude com MVPD | Não | Não | - Essa propriedade permite que o Adobe exponha a ID de usuário MVPD real de origem ao programador. </br> </br> - Na configuração do Adobe, ele pode ser definido como criptografado ou não (dependendo da preferência do MVPD). Se estiver criptografado, ele será criptografado com a chave pública do certificado do programador fornecido para o Adobe, para que não seja exposto claramente ao cliente. </br> </br> - Fornece ao Programador a ID de Usuário real do MVPD, portanto, é algo que pode ser usado para vinculação de contas ou investigação de fraude diretamente com o MVPD. |


**Em conclusão**

* Em geral, o MVPD fornece uma ID exclusiva persistente <u> e a transmite para o Adobe na autenticação bem-sucedida</u>. Geralmente é consistente em todas as redes. A exceção é Comcast MVPD, que fornece uma ID de usuário diferente para cada canal.

* A ID de usuário MVPD não contém PII e NÃO é um número de conta. Ela não precisa ser exposta em um formato criptografado, pois validamos com todos os MVPDs que nenhuma PII está sendo enviada.

A forma como você usa a ID de usuário depende do caso de uso:

* Se você precisar dele para rastreamento/análise, o local mais prático é obtê-lo de `sendTrackingData()`.
* Se você precisar dele no lado do servidor para lançamento de fluxo, fraude ou dados operacionais, é possível obtê-lo no validador de Token de mídia.
* Se precisar dela para vinculação de contas e fraude mais profunda, verifique a disponibilidade com seu contato do Adobe.
