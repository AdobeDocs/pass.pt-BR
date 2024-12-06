---
title: Visão geral para programadores
description: Visão geral para programadores
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# Visão Geral Para Programadores {#programmers-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#introduction}

Essa visão geral destina-se ao provedor de conteúdo (Programador) que planeja integrar o Adobe® Pass ao seu site ou aplicativo. Para obter a documentação adicional, incluindo o Kickstart e guias de integração, consulte Informações relacionadas abaixo.

Hoje, seus visualizadores podem acessar a Internet a qualquer hora, em qualquer lugar e solicitar acesso a conteúdo protegido diretamente de você, o Programador. Eles podem querer assistir a um evento único ou podem estar buscando direitos de visualização para uma série de televisão inteira que você está transmitindo.

Mas antes de permitir o acesso ao conteúdo protegido, é necessário determinar se um cliente tem direito a visualizar esse conteúdo. Eles têm uma assinatura com um Distribuidor de programação de vídeo multicanal (MVPD)? Em caso afirmativo, essa assinatura inclui sua programação?

Determinar o direito de um visualizador nem sempre é simples para um programador. Os MVPDs mantêm os privilégios de identificação de dados e acesso para seus clientes.  Adicione o fato de que os visualizadores que tentam acessar seu conteúdo protegido assinam uma variedade de MVPDs, cada um com sistemas diferentes, e é fácil ver que determinar o direito do visualizador ao conteúdo protegido pode se tornar rapidamente complicado e tecnicamente desafiador:

![](../assets/user-ent-by-progr.png)

*Figura: Direito Do Usuário Determinado Diretamente Pelo Programador*

A Autenticação do Adobe Pass para TV em todos os lugares medeia com segurança essas transações de direito entre Programadores e MVPDs. A Autenticação do Adobe Pass facilita, agiliza e protege os Programadores para fornecer conteúdo protegido a clientes válidos:

![](../assets/user-ent-mediatedby-authn.png)

*Figura: Direito de Usuário Mediado pela Autenticação Adobe Pass*

A Autenticação do Adobe Pass atua como seu proxy em intercâmbios com MVPDs participantes, para que você possa apresentar aos visualizadores uma interface consistente entre sites. A Autenticação Adobe Pass também permite fornecer aos visualizadores autenticação e autorização de logon único (SSO). A autenticação e a autorização são rastreadas para todos os serviços participantes, de modo que um assinante não precise fazer logon novamente após a primeira autenticação em seu próprio sistema.

* **Autenticação** - O processo de confirmar com um MVPD que um determinado usuário é um cliente conhecido.
* **Autorização** - O processo de confirmação com um MVPD de que um usuário autenticado tem uma assinatura válida para um recurso especificado.

### Como funciona a autenticação do Adobe Pass {#HowItWorks}

O aplicativo de visualização de conteúdo do programador interage com a autenticação do Adobe Pass usando o componente de cliente Access Enabler ou os serviços Web RESTful da API sem cliente (para dispositivos não compatíveis com a Web, como TVs inteligentes, consoles de jogos, decodificadores de sinais, etc.). O Access Enabler é executado no sistema do usuário, onde facilita todos os workflows de direito.  O componente Ativador de acesso é baixado do site de hospedagem na Adobe quando os clientes acessam seu site e solicitam conteúdo protegido.  Os servidores de Autenticação do Adobe Pass hospedam os serviços Web RESTful usados na solução sem cliente.

A Autenticação do Adobe Pass lida com os workflows de direito reais, enquanto fornece as primitivas que você usa para:

* Defina sua identidade. (O Programador é o &quot;solicitante&quot; no fluxo de direitos de Autenticação do Adobe Pass.)
* Autentique um usuário com um MVPD específico.  (O MVPD é o &quot;provedor de identidade&quot; ou &quot;IdP&quot; no fluxo de direito de Autenticação do Adobe Pass.)
* Autorizar um usuário com o MVPD para um recurso específico.
* Faça logout do usuário.

O Programador é responsável pela página da Web de nível superior ou pelo aplicativo do reprodutor que faz o seguinte:

* Implementa a interface do usuário
* Interage com os serviços da Web do Access Enabler ou da API sem cliente

O objetivo da Autenticação do Adobe Pass é criar uma maneira simples e modular para os Programadores e MVPDs lidarem com a verificação de direitos.

## Compreensão de tokens {#understanding-tokens}

A solução de direitos de autenticação da Adobe Pass se concentra na geração de dados específicos que são criados após a conclusão bem-sucedida dos workflows de autenticação e autorização. Esses dados são chamados de tokens. Os tokens têm uma duração limitada; quando expiram, os tokens precisam ser reemitidos por meio da reinicialização dos workflows de autenticação e autorização.

Para obter detalhes sobre tokens, consulte as seguintes seções:

* [Tipos de tokens](#token-types)
* [Armazenamento de token](#token-storage)
* [Segurança de token](#token-security)
* [Compartilhamento de token](#token-sharing)

### Tipos de tokens {#token-types}

Três tipos de tokens são emitidos durante os workflows de autenticação e autorização. Os tokens AuthN e AuthZ têm &quot;longa vida&quot;, fornecendo continuidade na experiência de visualização do usuário. O token de mídia é um token de vida curta que fornece suporte às práticas recomendadas do setor para evitar fraudes por meio da extração de fluxo. Os programadores especificam os valores de TTL (time-to-live) para cada tipo de token com base em acordos feitos com MVPDs. Os programadores decidem sobre um valor TTL que melhor atenda a sua empresa e seus clientes.

* **Token de Autenticação** (&quot;Vida longa&quot;): após a autenticação bem-sucedida, a Autenticação do Adobe Pass cria um token de Autenticação associado ao dispositivo solicitante e a um GUID (identificador global exclusivo).
   * A Autenticação do Adobe Pass envia o token de autenticação para o Ativador de acesso, que o armazena em cache com segurança no sistema do cliente.  Enquanto o token de autenticação estiver lá e não expirar, ele estará disponível para todos os aplicativos que usam a autenticação Adobe Pass. O Ativador de acesso usa o token de autenticação para o fluxo de autorização.
   * A qualquer momento, somente um token de AuthN é armazenado em cache. Sempre que um novo token de autenticação é emitido e um antigo já existe, a Autenticação do Adobe Pass substitui o token em cache.
* **Token de AuthZ** (&quot;Vida longa&quot;): após a autorização bem-sucedida, a Autenticação do Adobe Pass cria um token de AuthZ associado ao dispositivo solicitante e a um recurso protegido específico.  O recurso protegido é identificado por uma ID de recurso exclusiva.
   * A Autenticação Adobe Pass envia o token de AuthZ para o Ativador de acesso, que o armazena em cache com segurança no sistema local. O Ativador de acesso usa o token de AuthZ para criar o token de mídia de vida curta usado para acesso de visualização real.
   * Em um dado momento, somente um token de AuthZ por recurso é armazenado em cache. A Autenticação Adobe Pass pode armazenar em cache vários tokens AuthZ, desde que estejam associados a recursos diferentes. Sempre que um novo token de AuthZ for emitido e já existir um antigo para o mesmo recurso, a Autenticação do Adobe Pass substituirá o token em cache.
* **Token de mídia** (&quot;Vida curta&quot;): o Ativador de acesso usa o token AuthZ para gerar um token de mídia de vida curta (padrão: 7 minutos). Esse é o ponto no qual uma solicitação de reprodução bem-sucedida é considerada como tendo ocorrido.
   * Antes de fornecer acesso ao recurso protegido, o servidor de mídia deve usar um componente de Autenticação do Adobe Pass, o Verificador de token de mídia, para validar o token de mídia.
   * Como o token de mídia não está vinculado ao dispositivo, sua duração é significativamente menor (padrão: 7 minutos) do que a dos tokens AuthN e AuthZ de longa duração.
   * O token de mídia de vida curta é restrito ao uso único e nunca é armazenado em cache. Ele é recuperado do servidor de autenticação da Adobe Pass sempre que uma API de autorização é chamada.

### Armazenamento de token {#token-storage}

O Access Enabler armazena tokens de vida longa (AuthN e AuthZ) em locais específicos para seu ambiente:

* **Flash 10.1** (ou superior): os tokens de vida longa são armazenados como Objetos Compartilhados Locais.
* **HTML5**: os tokens de longa duração são mantidos em segurança no armazenamento local do navegador HTML5.
* **iOS**: os tokens de vida longa são armazenados em uma área de trabalho persistente, onde podem ser acessados por outros aplicativos cliente de Autenticação da Adobe Pass.
* **Android**: os tokens de vida longa são armazenados em um arquivo de banco de dados compartilhado, onde podem ser acessados por outros aplicativos cliente de Autenticação da Adobe Pass.
* **Dispositivos de API sem cliente**: os tokens são armazenados nos servidores de Autenticação do Adobe Pass.

### Segurança de token {#token-security}

O Servidor de autenticação da Adobe Pass assina digitalmente todos os tokens de longa vida usando a ID do dispositivo (derivada das características de hardware do dispositivo). A assinatura digital difere na forma como é gerada, protegida e validada dependendo do ambiente:

* **Flash 10.1** (ou superior) - A ID do dispositivo depende da credencial do dispositivo, um certificado exclusivo emitido do servidor de Individualização do Adobe. Esta segurança é equivalente à tecnologia FAXS DRM. Essa validação do lado do servidor compara a ID de dispositivo exclusiva no token com a credencial do dispositivo (comunicada com segurança do Flash Player para a autenticação da Adobe Pass). A credencial do dispositivo também identifica a versão do cliente FAXS e a versão do Flash Player (ou AIR) para a qual foi emitida. A ligação de dispositivo é mais forte do que com o HTML5, portanto, o TTL (time-to-live) para tokens normalmente é mais longo com o Flash.
* **HTML5** - O dispositivo é individualizado no lado do cliente. Ele usa características disponíveis por meio do JavaScript para produzir uma ID de pseudodispositivo que inclui versões de navegador e sistema operacional, um endereço IP e uma GUID de cookie de navegador (identificador global exclusivo). Essa ID de dispositivo de token é comparada à ID de pseudodispositivo atual do dispositivo. Como o endereço IP pode mudar durante o uso normal, mesmo na mesma sessão, a Autenticação Adobe Pass armazena tokens HTML5 em dois locais: localStorage e sessionStorage. Se o IP for alterado e o token sessionStorage ainda for válido, a sessão será mantida. Com o HTML5, a vinculação do dispositivo não é tão forte, portanto, o TTL dos tokens normalmente é menor do que o do Flash.
* **Clientes nativos** (iOS e Android) - Os tokens de longa vida mantêm informações de individualização de ID de dispositivo nativo e, portanto, estão vinculados ao dispositivo solicitante. As solicitações de autenticação e autorização são enviadas por HTTPS, e as informações da ID do dispositivo são assinadas digitalmente pela biblioteca do Access Enabler antes de enviá-las aos servidores de back-end. No lado do servidor, as informações de ID do dispositivo são validadas em relação à assinatura digital associada.
* **Clientes de API sem cliente** - A solução de API sem cliente tem seu conjunto de protocolos de segurança que envolvem a assinatura digital de todas as chamadas de API. Os tokens gerados durante os fluxos de direito são armazenados com segurança nos servidores de autenticação da Adobe Pass.

A Autenticação do Adobe Pass valida cada token de longa duração para garantir que o dispositivo que acessa o conteúdo seja o mesmo que emitiu o token. Para todos os tokens, uma validação no lado do cliente garante que a assinatura digital esteja intacta e que a integridade do token seja preservada. Quando a validação da ID de dispositivo falha, a sessão de autenticação é invalidada e o usuário é solicitado a fazer logon novamente, o que redefine os tokens.

### Compartilhamento de token {#token-sharing}

Os aplicativos em diferentes plataformas não compartilham tokens. Há várias razões para isso, incluindo as seguintes:

* Conforme descrito em [Armazenamento de tokens](#token-storage), o método de armazenamento de tokens varia entre plataformas (por exemplo, Objetos compartilhados locais para Flash, WebStorage para JavaScript).
* O grau de segurança do token difere entre as plataformas. Por exemplo, os tokens de Flash estão fortemente vinculados a um dispositivo que usa FAXS. Os tokens em um ambiente puro do JavaScript não têm o mesmo nível de suporte DRM que no Flash.  O compartilhamento de tokens JS com aplicativos do Flash aumentaria a possibilidade de tokens menos seguros explorarem um ambiente mais seguro.

## Ciclo de vida de integração do programador {#prog-integ-lifecycle}

![](../assets/progr-flow-int-lifecycle.png)

*Figura: Integrando a autenticação ao site e aplicativo do programador*


## Fluxos lógicos {#logical-flows}

### Fluxograma de Direitos {#chart}

O fluxograma a seguir apresenta o processo geral de confirmação de direitos (usando o componente de cliente Authentication Access Enabler da Adobe Pass):

![](../assets/ent-flowchart.png)

*Figura: processo de confirmação do direito*

### Etapas de autenticação {#authn-steps}

As etapas a seguir apresentam um exemplo do fluxo de autenticação da Autenticação do Adobe Pass.  Essa é a parte do processo de qualificação em que um Programador determina se o usuário é um cliente válido de um MVPD.  Nesse cenário, o usuário é um assinante válido de um MVPD.  O usuário está tentando exibir o conteúdo protegido usando um aplicativo de Flash do Programador:

1. O usuário navega até a página do Programador na Web, que carrega o aplicativo do Flash do Programador e os componentes do Adobe Pass Authentication Access Enabler no computador do usuário. O aplicativo Flash usa o Access Enabler para definir a identificação do Programador com a Autenticação do Adobe Pass, e a Autenticação do Adobe Pass prioriza o Ativador de acesso com os dados de configuração e estado desse Programador (o &quot;solicitante&quot;). O Ativador de acesso deve receber esses dados do servidor antes de executar outras chamadas de API. Nota técnica: o Programador definiu sua identidade com o método `setRequestor()` do Ativador de Acesso. Para obter detalhes, consulte o [Guia de Integração do Programador](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).
1. Quando o usuário tenta visualizar o conteúdo protegido do Programador, o aplicativo do Programador apresenta ao usuário uma lista de MVPDs, a partir dos quais o usuário seleciona um provedor.
1. O usuário é redirecionado para um servidor de Autenticação Adobe Pass, onde uma solicitação [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) criptografada para o MVPD selecionado pelo usuário é criada. Essa solicitação é enviada como uma solicitação de autenticação em nome do Programador para o MVPD. Dependendo do sistema do MVPD, o navegador do usuário é então redirecionado para o site do MVPD para fazer logon ou um iFrame de logon é criado no aplicativo do Programador.
1. Em ambos os casos (redirecionamento ou iFrame), o MVPD aceita a solicitação e exibe sua página de logon.
1. O usuário faz logon com o MVPD, o MVPD valida o status do usuário como um cliente pagante e, em seguida, o MVPD cria sua própria sessão HTTP.
1. Quando o usuário é validado, o MVPD cria uma resposta (SAML e criptografada), que o MVPD envia de volta para a Autenticação do Adobe Pass.
1. A Autenticação do Adobe Pass recebe a resposta do MVPD, vê que há uma sessão HTTP de Autenticação do Adobe Pass aberta, valida a resposta do SAML do MVPD e redireciona de volta para o site do Programador.
1. O site do Programador é recarregado, o Ativador de acesso é recarregado e o Programador chama setRequestor() novamente.  A segunda chamada para setRequestor() é necessária porque a configuração atual foi alterada - agora há um sinalizador presente que informa ao Ativador de acesso que um token de autenticação está aguardando para ser gerado no servidor.
1. O Ativador de acesso vê que há uma autenticação pendente e solicita o token do servidor de autenticação da Adobe Pass. O token é recuperado do servidor chamando os recursos DRM do Flash Player.
1. O token de autenticação é armazenado no cache LSO do Flash Player do programador; a autenticação agora está concluída e a sessão é destruída no servidor de autenticação da Adobe Pass.

### Etapas de autorização {#authz-steps}

As etapas a seguir continuam das [Etapas de autenticação](#authn-steps):

1. Quando o usuário tenta acessar o conteúdo protegido do Programador, o aplicativo do Programador primeiro verifica se há um token de Autenticação no computador ou dispositivo local do usuário.  Se esse token não estiver lá, as [Etapas de autenticação](#authn-steps) acima serão seguidas.  Se o token de autenticação estiver lá, o fluxo de autorização continuará com o aplicativo do programador iniciando uma chamada para o ativador de acesso com uma solicitação para obter os direitos de visualização do usuário para um item específico de conteúdo protegido.
1. O item específico de conteúdo protegido é representado por um &quot;identificador de recurso&quot;.  Pode ser uma sequência simples ou uma estrutura mais complexa, mas, em qualquer caso, a natureza do identificador de recursos é acordada antecipadamente entre o Programador e o MVPD.  O aplicativo do Programador passa o identificador de recurso para o Ativador de acesso.  O Ativador de acesso verifica um token de AuthZ no computador ou dispositivo local do usuário.  Se o token AuthZ não estiver lá, o Ativador de acesso passará a solicitação para o servidor de autenticação Adobe Pass de back-end.
1. O servidor de Autenticação do Adobe Pass se comunica com o endpoint de autorização MVPDs usando protocolos padronizados.  Se a resposta do MVPD indicar que o usuário tem direito a visualizar o conteúdo protegido, o servidor de Autenticação do Adobe Pass criará um token de AuthZ e o transmitirá de volta para o Ativador de acesso, que armazena o token de AuthZ no computador do usuário.
1. Com um token de AuthZ armazenado na máquina ou no dispositivo do usuário, o aplicativo do Programador chama o Ativador de acesso para obter um token de mídia do servidor de autenticação da Adobe Pass e fornece esse token para o aplicativo do Programador.
1. Por fim, o aplicativo do Programador usa o componente Verificador de token de mídia para confirmar se o usuário correto está visualizando o conteúdo correto e, com o token de mídia instalado, o usuário pode visualizar o conteúdo protegido.


## Registro e inicialização {#reg-and-init}

A primeira etapa ao trabalhar com a Autenticação da Adobe Pass é fazer o registro no Adobe ou em um parceiro autorizado para a Autenticação da Adobe Pass.

Ao se registrar, você fornece uma lista dos domínios a partir dos quais se comunicará com a Autenticação Adobe Pass. Por exemplo, os domínios do Turner Broadcasting System incluem tbs.com e tnt.tv como domínios registrados para autenticação da Adobe Pass. Cada um desses sites de conteúdo recebe acesso à autenticação da Adobe Pass e recebe sua própria ID de solicitante (por exemplo, &quot;TBS&quot; e &quot;TNT&quot;). Se decidir adicionar mais sites, você deverá informar ao Adobe os nomes de domínio adicionais para colocá-los na lista de domínios permitidos e receber IDs de solicitante adicionais.

>[!WARNING]
>
>Enquanto você está testando a integração, verifique se o servidor de teste está em um domínio registrado que pretende usar na produção. Solicitações, até mesmo solicitações de teste, provenientes de domínios que não estão na lista de permissões são ignoradas. Por exemplo, se você solicitou que domain.com seja usado na produção, certifique-se de implantar a integração de teste em domain.com, test.domain.com e staging.domain.com.
>
>Solicitações que contêm nome de usuário e/ou senha no URL são ignoradas mesmo se os domínios estiverem na lista de permissões. Exemplo: `//username@registered-domain/`

A ID do solicitante identifica exclusivamente o cliente do programador em todas as comunicações com o componente de cliente Access Enabler da autenticação da Adobe Pass. Todos os dados estáticos de Autenticação do Adobe Pass associados ao Programador são digitados para essa ID.

>[!TIP]
>
>Além da ID do solicitante, ao se registrar, você também recebe URLs funcionais para o componente de cliente do Access Enabler e o Verificador de token de mídia.

## Etapas de integração {#integration-steps}

>[!TIP]
>
>Se você estiver usando o Adobe Open Source Media Framework (&quot;OSMF&quot;) para o desenvolvimento de seu reprodutor de mídia, a maneira mais rápida de usar a Autenticação Adobe Pass é integrar o Plug-in OSMF *(Obsoleto)* ao código do reprodutor.
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [Configuração do solicitante](#requestor)
1. [Manipulando Autenticação e Autorização](#authn-authz)
1. [Suporte para logout único](#ssl)

### 1. Configuração do solicitante {#requestor}

#### 1-A. Registrando Com O Adobe

Seu primeiro passo é se registrar no Adobe ou em um parceiro autorizado de autenticação da Adobe Pass.  Ao se registrar, você receberá um ou mais identificadores exclusivos globais (GUIDs). Cada GUID emitida está associada a um domínio do qual o acesso é permitido para a autenticação da Adobe Pass. Você passa um GUID (chamado ID do solicitante) para o domínio solicitante registrar sua identidade para cada sessão em que você interage com o Access Enabler. Para obter detalhes, consulte [Registro e Inicialização](#reg-and-init) no Guia de Integração do Programador.

#### 1-B. Integração inicial do Access Enabler

A próxima etapa é integrar o Access Enabler ao aplicativo de reprodutor de mídia ou página da Web existente:

* Você pode incorporar a versão do Flash, `AccessEnabler.swf`, em um player de vídeo baseado em Flashes, ou pode incorporá-lo diretamente no HTML da sua página da Web. Você pode se comunicar com o SWF do Access Enabler no ActionScript ou no JavaScript. A API básica é o ActionScript, mas se você preferir trabalhar com o JavaScript, uma biblioteca completa de invólucro está disponível para inclusão em suas páginas.
* Para ambientes que não sejam de Flash, é possível:
   * Use a versão do HTML5/JavaScript, o AccessEnabler.js, e comunique-se com ele por meio da API do JavaScript
   * Usar a AIR Native Extension for Adobe Pass Authentication para combinar código nativo com classes de ActionScript integradas
   * Usar uma das versões nativas do cliente da biblioteca do Access Enabler (iOS ou Android)

### 2. Tratamento da autenticação e da autorização {#authn-authz}

#### 2-A. Comunicação com o Access Enabler

A comunicação entre o Ativador de acesso e sua página da Web ou aplicativo de reprodução é assíncrona. Seu aplicativo chama os métodos do Access Enabler, e o Access Enabler responde via retornos de chamada que você registra na biblioteca do Access Enabler.

* Quando o aplicativo faz uma solicitação de autorização, o Ativador de acesso inicia automaticamente uma solicitação de autenticação, se um token de autenticação válido ainda não estiver no sistema local. Quando a autenticação é bem-sucedida, o token de autenticação do usuário é armazenado localmente, para que ele não precise fazer logon novamente. Se eles tiverem sido autenticados com sucesso por meio da Autenticação do Adobe Pass em qualquer outro contexto (por exemplo, por meio do site MVPD ou com um Programador diferente), o Ativador de acesso terá acesso ao token de autenticação local e uma autenticação adicional não será necessária.
* Quando um usuário tenta acessar seu conteúdo protegido, você envia uma solicitação de autorização para o Ativador de acesso. Após verificar (ou iniciar) a autenticação, o Ativador de acesso entra em contato com o MVPD (por meio do servidor de autenticação da Adobe Pass) para determinar se o cliente tem direito a visualizar o conteúdo protegido. Seu aplicativo só precisa enviar a solicitação ao Ativador de acesso e, em seguida, manipular a resposta (sucesso ou falha na autorização). Se a autorização for bem-sucedida, um token de AuthZ será armazenado no sistema cliente.  Por fim, seu aplicativo recebe um token de mídia de vida curta para uso em seu próprio procedimento de autorização.

>[!NOTE]
>
>* A autenticação ocorre como uma troca SAML, entre a Adobe Pass Authentication como o Service Provider (SP) e o MVPD como o Identity Provider (IdP).
>
>* A autorização usa uma troca de serviço Web de canal de retorno (servidor para servidor) entre a Autenticação do Adobe Pass (o SP) e um MVPD (o IdP).


#### 2-B. Fornecer Uma Interface Do Usuário De Direito {#entitlement-ui}

Você fornece sua própria interface do usuário para acesso do usuário ao seu conteúdo. Alguns elementos, como o processo de logon real, são fornecidos pelo MVPD e alguns elementos estão opcionalmente disponíveis como parte da Autenticação do Adobe Pass. No mínimo, você deve fazer o seguinte:

* **Implemente uma interface de seleção MVPD que permita que um novo usuário identifique o MVPD e faça logon pela primeira vez**. Para desenvolvimento, o Access Enabler fornece uma interface básica de usuário que oferece ao cliente uma opção de MVPDs e inicia o processo de logon. Para produção, você deve implementar sua própria caixa de diálogo Seletor de MVPD. Alguns MVPDs redirecionam para seu próprio site para logon e outros exigem que suas páginas de logon sejam exibidas em um iFrame. Você deve implementar um retorno de chamada que crie esse iFrame para lidar com os casos em que o MVPD do usuário exibe a página de logon em um iFrame.
* **Identificar conteúdo protegido**. O conteúdo protegido exige autorização para acessar o. Sua interface deve indicar qual conteúdo está protegido e qual conteúdo foi autorizado.  O status da autorização geralmente é indicado com ícones &quot;desbloqueado&quot; e &quot;bloqueado&quot;.
* **Mostrar que um usuário está autenticado**. Você deve indicar o status de autenticação de um usuário como parte de qualquer meio usado para identificar o conteúdo protegido. Você pode consultar o Ativador de acesso para determinar se o cliente já foi autenticado.

#### 2-C. Integração do verificador de token de mídia {#int-media-token-ver}

É necessário integrar o componente Verificador de token de mídia de autenticação da Adobe Pass ao seu servidor de mídia.  Dessa forma, o verificador de token existente pode reconhecer os tokens de mídia de vida curta fornecidos pela Autenticação do Adobe Pass com uma autorização bem-sucedida. O Verificador de token de mídia valida os tokens de mídia como a última etapa de segurança antes de você fornecer ao usuário acesso ao conteúdo protegido. Você recebe o local de onde baixar o Media Token Verifier ao se registrar no Adobe.

### 3. Suporte ao logout único {#ssl}

Na maioria dos casos, o reprodutor de mídia é responsável por lidar com logouts de usuários por meio de uma simples API do Access Enabler. Quando você chama logout(), o Access Enabler faz o seguinte:

* Faz logoff do usuário atual
* Limpa todas as informações de autenticação e autorização do usuário desconectado
* Exclui todos os tokens AuthN e AuthZ do sistema local do usuário

Se o usuário deixar a máquina ociosa por tempo suficiente para que os tokens expirem, ele ainda poderá retornar à sessão e iniciar o logout com êxito. A autenticação da Adobe Pass garante que todos os tokens sejam excluídos e notifica o MVPD para excluir sua sessão também.

Quando o logout é iniciado a partir de um site que não está integrado à Autenticação Adobe Pass, o MVPD pode chamar o serviço de Logout único da Autenticação Adobe Pass por meio de um redirecionamento do navegador.

## Compreensão das IDs de usuário {#user-ids}

Conceitualmente, cada usuário que inicia um fluxo de direitos é associado a uma ID de usuário única.  No entanto, durante um fluxo de direitos, essa ID de usuário pode ser apresentada de diferentes maneiras, dependendo da API da qual você obtém a ID.

O sessionGUID no token de mídia curta é a forma segura da ID de usuário, que está disponível por meio da chamada sendTrackingData().   Em todas as integrações atuais, esse é um GUID persistente para o usuário no tempo e nos dispositivos, mas a origem do GUID começa com o UserID na resposta SAML do MVPD.   No entanto, alguns MVPDs podem mudar de ideia no futuro e começar a enviar um GUID transitório.  Se um Programador quiser garantir que a UserID de origem do MVPD na resposta do AuthN seja persistente, ele deverá providenciar isso em seus contratos com os MVPDs.

Estas são as diferentes maneiras de representar a ID de usuário nas APIs de autenticação da Adobe Pass:

* Propriedade GUID `sendTrackingData()` - Esta é a versão com hash do Adobe da UserID do MVPD.  Ela é transformada em hash para que essa ID de usuário não possa ser rastreada de volta para a origem no MVPD.   Essa ID é exclusiva e geralmente persistente, mas não pode ser compartilhada com o MVPD para comparar o comportamento de uso específico com o que os MVPDs têm a seu lado.   Ele não é assinado digitalmente, portanto, não é imune para a prevenção de fraudes, mas é bom o suficiente para a análise.  Esse formulário da ID de usuário é fornecido no lado do cliente em todos os eventos gerados pela Autenticação Adobe Pass no fluxo AuthN/AuthZ.
* Propriedade `sessionGUID` do Short Media Token - É a mesma que o UserID via `sendTrackingData()`, no entanto, este é assinado digitalmente para proteger sua integridade.  Isso torna esse valor bom o suficiente para rastrear fraudes de uso simultâneo. Ele deve ser processado no lado do servidor depois de usar nossa biblioteca de validação e pode ser analisado em busca de padrões de fraude antes de lançar o fluxo de vídeo para o cliente.  Fazer qualquer uma dessas tarefas depende do Programador.
* `getMetadata() userID `propriedade - Esta propriedade permitirá que o Adobe exponha a UserID MVPD real de origem ao Programador. Ele será criptografado com a chave pública do certificado que temos do Programador, para que não seja exposto ao cliente claramente. Isso fornece ao Programador a UserID real do MVPD, de modo que é algo que pode ser usado para vinculação de contas ou investigação de fraude diretamente com o MVPD.

**Em conclusão**

* A ID de Usuário do MVPD é uma ID exclusiva persistente, geralmente, embora não garantida, que é **gerada a partir dos MVPDs e passada para o Adobe na autenticação bem-sucedida**. Geralmente, ela é consistente em todas as redes, com algumas exceções.
* A ID de usuário MVPD não contém PII e NÃO é um número de conta. Ela não precisa ser exposta em um formato criptografado, pois validamos com todos os MVPDs que nenhuma PII está sendo enviada.


A forma como você usa a ID de usuário depende do caso de uso:

* Se você precisar dele para rastreamento/análise, o local mais prático é obtê-lo de `sendTrackingData()`.
* Se você precisar dele no lado do servidor para lançamento de transmissão, fraude ou dados operacionais, é possível obtê-lo no Media Token Validator.
* Se precisar dela para vinculação de contas e fraude mais profunda, verifique a disponibilidade com seu contato do Adobe.

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
