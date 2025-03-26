---
title: Metadados do usuário
description: Metadados do usuário
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '1902'
ht-degree: 0%

---

# Metadados do usuário {#user-metadata}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Os metadados do usuário se referem aos [atributos](#attributes) específicos do usuário (por exemplo, códigos postais, classificações dos pais, IDs de usuário etc.) que são mantidos pelos MVPDs e fornecidos aos Programadores por meio da [REST API V2](#apis) de Autenticação da Adobe Pass.

Os metadados do usuário ficam disponíveis após a conclusão do fluxo de autenticação, mas determinados atributos de metadados podem ser atualizados durante o fluxo de autorização, dependendo do MVPD e do atributo de metadados específico em questão.

Os metadados do usuário podem ser usados para aprimorar a personalização para os usuários, mas também podem ser usados para análises. Por exemplo, um programador pode usar o CEP de um usuário para fornecer notícias localizadas ou atualizações meteorológicas, ou para aplicar os controles dos pais.

A Autenticação Adobe Pass normaliza os valores de metadados do usuário quando os MVPDs fornecem dados em diferentes formatos. Além disso, para determinados atributos (por exemplo, código postal), os valores podem ser [criptografados](#encryption) usando um certificado de Programador.

A Autenticação da Adobe Pass permite aos Programadores revisar os metadados do usuário disponibilizados em suas integrações do MVPD e [gerenciá-los](#management) por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

## Atributos de metadados do usuário {#attributes}

A tabela a seguir lista alguns dos atributos de metadados do usuário que são disponibilizados aos programadores:

| Chave | Tipo | Amostra | Requer criptografia | Descrição | Detalhes |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | String | 1o7241p | Não | Identificador da conta. | O valor do atributo pode ser um identificador da família ou um identificador de subconta. O valor de `userID` será diferente de `householdID` se o MVPD oferecer suporte a subcontas e o usuário atual não for o titular primário da conta. |
| `upstreamUserID` | String | 1o7241p | Não | Identificador de conta para monitoramento de simultaneidade. | O valor do atributo pode ser usado para aplicar limites de simultaneidade em sites e aplicativos do MVPD e do Programador. O valor `upstreamUserID` é igual ao valor `userID` para a maioria dos MVPDs. |
| `householdID` | String | 1o7241p | Não | Identificador de conta para controle dos pais. | O valor do atributo pode ser utilizado para diferenciar entre a utilização da família e da subconta. Às vezes, ele pode ser usado como um substituto de controle dos pais se as classificações verdadeiras não estiverem disponíveis, se o usuário tiver feito logon com a conta da família, ele poderá assistir, caso contrário, o conteúdo classificado não será exibido. Há muita variação nos MVPDs sobre como isso é representado (por exemplo, ID de usuário doméstico, ID de chefe de família, sinalizador de chefe de família, etc.), se o MVPD não oferecer suporte a subcontas, ele será idêntico a `userID`. |
| `primaryOID` | String | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Não | Identificador da conta. | O atributo é específico da AT&amp;T. O valor `primaryOID` é igual ao valor `userID` quando o valor `typeID` é definido como &quot;Primário&quot;. |
| `typeID` | String | &quot;Principal&quot; | Não | Atributo que indica se o usuário atual é um titular de conta principal ou secundário. | O atributo é específico da AT&amp;T. O valor `primaryOID` é igual ao valor `userID` quando o valor `typeID` é definido como &quot;Primário&quot;. |
| `is_hoh` | String | &quot;1&quot; | Não | Atributo que indica se o usuário atual é ou não chefe de família. | O atributo é específico do Synacor. |
| `hba_status` | Booleano | &quot;true&quot; | Não | Atributo que indica se o usuário atual foi autenticado pelo HBA ou não. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | Booleano | &quot;true&quot; | Não | Atributo que indica se o dispositivo atual pode ou não espelhar a tela. | O atributo é específico do Spectrum. |
| `zip` | Matriz | \[&quot;77754&quot;, &quot;12345&quot;\] | Sim | CEP do usuário. | O valor do atributo pode ser usado para fornecer notícias localizadas, atualizações do tempo ou eventos esportivos. O valor `zip` representa dados confidenciais que precisam de contratos legais com a MVPD. Quando criptografada, a representação da chave `zip` será `String` em vez de `Array`. |
| `encryptedZip` | String | &quot;&quot; | Sim | CEP criptografado do usuário. | O atributo é específico para Comcast. |
| `channelID` | Matriz | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | Não | Lista de canais que o usuário está autorizado a visualizar. | O valor do atributo pode ser usado para filtrar vários canais de portais que agregam várias redes. Nossa recomendação é usar a [API pré-autorizada](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) em vez desse atributo de metadados de usuário para filtrar os canais que não estão disponíveis para o usuário. |
| `maxRating` | Objeto | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | Não | Classificação máxima dos pais para o usuário atual. | O valor do atributo pode ser usado para filtrar conteúdo não adequado para o usuário atual com base nas classificações &quot;MPAA&quot; ou &quot;VCHIP&quot;. |
| `language` | String | &quot;English&quot; | Não | Configurações de idioma. | O valor do atributo pode ser usado para exibir mensagens de acordo com as preferências de idioma do usuário. |

Os atributos de metadados do usuário disponibilizados para um Programador dependem do que um MVPD oferece. A tabela a seguir lista os atributos disponibilizados por vários MVPDs:

|                         | **Contrato Legal Assinado (somente zip)** | **ID de Usuário na Autenticação** | **ID de Usuário Upstream em AuthN** | **ID da Família na AuthN/Z** | **OID principal no AuthN** | **ID de Tipo na AuthN** | **Chefe de Família na AuthN** | **Status do HBA** | **Permitir Espelhamento em AuthZ** | **Cep no AuthN/Z** | **ID do canal no AuthN** | **Classificação em AuthN/Z** | **Idioma** | **onNet** | **inHome** | **Notas** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome formal** | n/d | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **Requer Criptografia** | n/d | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| **Sensível** | n/d | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Adobe IdP | **Sim** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim** | **Sim** | **Sim** | **Não** | **Não** | **Sim (somente AuthN)** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | Contrato legal não necessário. |
| Synacor | **Sim** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Sim (somente AuthN)** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | Contrato legal que não abrange todos os MVPDs com proxy. Esse é um suporte genérico para o Synacor e possivelmente não é acumulado em todos os MVPDs. |
| Prato | **Não** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | Ele compartilha a mesma lista de todos os MVPDs Synacor, mais `upstreamUserID`. |
| Comcast | **Não** | **Sim** | **Sim** | **Sim (somente AuthZ)** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Sim (somente AuthZ)** | **Não** | **Não** | **Não** |                                                                                                                                           |
| AT&amp;T | **Sim** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | Contrato legal assinado. |
| DTV | **Sim** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| COX | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Visão do cabo | **Sim** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Sim** | **Não** | **Não** | **Não** | **Não** | Contrato legal assinado. |
| Espectro | **Sim** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Carta | **Sim** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Verizon | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| HTC | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Rogers | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| RCN | **Sim** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Eastlink | **Não** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Cogeco | **Não** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Videotron | **Não** | **Sim** | **Sim** | **Sim*** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | Expõe `householdID` com o mesmo valor que `userID`. |
| Massilon de proxy | **Sim** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | Contrato legal assinado. |
| Clearleap do Proxy | **Sim** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Sim (somente AuthZ)** | **Sim** | **Não** | **Não** | Contrato legal assinado. |
| GLDS de proxy | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** |                                                                                                                                           |
| Outros MVPDs | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | Ainda sem contrato legal, metadados confidenciais não estão disponíveis para produção. Para todos os MVPDs, o `userID` está disponível sem trabalho extra. |

>[!IMPORTANT]
>
> Os contratos legais devem ser assinados com os MVPDs antes que os metadados confidenciais do usuário (por exemplo, código postal) sejam disponibilizados.

## Criptografia de metadados de usuário {#encryption}

Para criptografar e descriptografar atributos de metadados do usuário, o Programador precisa gerar um certificado (par de chaves públicas/privadas) e [autoconfigurar](#management) o certificado por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) ou compartilhar a chave pública com representantes da Autenticação da Adobe Pass.

Siga as etapas abaixo para garantir que o certificado seja gerado e configurado corretamente:

1. Baixe e instale o kit de ferramentas OpenSSL (http://www.openssl.org).

1. Gerar uma solicitação de assinatura de certificado (CSR):

   * Gere um par de chaves. No terminal de comando, execute o seguinte:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Gere a CSR. No terminal de comando, execute o seguinte:

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

1. A CA enviará o certificado no formato .p7b (PKCS#7, Padrão de Sintaxe de Mensagem Criptográfica).

1. Implante o certificado .p7b. Converta o arquivo PKCS#7 (.p7b) em PKCS#12 (arquivo PFX, Padrão de Sintaxe de Troca de Informações Pessoais) usando sua chave privada e gere o arquivo PEM (arquivo de contêiner de certificado concatenado):

   * Converta o arquivo PKCS#7 em um arquivo PEM temporário. Na linha de comando, execute o seguinte:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Converta o arquivo PEM temporário em um arquivo PFX. Na linha de comando, execute o seguinte:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Converta o arquivo PEM temporário em um arquivo PEM final. Na linha de comando, execute o seguinte:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Use o arquivo PEM para [configurar](#management) o certificado por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) ou enviar o arquivo PEM para os representantes de Autenticação da Adobe Pass.

   * Consulte a próxima seção para obter mais detalhes sobre como gerenciar certificados no [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

   * A Autenticação do Adobe Pass oferece suporte a um certificado primário e de backup. Se o certificado principal ficar comprometido de alguma forma, você poderá revogá-lo e alternar para o certificado secundário. Isso garantirá uma transição sem problemas entre certificados, com impacto mínimo no cliente.

## Gerenciamento de metadados do usuário {#management}

>[!IMPORTANT]
>
> Caso você não tenha acesso ao Painel do Adobe Pass TVE, crie um tíquete por meio do nosso [Zendesk](https://adobeprimetime.zendesk.com) e peça ao seu Gerente técnico de conta (TAM) para fazer as alterações apropriadas para você.

O Adobe Pass TVE Dashboard é uma ferramenta para clientes de autenticação da Adobe Pass (programadores) gerenciarem suas configurações e dados. Este painel de autoatendimento habilita uma série de funcionalidades descritas na documentação do [Guia do Usuário do Painel do Adobe Pass TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

Para revisar e gerenciar os atributos de metadados do usuário disponibilizados por uma MVPD, siga as etapas da documentação do [Guia do Usuário do Painel do TVE para Integrações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata).

Para revisar e gerenciar os certificados usados para criptografar atributos de metadados do usuário, siga as etapas do [Guia do Usuário do Painel TVE para Programadores](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) ou o [Guia do Usuário do Painel TVE para Documentos de Canais](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates).

## REST API V2 {#rest-api-v2}

Os atributos de metadados do usuário podem ser recuperados usando as seguintes APIs:

* [Recuperar perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Consulte as seções **Resposta** e **Amostras** das APIs acima para entender a estrutura dos atributos de metadados do usuário.

>[!IMPORTANT]
>
> Os metadados do usuário ficam disponíveis após a conclusão do fluxo de autenticação, portanto, o aplicativo cliente não precisa consultar um ponto de extremidade separado para recuperar as informações de [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), pois elas já estão incluídas nas informações do perfil.

Para obter mais detalhes sobre como e quando integrar as APIs acima, consulte os seguintes documentos:

* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Determinados atributos de metadados podem ser atualizados durante o fluxo de autorização, dependendo do MVPD e do atributo de metadados específico. Como resultado, o aplicativo cliente pode precisar consultar as APIs acima novamente para recuperar os metadados do usuário mais recentes.

>[!MORELIKETHIS]
>
> [Perguntas frequentes sobre a Fase de Autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
