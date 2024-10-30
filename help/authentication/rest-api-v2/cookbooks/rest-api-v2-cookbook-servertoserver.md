---
title: Cookbook REST API V2 (servidor para servidor)
description: Cookbook REST API V2 (servidor para servidor)
source-git-commit: 87d4d95a3bf4ace68bc71ca700b09da14ee316f4
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# Cookbook REST API V2 (servidor para servidor) {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


## Visão geral {#overview}

O objetivo deste documento de guia é detalhar as práticas recomendadas para implementar a Autenticação do Adobe Pass usando a REST API V2 em uma arquitetura de servidor para servidor.  Ele fornece requisitos básicos, implementação de fluxo passo a passo e considerações gerais para ambientes de produção e operação.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/throttling-mechanism.md).


## Componentes {#components}

Em uma solução de servidor para servidor em funcionamento, os seguintes componentes estão envolvidos:


| Tipo | Componente | Descrição |
| --- | --- | --- |
| Dispositivo de transmissão | Aplicativo de transmissão | O aplicativo Programador que reside no dispositivo de transmissão do usuário e reproduz o vídeo autenticado. |
| | \[Opcional\] Módulo AuthN | Se o dispositivo de transmissão tiver um agente do usuário (ou seja, navegador da Web), o módulo AuthN será responsável pela autenticação do usuário no IdP do MVPD. |
| \[Opcional\] Dispositivo AuthN | Aplicativo AuthN | se o dispositivo de transmissão não tiver um agente de usuário (ou seja, navegador da Web), o aplicativo AuthN será um aplicativo Web de programador acessado de um dispositivo separado do usuário usando um navegador da Web. |
| Infraestrutura do programador | Serviço de programador | Um serviço que vincula o dispositivo de transmissão ao serviço do Adobe Pass para obter decisões de autenticação e autorização. |
| Infraestrutura Adobe | Serviço Adobe Pass | Um serviço que se integra ao MVPD IdP e ao Serviço AuthZ e fornece decisões de autenticação e autorização. |
| Infraestrutura MVPD | IdP MVPD | Um ponto de extremidade MVPD que fornece um serviço de autenticação baseado em credenciais para validar a identidade do usuário. |
| | Serviço MVPD AuthZ | Um terminal MVPD que fornece decisões de autorização com base nas assinaturas do usuário, controles dos pais etc. |


Os termos adicionais usados no fluxo são definidos no
[Glossário](/help/authentication/glossary.md).

O diagrama a seguir ilustra todo o fluxo:

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### Etapas para implementar a REST API V2 na arquitetura de servidor para servidor {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Para implementar a API REST V2 do Adobe Pass, é necessário seguir as etapas abaixo agrupadas em fases.

## 0. Pré-requisitos {#prerequisites}

Em implementações de servidor para servidor, o Aplicativo de Streaming e o Serviço de Programador precisam estabelecer um protocolo para que o Serviço de Programador possa:
* identificar exclusivamente o aplicativo de streaming no dispositivo
* agir em nome do aplicativo de streaming e se comunicar com o serviço do Adobe Pass
* colete e armazene informações sobre o aplicativo de streaming e o dispositivo, como endereço IP, porta de origem, informações do dispositivo para passá-lo para o Adobe Pass
* retornar decisões e instruções para o aplicativo de streaming

Parâmetros como ID de dispositivo, ID do cliente, segredo do cliente (definido abaixo) podem ser armazenados no Aplicativo de streaming ou no Serviço de programador.

## A. Fase de registro {#registration-phase}

### Etapa 1: Registrar seu aplicativo {#step-1-register-your-application}

Para que o aplicativo possa chamar a API REST V2 do Adobe Pass, ele precisa de um token de acesso exigido pela camada de segurança da API. Para implementações de servidor para servidor, o Serviço de programador pode se registrar em nome de uma instância do aplicativo.
A ID do cliente e os valores de segredo do cliente precisam ser obtidos para cada dispositivo de streaming.

Para obter o token de acesso, o Serviço de Programação pode agir em nome de um Aplicativo de Streaming e precisa seguir as etapas descritas: [Registro Dinâmico do Cliente](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B. Fase de autenticação {#authentication-phase}

### Etapa 2: verificar perfis autenticados existentes {#step-2-check-for-existing-authenticated-profiles}

O Serviço Programador verifica, em nome do Aplicativo de Streaming, os perfis autenticados existentes: `/api/v2/{serviceProvider}/profiles` ([Recuperar perfis autenticados](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Se nenhum perfil for encontrado e o aplicativo de Streaming implementar um fluxo TempPass
   * Siga a documentação sobre como implementar [Fluxos de acesso temporário](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Se nenhum perfil for encontrado, o aplicativo de transmissão implementará um Fluxo de autenticação
   * <b>Etapa 2.a:</b> o Serviço de Programador recuperou a lista de MVPDs disponíveis para serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Recuperar lista de MVPDs](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) disponíveis)
   * o Serviço de Programadores pode implementar a filtragem na lista de MVPDs e exibir apenas MVPDs destinados ao ocultar outros (TempPass, MVPDs de teste, MVPDs em desenvolvimento etc.)
   * o Serviço de programador deve retornar uma lista MVPD filtrada para o Aplicativo de streaming exibir o seletor, o usuário seleciona o MVPD
   * com o MVPD selecionado no Aplicativo de Streaming, o Serviço Programador cria uma sessão: <b>/api/v2/{serviceProvider}/sessions</b><br>
([Criar sessão de autenticação](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * um CÓDIGO e um URL a serem usados para autenticação são retornados
      * se um perfil for encontrado, o Serviço de Programação poderá prosseguir para <a href="#preauthorization-phase">C. Fase de pré-autorização</a>
   * o Serviço de programador deve retornar o CÓDIGO e o URL para o aplicativo de streaming

### Etapa 3: Autenticar o usuário {#step-3-authenticate-the-user}

Usando um navegador ou um aplicativo baseado na Web de segunda tela:

* Opção 1. O aplicativo de streaming pode abrir um navegador ou visualização da Web, carregar o URL para autenticar e o usuário acessar a página de logon do MVPD, onde as credenciais precisam ser enviadas
   * usuário digita login/senha, redirecionamento final mostra uma página de sucesso
* Opção 2. O aplicativo de streaming não pode abrir um navegador e apenas exibir o CÓDIGO. <b>É necessário desenvolver um aplicativo Web separado, AuthN_APP</b> para solicitar que o usuário insira CÓDIGO, crie e abra a URL: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * usuário digita login/senha, redirecionamento final mostra uma página de sucesso

### Etapa 4: verificar perfis autenticados {#step-4-check-for-authenticated-profiles}

O Serviço do Programador verifica a autenticação com o MVPD para ser concluída no Navegador ou na Segunda Tela

* A sondagem a cada 15 segundos é recomendada em <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Recuperar perfis autenticados para MVPD específico](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Se a seleção de MVPD não for feita no aplicativo de Streaming como o seletor de MVPD é apresentado no aplicativo Segunda Tela, a sondagem deve ocorrer com CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Recuperar perfis autenticados para CÓDIGO específico](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* A sondagem não deve exceder 30 minutos. Caso 30 minutos sejam atingidos e o aplicativo de transmissão ainda esteja ativo, uma nova sessão precisa ser iniciada e um novo CÓDIGO e URL serão retornados
* Quando a autenticação estiver concluída, o retorno será de 200 com perfil autenticado
* O Serviço Programador pode prosseguir para <a href="#preauthorization-phase">C. Fase de pré-autorização</a>

## C. Fase de pré-autorização {#preauthorization-phase}

### Etapa 5: verificar recursos pré-autorizados {#step-5-check-for-preauthorized-resources}

Com um perfil de autenticação válido para um usuário, o Serviço de programador tem a possibilidade de verificar o
acesse os vídeos disponíveis e transmita a lista para o Aplicativo de transmissão para exibição.

* A etapa é opcional e executada se o aplicativo quiser filtrar os recursos não disponíveis no pacote de usuário autenticado
* Chamada para <b>/api/v2/{serviceProvider}/decision/preauthorize/{mvpd}</b><br>
([Recuperar decisão de pré-autorização usando MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) específico)

## D. Fase de autorização {#authorization-phase}

### Etapa 6: verificar recursos autorizados {#step-6-check-for-authorized-resources}

O aplicativo de streaming se prepara para reproduzir um vídeo/ativo/recurso selecionado pelo usuário.

* Etapa necessária para cada início de reprodução
* O aplicativo de streaming transmite essas informações para o serviço de programação
* o Serviço Programador em nome do Aplicativo de Streaming, chame <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Recuperar decisão de autorização usando MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específico)
   * decision = &#39;Permit&#39; , Serviço do programador instruir o aplicativo de streaming para iniciar o streaming
   * decisão = &#39;Negar&#39;, o Serviço de programação instrui o Aplicativo de streaming a informar o usuário de que ele não terá acesso a esse vídeo
   * durante o processo, o Serviço de programador pode avaliar outras regras de negócios e retornar a decisão apropriada ao Aplicativo de streaming

## E. Fase de saída {#logout-phase}

### Etapa 7: Logout {#step-7-logout}

Aplicativo de transmissão: o usuário deseja fazer logoff do MVPD

* O aplicativo de streaming informa ao serviço do programador que ele precisa fazer logoff do MVPD para este aplicativo específico.
* o Serviço de programadores pode limpar as informações armazenadas sobre o usuário autenticado
* chamada do Serviço Programador <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Iniciar logout para MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específico)
* Se a resposta actionType=&#39;interative&#39; e o URL estiverem presentes, o Serviço de programador retornará o URL ao aplicativo de streaming
* Com base nos recursos existentes, o aplicativo de streaming pode abrir o URL no navegador (geralmente o mesmo usado para autenticação)
* Se o aplicativo de streaming não tiver um navegador ou se for uma instância diferente da que está na autenticação, o fluxo poderá ser interrompido, pois a sessão MVPD não foi mantida no cache do navegador.

## Ambientes e requisitos funcionais{#environments}

Um Programador deve criar pelo menos dois ambientes: um para produção e um ou mais para preparo.


### Produção

O ambiente de produção deve estar altamente disponível e dimensionado de forma adequada para picos grandes ou inesperados (por exemplo, esportes ao vivo, quebra)
notícias).



O serviço Adobe Pass é executado em vários data centers geograficamente dispersos nos EUA.  Para obter o melhor tempo de resposta (ou seja, a latência mais baixa) do serviço Adobe Pass, o programador também deve criar um serviço geograficamente disperso semelhante
infraestrutura.


O serviço do Programador deve limitar o cache DNS a um máximo de 30s, caso o Adobe precise redirecionar o tráfego. Isso pode ocorrer se um data center se tornar indisponível.


O Programador deve fornecer o intervalo de IP público do ambiente de produção. Eles serão inseridos em uma lista de permissões de IPs na infraestrutura do Adobe Pass para acesso e gerenciados pelas políticas de uso da API Adobe Fair.

### Estágios

O ambiente de preparo pode ser mínimo, mas deve incluir todos os componentes do sistema e a lógica de negócios. Ela deve funcionar de forma semelhante à produção e permitir o teste de versões fora da produção. Idealmente, o ambiente de preparo pode ser conectado aos ambientes de teste do Adobe Pass para uso do Programador e, quando necessário, por Adobe para que possamos ajudar no teste e na solução de problemas.

### Requisitos funcionais

O Serviço do programador deve transmitir informações precisas de identificação do dispositivo para o qual está executando os fluxos. Além disso, o serviço Programador deve passar o IP do dispositivo para o qual está executando os fluxos (em um cabeçalho x-forwarded-for) junto com a porta de origem da conexão (no campo device info ):

O Serviço Programador deve enviar dados e formatação exigidos por MVPDs individuais ou aplicativos integrados (por exemplo, IP do dispositivo, porta de origem, informações do dispositivo, MRSS, dados opcionais, como ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

O Serviço de programador deve respeitar o perfil de autenticação e a validade das decisões ao armazenar em cache e invalidar as autenticações ou decisões quando notificado.

O Serviço do programador deve manter certificados compartilhados com o Adobe (para metadados de usuário criptografados).

## Informações relacionadas {#related}

* [Referência da REST API V2](/help/authentication/rest-api-v2/rest-api-v2-flows-overview.md)
