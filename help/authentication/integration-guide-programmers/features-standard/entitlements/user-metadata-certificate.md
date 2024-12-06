---
title: Certificado de metadados do usuário para criptografia
description: Certificado de metadados do usuário para criptografia
exl-id: 6f5d9a31-945e-418b-a9df-985bdbf29dff
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Certificado de metadados do usuário para criptografia

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Para integração de autenticação Adobe Pass de metadados de usuário criptografados, é necessário ter um par de chaves privado/público.

Este documento descreve um processo para gerar certificados de chave pública para uso na autenticação Adobe Pass. O processo descrito aqui usa o kit de ferramentas OpenSSL.

## Apresentação do processo de geração de certificado (#generation)

1. Baixe e instale o kit de ferramentas OpenSSL (http://www.openssl.org).

1. Gerar uma solicitação de assinatura de certificado (CSR):

   * Gere um par de chaves.  Abra uma janela Command / terminal e execute o seguinte comando:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Gere a CSR. Na linha de comando, execute o seguinte:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Você será solicitado a digitar a senha da chave privada.

   * Crie uma cópia de backup da sua chave privada e senha. CSR de exemplo:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Envie a CSR a uma autoridade de certificação (CA) (por exemplo, Verisign).

1. A autoridade de certificação enviará o certificado no formato .p7b (PKCS#7, Padrão de Sintaxe de Mensagem Criptográfica)

1. Implante o certificado .p7b. Converta o arquivo PKCS#7 (.p7b) em PKCS#12 (arquivo PFX, Padrão de Sintaxe de Troca de Informações Pessoais) usando sua chave privada e gere o arquivo PEM (arquivo de contêiner de certificado concatenado):

   * Converta o arquivo PKCS#7 em um arquivo PEM temporário. Na linha de comando, execute o seguinte:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Converta o arquivo PEM temporário em um arquivo PFX.  Na linha de comando, execute o seguinte:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Converta o arquivo PEM temporário em um arquivo PEM final. Na linha de comando, execute o seguinte:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Envie o arquivo PEM final para o Adobe para configurá-lo.

   * A pessoa que precisa receber o arquivo PEM é o engenheiro de ativação de Adobe atribuído à integração/validação. Se você não estiver trabalhando diretamente com essa pessoa, poderá descobrir para quem enviar o arquivo do seu representante da Adobe.
   * O Adobe suporta um certificado primário e um certificado de backup. Se o certificado principal for comprometido de alguma forma, você poderá revogá-lo e alternar para o certificado secundário para assinar a ID do solicitante no aplicativo. Isso garantirá uma transição tranquila entre os certificados em produção, com impacto mínimo no cliente.
   * Depois que o Adobe receber o arquivo PEM, os engenheiros de autenticação o adicionarão à sua configuração no lado do servidor e confirmarão o recebimento do arquivo.
