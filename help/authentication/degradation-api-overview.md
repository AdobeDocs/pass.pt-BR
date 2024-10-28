---
title: Visão geral da API de degradação
description: Visão geral da API de degradação
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 95c3b1cbce4a591ce387ae3b242721e50ba2ddb1
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Visão geral da API de degradação {#degradation-api-overview}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Antes de usar a API de degradação, verifique se os seguintes pré-requisitos foram atendidos:
>
> * Obtenha as credenciais do cliente conforme descrito na documentação da API [Recuperar credenciais do cliente](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenha o token de acesso conforme descrito na documentação da API [Recuperar token de acesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte a documentação [Visão Geral do Registro Dinâmico do Cliente](./dcr-api/dynamic-client-registration-overview.md) para obter mais informações sobre como criar um aplicativo registrado e baixar a instrução de software.

## Informações gerais {#general_info}

>[!NOTE]
>
>Essa API geralmente não está disponível. Entre em contato com seu representante da Adobe para obter atualizações de disponibilidade.

Esse recurso fornece a qualquer uma das três partes principais em uma integração (Programadores, MVPDs e Adobe) a capacidade de ignorar temporariamente endpoints específicos de Autenticação e Autorização do MVPD. Normalmente, é o Programador que inicia essa ação, mas independentemente de quem aciona um evento de degradação, a ação depende de acordos previamente acordados com os MVPDs afetados.

O principal caso de uso desse recurso ocorre durante esportes ao vivo ou grandes eventos. Em tais cenários de tráfego alto, é possível que a carga em um endpoint de MVPD específico se torne muito alta, resultando em tempos de resposta muito longos para os usuários. Para preservar uma boa experiência do usuário durante esse cenário, o Programador pode decidir acionar uma regra de degradação que pode temporariamente autenticar/autorizar automaticamente os usuários ou desativar um MVPD, removendo-o da lista de MVPDs disponíveis.

Uma regra de degradação é aplicada somente por um período fixo. Embora os principais clientes desse recurso sejam canais de esportes e canais de notícias ao vivo, qualquer Programador pode querer ter acesso a esse recurso, já que os serviços do MVPDs ficam inativos de tempos em tempos.

Notas de degradação:

- Esse recurso foi projetado para ser usado junto com a API de monitoramento de uso, que fornece informações em tempo real sobre o número de autenticações e autorizações por MVPD, latência média de autorização e outras métricas necessárias para obter uma visão geral completa do serviço.
- Esse recurso não permite ignorar o serviço de Autenticação do Adobe Pass. Se a Autenticação Adobe Pass estiver inativa, não há nenhum mecanismo no serviço que possa ser usado para permitir que os usuários vejam o conteúdo. No entanto, os sites ou aplicativos podem rotear pela Autenticação do Adobe Pass sozinhos.
- Adobe não irá desencadear a degradação diretamente, a decisão deve sempre residir com um programador específico que concordou com essas condições com MVPDs. No futuro, a Autenticação do Adobe Pass poderá ser proativa no acionamento de regras de degradação se for possível alcançar contratos (proteção do SLA) com MVPDs.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
