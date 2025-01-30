---
title: Tokens de mídia
description: Tokens de mídia
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '667'
ht-degree: 0%

---

# Tokens de mídia {#media-tokens}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

O token de mídia é um token gerado pela [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) da Autenticação da Adobe Pass como resultado de uma decisão de autorização destinada a fornecer acesso de exibição a conteúdo protegido (recurso).

O token de mídia é válido por um período limitado e curto (padrão de 7 minutos) especificado no momento do problema, indicando o limite de tempo antes que ele seja verificado e usado pelo aplicativo cliente. O token de mídia é restrito ao uso único e nunca deve ser armazenado em cache.

O token de mídia consiste em uma sequência de caracteres assinada com base na Infraestrutura de Chave Pública (PKI) enviada em texto não criptografado. Com a proteção baseada em PKI, o token é assinado usando uma chave assimétrica emitida para o Adobe por uma Autoridade de Certificação (CA).

O token de mídia é transmitido ao programador, que pode validá-lo usando o Verificador de token de mídia antes de iniciar o fluxo de vídeo para garantir a segurança de acesso desse recurso.

O Media Token Verifier é uma biblioteca distribuída pela autenticação da Adobe Pass responsável por verificar a autenticidade de um token de mídia.

## Verificador de token de mídia {#media-token-verifier}

A autenticação da Adobe Pass recomenda que os programadores enviem o token de mídia para seu próprio serviço de backend, integrando a biblioteca Verificador de token de mídia, para garantir acesso seguro antes de iniciar o fluxo de vídeo. O TTL (time-to-live) do token de mídia foi projetado para levar em conta possíveis problemas de sincronização de relógio entre o servidor que gera o token e o servidor de validação.

A autenticação da Adobe Pass recomenda não analisar o token de mídia e extrair diretamente os dados, pois o formato do token não é garantido e pode mudar no futuro. A biblioteca do Verificador de token de mídia deve ser a única ferramenta usada para analisar o conteúdo do token.

A biblioteca Media Token Verifier pode ser baixada no seguinte link:

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

A biblioteca Verificador de Token de Mídia requer o JDK versão 1.5 ou superior e oferece suporte ao uso de um provedor JCE (extensão de criptografia Java) preferencial para o algoritmo de assinatura (`SHA256WithRSA`).

A biblioteca Verificador de Token de Mídia representada pelo arquivo Java `mediatoken-verifier-VERSION.jar` inclui:

* Adobe chave pública.
* API de verificação de token (`ITokenVerifier.java`).
* Implementação de referência (`com.adobe.entitlement.test.EntitlementVerifierTest.java`).
* Dependências e armazenamentos de chaves de certificado.

>[!IMPORTANT]
> 
> A senha padrão para o armazenamento de chaves de certificado incluído é `123456`.

### Métodos {#methods}

A classe `ITokenVerifier` define os seguintes métodos:

* O método `isValid()` usado para validar o token de mídia. Ele aceita um único argumento, o [identificador de recurso](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier). Se o identificador de recurso fornecido for `null`, o método validará somente a autenticidade e o período de validade do token de mídia.

  O método `isValid()` retorna um dos seguintes valores de status:

  | VALID_TOKEN | Validações de token bem-sucedidas |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | O formato do token é inválido |
  | INVALID_SIGNATURE | Não foi possível validar a autenticidade do token |
  | TOKEN_EXPIRED | O TTL do token não é válido |
  | INVALID_RESOURCE_ID | Token inválido para o recurso fornecido |
  | ERRO_DESCONHECIDO | O token ainda não foi validado |

* O método `getResourceID()` usado para recuperar o identificador de recurso associado ao token de mídia e compará-lo ao identificador retornado da resposta de decisão de autorização.

* O método `getTimeIssued()` usado para recuperar a hora em que o token de mídia foi emitido.

* O método `getTimeToLive()` usado para recuperar o TTL do token de mídia.

* O método `getUserSessionGUID()` usado para recuperar uma GUID anônima definida pela MVPD.

* O método `getMvpdId()` usado para recuperar o identificador do MVPD que autenticou o usuário.

* O método `getProxyMvpdId()` usado para recuperar o identificador do Proxy MVPD que autenticou o usuário.

### Amostra {#sample}

O arquivo Verificador de Token de Mídia contém uma implementação de referência (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) e um exemplo de chamada da API com a classe de teste. Este exemplo (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) ilustra a integração da biblioteca do Verificador de Token de Mídia em um servidor de mídia.

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST API V2 {#rest-api-v2}

O token de mídia pode ser recuperado usando a seguinte API:

* [Recuperar decisões de autorização usando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulte as seções **Resposta** e **Amostras** da API acima para entender a estrutura das decisões de autorização e dos tokens de mídia.

Para obter mais detalhes sobre como e quando integrar a API acima, consulte o seguinte documento:

* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> O aplicativo cliente deve passar o valor `serializedToken` do `token` retornado para o [Verificador de Token de Mídia](#media-token-verifier) para validação.
