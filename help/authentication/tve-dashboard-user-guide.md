---
title: Guia do usuário do Painel Primetime TVE
description: Guia do usuário do Painel Primetime TVE
exl-id: 6f7f7901-db3a-4c68-ac6a-27082db9240a
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '4377'
ht-degree: 0%

---

# Guia do usuário do Painel Primetime TVE {#tve-db-user-guide}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#tve-db-intro}

O [[!DNL Adobe] Painel do TVE (Painel do TVE)](https://console.auth.adobe.com/) é um painel de autoatendimento destinado a usuários que trabalham para empresas de mídia (Programadores) que têm um relacionamento comercial com a equipe de produtos de Autenticação da Adobe Pass.

Entre em contato com o gerente técnico de conta (TAM) para obter acesso. Para obter acesso, você precisará de dois novos grupos de usuários a serem configurados em sua organização da Adobe Marketing Cloud:

* Leitura-gravação do painel TVE - os membros deste grupo têm direitos totais em todas as seções editáveis do painel
* Painel TVE Somente Leitura - os membros deste grupo só têm direitos de visualização em todo o painel


Antes de aprofundar esse guia do usuário, recomendamos que você passe pelos seguintes recursos para ter uma boa compreensão dos fluxos e recursos fornecidos pela equipe de produtos de Autenticação do Adobe Pass e se familiarizar com os termos usados no presente documento:

* [Documento técnico da TVE](/help/authentication/technical-paper.md)
* [Guia de início rápido do programador](/help/authentication/programmer-kickstart-guide.md)
* [Fluxo de direitos](/help/authentication/entitlement-flow.md)
* [Glossário](/help/authentication/glossary.md)


Prosseguindo para as próximas seções deste guia do usuário, você descobrirá maneiras de administrar diferentes configurações para os Canais, Programadores ou as Integrações entre Canais e MVPDs (Distribuidores de Programas de Vídeo Multicanais) da sua empresa.

>[!IMPORTANT]
>O TVE Dashboard oferece a opção de alternar entre um Workspace básico e um avançado. Você pode fazer isso alternando o ícone no canto superior direito. O Workspace avançado destina-se a usuários com conhecimento técnico substancial, bem como conhecimento avançado dos recursos oferecidos pela equipe de produtos de autenticação da Adobe Pass.

![Espaços de trabalho do painel do TVE](assets/tve-basic-advanced-workspace.png)

*Figura 1: o menu suspenso &quot;Workspace Básico / Avançado&quot; do Painel do Adobe Primetime TVE*

## Ambientes {#authn-environments}

Dependendo das tarefas que um usuário pode ter de realizar, ele pode precisar alternar entre os ambientes de Autenticação do Adobe Pass. Para obter informações detalhadas sobre os ambientes de Autenticação do Adobe Pass, consulte o seguinte documento: [Entendendo os ambientes de Autenticação do Adobe Pass](/help/authentication/understanding-the-adobe-environments.md).

O Painel do TVE fornece dois ambientes chamados Pré-qualificação e Versão, cada um com dois perfis chamados Preparo e Produção, conforme mostrado abaixo:

* [Preparo Diferente](https://console-prequal.auth-staging.adobe.com/)
* [Produção Pré-Igual](https://console-prequal.auth.adobe.com/)
* [Estágios de versão](https://console.auth-staging.adobe.com/)
* [Produção de versão](https://console.auth.adobe.com/)

Para alternar entre ambientes, o usuário pode clicar no ambiente desejado representado pela entrada do elemento suspenso representado abaixo:

![Lista suspensa de ambientes do Painel do TVE](assets/tve-dashboard-env.png)

*Figura 2: o menu suspenso de ambientes do Painel do Adobe Pass TVE*

>[!IMPORTANT]
>
>É muito importante observar que, ao fazer alterações administrativas na configuração da Autenticação do Adobe Pass por meio do Painel do TVE, recomendamos que você siga a sequência abaixo para garantir a funcionalidade adequada.

Para fazer alterações administrativas na configuração da Autenticação Adobe Pass por meio do Painel TVE:

* Execute as alterações em [Estágios de versão e valide-as](http://sp.auth-staging.adobe.com/apitest/api.html).
* Executar as alterações em [Produção em Paralelo e validá-las](http://sp.auth-staging.adobe.com/apitest/api.html).
* Execute as alterações em [Liberar Produção e valide-as](http://sp.auth-staging.adobe.com/apitest/api.html).

>[!IMPORTANT]
>
>Para que as alterações administrativas sejam ativadas, os usuários devem navegar até a seção &quot;Revisar e enviar alterações&quot; selecionando o botão, que aparecerá na parte inferior esquerda da barra lateral. Para revisar as alterações, adicione uma descrição das alterações recém-criadas e confirme a atualização de configuração selecionando a &quot;Configuração de envio&quot;.

![Exibir notificação por push e revisão do painel](assets/tve-review-push-notifications.png)

*Figura 3: A notificação Revisar e enviar alterações do painel do Adobe Primetime TVE*

## Seções {#sections}

Os usuários que trabalham para empresas de mídia (programadores) podem acessar as seguintes seções do Painel TVE na barra lateral:

* **Canais** - Contém configurações relacionadas a provedores de conteúdo
* **Programadores** - Contém configurações relacionadas à organização principal agregando um ou vários **Canais**
* **Integrações** - Contém configurações relacionadas à integração entre **Canais** e **MVPDs**
* **MVPDs** - Contém configurações relacionadas aos **MVPDs** disponíveis
* **Relatórios** - Contém dados agregados para três tipos de relatórios: AuthN TTL, AuthZ TTL, SSO
* **Log de Alterações** - Contém as modificações mais recentes aplicadas à configuração do Painel TVE

![Seções do painel do TVE](assets/tve-dashboard-sections.png)

*Figura 4: Seções do Adobe Primetime TVE Dashboard*

### Canais {#tve-db-channels-section}

Esta seção permite visualizar e editar as configurações dos Canais disponíveis ou criar um novo. Clicar em um dos canais disponíveis retornará uma tela com as seguintes guias:

* **Dados do canal**
   * **ID do Canal** - A ID exclusiva do Canal usada em nosso sistema, também chamada de &quot;ID do solicitante&quot;.
   * **Nome para Exibição** - O nome comercial do Canal.
* **Configurações gerais**
   * **Configuração do Analytics** - Configure os eventos de Autenticação do Adobe Pass para serem encaminhados ao Adobe Analytics. Entre em contato com o Adobe para obter mais detalhes sobre como a ID do conjunto de relatórios (RSID) precisa ser configurada antes de habilitar esse recurso.
* **Certificados**

  Contém a lista de certificados utilizados no fluxo de autenticação juntamente com a organização emissora, a data de emissão e a data de expiração. Esses certificados servem como chaves privadas/públicas e são usados para fins de validação.
* **Domínios**

  Contém a lista de domínios a partir dos quais o respectivo Canal se comunicará com a Autenticação Adobe Pass.
* **Integrações**

  Contém a lista de integrações com os MVPDs disponíveis, juntamente com o status de cada integração que pode ser ativada ou não. Navegar para a página Integração está disponível ao clicar em uma entrada específica.
* **Aplicativos Registrados**

  Contém a lista de registros de solicitações de emprego. Para obter mais detalhes, consulte o documento [Gerenciamento dinâmico de registro de cliente](/help/authentication/dynamic-client-registration-management.md).

* **Esquemas personalizados**

  Contém a lista de esquemas personalizados. Para obter mais detalhes, consulte [registro de aplicativo iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gerenciamento dinâmico de registro de cliente](/help/authentication/dynamic-client-registration-management.md)


#### Adicionar/Excluir domínios {#add-delete-domains}

Para iniciar o processo de adição de um novo domínio para o canal selecionado, você precisa clicar no botão &quot;Adicionar novo domínio&quot; abaixo da lista Domínios. Isso criará uma nova entrada de domínio onde você pode especificar o nome do domínio. Se um domínio mais genérico já existir na lista de domínios, você não deve adicionar um novo subdomínio.

![Adicionar um novo domínio a uma seção de canal selecionada](assets/add-domain-to-channel-sec.png)

*Figura: guia Domínios nos canais*

### Programadores {#tve-db-programmers-section}

Esta seção permite visualizar e editar configurações para Programadores disponíveis ou criar um novo. Clicar em um dos programadores disponíveis retornará uma tela com as seguintes guias:

* **Dados do programador**
   * **ID do Programador** - A ID exclusiva do Programador usada em nosso sistema.
   * **Nome para Exibição** - O nome comercial do Programador.
   * **URL do Logotipo** - O URL do logotipo comercial do programador.
   * **Visualização do logotipo** - A visualização do logotipo comercial do programador baixando-o da URL acima.

* **Certificados**

  Contém a lista de certificados utilizados no fluxo de autenticação juntamente com a organização emissora, a data de emissão e a data de expiração. Esses certificados servem como chaves privadas/públicas e são usados para fins de validação.

* **Canais**

  Contém a lista de canais que pertencem a este programador específico. Navegar até a seção Canais está disponível ao clicar em uma entrada específica.

* **Aplicativos registrados**

  Contém a lista de registros de solicitações de emprego. Para obter mais detalhes, consulte [Gerenciamento dinâmico de registros de clientes](/help/authentication/dynamic-client-registration-management.md).

* **Esquemas personalizados**

  Contém a lista de esquemas personalizados. Para obter mais detalhes, consulte [registro de aplicativo iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gerenciamento dinâmico de registro de cliente](/help/authentication/dynamic-client-registration-management.md).


### Integrações {#tve-db-integrations-sec}

Esta seção permite visualizar e editar configurações para integrações entre Canais e MVPDs disponíveis ou criar uma nova. Clicar em uma das integrações disponíveis retornará uma única página ao usar o Workspace básico ou uma tela com as seguintes guias ao usar o Workspace avançado:

* **Dados de integração**
   * **Id de Integração**- O resultado do acréscimo da ID exclusiva MVPDs à ID exclusiva do Canal separada pelo caractere &quot;_&quot;.
   * **Nome para Exibição do Canal** - O nome comercial do Canal.
   * **ID do Canal** - A ID exclusiva do Canal usada em nosso sistema, também chamada de &quot;ID do solicitante&quot;.
   * **Nome para Exibição do MVPD** - O nome comercial do MVPD.
   * **ID do MVPD** - A ID exclusiva do MVPD usada em nosso sistema.
* **Configurações gerais**
   * **Chaves de metadados do usuário** - Configure as chaves de metadados disponíveis para a integração específica.
   * **Configurações Específicas de Plataforma** - Defina configurações diferentes para uma plataforma específica (por exemplo, TTLs, SSO e IFrames).

* **Configurações de autenticação**
   * Contém configurações relacionadas ao recurso de autenticação Autenticação do Adobe Pass.
* **Configurações de autorização**
   * Contém configurações relacionadas ao recurso de autorização Autenticação do Adobe Pass.
* **Configurações de logoff**
   * Contém configurações relacionadas ao recurso de logout de Autenticação do Adobe Pass.

#### Criar integração {#create-integration}

Para criar uma nova integração, siga as etapas abaixo:

* clique no botão &quot;Adicionar nova integração&quot;
* pesquisar e selecionar um canal
* pesquisar e selecionar um MVPD
* aguarde o TVE Dashboard calcular a &quot;ID de Integração&quot; e exibir os pontos de extremidade MVPD disponíveis
* selecione os endpoints de autenticação, autorização e logout ou use os valores padrão
* clique no botão &quot;Criar integração&quot;
* dependendo das configurações do MVPD, um pop-up pode ser exibido e solicitar propriedades adicionais, que devem ter sido fornecidas pelo MVPD anteriormente. Caso contrário, ocorrerá um redirecionamento para a página de integração recém-criada

![](assets/new-integration-window.png)



*Figura 5. A janela Nova Integração do Painel do Adobe Primetime TVE*


#### Atualizar integração {#update-integration}

Para atualizar uma integração existente, clique na entrada da tabela dessa integração específica na seção Integrações ou na seção Canais, que contém uma guia Integrações.

Ao usar o modo Workspace básico, esta seção permitirá visualizar e editar as configurações mais atualizadas, como TTLs de token de autenticação e autorização (tempo de vida útil), bem como configurações de iFrame. Lembre-se de que as configurações de TTL podem estar ausentes nas integrações com MVPDs que oferecem suporte a TTL de Persistência de Token Definido Dinamicamente (consulte a entrada 1.19 de [Requisitos de Integração de MVPD](/help/authentication/mvpd-integr-features.md)).



Ao usar o modo Workspace avançado, esta seção permitirá visualizar e editar configurações menos comuns.



No caso dos modos Básico e Avançado do Workspace, essas configurações podem ser alteradas no nível da plataforma (por exemplo, selecione um valor personalizado para o token de autorização TTL no Android, padrão em todas as outras plataformas).



>[!IMPORTANT]
>É importante entender as configurações da cadeia de herança: MVPD -> Ponto de extremidade MVPD -> Integração -> Plataforma, onde Plataforma tem o valor mais específico e MVPD o padrão mais genérico.

![](assets/inheritance-chain-component.png)


*Figura 6. O componente da cadeia de herança de propriedades do Painel do Adobe Primetime TVE*


#### Configurações específicas da plataforma {#platform-sp-settings}

Esta subseção pode ser usada para substituir as configurações de plataformas específicas. As plataformas disponíveis são:

* **Todas as Plataformas** - Defina valores que serão aplicados a todas as plataformas, independentemente das implementações do Programador, caso não haja outros valores definidos para uma plataforma específica.
* **Android** - Defina valores que serão aplicados às implementações do Programador por meio do SDK Android de Autenticação da Adobe Pass.
* **API REST sem cliente** - Defina valores que serão aplicados às implementações do Programador na API REST de Autenticação Adobe Pass.
* **Fire TV** - Defina valores que serão aplicados às implementações do Programador no SDK FireTV de Autenticação do Adobe Pass.
* **SDK do Flash** - Esta plataforma foi descontinuada. **obsoleto**
* **SDK do JavaScript** - Defina valores que serão aplicados às implementações do Programador pelo SDK JavaScript de Autenticação da Adobe Pass.
* **Roku** - Defina valores que serão aplicados às implementações do Programador pela API REST de Autenticação Adobe Pass e que estão enviando &quot;Roku&quot; como o tipo de dispositivo. No caso de dispositivos Roku, isso tem prioridade sobre os valores definidos para a plataforma da API REST sem cliente.
* **SDK nativo do Xbox** - Esta plataforma foi descontinuada. **obsoleto**
* **API REST do Xbox 360** - Defina valores que serão aplicados às implementações do Programador pela API REST de Autenticação Adobe Pass e que estão enviando &quot;xbox&quot; como o tipo de dispositivo. No caso de dispositivos Xbox 360, isso tem prioridade sobre os valores definidos para a plataforma REST API sem cliente.
* **API REST do Xbox One** - Defina valores que serão aplicados às implementações do Programador pela API REST de Autenticação Adobe Pass e que estão enviando &quot;xboxOne&quot; como o tipo de dispositivo. No caso de dispositivos XboxOne, esses valores têm prioridade sobre os valores definidos para a plataforma REST Api sem Cliente.
* **iOS** - Defina valores que serão aplicados às implementações do Programador por meio do SDK iOS de Autenticação da Adobe Pass.
* **tvOS** - Defina valores que serão aplicados às implementações do Programador através do SDK tvOS de Autenticação da Adobe Pass.


![](assets/platform-sp-settings.png)

*Figura 7. As Configurações Específicas da Plataforma de Painel do Adobe Primetime TVE*


#### Habilitar logon único da plataforma {#enable-platform-sso}

Siga as etapas abaixo para habilitar/desabilitar o Logon único para uma integração e plataforma específicas:

* verifique se você está usando o modo Workspace avançado
* navegue até a integração desejada
* navegue até a guia **Configurações gerais**
* selecione a plataforma desejada na qual você deseja ativar ou desativar o Single Sign-On
* alterne o sinalizador **Habilitar Logon Único** para o valor desejado (Sim/Não)

  >[!IMPORTANT]
  >É importante observar que o sinalizador **Habilitar Logon Único** está disponível somente para plataformas iOS, tvOS, Roku e FireTV, e somente para integrações com MVPDs que oferecem suporte a Logon Único para essas plataformas.

* alterne o sinalizador **Impor Permissão de Plataforma** para o valor desejado (Sim/Não)

  >[!IMPORTANT]
  >É importante observar que o sinalizador **Impor permissão da plataforma** controla se a decisão do usuário de Permitir ou Negar acesso à plataforma para sua assinatura do Provedor de TV será imposta ou não. Considerando o cenário em que o sinalizador **Habilitar Logon Único** está definido como &quot;Sim&quot;, o sinalizador **Impor Permissão de Plataforma** também está definido como &quot;Sim&quot;, e o usuário opta por Negar acesso à plataforma para sua assinatura do Provedor de TV. Assim, o respectivo aplicativo (canal) não poderá usar o token de Autenticação do Adobe Pass obtido por outro aplicativo (canal).


#### Ativar autenticação baseada em casa {#enable-hba}

Siga as etapas abaixo para habilitar/desabilitar a Autenticação Home-Base para MVPDs baseados em **OAuth2**:

* verifique se você está usando o modo Workspace avançado
* navegue até a integração desejada
* navegue até a guia **Configurações de autenticação**
* navegar até a subguia **Regras dinâmicas de autenticação**
* alterne o sinalizador de **Tentativa de HBA** para o valor desejado (Sim/Não)


>[!IMPORTANT]
>Lembre-se de que o valor &quot;HBA AuthN TTL&quot; nunca deve ser substituído, caso contrário, o fluxo de autorização pode falhar inesperadamente.

Entre em contato com **tve-support@adobe.com** para obter informações sobre como habilitar a Autenticação Home-Base para MVPDs baseados em SAML.

### MVPDs {#tve-db-mvpds-sec}

Esta seção permite exibir configurações para MVPDs disponíveis. Clicar em um dos MVPDs disponíveis retornará uma tela com as seguintes guias:

* **Dados MVPD**
   * **ID do MVPD** - A ID exclusiva do MVPD usada em nosso sistema.
   * **Nome para Exibição** - O nome comercial do MVPD que pode ser usado no seletor do usuário.
   * **URL do Logotipo** - A URL do logotipo comercial do MVPD.
   * **Visualização do logotipo** - A visualização do logotipo comercial do MVPD baixando-o da URL acima.
* **Configurações gerais**
   * **Chaves de metadados do usuário**
      * Chaves de metadados disponíveis para o MVPD específico.
   * **Propriedades dos Dados do Cliente**
      * **Autenticação/Agregador** - Se definido como &quot;Sim&quot;, um novo token de autenticação será necessário para cada novo Canal que o usuário estiver tentando acessar.
      * **Autenticação Passiva Habilitada** - Se o sinalizador Autenticação/Agregador estiver definido como &quot;Sim&quot; e se Autenticação Passiva Habilitada estiver definido como &quot;Sim&quot;, o processo de autenticação com outro Canal ocorrerá em segundo plano, sem a necessidade de um redirecionamento completo do navegador e a exibição do seletor.
      * **Autenticação/sessão do navegador** - Se definido como &quot;Sim&quot;, o usuário será desconectado após fechar o navegador. Se definido como &quot;Não&quot;, o usuário poderá reiniciar o navegador e permanecer conectado.
      * **IFrame Necessário** - Se definido como &quot;Sim&quot;, isso indica que a janela de logon do MVPD requer um iFrame. Os campos &quot;Largura do iFrame&quot; e &quot;Altura do iFrame&quot; representam o tamanho necessário para o iFrame carregar a página de logon do MVPD.
* **Configurações de autenticação**
   * **Selecionar Ponto de Extremidade**
      * Esse campo indica os endpoints de autenticação expostos pelo MVPD. O endpoint pode diferir de acordo com o protocolo de autenticação usado.
   * **Configurações Gerais de AuthN**
      * Esta subguia exibe o protocolo de autenticação usado pelo MVPD e informações relacionadas ao protocolo.
   * **Certificados AuthN**
      * Esta subguia exibe os certificados que o MVPD usa no fluxo de autenticação junto com sua organização emissora, data de emissão e data de expiração. Esses certificados servem como chaves privadas/públicas e são usados para fins de validação.
   * **Regras Dinâmicas de Autenticação**
      * Esta subguia exibe as regras que se aplicam ao processo de autenticação. Ao pressionar a opção Request / Response / Token do diagrama, você pode ver como destacados os parâmetros aplicados a essa parte do fluxo de autenticação.
* **Configurações de autorização**
   * **Selecionar Ponto de Extremidade**
      * Esse campo indica o endpoint de autorização exposto pelo MVPD. O endpoint pode diferir dependendo do protocolo de autorização usado. Os protocolos de autorização disponíveis são SOAP, REST (para dispositivos sem cliente), SAML, XACML e OAUTH.
   * **Configurações Gerais de AuthZ**
      * Esta subguia exibe o protocolo de autorização usado pelo MVPD e informações relacionadas ao protocolo.
      * **Configuração de simulação**
         * Ele descreve o número de recursos que podem ser pré-autorizados por um MVPD em uma única chamada, o modelo PreFlight usado, bem como o limite de tempo limite. Ocasionalmente, o número de recursos pode ser diferente para uma determinada integração. Isso pode ser gerenciado editando a propriedade &quot;**Número máximo de recursos de comprovação**&quot;, disponível na guia Configurações gerais. Essa propriedade está disponível somente para uma determinada integração e, se definida, será usada em vez do valor definido em Configurações de autorização -> Configuração pré-voo -> Recursos máximos pré-voo.
      * **Proteção DOS**
         * Ele descreve a proteção de Negação de serviço no endpoint de autorização do MVPD. Para obter uma descrição exata de cada campo, consulte as dicas de ferramentas, passando o mouse sobre os campos de Proteção DOS.
      * Se o MVPD for um **TempPass**, as **Configurações Gerais do AuthZ** também conterão informações sobre a duração do TempPass.
      * Se o MVPD for um **FlexibleTempPass**, as **Configurações Gerais de AuthZ** também conterão informações sobre a duração do TempPass, o número máximo de recursos e o campo de identificação (consulte a imagem abaixo).
   * **Certificados AuthZ**
      * Esta subguia exibe os certificados que o MVPD usa no fluxo de autorização juntamente com sua organização emissora, data de emissão e data de expiração. Esses certificados servem como chaves privadas/públicas e são usados para fins de validação.
   * **Regras Dinâmicas de AuthZ**
      * Esta subguia exibe as regras que se aplicam ao processo de autorização. Ao pressionar a **Solicitação / Resposta / Token** do diagrama, você pode ver como destacados os parâmetros aplicados a essa parte do fluxo de autorização.
* **Configurações de logoff**
   * **Selecionar Ponto de Extremidade**
      * Esse campo indica o ponto de extremidade de logout exposto pelo MVPD. Os protocolos fornecidos podem ser SAML ou OAuth2.
      * **Fazer logoff das Configurações Gerais**
         * Esta subguia exibe o protocolo de logout usado pelo MVPD e informações relacionadas ao protocolo.
         * **Requer Resposta de Logoff Assinada** - Se definida como &quot;Sim&quot;, a resposta deverá ser assinada por um certificado confiável.
      * **Certificados de saída**
         * Esta subguia exibe os certificados que o MVPD usa no fluxo de logout ao lado de sua organização emissora, data de emissão e data de expiração. Esses certificados servem como chaves privadas/públicas e são usados para fins de validação.
      * **Fazer Logout das Regras Dinâmicas**
         * Esta subguia exibe as regras que se aplicam ao processo de logout. Ao pressionar a **Solicitação / Resposta / Token** do diagrama, você pode ver como destacados os parâmetros aplicados a essa parte do fluxo de logout.

### Relatórios {#tve-db-reports-sec}

Para navegar até esta seção, clique em &quot;Relatórios&quot; no menu &quot;[Seções do Painel](#sections)&quot;. Isso navegará para uma tela com 3 guias, que serão apresentadas em detalhes nas seguintes subseções: [Relatórios AuthN TTL](#authn-ttl-reports), [Relatórios AuthZ TTL](#authz-ttl-reports), [Relatórios SSO](#sso-reports).

Esta seção permite visualizar e exportar dados agregados para vários tipos de relatórios para suas integrações de Canal/s com vários MVPDs em todas as plataformas.

#### Plataformas {#report-platforms}

Todos os relatórios agregam valores nas seguintes plataformas:

**NAVEGADORES**
Exibe valores que serão aplicados às implementações do Programador por meio do SDK JavaScript de autenticação da Adobe Pass.

**DISPOSITIVO MÓVEL: IOS**
Exibe valores que serão aplicados às implementações do Programador por meio do SDK iOS de autenticação da Adobe Pass.

**DISPOSITIVO MÓVEL: ANDROID**
Exibe valores que serão aplicados às implementações do Programador por meio do SDK Android de autenticação da Adobe Pass.

**CELULAR: OUTROS**
Exibe valores que serão aplicados às implementações do Programador por meio da API REST de autenticação da Adobe Pass desenvolvida para dispositivos móveis.

**TVCD: ROKU**
Exibe valores que serão aplicados às implementações do Programador por meio da API REST de autenticação da Adobe Pass e que estão enviando &quot;Roku&quot; como o tipo de dispositivo.

**TVCD: FIRETV**
Exibe valores que serão aplicados às implementações do Programador por meio do SDK FireTV de autenticação da Adobe Pass.

**TVCD: APPLETV**
Exibe valores que serão aplicados às implementações do programador por meio do SDK tvOS de autenticação da Adobe Pass.

**TVCD: OUTROS**
Exibe valores que serão aplicados às implementações do Programador por meio da API REST de Autenticação do Adobe Pass desenvolvida para dispositivos conectados à TV.

**PLATAFORMA: DESCONHECIDA**
Exibe valores que serão aplicados às implementações do Programador para as quais os serviços de Autenticação do Adobe Pass detectam um tipo de dispositivo desconhecido.

Revise o mecanismo de [transmissão de informações do cliente](/help/authentication/passing-client-information-device-connection-and-application.md) para APIs REST de Autenticação do Adobe Pass ou SDKs para obter mais detalhes sobre como enviar o tipo de dispositivo desejado (por exemplo,&quot;Roku&quot;).

Todos os relatórios agregam valores calculados com base na configuração específica de cada ambiente de autenticação do Adobe Pass. Portanto, você pode esperar dados de relatório diferentes ao alternar entre diferentes ambientes do Painel de TVE.

Revise a seção [Ambientes](#authn-environments) para obter mais detalhes relacionados aos ambientes disponíveis de Autenticação do Adobe Pass.


##### Seleção de Canais/MVPDs específicos {#selecting-specific-channels-mvpds}

Todos os relatórios permitem usar filtros selecionando Canais específicos ou MVPDs específicos a serem incluídos nos relatórios resultantes.

Para selecionar um ou vários Canais, use a **lista suspensa** colocada após o rótulo &quot;Canais selecionados para relatório&quot;. Consulte a Figura 8./9./10. imagens abaixo.

Para selecionar um ou vários MVPD/s, use a **lista suspensa** colocada após o rótulo &quot;MVPDs selecionados para relatório&quot;. Consulte a Figura 8./9./10. imagens abaixo.

Por padrão, os dados são agregados em todos os Canais da empresa (&quot;Todos os canais&quot;) e os MVPDs com os quais estão integrados (&quot;Todos os MVPDs&quot;).

Caso opte por desmarcar &quot;Todos os canais&quot; ou &quot;Todos os MVPDs&quot; sem escolher opções específicas, a interface mostrará um espaço reservado &quot;Nenhum dado disponível&quot;.


##### Exportar relatório {#export-report}

Todos os relatórios permitem exportar dados em um arquivo de formato CSV (Valores separados por vírgula).

Para exportar dados, use o botão &quot;Exportar relatório&quot;, posicionado no canto superior direito da janela. Consulte a Figura 8./9./10. imagens abaixo.

Um arquivo chamado **Report.csv** será baixado automaticamente no computador. Portanto, verifique se as configurações do seu navegador permitem o download de arquivos.

O ícone de carregamento &quot;Exportando Dados&quot; estará presente na tela enquanto o arquivo Report.csv é computado, o que pode levar de **a alguns minutos**, dependendo do tamanho dos dados que você deseja exportar.

#### Relatórios AuthN TTL (#authn-ttl-reports)

Este relatório exibe o TTL (Time-To-Live) do token de autenticação configurado para sua integração com vários MVPDs em todas as plataformas.

O tempo de vida do token de autenticação, que também é conhecido como **AuthN TTL**, é exibido em valores legíveis por humanos como: **dias, horas, minutos, segundos**.

Em termos de experiência do usuário, os relatórios AuthN TTL permitem que você inspecione visualmente a quantidade de tempo que um usuário será autenticado considerando um MVPD específico e uma plataforma específica.

Para navegar até este tipo de relatório, clique na guia &quot;AuthN TTL Reports&quot; na seção &quot;Reports&quot;.

![Relatórios TTL de Autenticação](assets/authn-ttl-reports.png)

*Figura 8: Guia AuthN TTL Report do painel do Adobe Primetime TVE*

A tabela Relatórios AuthN TTL contém páginas e pode ser rolada horizontal e verticalmente, dependendo do tamanho da tela.

Caso considere fazer uma alteração em um valor AuthN TTL, revise a seção [Integrações](#tve-db-integrations-sec).

>[!IMPORTANT]
>O espaço reservado &quot;**Set by MVPD**&quot; é usado nos casos em que o MVPD será aquele que impõe o valor AuthN TTL e não a configuração de Autenticação do Adobe Pass.


#### Relatórios TTL AuthZ {#authz-ttl-reports}

Esse relatório exibe o TTL (Time-To-Live) do token de autorização configurado para sua integração com vários MVPDs em todas as plataformas.

O token de autorização Time-To-Live, também conhecido como **AuthZ TTL**, é exibido em valores legíveis por humanos, como: **dias, horas, minutos, segundos**.

Em termos de experiência do usuário, os relatórios AuthZ TTL permitem que você inspecione visualmente a quantidade de tempo que um usuário será autorizado considerando um MVPD específico e uma plataforma específica.

Para navegar para este tipo de relatório, clique na guia &quot;AuthZ TTL Reports&quot; na seção &quot;Reports&quot;.

![Relatórios TTL de AuthZ](assets/authz-ttl-reports.png)

*Figura 9. Guia AuthZ TTL Report do painel do Adobe Primetime TVE*

A tabela Relatórios TTL AuthZ contém páginas e pode ser rolada horizontal e verticalmente, dependendo do tamanho da tela.

Se você considerar fazer uma alteração em um valor TTL AuthZ, consulte a seção [Integrações](#tve-db-integrations-sec).

>[!IMPORTANT]
>O espaço reservado &quot;**Set by MVPD**&quot; é usado nos casos em que o MVPD será aquele que impõe o valor TTL AuthZ e não a configuração de Autenticação do Adobe Pass.


#### Relatórios de SSO {#sso-reports}

Este relatório exibe o status do Single Sign-On (SSO) configurado para suas integrações de canal/s com vários MVPDs em todas as plataformas.

O status do Logon Único, que também é conhecido como **status de SSO**, é exibido como um triestado com os seguintes valores possíveis: **SSO Desabilitado, SSO Habilitado, SSO Incerto**.

Em termos de experiência do usuário, os relatórios de SSO permitem inspecionar visualmente a experiência de SSO de autenticação do usuário esperada, considerando um MVPD específico e uma plataforma específica.

Para navegar até este tipo de relatório, clique na guia &quot;**Relatórios de SSO**&quot; da seção &quot;**Relatórios**&quot;.


![Guia de relatórios SSO do painel do TVE](assets/sso-reports.png)


*Figura 10: guia Relatórios de SSO do painel do Adobe Primetime TVE*

A tabela Relatórios de SSO contém páginas e pode ser rolada horizontal e verticalmente, dependendo do tamanho da tela.

Caso considere fazer uma alteração no status de um SSO, revise a seção [Integrações](#tve-db-integrations-sec).

>[!IMPORTANT]
>O espaço reservado &quot;**SSO Incerto**&quot; é usado nos casos em que o SSO está habilitado e é possível, mas as configurações da plataforma do usuário/decisões do usuário (por exemplo, a opção do navegador do usuário de bloquear cookies de terceiros, a opção do usuário de negar acesso à plataforma para sua assinatura do Provedor de TV) ou as configurações de MVPD (por exemplo, o MVPD que solicita a autenticação para cada Canal) podem impedir que o SSO ocorra.

### Log de alterações {#tve-db-changelog-sec}

Esta seção exibe uma lista de todas as modificações enviadas por meio do Painel TVE para o ambiente e a configuração de Autenticação do Adobe Pass.

Há colunas que indicam a data do push, o usuário que operou a modificação e o status do push.

Esta seção também permite a comparação de duas entradas da tabela para restringir as modificações específicas que você deseja inspecionar e até mesmo compartilhar a comparação como um item de e-mail.

### Feedback {#tve-db-feedback-sec}

Esta seção permite que os usuários enviem comentários. Siga as etapas para fornecer feedback à equipe de produtos de autenticação da Adobe Pass:

* clique no botão &quot;Feedback&quot; no lado direito da tela
* insira o assunto
* insira a mensagem
* se necessário, faça upload de uma captura de tela para a mensagem clicando no botão &quot;Fazer upload da captura de tela&quot;
* clique no botão &quot;Enviar&quot;

![Formulário de comentários do painel](assets/tve-dashboard-feedback.png)

*Figura 11: Seção de Comentários do Painel do Adobe Primetime TVE*

Para obter instruções sobre como capturar capturas de tela, exiba os links abaixo:

* [Como capturar capturas de tela no Windows](https://support.microsoft.com/en-us/windows/use-snipping-tool-to-capture-screenshots-00246869-1843-655f-f220-97299b865f6b#1TC=windows-7)

* [Como capturar capturas de tela no Mac](https://support.apple.com/en-us/HT201361)

## Solução de problemas {#tve-db-troubleshoot}

### Modo de manutenção {#maintenance-mode}

![Aplicativo TVE no modo de manutenção](assets/tveapp-maintenance-mode.png)


*Figura: aplicativo TVE no modo de manutenção*


Caso o painel TVE esteja no &quot;modo de manutenção&quot;, os usuários não poderão visualizar ou fazer novas alterações.

Se isso ocorrer, será necessário aguardar a equipe de engenharia de Autenticação do Adobe Pass concluir o trabalho de manutenção no Painel da TVE.

### Estado degradado {#degraded-state}

![Aplicativo TVE em estado degradado](assets/tve-degraded-state.png)


*Figura: aplicativo TVE em estado degradado*

Caso o painel TVE esteja em &quot;estado degradado&quot;, os usuários não terão recursos de pesquisa e classificação, mas eles poderão visualizar ou fazer novas alterações.

Se isso ocorrer, será necessário aguardar a equipe de engenharia de Autenticação do Adobe Pass concluir o trabalho de manutenção no Painel da TVE.
