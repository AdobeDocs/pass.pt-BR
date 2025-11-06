---
title: Cookbook da API REST (servidor para servidor)
description: Servidor do guia da API rest para o servidor.
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# Cookbook da API REST (herdada) (servidor para servidor) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Visão geral {#overview}

O objetivo deste documento de guia é detalhar as práticas recomendadas para implementar a Autenticação do Adobe Pass usando as arquiteturas de servidor para servidor.  Ele fornece requisitos básicos, implementação de fluxo passo a passo e considerações gerais para ambientes de produção e operação.

### Mecanismo de limitação

A API REST de Autenticação do Adobe Pass é regida por um [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).


## Componentes {#components}

Em uma solução de servidor para servidor em funcionamento, os seguintes componentes estão envolvidos:


| Tipo | Componente | Descrição |
| --- | --- | --- |
| Dispositivo de transmissão | Aplicativo de transmissão | O aplicativo Programador que reside no dispositivo de transmissão do usuário e reproduz o vídeo autenticado. |
| | \[Opcional\] Módulo AuthN | se o dispositivo de transmissão tiver um agente do usuário (ou seja, navegador da Web), o módulo AuthN será responsável pela autenticação do usuário no MVPD IdP. |
| \[Opcional\] Dispositivo AuthN | Aplicativo AuthN | se o dispositivo de transmissão não tiver um agente de usuário (ou seja, navegador da Web), o aplicativo AuthN será um aplicativo Web de programador acessado de um dispositivo separado do usuário usando um navegador da Web. |
| Infraestrutura do programador | Serviço de programador | Um serviço que vincula o dispositivo de transmissão ao serviço do Adobe Pass para obter decisões de autenticação e autorização. |
| Infraestrutura Adobe | Serviço Adobe Pass | Um serviço que se integra ao MVPD IdP e ao Serviço AuthZ e fornece decisões de autenticação e autorização. |
| Infraestrutura MVPD | MVPD IdP | Um terminal da MVPD que fornece um serviço de autenticação baseado em credenciais para validar a identidade do usuário. |
| | Serviço MVPD AuthZ | Um terminal MVPD que fornece decisões de autorização com base nas assinaturas do usuário, controles dos pais etc. |

## Fluxos {#flows}

### Registro dinâmico de cliente (DCR)


O Adobe Pass usa DCR para proteger as comunicações do cliente entre um aplicativo ou servidor do programador e os serviços da Adobe Pass. O fluxo do DCR é separado e descrito na [Documentação de Visão geral do Registro Dinâmico de Clientes](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


### Autenticação (authN)

O fluxo de autenticação é usado para permitir que um usuário se identifique
à MVPD para determinar se o usuário tem uma conta válida.

1. O usuário inicia o aplicativo Dispositivo de streaming e tenta fazer logon ou visualizar conteúdo protegido.
2. O aplicativo Dispositivo de transmissão faz uma solicitação ao Serviço do programador para determinar se o dispositivo já está autenticado.
3. O Serviço do programador registra o aplicativo usando DCR.
4. O Serviço Programador verifica o status de authN do Dispositivo de Streaming chamando a API **checkauthn** do Serviço Adobe Pass.
5. Caso a chamada **checkauthn** retorne o status de autenticação do Dispositivo do usuário, o aplicativo poderá prosseguir para o Fluxo de autorização.
6. Caso a chamada **checkauthn** retorne o status de que o Dispositivo do usuário NÃO está autenticado, o aplicativo deve aguardar uma solicitação do usuário fazer logon.
7. Quando o usuário solicita o logon diretamente (por exemplo, seleciona o botão de logon) ou indiretamente (por exemplo, seleciona o conteúdo protegido quando ainda não está autenticado), o aplicativo Dispositivo de streaming faz uma solicitação ao Serviço do programador para iniciar a autenticação do usuário. O Serviço Programador solicita e recebe um código de registro exclusivo (regcode) chamando a API **regcode** do Serviço Adobe Pass.
8. O Serviço de Programador também recupera a lista de MVPDs e atributos atuais chamando a API **config** do Serviço Adobe Pass. Observação: essa API também pode ser chamada anteriormente no fluxo e armazenada em cache.
9. O Serviço do programador retorna o regcode para o aplicativo Dispositivo de transmissão e para a lista de MVPD processada solicitada na etapa \#7. Observação: o formato de lista do MVPD processado é especificado pelo Programador e pode ser filtrado para permitir ou bloquear explicitamente MVPDs específicos (ou seja, listas de permissão ou de bloqueio).
10. Se for diferente do Dispositivo de autenticação (ou seja, &quot;segunda tela&quot;), por escolha ou necessidade (ou seja, o Dispositivo de transmissão não é compatível com um Agente do usuário), o Dispositivo de transmissão deverá exibir o regcode e um URI para que o usuário acesse o Aplicativo de autenticação. O usuário digita o URI no Agente do Usuário no Dispositivo AuthN para iniciar o Aplicativo AuthN e digita o regcode nesse aplicativo. Se o Dispositivo de transmissão for o mesmo que o Dispositivo AuthN, o regcode poderá ser transmitido programaticamente para o Módulo AuthN.
11. O módulo AuthN inicia a autenticação do usuário com o MVPD exibindo um seletor de MVPD. Depois que o usuário seleciona o MVPD, o Módulo AuthN chama **authenticate** com o regcode, que redireciona o Agente do Usuário para o MVPD IdP. Quando o usuário é autenticado com êxito na MVPD, o Agente do usuário é redirecionado de volta por meio do Serviço da Adobe Pass, em que a autenticação bem-sucedida é registrada com o regcode e, em seguida, redirecionada de volta para o Módulo de autenticação.
12. Se o dispositivo de transmissão for diferente do dispositivo de autenticação, o dispositivo de autenticação deverá exibir uma mensagem de autenticação bem-sucedida para o usuário e as etapas para continuar (por exemplo, &quot;Success\! Agora você pode voltar ao console de jogos para continuar ([...\]&quot;). Se o Dispositivo de transmissão for o mesmo que o Dispositivo de autenticação, o Dispositivo de transmissão pode detectar programaticamente a conclusão da autenticação.



O diagrama a seguir ilustra o fluxo de autenticação:

![](../../../../assets/authn-flow.png)

### Autorização (authZ)

O fluxo de autorização é usado para determinar se um usuário tem direito a acessar o conteúdo solicitado.

1. Toda vez que o usuário tenta visualizar o conteúdo protegido no aplicativo Dispositivo de transmissão, o aplicativo Dispositivo de transmissão chama o Serviço do programador identificando o conteúdo e solicitando a permissão e as informações necessárias para iniciar o fluxo.
1. O Serviço Programador chama a API **authorize** da Adobe Pass transmitindo a ID do Recurso junto com outros parâmetros necessários. O Serviço da Adobe chama o Serviço MVPD AuthZ com a ID do recurso, recebe e recebe uma decisão de autorização que, em seguida, é repassada ao Serviço do programador. Essa decisão de autorização será armazenada em cache pelo Serviço do Adobe Pass por um período configurável. Nas chamadas **authorize** subsequentes do Serviço de Programador para o Serviço Adobe Pass, o valor em cache será retornado enquanto for válido.
1. Se a autorização for concedida, o Serviço de Programação deverá chamar a API **/tokens/media** do Adobe Pass, que retornará um token de mídia assinado. O Serviço de programador deve validar o token de mídia usando a biblioteca Media Token Verifier (JAR). Se for válido, o Serviço do programador deverá retornar a permissão e a permissão necessária para iniciar o fluxo (por exemplo, o URL do fluxo) solicitado na etapa \#1.
1. Se a autorização for negada, a chamada **authorize** retornará um código de erro e uma descrição para o Serviço do Programador. O Serviço do programador deve retornar o código de erro e a descrição (ou uma mensagem modificada pelo programador) para a solicitação na etapa \#1.

O diagrama a seguir ilustra o fluxo de autorização:

![](../../../../assets/authz-flow.png)

### Sair

O fluxo de logout permite que um usuário remova a identidade atual
associado ao aplicativo.

1. Quando o usuário solicita o logout (ou seja, remove do dispositivo a conta atual do MVPD associada ao aplicativo), o aplicativo Dispositivo de transmissão chama o Serviço do programador, informando para fazer logout do dispositivo.
1. O Serviço Programador deve chamar a API **logout** do Adobe Pass.

O diagrama a seguir ilustra o fluxo de logout:

![](../../../../assets/logout-flow.png)

### \[Opcional\] Pré-autorização (também conhecido como Pré-voo)

A pré-autorização pode ser usada para determinar rapidamente, a partir de um conjunto de recursos, aqueles que um usuário pode ter acesso.  O resultado desta chamada geralmente é usado para personalizar a interface do usuário de um usuário individual.

1. Depois que o usuário é autenticado, o dispositivo de streaming pode chamar o serviço de programador para solicitar o conteúdo para o qual o usuário tem direito a streaming.

1. O Serviço Programador deve chamar a API **pré-autorizar** do Adobe Pass com uma lista de IDs de Recursos, que são uma cadeia de caracteres simples que geralmente representa um canal que um usuário pode ter direito a transmitir. *Observação: Atualmente, a chamada* **&#x200B;**&#x200B;**&#x200B; pré-autorizar *está configurada para limitar a lista a cinco (5) IDs de Recursos. Quando são necessários mais de cinco recursos, várias chamadas* &#x200B;**&#x200B;**&#x200B;** pré-autorizadas *podem ser feitas, ou a chamada pode ser configurada para aceitar mais de cinco recursos com um contrato dos MVPDs. Os implementadores devem ter em mente o custo de uma chamada* ***pré-autorizada*** *tanto para os recursos da MVPD quanto para o tempo de resposta ao Programador e estruturar seu uso da chamada criteriosamente.*

1. A chamada **pré-autorizar** responderá ao Serviço do Programador com um objeto JSON contendo um valor TRUE ou FALSE para cada ID de Recurso na solicitação que indica se o usuário tem direito ao canal associado ou não. *Observação: se uma MVPD não fornecer uma resposta para uma determinada ID de Recurso (por exemplo, devido a erros de rede ou tempos limite), o valor padrão será FALSE.*

1. O Serviço de Programador deve usar a resposta de chamada **pré-autorizar** para criar uma resposta personalizada definida pelo Programador para o Dispositivo de Streaming, normalmente para personalizar a apresentação ao usuário com base em suas autorizações.

O diagrama a seguir ilustra o fluxo de pré-autorização:

![](../../../../assets/preauthz-flow.png)


### \[Opcional\] Metadados

Os metadados podem ser usados para recuperar informações do usuário compartilhadas pela MVPD.
Exemplos disso podem incluir ID de usuário, código postal, etc.

1. Depois que o usuário é autenticado, o Serviço de Programador pode chamar a API **usermetadata** do Adobe Pass para solicitar informações sobre o usuário autenticado.

1. A resposta incluirá todos os metadados disponíveis para o usuário especificado. Os campos específicos são configurados separadamente para cada integração de Programador/MVPD.

O diagrama a seguir ilustra o fluxo de pré-autorização:



![](../../../../assets/user-metadata-api-preauthz.png)



## Ambientes e requisitos funcionais{#environments}



Um Programador deve criar pelo menos dois ambientes: um para produção e um ou mais para preparo.


### Produção

O ambiente de produção deve estar altamente disponível e dimensionado de forma adequada para picos grandes ou inesperados (por exemplo, esportes ao vivo, quebra)
notícias).



O serviço Adobe Pass é executado em vários data centers geograficamente dispersos nos EUA.  Para obter o melhor tempo de resposta (ou seja, a latência mais baixa) do serviço Adobe Pass, o programador também deve criar um serviço geograficamente disperso semelhante
infraestrutura.


O serviço Programador deve limitar o cache DNS a no máximo 30 s caso o Adobe precise redirecionar o tráfego. Isso pode ocorrer se um data center se tornar indisponível.


O Programador deve fornecer o intervalo de IP público do ambiente de produção. Eles serão inseridos em uma lista de permissões de IPs na infraestrutura do Adobe Pass para acesso e gerenciados pelas políticas justas de uso de API da Adobe.

### Estágios

O ambiente de preparo pode ser mínimo, mas deve incluir todos os componentes do sistema e a lógica de negócios. Ela deve funcionar de forma semelhante à produção e permitir o teste de versões fora da produção. Idealmente, o ambiente de preparo pode ser conectado aos ambientes de teste do Adobe Pass para uso pelo Programador e pela Adobe quando necessário, para que possamos ajudar no teste e na solução de problemas.

### Requisitos funcionais

O serviço Programador deve transmitir informações precisas de identificação do dispositivo para o qual está executando os fluxos. Além disso, o serviço Programador deve passar o IP do dispositivo para o qual está executando os fluxos (em um cabeçalho x-forwarded-for) junto com a porta de origem da conexão (no campo device info ):

    **X-Forwarded-For : \&lt;client\_ip\>**
    
    onde \&lt;client\_ip\> é o endereço IP público do cliente
    
    
    
    O cabeçalho precisa ser adicionado em **regcode** e **authorize** chamadas
    
    Exemplos:
    
    POST /reggie/v1/{req\_id}/regcode HTTP/1.1
    
    X-Forwarded-For:203.45.101.20
    
    
    
    GET /api/v1/authorize HTTP/1.1
    
    X-Forwarded-For:203.45.101.20



O serviço Programador deve enviar dados e formatação exigidos por MVPDs individuais ou aplicativos integrados (por exemplo, IP do dispositivo, porta de origem, informações do dispositivo, MRSS, dados opcionais, como ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.


O serviço Programador deve respeitar os TTLs authN e authZ ao armazenar em cache e invalidar as sessões authN ou authZ quando notificadas.

O Programador deve manter os certificados compartilhados com a Adobe.
