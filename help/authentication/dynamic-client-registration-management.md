---
title: Gerenciamento dinâmico de registro de clientes
description: Gerenciamento dinâmico de registro de clientes
exl-id: 2c3ebb0b-c814-4b9e-af57-ce1403651e9e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# Gerenciamento dinâmico de registro de clientes {#dynamic-client-registration-management}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

Com a adoção generalizada das [Guias Personalizadas do Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target_blanck} e do [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target_blanck} nos aplicativos de nossos clientes, estamos atualizando o fluxo de autenticação de usuário na Autenticação do Adobe Pass. Mais especificamente, não podemos mais atingir o objetivo de manter o estado para que o fluxo de agente do usuário de autenticação de um assinante MVPD possa ser rastreado entre os redirecionamentos. Isso foi feito anteriormente usando cookies HTTP. Essa limitação é o driver para começar a migrar todas as APIs para o OAuth 2.0 [RFC6749](https://tools.ietf.org/html/rfc6749){target_blanck}.

Com essa atualização, os Clientes de autenticação Adobe se tornam clientes OAuth 2.0 e um servidor de autorização OAuth 2.0 personalizado é implantado para atender às necessidades do Serviço de autenticação da Adobe Pass.

Para que os aplicativos cliente utilizem a autorização OAuth 2.0, o servidor deve se registrar dinamicamente para obter informações específicas (credenciais do cliente) e poder interagir com ele. Como parte do processo de registro, o cliente deve apresentar um conjunto de metadados incorporados ao endpoint de registro do cliente.

Esses metadados são comunicados como uma declaração de software, que contém um &quot;software_id&quot; para permitir que nosso servidor de autorização correlacione diferentes instâncias de um aplicativo usando a mesma declaração de software.

Uma **instrução de software** é um JSON Web Token (JWT) que declara valores de metadados sobre o software cliente como um pacote. Quando apresentada ao servidor de autorização como parte de uma solicitação de registro do cliente, a instrução de software deve ser assinada digitalmente ou MACed usando JSON Web Signature (JWS).

Você pode encontrar uma explicação mais detalhada sobre o que são as instruções de software e como elas funcionam na documentação oficial [RFC7591](https://tools.ietf.org/html/rfc7591).

A instrução de software deve ser implantada com o aplicativo no dispositivo do usuário.

Antes desta atualização, tínhamos dois mecanismos para permitir que os aplicativos executassem chamadas para a Autenticação Adobe Pass:

* os clientes baseados em navegador são registrados por meio da [lista de domínios](/help/authentication/programmer-overview.md#reg-and-init) permitida
* os clientes de aplicativos nativos, como aplicativos iOS e Android, são registrados por meio do mecanismo **solicitante assinado**


Com o mecanismo de autorização Registro do cliente, você deve adicionar seus aplicativos ao painel da TVE.

Para que um cliente comece a implementar o novo Android SDK e o futuro iOS SDK, ele precisa de uma declaração de software. Uma instrução de software identifica um aplicativo criado no Painel TVE.

Siga as etapas nas seções abaixo para criar um Aplicativo registrado no painel TVE.

## Criar um aplicativo registrado {#create_app}

Há duas maneiras de criar um Aplicativo Registrado no Painel TVE:

* [Nível do programador](#prog-level) - permite criar um Aplicativo registrado e vinculá-lo a qualquer um ou a todos os canais do programador.

* [Nível de canal](#channel-level) - permite criar um Aplicativo Registrado que esteja permanentemente vinculado apenas a este Canal.

### Criar um aplicativo registrado no nível do programador {#prog-level}

Vá para a guia **Programadores** > **Aplicativos Registrados**.

![](assets/reg-app-progr-level.png)

Na guia Registered Applications, clique em **Add New Application**. Preencha os campos obrigatórios na nova janela.

Como visto na imagem abaixo, os campos que você deve preencher são:

* **Nome do Aplicativo** - o nome do aplicativo

* **Atribuído ao Canal** - o nome do seu canal, ao qual este aplicativo está vinculado. </span> A configuração padrão na máscara suspensa é **Todos os canais.** A interface permite selecionar um canal ou todos os canais.

* **Versão do Aplicativo** - por padrão, está definido como &quot;1.0.0&quot;, mas é altamente recomendável que você o modifique com sua própria versão do aplicativo. Como prática recomendada, se você decidir alterar a versão do aplicativo, reflita-a criando um novo aplicativo registrado para ela.

* **Plataformas de Aplicativos** - as plataformas às quais o aplicativo será vinculado. Você tem a opção de selecionar todos eles ou vários valores.

* **Nomes de Domínio** - os domínios aos quais o aplicativo será vinculado. Os domínios na lista suspensa são uma seleção unificada de todos os domínios de todos os canais. Você tem a opção de selecionar vários domínios na lista. O significado dos domínios são URLs de redirecionamento [RFC6749](https://tools.ietf.org/html/rfc6749). No processo de registro do cliente, o aplicativo cliente pode solicitar permissão para usar um URL de redirecionamento para a finalização do fluxo de autenticação. Quando um aplicativo cliente solicita um URL de redirecionamento específico, ele é validado em relação aos domínios da lista de permissões neste Aplicativo registrado associado à instrução do software.


![](assets/new-reg-app.png)


Depois de preencher os campos com os valores apropriados, clique em &quot;Concluído&quot; para que o aplicativo seja salvo na configuração.

Esteja ciente de que não há **nenhuma opção para modificar um aplicativo já criado**. Caso se descubra que algo criado não atende mais aos requisitos, um novo aplicativo registrado precisará ser criado e usado com o aplicativo cliente cujos requisitos ele atende.


### Registrar um novo aplicativo no nível do canal {#channel-level}

Para criar um aplicativo registrado no nível do canal, navegue até o menu &quot;Canais&quot; e escolha aquele para o qual deseja criar um aplicativo. Depois, depois de navegar até a guia &quot;Aplicativos registrados&quot;, clique no botão &quot;Adicionar novo aplicativo&quot;.

![](assets/reg-new-app-channel-level.png)

Como mostrado abaixo, o que é um pouco diferente aqui, em comparação com a mesma ação executada no nível do Programador, é a lista suspensa &quot;Canais atribuídos&quot;, que não está ativada, portanto, não há opção para vincular o aplicativo registrado a outro canal que não o atual.

![](assets/new-reg-app-channel.png)

## Listar aplicativos {#list-reg-app}

Após a criação do aplicativo registrado, existe a possibilidade de obter uma declaração de software para apresentar o servidor de autorização como parte de uma solicitação.

Isso pode ser feito navegando até o Programador ou Canal para o qual os aplicativos registrados foram criados, onde estão listados.

Como ilustrado abaixo , cada entrada na lista será identificada por um nome, versão e símbolos para plataformas às quais foi vinculada.

![](assets/reg-app-list.png)

Para cada um deles você pode:

* [Exibir](#view)
* [Baixar uma instrução de software](#download-statement)

### Exibir um aplicativo registrado {#view}

Na lista de aplicativos, escolher um deles e clicar no botão &quot;Ver&quot; mostrará os detalhes usados quando ele foi criado. Como mencionado anteriormente, não há opção para modificar nada.


![](assets/view-reg-app.png)


### Baixar Declaração de Software {#download-statement}

Clicar no botão &quot;Download&quot; na entrada da lista para a qual uma instrução de software é necessária gerará um arquivo de texto. Esse arquivo conterá algo semelhante ao exemplo de saída abaixo.


![](assets/download-software-statement.png)

O nome do arquivo é identificado exclusivamente ao prefixá-lo com &quot;software_statement&quot; e adicionar o carimbo de data e hora atual.

Observe que, para o mesmo aplicativo registrado, serão recebidas instruções de software diferentes sempre que o botão de download for clicado, mas isso não invalida as instruções de software obtidas anteriormente para esse aplicativo. Isso acontece porque elas são geradas no local, por solicitação de ação.

Há uma **limitação** relacionada à ação de download. Se for solicitada uma instrução de software clicando no botão &quot;Download&quot; logo após a criação do aplicativo registrado e isso ainda não tiver sido salvo e o json de configuração não tiver sido sincronizado, a seguinte mensagem de erro aparecerá na parte inferior da página.

![](assets/error-sw-statement-notready.png)

Isso envolve um código de erro HTTP 404 Não encontrado recebido do núcleo, pois a id do aplicativo registrado ainda não foi propagada e o núcleo não tem conhecimento sobre isso.

Após criar o aplicativo registrado, a solução é esperar no máximo 2 minutos pela sincronização da configuração. Depois que isso acontecer, a mensagem de erro não será mais recebida e o arquivo de texto com a instrução de software estará disponível para download.

Para obter detalhes sobre como o processo completo funciona ou para obter alguns insights sobre como as solicitações são executadas e quais respostas esperar, consulte o link em Informações relacionadas abaixo, juntamente com outros links úteis.

<!--
## Related Information {#related}

* [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
* [TVE Dashboard User Guide](/help/authentication/tve-dashboard-user-guide.md)
-->

## Demonstração de recursos {#tutorial}

Assista a [este webinário](https://my.adobeconnect.com/pzkp8ujrigg1/), que fornece mais contexto sobre os recursos e contém uma demonstração sobre como gerenciar as instruções de software usando o Painel TVE e como testar as geradas usando um aplicativo de demonstração fornecido pelo Adobe como parte do SDK do Android.
