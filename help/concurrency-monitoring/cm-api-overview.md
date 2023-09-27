---
title: Visão geral da API
description: Visão geral da API do monitoramento de simultaneidade
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1425'
ht-degree: 0%

---


# Visão geral da API {#api-overview}

Exibir o [Documentação da API online](http://docs.adobeptime.io/cm-api-v2/) para obter mais detalhes.

## Finalidade e pré-requisitos {#purpose-prerequisites}

Este documento ajuda os desenvolvedores de aplicativos a usar nossa especificação da API do Swagger ao implementar uma integração com o Monitoramento de simultaneidade. É altamente recomendável que o leitor tenha uma compreensão anterior dos conceitos definidos pelo serviço antes de seguir essa diretriz. Para obter este entendimento, é necessário dispor de uma visão geral da [documentação do produto](/help/concurrency-monitoring/cm-home.md) e a variável [Especificação da API do Swagger](http://docs.adobeptime.io/cm-api-v2/).


## Introdução {#api-overview-intro}

Durante o processo de desenvolvimento, a documentação pública do Swagger representa a diretriz de referência para entender e testar os fluxos da API. Este é um ótimo local para começar, a fim de ter uma abordagem prática e se familiarizar com a forma como os aplicativos reais se comportariam em diferentes cenários de interação do usuário.

Enviar um tíquete no [Zendesk](mailto:tve-support@adobe.com) para registrar sua empresa e aplicativos no Monitoramento de simultaneidade. O Adobe atribuirá uma ID de aplicativo a cada entidade. Neste guia, usaremos dois aplicativos de referência com ids **demo-app** e **demo-app-2** que estará sob o Adobe do locatário.


## Casos de uso {#api-use-case}

A primeira etapa no teste de um fluxo usando o Swagger é inserir a ID do aplicativo na parte superior direita da página, da seguinte maneira:

![](assets/setting-app-id.png)

Depois disso, pressionamos **Explorar** para definir a id que será usada no cabeçalho Autorização para todas as chamadas feitas para a API REST.  Cada chamada de API espera que a ID do aplicativo seja passada pela autenticação básica HTTP. O nome de usuário é a ID do aplicativo e a senha está vazia.


### Primeira aplicação {#first-app-use-cases}

Aplicativo com id **demo-app** foi atribuído pela equipe de Adobe uma política com uma regra que restringe o número de fluxos simultâneos a 3. Uma política é atribuída a um aplicativo específico com base na solicitação enviada no Zendesk.


#### Recuperando metadados {#retrieve-metadata-use-case}

A primeira chamada que fazemos é para o recurso Metadados para obter a lista de atributos de metadados que precisam ser passados como dados de formulário durante a inicialização da sessão. Esses metadados serão usados para avaliar as políticas atribuídas para este aplicativo.

![](assets/retrieving-metadata.png)

Depois de pressionar &quot;Experimentar&quot;, para o aplicativo com id **demo-app** obteremos o seguinte resultado:

![](assets/empty-metadata-call.png)

Como podemos ver no campo de corpo da resposta, a lista de atributos de metadados está vazia. Isso significa que os atributos exigidos pelo design são suficientes para avaliar a política de três fluxos atribuída a esse aplicativo. Consulte também a seção [Documentação dos Campos de metadados padrão](/help/concurrency-monitoring/standard-metadata-attributes.md). Após esta chamada, podemos continuar e criar uma nova sessão no recurso Sessões REST.


#### Inicialização da sessão {#session-initial}

A chamada de inicialização de sessão é feita por um aplicativo após obter todas as informações necessárias para executá-la.

![](assets/session-init.png)

Não há necessidade de fornecer um código de encerramento na primeira chamada porque não temos outros fluxos ativos. E nenhum atributo de metadados porque nenhum foi retornado da chamada Recuperando metadados.

A variável **assunto** e a variável **idp** Os parâmetros são obrigatórios, eles serão especificados como variáveis de caminho de URI. Você pode obter a **assunto** e **idp** parâmetros fazendo uma chamada para a variável **mvpd** e **upstreamUserID** campos de metadados da Autenticação Adobe Pass. Consulte também a seção [visão geral das APIs de metadados](https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/user-metadat/user-metadata-feature.html?lang=en#). Neste exemplo, forneceremos o valor &quot;12345&quot; como o assunto e &quot;adobe&quot; como o idp.


![](assets/session-init-params-frstapp.png)

Efetuar a chamada de inicialização de sessão. Você receberá a seguinte resposta:


![](assets/session-init-result-first-app.png)


Todos os dados que precisamos estão contidos nos cabeçalhos de resposta. A variável **Localização** cabeçalho representa a id da nova sessão criada e a variável **Data** e **Expira** os cabeçalhos representam os valores usados para agendar o aplicativo para fazer o próximo heartbeat, a fim de manter a sessão ativa.

#### Heartbeat {#heartbeat}

Faça uma chamada de heartbeat. Forneça o **session id** obtido na chamada de inicialização de sessão, junto com o parâmetro **assunto** e **idp** parâmetros usados.

![](assets/heartbeat.png)


Se a sessão ainda for válida (não expirou ou foi excluída manualmente), você receberá um resultado bem-sucedido:

![](assets/heartbeat-succesfull-result.png)

Como no primeiro caso, usaremos a variável **Data** e **Expira** cabeçalhos para agendar outro heartbeat para esta sessão específica. Se a sessão não for mais válida, essa chamada falhará com um código de Status HTTP 410 GONE.

Você pode usar a opção &quot;Manter o fluxo ativo&quot; disponível na interface do Swagger para executar pulsações automáticas em uma sessão específica. Isso pode ajudá-lo a testar uma regra sem precisar se preocupar com a placa intermediária necessária para fazer pulsações de sessão em tempo hábil. Esse botão é colocado junto com o botão &quot;Experimente&quot; na guia Swagger Heartbeat. Para definir uma pulsação automática para todas as sessões criadas, é necessário programá-las em uma interface separada do Swagger aberta em uma guia do navegador da Web.

![](assets/keep-stream-alive.png)

#### Término da sessão {#session-termination}

O caso comercial de sua empresa pode exigir o Monitoramento de simultaneidade para encerrar uma sessão específica quando, por exemplo, um usuário para de assistir a um vídeo. Isso pode ser feito fazendo uma chamada DELETE no recurso Sessões.

![](assets/delete-session.png)

Use os mesmos parâmetros para a chamada e para a pulsação da sessão. Os códigos de status HTTP de resposta são:

* 202 ACEITO para uma resposta bem-sucedida
* 410 DESAPARECEU se a sessão já tivesse sido interrompida.

#### Quebrando a política {#breaking-policy-app-first}


Para simular o comportamento do aplicativo quando a política de três fluxos atribuída a ele é quebrada, precisamos fazer três chamadas para a inicialização da sessão. Para que a política entre em vigor, as chamadas precisam ser feitas antes que uma da sessão expire devido à falta de heartbeats. Veremos que todas essas chamadas foram bem-sucedidas, mas se fizermos uma quarta, ela falhará com o seguinte erro:

![](assets/breaking-policy-frstapp.png)


Recebemos uma resposta 409 CONFLICT junto com um objeto de resultado de avaliação na carga. Leia uma descrição completa do resultado da avaliação na seção [Especificação da API do Swagger](http://docs.adobeptime.io/cm-api-v2/#evaluation-result).

O aplicativo pode usar as informações do resultado da avaliação para exibir uma determinada mensagem ao usuário ao interromper o vídeo e tomar outras ações, se necessário. Um caso de uso pode ser o de interromper outros fluxos existentes para iniciar um novo. Isso é feito usando o **terminoucode** valor presente no **conflitos** para um atributo conflitante específico. O valor será fornecido como o cabeçalho HTTP X-Terminate na chamada para uma nova inicialização de sessão.

![](assets/session-init-termination-code.png)

Ao fornecer um ou mais códigos de encerramento na inicialização da sessão, a chamada será bem-sucedida e uma nova sessão será gerada. Se tentarmos fazer um heartbeat com uma das sessões que foram remotamente interrompidas, obteremos uma resposta 410 GONE com uma carga de resultado da avaliação que descreve o fato de que a sessão foi encerrada remotamente, como no exemplo:

![](assets/remote-termination.png)

### Segunda aplicação {#second-application}

O outro aplicativo de exemplo que usaremos é aquele com id **demo-app-2**. A este foi atribuída uma política com uma regra que limita o número de fluxos disponíveis para um canal a no máximo 2.   Você deve fornecer a variável de canal para avaliar essa política.

#### Recuperando metadados {#retrieving-metadata}

Defina a nova ID do aplicativo no canto superior direito da página e faça uma chamada para o recurso Metadados. Você receberá a seguinte resposta:

![](assets/non-empty-metadata-secndapp.png)

Dessa vez, o corpo da resposta não é mais uma lista vazia, como no exemplo da primeira aplicação. Agora, o Serviço de monitoramento de simultaneidade declara no corpo da resposta que **channel** os metadados são necessários na inicialização da sessão para avaliar a política.

Se você fizer uma chamada sem fornecer um valor para a variável **channel** , você obterá:

* Código de resposta - 400 BAD REQUEST
* Corpo da Resposta - uma carga de resultado da avaliação que descreve no **obrigações** campo o que é esperado na solicitação para inicialização de sessão para que a operação seja bem-sucedida.

![](assets/metadata-request-secndapp.png)


#### Inicialização da sessão {#session-init}

Atribua um valor para a chave de metadados necessária e defina-o como um parâmetro de formulário na solicitação de inicialização de sessão, conforme mostrado abaixo:

![](assets/session-init-params-secndapp.png)

Agora, a chamada será bem-sucedida e uma nova sessão será gerada.


#### Quebrando a política {#breaking-policy-second-app}

Para romper a regra que temos na política atribuída a este aplicativo, precisamos fazer duas chamadas com o mesmo valor de canal. Como no primeiro exemplo, a segunda chamada precisa ser feita enquanto a primeira sessão gerada ainda é válida.

![](assets/breaking-policy-secnd-app.png)

Se utilizarmos valores diferentes para os metadados do canal sempre que criarmos uma nova sessão, todas as chamadas serão bem-sucedidas, pois o limite 2 tem escopo para cada valor individualmente.

Como no primeiro exemplo, podemos usar o código de terminação para interromper remotamente fluxos conflitantes ou podemos esperar que um dos fluxos expire, supondo que nenhum heartbeat será operado neles.

