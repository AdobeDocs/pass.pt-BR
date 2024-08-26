---
title: Referência da API REST
description: Referência da API Rest
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 3%

---

# Referência da API REST {#rest-api-reference}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Mecanismo de limitação

A API REST de Autenticação do Adobe Pass é regida por um [Mecanismo de limitação](/help/authentication/throttling-mechanism.md).

## Formatos de resposta {#response-formats}


>[!NOTE]
>
> As APIs fornecidas nesses serviços podem retornar respostas em XML ou JSON (para APIs que retornam uma resposta). Há três maneiras diferentes de especificar o formato de resposta na solicitação:
>
>* Defina o Cabeçalho Aceitar HTTP como `application/xml` ou `application/json`.
>* Na carga da solicitação, especifique o parâmetro `format=xml` ou `format=json`.
>* Chame o ponto de extremidade do serviço Web com extensão `.xml` ou `.json`. Por exemplo, `/regcode.xml` ou `/regcode.json`
>
>Você pode especificar qualquer um dos métodos acima. A especificação de vários métodos com formatos conflitantes pode resultar em erros ou em saídas indesejáveis.

## Endpoints da REST API {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Resumo dos serviços da Web {#web_srvs_summary}

A tabela abaixo lista os serviços Web disponíveis para a abordagem sem cliente. Clique nos pontos finais dos serviços da Web para obter mais informações (amostra de solicitação e resposta, parâmetros de entrada, métodos HTTP etc.)


| Sr | Ponto Final do Serviço Web | Descrição | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Hospedado em | Chamado por |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode](/help/authentication/registration-code-request.md) | Retorna o código de registro gerado aleatoriamente e o URI da página de logon | 2 | Serviço de código Adobe </br>Reg | Dispositivo inteligente |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/return-registration-record.md) | Retorna o registro do código de registro contendo o código de registro UUID, o código de registro e a ID do dispositivo com hash | 8 | Serviço de código Adobe </br>Reg | Adobe Pass Authentication |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId}](/help/authentication/provide-mvpd-list.md) | Retorna a lista de MVPDs configurados para o solicitante | 5 | Serviço </br>de autenticação </br>do Adobe </br>Adobe Pass | Logon </br>Aplicativo </br>Web |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/initiate-authentication.md) | Inicia o processo de Autenticação informando o evento de seleção MVPD. Cria um registro no banco de dados de autenticação, que é reconciliado quando uma resposta bem-sucedida é recebida do MVPD (Etapa 13) | 7 | Serviço </br>de autenticação </br>do Adobe </br>Adobe Pass | Logon </br>Aplicativo </br>Web |
| 5. | Consumidor de Asserção SAML | Fluxo de trabalho SAML existente entre a Autenticação do Adobe Pass e o MVPD | 13 | Serviço </br>de autenticação </br>do Adobe Pass | Adobe Pass Authentication |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode}](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | O Aplicativo Web de Logon pode verificar se a tentativa de fluxo de logon foi bem-sucedida |     | Autenticação </br> do Adobe Pass   </br>Serviço | Logon   </br>Web   </br>Aplicativo |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Obtém metadados relacionados ao token de Autenticação | 15 | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/delete-registration-record.md) | Exclui o registro de código regulamentar e libera o código regulamentar para reutilização | 16 | Serviço de código Adobe </br>Reg | Adobe Pass Authentication |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/initiate-authorization.md) | Obtém a resposta de autorização. | 17 | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/check-authentication-token.md) | Indica se o dispositivo tem um token de autenticação não expirado. |     | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Retorna o token de autenticação se for encontrado. |     | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | Retorna o token de AuthZ, se encontrado. |     | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | Retorna o token de mídia curta se encontrado - o mesmo que /api/v1/mediatoken |     | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/obtain-short-media-token.md) | Obtém O Token De Mídia Curta |     | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/retrieve-list-of-preauthorized-resources.md) | Recupera a lista de recursos pré-autorizados |     | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Recupera a lista de recursos pré-autorizados |     | Serviço </br>de autenticação </br>do Adobe Pass | Aplicativo Web de Logon |
| 17. | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/initiate-logout.md) | Remover tokens AuthN e AuthZ do armazenamento |     | Autenticação </br> do Adobe Pass   </br>Serviço | Dispositivo inteligente |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Obtém metadados do usuário após a conclusão do fluxo de autenticação | N/A | N/A | Dispositivo inteligente |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Criar um token de autenticação para Aprovação Temporária ou Aprovação Temporária Promocional | N/A | Serviço </br>de autenticação </br>do Adobe Pass | Dispositivo inteligente |


## Segurança da API REST {#security}

Todas as APIs REST de Autenticação do Adobe Pass devem ser chamadas usando o protocolo HTTPS para comunicação segura. Além disso, a maioria das APIs chamadas deve conter um token de acesso obtido conforme descrito na documentação da API [Recuperar token de acesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
