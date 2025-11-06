---
title: Guia do Amazon SSO (REST API V2)
description: Guia do Amazon SSO (REST API V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Guia do Amazon SSO (REST API V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

A API REST V2 de autenticação do Adobe Pass é compatível com o Logon único (SSO) da plataforma para usuários finais de aplicativos clientes em execução no FireOS.

Este documento atua como uma extensão para a [Visão geral da REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existente, que fornece uma exibição de alto nível e o documento que descreve como implementar o [Logon único usando fluxos de identidade de plataforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Logon único do Amazon usando fluxos de identidade da plataforma {#cookbook}

A Autenticação do Adobe Pass colabora com o Amazon para melhorar a experiência de logon do usuário e facilitar o Logon único (SSO) em aplicativos do TV Everywhere para assinantes de TV.

### Pré-requisitos {#prerequisites}

Antes de prosseguir com o logon único da Amazon usando fluxos de identidade da plataforma, verifique se os seguintes pré-requisitos foram atendidos.

#### Integrar o Amazon SSO SDK {#integrate-amazon-sso-sdk}

O aplicativo de streaming deve integrar a biblioteca [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) para Logon Único (SSO) em sua compilação.

* Baixe e copie a biblioteca Amazon SSO SDK mais recente em uma pasta `/SSOEnabler` paralela ao diretório do aplicativo.

* Atualize os arquivos manifest e Gradle para usar a biblioteca SDK de SSO do Amazon.

  **Manifesto:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Grade:**

  Em repositórios:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  Em dependências:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Usar o Amazon SSO SDK {#use-amazon-sso-sdk}

O aplicativo de transmissão deve usar o SDK de SSO do Amazon para obter a carga do token de SSO (identidade da plataforma).

O Amazon SSO SDK fornece APIs síncronas e assíncronas para obter a carga do token de SSO (identidade da plataforma).

O aplicativo de transmissão pode escolher uma das duas opções com base em sua arquitetura.

##### APIs assíncronas

* Obtenha a instância `SSOEnabler` e defina o `SSOEnablerCallback`:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  Isso pode ser feito durante a inicialização do aplicativo de streaming.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  O pacote de resposta de sucesso do token de SSO conterá:
   * Um token SSO como um `string` com a chave &quot;SSOToken&quot;.

  <br/>

  O pacote de resposta de falha do token de SSO conterá:
   * Um código de erro como um `int` com a chave &quot;ErrorCode&quot;.
   * Uma descrição de erro como `string` com a chave &quot;ErrorDescription&quot;.

  <br/>

* Obtenha o token SSO:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Essa API fornecerá a resposta por meio do conjunto de retorno de chamada durante a inicialização.

##### APIs síncronas

* Obter a instância `SSOEnabler`:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Obtenha o token SSO:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Essa API bloqueará o thread do chamador e responderá com o pacote de resultados. Como esta é uma chamada síncrona, certifique-se de não usá-la no thread principal.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Essa API definirá o valor de tempo limite para a chamada síncrona. O valor de tempo limite padrão é de 1 minuto.

#### Fallback para Amazon SSO {#fallback-amazon-sso}

O aplicativo de transmissão deve lidar com cenários de fallback do fluxo de SSO do Amazon para o fluxo de autenticação regular.

Verifique se o aplicativo de transmissão está lidando com:

* A ausência do aplicativo associado do Amazon que deve estar em execução no dispositivo Amazon.
   * O aplicativo de streaming pode encontrar um `ClassNotFoundException` em tempo de execução na seguinte classe `com.amazon.ottssotokenlib.SSOEnabler`.

* A ausência da carga do token SSO (identidade da plataforma) que deve ser retornada pelas APIs acima.
   * O aplicativo de transmissão pode entrar em contato com os representantes da Amazon e da Adobe para investigar.

### Fluxo de trabalho (WRK) {#workflow}

A carga do token SSO (identidade da plataforma) do Amazon precisa estar presente em todas as solicitações HTTP feitas nos pontos de extremidade da API REST V2 de autenticação da Adobe Pass:

```
/api/v2/*
```

A API REST V2 de autenticação do Adobe Pass oferece suporte aos seguintes métodos para receber a carga do token SSO (identidade da plataforma) que é um identificador com escopo de dispositivo ou de plataforma:

* Como um cabeçalho chamado: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

#### Amostras

**Enviando como cabeçalho**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> Caso o valor do cabeçalho `Adobe-Subject-Token` esteja ausente ou seja inválido, a Autenticação da Adobe Pass atenderá às solicitações sem considerar o Logon único.
