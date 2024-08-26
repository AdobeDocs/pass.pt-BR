---
title: Amazon FireOS SSO usando livro de cookies de API sem cliente
description: Amazon FireOS SSO usando livro de cookies de API sem cliente
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '761'
ht-degree: 0%

---

# Amazon FireOS SSO usando livro de cookies de API sem cliente {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

## Introdução {#Introduction}

Este documento fornece instruções para implementar a versão SSO do Amazon do fluxo de autenticação da Adobe Pass usando a API sem clientes. A primeira parte deste documento se concentra na especificidade da versão da arquitetura do Amazon para os muitos parceiros já familiarizados e experientes com sua implementação.

A segunda parte do documento aborda as etapas principais para implementar a API sem cliente do Adobe Pass Authentication.

Para obter uma visão geral técnica abrangente de como a solução sem cliente funciona, consulte a [Visão geral da API REST](/help/authentication/rest-api-overview.md). O Adobe é o contato preferido para suporte sobre a arquitetura geral e as primeiras implementações.

## SSO sem cliente do Amazon {#AMZ-Clientless-SSO}

### Arquitetura de alto nível {#High-Level-Arch}

A implementação do SSO sem cliente do Amazon é simples e basicamente idêntica às APIs sem cliente da Autenticação Adobe Primetime comuns.

Você precisará usar o SDK da Amazon para recuperar uma carga personalizada e usá-la ao chamar as APIs sem cliente do Adobe.

Se a carga for reconhecida e corresponder a uma sessão autenticada, as APIs sem cliente retornarão imediatamente com o token da sessão.

### Como criar o aplicativo para usar o SDK do Amazon {#Build-entries}

* Baixe e copie o [Amazon Stub SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) mais recente em uma pasta /SOEnabler paralela ao diretório de aplicativos
* Atualizar arquivos manifest/gradle para usar a biblioteca:

  **Adicionar a seguinte linha ao arquivo Manifesto:**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Entradas de arquivo simples:**

  Em repositórios:

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  Em dependências, adicione:

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* Lidar com a ausência do aplicativo associado do Amazon:

  Embora isso não seja provável, se o companheiro não estiver presente no dispositivo Amazon que seu aplicativo está executando, você deve encontrar um ClassNotFoundException no tempo de execução na seguinte classe: `com.amazon.ottssotokenlib.SSOEnabler`.

  Se isso acontecer, tudo o que você precisa fazer é ignorar a etapa de carga e voltar ao fluxo normal do PrimeTime. O SSO não será habilitado, mas o fluxo de autenticação regular ocorrerá normalmente.

</br>

### Como obter a carga do SSO do Amazon usando o SDK do Amazon {#UseAmazonSSO}

Durante a inicialização do aplicativo, obtenha uma instância do SOEnabler. Com base na arquitetura do aplicativo, você deve decidir entre uma implementação síncrona ou assíncrona.

Se, por qualquer motivo, as chamadas de API não retornarem uma carga, use o fluxo não-SSO regular e entre em contato com seus parceiros Amazon e Adobe para investigar.

**API assíncrona**

* Obter instância do SSO Enabler:

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* Definir o retorno de chamada

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * O pacote de resposta de sucesso conterá:
      * Token SSO como uma string com a chave &quot;SSOToken&quot;
   * O pacote de respostas de falha conterá:
      * código de erro como um int com a chave &quot;ErrorCode&quot;
      * descrição do erro como uma string com a chave &quot;ErrorDescription&quot;


* Obter token de SSO

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* Essa API fornecerá a resposta por meio do callback definido durante a inicialização.

  **Ex**. chamar usando instância singleton criada durante a inicialização:

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**APIs síncronas**

* Obtenha a instância do SSO Enabler e defina o retorno de chamada

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* Obter token de SSO

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * Essa API bloqueará o thread do chamador e responderá com o pacote de resultados. Como esta é uma chamada síncrona, certifique-se de não usá-la no thread principal.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * Valor em milissegundos. se definido, substitua o valor de tempo limite padrão de 1 minuto para a API de sincronização.


### Atualização da API sem cliente do Adobe Pass para usar o Registro de cliente dinâmico {#clientlessdcr}

Se esta for sua primeira implementação, consulte a **Visão geral técnica do sem cliente** e entre em contato com o Adobe caso precise de suporte.

A API sem cliente do Adobe exige que os aplicativos usem o Registro de cliente dinâmico para fazer chamadas para servidores Adobe.

* Para usar o Registro de Cliente Dinâmico em seu aplicativo, siga as instruções em [Gerenciamento de Registro de Cliente Dinâmico](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management) para criar um aplicativo registrado e baixar uma instrução de software.

* Para implementar a API Dynamic Client Registration para executar solicitações de autenticação e autorização para servidores Adobe Pass, siga as instruções em [Fluxo de Registro Dinâmico do Cliente](./dcr-api/flows/dynamic-client-registration-flow.md).

### Atualização da API sem cliente do Adobe Pass para usar o SSO do Amazon {#clientlesssso}

A carga de SSO do Amazon obtida do SDK do Amazon precisa estar presente nas solicitações feitas para os endpoints de autenticação da Adobe Pass:

```
      /adobe-services/*
      /reggie/*
      /api/*
```


Todos os endpoints de Autenticação do Adobe Pass são compatíveis com os seguintes métodos para receber o identificador de escopo de dispositivo ou o identificador de escopo de plataforma (presente na carga do SSO do Amazon):

* Como cabeçalho : &quot;Adobe-Subject-Token&quot;
* Como parâmetro de consulta : &quot;ast&quot;
* Como parâmetro de publicação : &quot;ast&quot;


>[!NOTE]
>
>Se o identificador com escopo de dispositivo ou identificador com escopo de plataforma for enviado como parâmetro de consulta/publicação, ele deverá ser incluído ao gerar a assinatura da solicitação.

>[!NOTE]
>
>Usando o parâmetro de consulta &quot;ast&quot;, o URL inteiro pode se tornar muito longo e ser rejeitado. Na chamada /authenticate, esse parâmetro pode ser ignorado porque foi fornecido na chamada /regcode

**Exemplos:**

**Enviando como cabeçalho personalizado**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Enviando como parâmetro de consulta**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**Enviando como parâmetro de postagem**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Se o SSO do Amazon não estiver presente ou for inválido, a Autenticação do Adobe Pass ignorará o atributo e as chamadas serão executadas como se o SSO não estivesse presente.
