---
title: Autenticação usando o protocolo OAuth 2.0
description: Autenticação usando o protocolo OAuth 2.0
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# Autenticação usando o protocolo OAuth 2.0

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

Embora o SAML ainda seja o principal protocolo usado para autenticação por MVPDs dos EUA e empresas em geral, há uma tendência clara para mudar para o OAuth 2.0 como o protocolo de autenticação principal. O protocolo OAuth 2.0 (https://tools.ietf.org/html/rfc6749) foi desenvolvido principalmente para sites de consumidores e foi rapidamente adotado por gigantes da Internet como Facebook, Google e Twitter.

O OAuth 2.0 é extremamente bem-sucedido e isso levou as empresas a atualizar lentamente sua infraestrutura para oferecer suporte a ele.



## Vantagens de migrar para o OAuth 2.0 {#adv-oauth2}

Em um alto nível, o protocolo OAuth 2.0 oferece a mesma funcionalidade do protocolo SAML, mas há algumas distinções importantes.

Um deles é o fato de que o fluxo do token de atualização pode ser usado como uma maneira de atualizar a autenticação em segundo plano. Isso permite que o IdP (os MVPDs, neste caso) mantenha o controle enquanto ainda permite uma boa experiência do usuário, pois ele não é mais necessário fazer logon com frequência devido a preocupações de segurança.

O protocolo também oferece mais flexibilidade em termos de dados expostos, já que um provedor de serviços agora pode usar os tokens para acessar outras APIs e obter informações adicionais. Isso, por sua vez, resulta em um protocolo &quot;chattier&quot; para casos de uso de TVE, mas permite a flexibilidade necessária para fluxos de trabalho complexos.





## Requisitos para alternar para o OAuth 2.0 {#oauth-req}

Para ser compatível com a autenticação com o OAuth 2.0, um MVPD precisa atender aos seguintes pré-requisitos:

Primeiramente, a MVPD deve se certificar de que ela suporta o fluxo [Concessão de código de autorização](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html).

Depois de confirmar que ele é compatível com o fluxo, a MVPD deve nos fornecer as seguintes informações:

* o ponto final de autenticação
   * o ponto de extremidade fornecerá o código de autorização que será usado posteriormente em troca do token de atualização e acesso
* o ponto final /token
   * isso fornecerá o token de atualização e de acesso
   * o token de atualização precisa ser estável (ele não deve ser alterado sempre que solicitarmos um novo token de acesso)
   * o MVPD precisa permitir vários tokens de acesso ativos para cada token de atualização
   * este ponto de extremidade também trocará um token de atualização por um token de acesso
* precisamos de um **ponto final para perfil de usuário**
   * esse terminal fornecerá a ID do usuário, que precisa ser exclusiva para uma conta e não deve conter informações pessoais identificáveis
* o ponto de extremidade **/logout** (opcional)
   * A Autenticação do Adobe Pass será redirecionada para esse ponto de extremidade, fornecerá ao MVPD um URI de retorno de redirecionamento; nesse ponto de extremidade, o MVPD pode apagar os cookies na máquina cliente ou aplicar qualquer lógica desejada para o logout
* é altamente recomendável ter suporte para clientes autorizados (aplicativos clientes que não acionam uma página de autorização do usuário)
* também precisaremos de:
   * **clientID** e **client secret** para as configurações de integração
   * Valores de **tempo de vida** (TTL) para o token de atualização e o token de acesso
   * Podemos fornecer à MVPD um URI de retorno de chamada de autorização e de retorno de chamada de logout. Além disso, se necessário, podemos fornecer aos MVPDs uma lista de IPs que devem ser incluídos na lista de permissões nas configurações do firewall.


## Fluxo de autenticação {#authn-flow}

No fluxo de autenticação, a Autenticação do Adobe Pass se comunicará com a MVPD no protocolo selecionado na configuração. O fluxo do OAuth 2.0 é representado na figura abaixo:



![Diagrama para mostrar o Fluxo de autenticação na Autenticação do Adobe que se comunica com o MVPD no protocolo selecionado na configuração.](../assets/authn-flow.png)

**Figura 1: Fluxo de autenticação do OAuth 2.0**



## Solicitação e resposta de autenticação {#authn-req-response}

Resumindo, o fluxo de autenticação para MVPDs que oferecem suporte ao protocolo OAuth 2.0 segue estas etapas:

1. O usuário final navega até o site do Programador e seleciona para fazer logon com suas credenciais do MVPD
1. O AccessEnabler instalado no lado do programador com o envio de uma solicitação de autenticação na forma de uma solicitação HTTP para o endpoint de autenticação da Adobe Pass, que o endpoint de autenticação da Adobe Pass redireciona para o endpoint de autorização da MVPD.
1. O endpoint de autorização do MVPD envia um código de autorização para o endpoint de autenticação da Adobe Pass
1. A Autenticação Adobe Pass usa o código de autorização recebido para solicitar um token de atualização e um token de acesso do endpoint de token da MVPD
1. Uma chamada para buscar informações e metadados do usuário pode ser enviada para o endpoint do perfil do usuário caso as informações do usuário não estejam incluídas no token
1. O token de autenticação é passado para o usuário final que agora pode navegar com êxito pelo site do Programador

   >[!NOTE]
   >
   >O token de atualização é usado para obter um novo token de acesso depois que o token de acesso atual se tornar inválido ou expirar.


>[!IMPORTANT]
>
>O token de atualização não deve ser alterado quando trocado por um token de acesso.

Essa limitação vem dos fluxos de cliente que não permitem que o servidor atualize o AuthNToken que, para o protocolo OAuth 2.0, também contém o token de atualização.

Um fluxo de autorização típico executa uma troca do token de atualização salvo no AuthNToken por um token de acesso que é usado subsequentemente para executar a chamada de autorização no nome do usuário que foi autenticado em primeiro lugar. Se o Servidor de autorização (o MVPD) alterasse o token de atualização e invalidasse o antigo, não seria possível atualizar o AuthNToken válido. Por esse motivo, os MVPDs precisam oferecer suporte a tokens de atualização estáveis para poderem configurar integrações OAuth 2.0 para eles.


## Migração do SAML para OAuth 2.0 {#saml-auth2-migr}

A migração de integrações do SAML para o OAuth 2.0 será realizada pela Adobe e pela MVPD. Não há necessidade de qualquer mudança técnica por parte do programador, embora ele queira verificar/testar o compartilhamento de marcas na página de login do MVPD. Do ponto de vista da MVPD, os endpoints e outras informações solicitadas nos requisitos do OAuth 2.0 são necessários.

Para **preservar SSO**, os usuários que já têm um token de autenticação obtido via SAML ainda serão considerados autenticados e suas solicitações serão roteadas por meio da integração SAML antiga.

Do ponto de vista técnico:

1. O Adobe habilitará uma integração OAuth 2.0 entre o programador e o MVPD, SEM excluir a integração SAML.
1. Após a ativação, todos os novos usuários usarão os fluxos do OAuth 2.0.
1. Os usuários já autenticados, que já têm um token de Autenticação local que contém a Id de assunto SAML, serão roteados automaticamente pelo Adobe por meio da integração SAML.
1. Para os usuários na etapa 3, depois que o token de Autenticação gerado pelo SAML expirar, o Adobe os tratará como novos usuários e se comportará como os usuários na etapa 2.
1. O Adobe revisará os padrões de uso para determinar o momento em que a integração SAML poderá ser desativada com segurança.
