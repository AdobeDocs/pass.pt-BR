---
title: SSO via autenticação passiva
description: SSO via autenticação passiva
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 0%

---

# SSO (herdado) via autenticação passiva

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


## Introdução

O escopo deste documento é descrever a implementação do fluxo de autenticação passiva e como ele funciona com nossa abordagem padrão de logon único.

## Usecases

A Autenticação Adobe Pass habilita o Logon único (SSO) entre aplicativos/sites. Depois que um usuário faz logon com suas credenciais do MVPD, a Autenticação da Adobe Pass gera um token seguro que representa a sessão de autenticação da MVPD e vincula esse token ao dispositivo do usuário usando uma ID do dispositivo. A Autenticação do Adobe Pass armazena o token/ID do dispositivo em um servidor ou no dispositivo.

Desde que o token ainda seja válido, os usuários aparecerão diretamente como autenticados. Isso permite que os usuários insiram suas credenciais com menos frequência, mantendo as transações seguras.



O caso de uso comercial detalhado aqui é um requisito muito específico: o usuário DEVE ser autenticado pelo menos uma vez para cada site visitado. Isso permite que o MVPD aplique regras de negócios associadas à sessão de autenticação que podem variar por rede. Ele está em conflito com a promessa atual da TVE de que um usuário precisa fazer logon apenas uma vez e será autenticado em todos os sites que fazem parte do ecossistema de autenticação da Adobe Pass.



Para manter a regra de negócios, mas também uma boa experiência do usuário, o MVPD NÃO exige que um usuário forneça manualmente informações de credencial. Podemos nos beneficiar do cookie de sessão definido anteriormente e tentar fazer uma reautenticação automática usando o fluxo passivo; da perspectiva do usuário, ele parecerá estar conectado automaticamente.



Para resolver isso, implementamos dois recursos distintos: autenticação por rede e suporte à autenticação passiva. Os MVPDs oferecerão suporte à autenticação passiva SAML, na qual eles simplesmente reautenticarão um usuário se uma sessão de autenticação existir no IdP, independentemente do site em que essa sessão foi criada.



## Autenticação por rede

Esse recurso garante que o MVPD receba uma solicitação de autenticação uma vez para cada site visitado. Essa funcionalidade significa que um token de autenticação da Autenticação do Adobe Pass será vinculado ao requestorID, sendo válido somente para a rede que o solicitou. Como resultado, uma vez que o usuário se autentique no site &quot;A&quot; e depois acesse o site &quot;B&quot;, será necessário autenticar.



Observe que, como no MVPD IdP o usuário já está autenticado, ele não precisará fornecer informações de logon, mas o navegador será simplesmente redirecionado do site &quot;B&quot; para o MVPD IdP e depois de volta. O mesmo usuário ainda será autenticado no site &quot;A&quot; se retornar.



O fluxo a seguir descreve o recurso básico Autenticação por rede:





## Autenticação passiva

O objetivo é fazer com que o processo de reautenticação ocorra em segundo plano, sem a necessidade de um redirecionamento completo do navegador e a exibição do seletor. Como resultado, um usuário que mudar do site A para o site B será autenticado automaticamente.



Da perspectiva do UX, não haverá diferença entre esse fluxo e um fluxo executado com um MVPD regular. O que o usuário vê é que depois de inserir credenciais como resultado de visitar o site A, ele será automaticamente autenticado no site B.



Para tornar esse fluxo viável, o MVPD precisará suportar autenticação passiva para que o iframe oculto não fique &quot;preso&quot; na página de logon, caso o MVPD não tenha mais uma sessão. Isso é feito por meio do atributo padrão &quot;isPassive&quot;.



O diagrama a seguir descreve o fluxo aprimorado e a autenticação passiva &quot;nos bastidores&quot;:





Exemplo de solicitação SAML
Esta é uma amostra de solicitação SAML para o fluxo de autenticação passivo:


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## Regras de negócios

Os MVPDs têm restrições específicas de domínio de escopo de SSO. Por exemplo, apenas alguns domínios podem ser permitidos por alguns MVPDs (SSO para a mesma empresa de mídia, mas não entre empresas).
Alguns MVPDs podem exigir regras de autenticação diferentes para serem aplicados. Por exemplo, os MVPDs podem ter TTLs de autenticação diferentes para redes diferentes. Além disso, os MVPDs podem habilitar a autenticação baseada em casa para algumas redes, mas não para outras (os casos de uso de controle dos pais são fortemente representados aqui).


Esses requisitos comerciais devem ter em mente que o principal caso de uso é que o usuário não deve ser solicitado a fazer logon novamente após fazer logon com sua MVPD.

Isso pode ser feito usando autenticação por rede com sinalizador de autenticação passiva.



## Limitações conhecidas

iOS - devido à natureza do armazenamento local do iOS, os fluxos de SSO não funcionam no iOS para aplicativos desenvolvidos por diferentes fornecedores. Para obter mais informações sobre SSO no iOS 8 e superior, consulte esta Nota técnica.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
