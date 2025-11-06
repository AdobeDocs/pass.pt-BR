---
title: Visão geral da REST API
description: Visão geral para APIs Rest
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1635'
ht-degree: 0%

---

# Visão geral da API REST (herdada) {#rest-api-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Visão geral {#over}

A API REST de autenticação da Adobe Pass fornece acesso direto aos serviços de autenticação e autorização da TV em qualquer lugar (TVE). Essa API é compatível com duas arquiteturas principais: servidor para servidor ou dispositivos conectados (por exemplo, consoles de jogos, TVs inteligentes, decodificadores de sinais, etc.) aplicativos que não têm recursos de navegação na Web.

### Mecanismo de limitação

A API REST de Autenticação do Adobe Pass é regida por um [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).


### Servidor para servidor

As soluções de servidor para servidor envolvem aplicativos cliente de programador que se integram aos serviços de programador que se conectam aos serviços de autenticação da Adobe Pass para fluxos TVE. Essa abordagem transfere a maior parte da implementação da TVE do cliente para o servidor, onde um único módulo de autorização unificado pode ser criado e mantido. A principal responsabilidade restante dos aplicativos clientes é o gerenciamento de uma visualização da Web para autenticação de usuário.



### Dispositivos conectados

Os aplicativos Dispositivos conectados se comunicam diretamente com a Autenticação do Adobe Pass por meio das APIs REST para executar configurações, registros, verificações de status de autenticação e fluxos de autorização, enquanto um segundo aplicativo de tela (navegador) é necessário para o fluxo de autenticação. Dessa forma, os SDKs nativos não são usados.



### Outras arquiteturas

Além das duas arquiteturas principais baseadas na API REST, servidor para servidor e soluções de cliente direto para dispositivos inteligentes, há outras arquiteturas.  A arquitetura do SDK é a principal delas, pois usa um componente do cliente chamado Ativador de acesso que a Autenticação do Adobe Pass fornece aos Programadores.  O aplicativo usa APIs do Access Enabler para lidar com inicialização, autenticação, autorização e logout.  Toda a comunicação entre o aplicativo do programador e os servidores de autenticação da Adobe Pass ocorre por meio do Access Enabler.  Um tipo diferente de Access Enabler está disponível para as seguintes plataformas: JavaScript, iOS, tvOS, Android e FireTV.

Embora seja possível usar a REST API diretamente em plataformas clientes que oferecem suporte a SDKs nativos fora de uma solução de servidor para servidor, essa abordagem não é recomendada.



## Prós e contras da API REST {#ProsAndCons}

A API REST de autenticação do Adobe Pass foi criada para fornecer uma solução de TV em todos os lugares (TVE) para dispositivos que não têm recursos de navegação na Web ou armazenamento persistente. A API REST fornece suporte para todos os fluxos de autenticação e autorização, mas porque falta um componente nativo do SDK. Os SDKs fornecidos e mantidos pela Autenticação Adobe Pass vêm com funcionalidades prontas para uso que implementam regras de negócios que, no caso da REST API, devem ser implementadas e mantidas pelos Programadores. Na tabela Responsabilidades do programador abaixo, descrevemos as limitações da API REST atual que precisam ser abordadas pelos programadores.



### Prós e contras baseados em servidor versus servidor

Uma arquitetura de servidor para servidor fornece uma maneira de consolidar a maior parte da lógica relacionada à autenticação e autorização em uma única unidade lógica ou implementação.  Essa abordagem tem prós e contras.  As vantagens incluem:

* Implementação única para lógica de negócios de autenticação e autorização.
* Evite a necessidade de implementar essa lógica em cada plataforma compatível usando as ferramentas nativas dessas plataformas.
* A capacidade de atualizar recursos sem precisar atualizar clientes com todos os requisitos associados (por exemplo, atualizações da loja de aplicativos).
* **Mais facilmente** estenda e personalize os recursos de authN e authZ (por exemplo, adicionar D2C).
* Gestão direta do tráfego associado para maior controle, qualidade e monitoramento.



Novamente, os contras são listados nas responsabilidades do Programador, mas incluem o seguinte:

* O SSO deve ser implementado para cada cliente para plataformas sem SSO de plataforma.
* Os programadores devem implementar uma lógica específica do MVPD, se necessário.
* Todas as plataformas que usam a API REST compartilham uma única configuração que controla propriedades, como TTLs de autenticação.



### Dispositivos conectados

Para a maioria dos dispositivos conectados, a API REST deve ser usada de uma forma ou de outra, pois uma SDK não está disponível. O dispositivo conectado usará a REST API diretamente ou se integrará a uma solução de servidor para servidor que usa a REST API.

## Responsabilidades do programador {#programmer-responsibilities}

Os itens a seguir se aplicam aos aplicativos Servidor para servidor e Dispositivo conectado.

| **Funcionalidade** | **Responsável pela manipulação da funcionalidade** | **Descrição das limitações da API sem cliente atual e diferenças em relação aos SDKs nativos** |
| --- | --- | --- |
| Configurações aplicadas por plataforma | Adobe | Uma **grande limitação** no uso da API REST em todas as plataformas (incluindo dispositivos móveis como iOS e Android) é que as definições de configuração correspondentes à API REST em nossa ferramenta de configuração do Painel TVE são aplicadas em todos os dispositivos (mesmo se houver um dispositivo iOS que executa um aplicativo nativo implementado sobre nossa API REST). Esta limitação **pode quebrar** os TTLs e as configurações de plataforma acordadas com os MVPDs, se diferirem entre si em cada plataforma. [1](#1) |
| Logon único | Programadores | Usando a API REST, o SSO está disponível apenas em plataformas que oferecem suporte ao SSO da plataforma (por exemplo, Apple, Roku, Amazon), enquanto o SSO não pode ser garantido para outras plataformas ao usar a API REST. Os SDKs armazenam dados em cache entre sites/aplicativos. Isso significa que o usuário faz logon uma vez em um site/aplicativo e já está conectado nos sites participantes, sem nenhuma interação do usuário necessária. [2](#2) |
| Logout único | Programadores | Em um cenário nativo de SSO do SDK, encerrar a sessão em um aplicativo participante encerra a sessão do usuário em qualquer lugar. Na API REST atual, não oferecemos suporte ao SLO. Fazer logoff de um aplicativo desconectará o usuário somente desse aplicativo específico. |
| Armazenamento em cache | Programadores | As implementações da REST API terão que implementar seu próprio mecanismo de armazenamento em cache para itens de dados acordados com a empresa. Os SDKs estão armazenando em cache automaticamente vários itens de dados, enquanto consideram várias regras de negócios. Por exemplo, os metadados do usuário são armazenados em cache com o mesmo TTL do token de autenticação, enquanto alguns itens podem ser excluídos programaticamente do cache (simulação). |
| Mecanismo detalhado de relatório de erros | Programadores | A REST API depende principalmente de códigos de erro HTTP para relatar erros de aplicativo, enquanto os SDKs têm um mecanismo detalhado de relatório de erros que ajuda os desenvolvedores de aplicativos a entender melhor o que está acontecendo. |
| Recuperação de erros de aplicativo (tentativa de erro, detecção de loop etc.) | Programadores | As implementações da API REST precisam criar seus próprios sistemas de recuperação de aplicativos, enquanto as implementações sobre os SDKs se beneficiam do sistema de recuperação de erros dos SDKs: recuperação de erros de rede transitórios, repetindo determinadas chamadas de rede com lógica em vigor para evitar &quot;loops&quot;. |
| Autenticação por solicitante | Programadores | Alguns MVPDs exigem autenticação para cada site/aplicativo, seja por motivos comerciais ou devido a desafios técnicos. Os SDKs impõem isso automaticamente com base na configuração do Painel TVE. Os implementadores da REST API devem implementar isso sozinhos, a fim de não violar os contratos comerciais ou para poder concluir a autorização para os MVPDs que definem seus dados de autenticação por aplicativo. |
| Autenticação passiva | Programadores | Alguns MVPDs suportam autenticação &quot;passiva&quot;, em que o usuário não é obrigado a inserir as credenciais, e uma tentativa de autenticar o usuário é feita automaticamente. Isso é especialmente útil para MVPDs que também têm &quot;Autenticação por solicitante&quot; como um requisito. Para esse caso, o UX habilitado pelos SDKs é ininterrupto, em que o usuário se autentica apenas uma vez em um aplicativo e o SDK executa autenticação &quot;passiva&quot; para outros aplicativos no ecossistema. O usuário não vê as chamadas passivas adicionais, ele simplesmente já estará autenticado, enquanto o MVPD atinge o objetivo de ter sessões de autenticação separadas para cada aplicativo. |
| Detecção implícita e uniforme de dispositivos | Programadores | Os SDKs detectam automaticamente o tipo de dispositivo e enviam essas informações de forma padrão. As implementações da REST API são propensas ao envio de informações diferentes, dependendo do implementador, distorcendo/quebrando regras de negócios e estatísticas entre sites. **Os programadores precisam verificar se nos enviaram as informações corretas sobre o dispositivo** em cada um de seus aplicativos. Para implementações de servidor para servidor, a API REST não identifica corretamente o endereço IP do usuário final, a menos que etapas adicionais sejam executadas. O endereço IP é importante em certos casos de uso, como prevenção de fraudes ou HBA. |
| TempPass à prova de adulteração | Programadores | A imposição de TempPass é baseada em uma ID de dispositivo estável. Os SDKs usam informações de hardware/técnicas de impressão digital para identificar o dispositivo e esse mecanismo não é público e, portanto, mais seguro e livre de colisões. Para implementações da API REST, os programadores precisam implementar seus próprios algoritmos de identificação/impressão digital do dispositivo. |
| Vinculação de dispositivo | Programadores | Os tokens gerados em um dispositivo com um aplicativo por meio do SDK não são portáteis, dificultando que um usuário mal-intencionado compartilhe seus tokens e forneça acesso a outros usuários. Isso se baseia no mesmo mecanismo de ID de dispositivo que o &quot;TempPass à prova de adulteração&quot;. Para a API REST, os tokens permanecem na nuvem, o que significa que dois dispositivos com as mesmas deviceIDs fazendo as mesmas chamadas obterão as mesmas respostas/acesso. Os programadores precisam garantir que tenham um mecanismo robusto, seguro e sem colisões para gerar/alocar IDs de dispositivos. |
| Conjunto de recursos previsível e certificado | Programadores | O conjunto existente de recursos em cada SDK é consistente, previsível e totalmente certificado e testado. Alguns requisitos de negócios são aplicados por meio dos SDKs, tornando arriscado para o programador violar contratos ao usar a REST API. Embora a REST API possa ser mais flexível, a Adobe garante que as implementações do SDK reforcem as decisões e as configurações dos negócios no TVE Dashboard. No caso dos sem clientes, cada Programador implementa seu próprio subconjunto de regras de negócios que suíça seus aplicativos em um determinado momento. |


1. Como parte de nossa nova iniciativa Uma API, pretendemos corrigir essa limitação e poder aplicar regras por plataforma com base na identificação do dispositivo.

2. A Adobe continua trabalhando com todas as principais plataformas para implementar o Platform SSO, que pode ser usado com nossa API REST. Nossa iniciativa Uma API oferecerá suporte a SSO entre aplicativos implementados com SDKs nativos e aplicativos implementados com a API REST.

## Requisitos mínimos do dispositivo {#min_reqs}

Para usar a API REST de Autenticação do Adobe Pass, os dispositivos devem atender ou exceder os requisitos técnicos mínimos listados na seção API REST do [documento Requisitos da Plataforma de Autenticação/Dispositivo/Ferramentas do Adobe Pass](#general_clientless_reqs).
