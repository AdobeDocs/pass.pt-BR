---
title: Visão geral do SDK do Android
description: Visão geral do SDK do Android
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '2731'
ht-degree: 0%

---

# Visão geral do SDK do Android {#android-sdk-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

O Android AccessEnabler é uma biblioteca Java Android que permite que aplicativos móveis usem os serviços de direito da Adobe Pass Authentication for TV Everywhere. Uma implementação do Android consiste na interface do AccessEnabler que define a API de direito e em um protocolo EntitlementDelegate que descreve os retornos de chamada acionados pela biblioteca. A interface junto com o protocolo é mencionada com um nome comum: a biblioteca Android do AccessEnabler.

## Requisitos do Android {#reqs}

Para conhecer os requisitos técnicos atuais relacionados à plataforma Android e à Autenticação Adobe Pass, consulte [Requisitos de Plataforma/Dispositivo/Ferramenta](#android) ou consulte as notas de versão incluídas no download do SDK da Android.

## Noções básicas sobre fluxos de trabalho de clientes nativos {#native_client_workflows}

Os workflows do cliente nativo normalmente são os mesmos ou muito semelhantes aos dos clientes de Autenticação do Adobe Pass com base em navegador. No entanto, há algumas exceções, conforme descrito abaixo.

- [Fluxo de trabalho de pós-inicialização](#post-init)
- [Fluxo de trabalho genérico de autenticação inicial](#generic)
- [Fluxo de trabalho de logout](#logout)

### Fluxo de trabalho de pós-inicialização {#post-init}

Todos os fluxos de trabalho de direito suportados pelo AccessEnabler supõem que você tenha chamado anteriormente [`setRequestor()`](#setRequestor) para estabelecer sua identidade. Você faz essa chamada para fornecer a ID do solicitante apenas uma vez, normalmente durante a fase de inicialização/configuração do aplicativo.


Com os clientes nativos (por exemplo, Android), após sua chamada inicial para [`setRequestor()`](#setRequestor), você tem uma escolha sobre como proceder:

- Você pode começar a fazer chamadas de direito imediatamente e permitir que elas sejam enfileiradas silenciosamente, se necessário.

- Ou você pode receber uma confirmação do sucesso/falha de [`setRequestor()`](#setRequestor) implementando o retorno de chamada setRequestorComplete().

- Ou faça as duas coisas.

Fica a seu critério aguardar a notificação do sucesso de [`setRequestor()`](#setRequestor) ou confiar no mecanismo de fila de chamadas do AccessEnabler. Como todas as solicitações de autorização e autenticação subsequentes precisam da ID do solicitante e das informações de configuração associadas, o método [`setRequestor()`](#setRequestor) bloqueia efetivamente todas as chamadas de API de autenticação e autorização até que a inicialização seja concluída.

### Fluxo de trabalho genérico de autenticação inicial {#generic}

A finalidade desse fluxo de trabalho é fazer logon em um usuário com seu MVPD.  Após um logon bem-sucedido, o servidor de back-end emite um token de autenticação para o usuário. Embora a autenticação seja normalmente feita como parte do processo de autorização, a seguir está uma descrição de como ela pode funcionar isoladamente, sem incluir nenhuma etapa de autorização.

Observe que, embora o fluxo de trabalho do cliente nativo a seguir seja diferente do fluxo de trabalho típico de autenticação baseada em navegador, as etapas de 1 a 5 são as mesmas para clientes nativos e clientes baseados em navegador:

1. Sua página ou reprodutor inicia o fluxo de trabalho de autenticação com uma chamada para [getAuthentication()](#getAuthN), que verifica se há um token de autenticação em cache válido. Este método tem um parâmetro `redirectURL` opcional; se você não fornecer um valor para `redirectURL`, após uma autenticação bem-sucedida o usuário retornará à URL da qual a autenticação foi inicializada.
1. O AccessEnabler determina o status de autenticação atual. Se o usuário estiver autenticado no momento, o AccessEnabler chamará sua função de retorno de chamada do `setAuthenticationStatus()`, transmitindo um status de autenticação indicando sucesso (Etapa 7 abaixo).
1. Se o usuário não for autenticado, o AccessEnabler continuará o fluxo de autenticação determinando se a última tentativa de autenticação do usuário foi bem-sucedida com um determinado MVPD. Se uma ID de MVPD for armazenada em cache E o sinalizador `canAuthenticate` for verdadeiro OU um MVPD tiver sido selecionado usando [`setSelectedProvider()`](#setSelectedProvider), o usuário não será avisado com a caixa de diálogo de seleção de MVPD. O fluxo de autenticação continua usando o valor em cache do MVPD (ou seja, o mesmo MVPD usado durante a última autenticação bem-sucedida). Uma chamada de rede é feita ao servidor de back-end e o usuário é redirecionado para a página de logon do MVPD (Etapa 6 abaixo).
1. Se nenhuma ID de MVPD estiver armazenada em cache E nenhum MVPD tiver sido selecionado usando [`setSelectedProvider()`](#setSelectedProvider) OU o sinalizador `canAuthenticate` estiver definido como falso, o retorno de chamada [`displayProviderDialog()`](#displayProviderDialog) será chamado. Esse retorno de chamada direciona a página ou o reprodutor para criar a interface do usuário que apresenta ao usuário uma lista de MVPDs para escolha. Uma matriz de objetos MVPD é fornecida, contendo as informações necessárias para que você crie o seletor de MVPD. Cada objeto MVPD descreve uma entidade MVPD e contém informações como a ID do MVPD (por exemplo, XFINITY, AT\&amp;T etc.) e o URL onde o logotipo do MVPD pode ser encontrado.
1. Depois que um MVPD específico é selecionado, sua página ou player deve informar o AccessEnabler da escolha do usuário. Para clientes que não sejam do Flash, depois que o usuário selecionar o MVPD desejado, você informará o AccessEnabler sobre a seleção do usuário por meio de uma chamada para o método [`setSelectedProvider()`](#setSelectedProvider). Clientes Flash despacham um `MVPDEvent` compartilhado do tipo &quot;`mvpdSelection`&quot;, transmitindo o provedor selecionado.
1. Para aplicativos do Android, se com.android.chrome estiver disponível, o URL de autenticação será carregado nas Guias personalizadas do Chrome.
1. Por meio das Guias personalizadas do Chrome, o usuário chega à página de logon do MVPD e insere suas credenciais. Observe que várias operações de redirecionamento ocorrem durante essa transferência.
1. Quando as Guias personalizadas do Chrome detectam que um URL corresponde ao esquema (adobepass://) e ao deep link do recurso &quot;redirect\_uri&quot; (ou seja, adobepass://com.adobepass ), o AccessEnabler recupera o token de autenticação real dos servidores de back-end. Observe que os URLs de redirecionamento finais são realmente inválidos e não se destinam a que as Guias personalizadas do Chrome realmente os carreguem. Eles só devem ser interpretados pelo SDK como um sinal de que o fluxo de autenticação foi concluído.
1. O AccessEnabler informa ao aplicativo que o fluxo de autenticação foi concluído. O AccessEnabler chama o retorno de chamada [`setAuthenticationStatus()`](#setAuthNStatus) com um código de status 1, indicando sucesso. Se houver um erro durante a execução dessas etapas, o retorno de chamada [`setAuthenticationStatus()`](#setAuthNStatus) será disparado com um código de status 0, juntamente com um código de erro correspondente, indicando falha de autenticação.

### Fluxo de trabalho de logout {#logout}

Para clientes nativos, os logouts são tratados de forma semelhante ao processo de autenticação descrito acima. Seguindo esse padrão, o AccessEnabler abre as Guias personalizadas do Chrome e carrega o URL do endpoint de logout no servidor de back-end.



**Observação:** sair de uma sessão de Programador/MVPD limpará
armazenamento subjacente para essa MVPD específica, incluindo todos os
outros tokens de autenticação de Programador obtidos por SSO em
desse dispositivo. Os tokens obtidos para outros MVPDs ou não por meio do SSO não
ser excluídos.


## Tokens {#tokens}

- [Definições e uso](#definitions)
- [Diretrizes de armazenamento em cache](#caching)
- [Persistência](#persistence)
- [Formato](#format)
- [Vinculação de dispositivo](#device_binding)



### Definições e uso {#definitions}

A solução de direito de autenticação da Adobe Pass gira em torno da geração de dados (tokens) específicos que a autenticação da Adobe Pass gera após a conclusão bem-sucedida dos fluxos de trabalho de autenticação e autorização. Esses tokens são armazenados localmente no dispositivo Android do cliente.

Os tokens têm uma duração limitada; após a expiração, os tokens precisam ser reemitidos por meio da reinicialização dos workflows de autenticação e/ou autorização.

Há três tipos de tokens emitidos durante os workflows de direito:

- **Token de autenticação** - O resultado final do fluxo de trabalho de autenticação do usuário será uma GUID de autenticação que o AccessEnabler poderá usar para fazer consultas de autorização em nome do usuário. Este GUID de autenticação terá um valor TTL (time-to-live) associado que pode ser diferente da própria sessão de autenticação do usuário. A Autenticação do Adobe Pass gera um token de autenticação vinculando o GUID de autenticação ao dispositivo que inicia as solicitações de autenticação.
- **Token de autorização** - Concede acesso a um recurso protegido específico identificado por um `resourceID` exclusivo. Consiste em uma concessão de autorização emitida pela parte autorizadora junto com o `resourceID` original. Essas informações estão vinculadas ao dispositivo que inicia a solicitação.
- **Token de mídia de vida curta** - O AccessEnabler concede acesso ao aplicativo de hospedagem para um determinado recurso, retornando um Token de Mídia de vida curta. Esse token é gerado com base no token de autorização adquirido anteriormente para esse recurso específico. Além disso, esse token não está vinculado ao dispositivo e a duração associada é significativamente menor (padrão: 5 minutos).

Após a autenticação e a autorização bem-sucedidas, a Autenticação do Adobe Pass emitirá tokens de autenticação, autorização e de mídia de vida curta. Esses tokens devem ser armazenados em cache no dispositivo do usuário e usados pela duração de suas vidas associadas.



### Diretrizes de armazenamento em cache {#caching}

- Token de autenticação
- Token de autorização
- Token de mídia


#### Token de autenticação

- **AccessEnabler 1.6 e posterior** - A maneira como os tokens de autenticação são armazenados em cache no dispositivo depende do sinalizador &quot;**Autenticação por Solicitante&quot;** associado ao MVPD atual:


1. Se o recurso &quot;Autenticação por Solicitante&quot; estiver *desabilitado*, um único token de autenticação será armazenado localmente na área de trabalho global. Esse token será compartilhado entre todos os aplicativos integrados ao MVPD atual.
1. Se o recurso &quot;Autenticação por Solicitante&quot; estiver *habilitado*, um token será explicitamente associado ao Programador que executou o fluxo de autenticação (o token não será armazenado na área de trabalho global, mas em um arquivo privado visível somente para o aplicativo desse Programador). Mais especificamente, o Logon único (SSO) entre diferentes aplicativos será desativado; o usuário precisará executar o fluxo de autenticação explicitamente ao alternar para um novo aplicativo (desde que o Programador do segundo aplicativo esteja integrado ao MVPD atual e que não exista um token de autenticação para esse Programador no cache local).

   **Observação:** AEM 1.6 Nota técnica da Google GSON: [Como resolver dependências de Gson](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** - Este SDK apresenta um novo método de armazenamento de token, permitindo vários buckets de Programmer-MVPD e, portanto, vários tokens de autenticação. A partir do AEM 1.7, o mesmo layout de armazenamento é usado para o cenário &quot;Autenticação por solicitante&quot; e para o fluxo de autenticação normal. A única diferença entre os dois é a forma como a autenticação é realizada: &quot;Autenticação por solicitante&quot; contém uma nova melhoria (autenticação passiva) que permite que o AccessEnabler execute a autenticação de canal de retorno, com base na existência de um token de autenticação no armazenamento (para um programador diferente). O usuário só precisa se autenticar uma vez e essa sessão será usada para obter tokens de autenticação em aplicativos subsequentes. Esse fluxo de canal traseiro ocorre durante a chamada do [`setRequestor()`](#setRequestor) e é predominantemente transparente para o Programador. Entretanto, há um requisito importante aqui: o Programador DEVE chamar [`setRequestor()`](#setRequestor) do thread da interface do usuário principal e de uma Atividade.


#### Token de autorização

A qualquer momento, somente UM token de autorização por recurso é armazenado em cache pelo AccessEnabler. Pode haver vários tokens de autorização em cache, mas eles estão associados a recursos diferentes. Sempre que um novo token de autorização for emitido e um antigo já existir para o mesmo recurso, o novo token substituirá o valor em cache existente.



#### Token de mídia

O token de mídia de vida curta NÃO deve ser armazenado em cache. O token de mídia deve ser recuperado do servidor sempre que uma API de autorização for chamada, pois está restrita ao uso único.



### Persistência {#persistence}

Os tokens precisam ser persistentes em execuções consecutivas do mesmo aplicativo. Isso significa que após a aquisição dos tokens de autenticação e autorização e o usuário fechar o aplicativo, os mesmos tokens ficarão disponíveis para o aplicativo quando o usuário reabrir o aplicativo. Além disso, é desejável que esses tokens sejam mantidos em vários aplicativos. Em outras palavras, depois que um usuário usa um aplicativo para fazer logon com um provedor de identidade específico (obtendo com êxito tokens de autenticação e autorização), os mesmos tokens podem ser usados por meio de um aplicativo diferente, e o usuário não recebe mais a solicitação de credenciais ao fazer logon por meio do mesmo provedor de identidade.



Esse tipo de fluxo de trabalho de autenticação/autorização ininterrupta é o que torna a solução de autenticação da Adobe Pass uma implementação real do TV-Everywhere. Do ponto de vista de engenharia, a biblioteca do Android AccessEnabler resolve os problemas do compartilhamento de dados entre aplicativos, armazenando os dados do token em um arquivo de banco de dados localizado no armazenamento externo. Esses recursos compartilhados a nível de sistema fornecem os principais ingredientes que permitem a implementação do caso de uso de tokens persistentes desejado:

- Suporte para armazenamento estruturado - O armazenamento de token de autenticação da Adobe Pass não é apenas uma estrutura linear simples de memória semelhante a um buffer. Ele fornece um mecanismo de armazenamento semelhante a um dicionário que permite a indexação de dados com base em valores de chave especificados pelo usuário.
- Suporte para persistência de dados usando o sistema de arquivos subjacente - O conteúdo do arquivo de banco de dados é mantido por padrão e os dados são salvos na memória externa do dispositivo.



Depois que um token específico for colocado no cache de token, sua validade será verificada em momentos diferentes pela biblioteca do AccessEnabler.  Um token válido é definido como:

- O TTL do token não expirou
- O emissor do token está incluído na lista de provedores de identidade permitidos



A partir do AccessEnabler 1.7, o armazenamento de token pode suportar várias combinações de Programador-MVPD, dependendo de uma estrutura de mapa aninhada de vários níveis que possa conter vários tokens de autenticação. Esse novo armazenamento não afeta a API pública do AccessEnabler de nenhuma maneira e não requer alterações por parte do programador. Veja um exemplo que ilustra essa funcionalidade mais recente:

1. Open App1 (desenvolvido pelo Programmer1).
1. Autentique com MVPD1 (que está integrado ao Programador1).
1. Suspender / Fechar o aplicativo atual e abrir o App2 (desenvolvido pelo Programador2).
1. Vamos supor que o Programador 2 não esteja integrado ao MVPD2; portanto, o usuário NÃO será autenticado no App 2.
1. Autentique com MVPD2 (que está integrado ao Programador2) no App2.
1. Volte para o App1; o usuário ainda será autenticado com o Programador1.

Nas versões anteriores do AccessEnabler, a Etapa 6 renderizava o usuário como não autenticado, pois o armazenamento do token anteriormente só tinha suporte para um token de autenticação.


**OBSERVAÇÃO:** Fazer logoff de uma sessão de Programador/MVPD limpará o armazenamento subjacente, incluindo todos os outros tokens de autenticação de Programador no dispositivo com SSO. Os tokens obtidos para outros MVPDs ou não por meio do SSO não serão excluídos. Cancelar o fluxo de autenticação (invocando [`setSelectedProvider(null)`](#setSelectedProvider)) NÃO limpará o armazenamento subjacente, mas afetará somente a tentativa de autenticação atual de Programador/MVPD (apagando o MVPD do Programador atual).


Outro recurso relacionado ao armazenamento incluído no AccessEnabler 1.7 permite importar tokens de autenticação de áreas de armazenamento mais antigas. Esse &quot;Importador de tokens&quot; ajuda a obter compatibilidade entre versões consecutivas do AccessEnabler, mantendo o estado do SSO mesmo quando a versão de armazenamento é atualizada.

O importador é executado durante o fluxo [`setRequestor()`](#setRequestor) e é executado nos dois cenários a seguir (supondo que nenhum token de autenticação válido para o Programador atual esteja presente no armazenamento atual):

- A primeira instalação de um aplicativo 1.7 desenvolvido por um programador específico
- Caminho de upgrade para um AccessEnabler futuro que usa um novo armazenamento

A operação de importação é transparente para o Programador e não requer nenhuma alteração de código no aplicativo cliente.



### Formato {#format}

- [Token de autenticação](#authn_token)
- [Token de autorização](#authz_token)
- [Token de mídia curta](#short_media_token)
- [Vinculação de dispositivo](#device_binding)

#### Token de autenticação {#authn_token}

A listagem abaixo apresenta o formato do token de autenticação:

```XML
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### Token de autorização {#authz_token}

A lista abaixo apresenta o formato do token de autorização:

```XML
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### Token de mídia curta {#short_media_token}

A lista abaixo apresenta o formato do token de mídia curta.  Esse token é exposto ao aplicativo do Programador.  Ele é passado para o aplicativo do Programador ao final de um processo de qualificação bem-sucedido:

```XML
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### Vinculação de dispositivo {#device_binding}

Nas listagens de XML acima, observe a marca intitulada `simpleTokenFingerprint`. A finalidade dessa tag é manter informações de individualização de ID de dispositivo nativas. A biblioteca do AccessEnabler pode obter essas informações de individualização e disponibilizá-las para os serviços de autenticação da Adobe Pass durante as chamadas de qualificação. O serviço usará essas informações e as incorporará aos tokens reais, vinculando, assim, efetivamente os tokens a um dispositivo específico. O objetivo final disso é tornar os tokens intransferíveis entre dispositivos.



Nas listagens XML acima, observe a tag denominada simpleTokenFingerprint. A finalidade dessa tag é manter informações de individualização de ID de dispositivo nativas. A biblioteca do AccessEnabler pode obter essas informações de individualização e disponibilizá-las para os serviços de autenticação da Adobe Pass durante as chamadas de qualificação. O serviço usará essas informações e as incorporará aos tokens reais, vinculando, assim, efetivamente os tokens a um dispositivo específico. O objetivo final disso é tornar os tokens intransferíveis entre dispositivos.



Como esse é obviamente um recurso relacionado à segurança, essas informações são inerentemente &quot;confidenciais&quot; do ponto de vista da segurança. Como resultado, essas informações precisam ser protegidas contra violações e espionagem. O problema de espionagem é resolvido enviando as solicitações de autenticação/autorização pelo protocolo HTTPS. A proteção contra violação é tratada assinando digitalmente as informações de identificação do dispositivo. A biblioteca do AccessEnabler calcula uma ID de dispositivo a partir das informações fornecidas pelo dispositivo e, em seguida, envia a ID de dispositivo &quot;em branco&quot; para os servidores de autenticação da Adobe Pass como um parâmetro de solicitação.  Os servidores de Autenticação do Adobe Pass assinam digitalmente a ID do dispositivo com a chave privada Adobe e a adicionam ao token de autenticação retornado ao AccessEnabler. Assim, a ID do dispositivo é vinculada ao token de autenticação.  Durante o fluxo de autorização, o AccessEnabler envia novamente a ID do dispositivo sem criptografia, juntamente com o token de autenticação.  A falha do processo de validação resultará automaticamente na falha dos workflows de autenticação/autorização.  Os servidores de Autenticação do Adobe Pass aplicam a chave privada à ID do dispositivo e a comparam com o valor no token de autenticação.  Se não corresponderem, o fluxo de direitos falhará.


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
