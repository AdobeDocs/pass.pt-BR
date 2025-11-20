---
title: Casos de uso
description: Casos de uso no monitoramento de simultaneidade.
exl-id: 6cc30bb6-e985-4d9a-9f99-a7f04ae8deb7
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Casos de uso {#use-cases}

O principal caso de uso do Serviço de contagem de fluxo é contar o número de fluxos de vídeo simultâneos assistidos por um usuário e fornecer uma decisão sobre o uso simultâneo para a mesma id de conta.

Para monitorar o uso pelo assinante, é necessário um serviço centralizado que possa agregar a atividade do usuário independentemente de ela ocorrer no site ou aplicativo do programador, no portal de conteúdo do MVPD ou em uma propriedade sindicalizada.

Os principais casos de uso compatíveis com esse serviço centralizado devem ser:

1. Assim que um assinante começar a assistir a um vídeo, o aplicativo poderá **inicializar uma sessão de streaming** e iniciar os dados de **atividade de relatório**.
1. No mesmo serviço central, outra instância receberá ***decisões de CM*** - caso o aplicativo tenha uma ou mais políticas registradas no serviço CM, o serviço responderá com uma decisão de acesso com base na atividade atual.

## Casos de uso comuns {#common-use-cases}

### Limitação básica de fluxo

Limite o número de fluxos simultâneos por assinante em todos os seus aplicativos.

### Restrições baseadas em dispositivo

Permitir apenas um determinado número de fluxos por tipo de dispositivo (celular, tablet, TV, etc.).

### Regras específicas de conteúdo

Aplique limites diferentes para conteúdo ao vivo e conteúdo VOD.

### Políticas baseadas em localização

Restringir a transmissão com base na localização geográfica ou no tipo de rede.

## Criação de uma sessão {#create-session}

Essa chamada de API permite que o cliente crie uma nova sessão CM quando o usuário pressionar o botão &quot;reproduzir&quot; para assistir algum conteúdo. A resposta do servidor conterá o novo URL de fluxo (contendo a ID de fluxo) para mantê-lo ativo e o tempo em que o fluxo expirará. Espera-se que o aplicativo cliente relate a atividade pelos heartbeats. A chamada de inicialização de sessão deve incluir metadados na forma de pares de chave/valor enviados como dados de formulário (ou parâmetros de sequência de consulta). Além disso, a resposta também incluirá um sinalizador para indicar se a reprodução é &quot;compatível com a política&quot;. Se não estiver, a reprodução não será permitida.

## Atividade de relatórios {#reporting-activity}

Depois que uma sessão é criada, o aplicativo precisa enviar heartbeats regularmente para que esse fluxo permaneça ativo. Além disso, é recomendável que o aplicativo cliente pare o fluxo assim que o usuário interromper a reprodução, para que o fluxo não conte como ativo até o tempo limite.

A resposta da chamada de heartbeat pode permitir que o aplicativo cliente continue a reprodução do vídeo (quando for compatível com a política) ou pode instruí-lo a interromper a reprodução do vídeo. No caso de o fluxo de vídeo não ser compatível, o aplicativo cliente deve interrompê-lo. A resposta fornecerá informações para que o aplicativo cliente exiba uma mensagem de erro e/ou ações disponíveis para o usuário continuar a reprodução.
