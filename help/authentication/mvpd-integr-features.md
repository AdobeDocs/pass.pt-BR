---
title: Recursos de integração do MVPD
description: Recursos de integração do MVPD
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 3%

---

# Recursos de integração do MVPD

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#mvpd-int-features-overview}

A autenticação do Adobe Pass é compatível com os padrões emergentes da TV Everywhere. Ele é compatível com a **especificação CableLabs OLCA (Online Content Access)**, que fornece requisitos técnicos e arquitetura para a entrega de vídeo para um cliente de TV por assinatura a partir de fontes online. A Adobe participou do projeto conjunto de testes de interopt de CableLabs em junho de 2011 e passou no processo de teste para uma implementação do Provedor de serviços.  A Adobe também é membro ativo do **OATC (Open Authentication Technical Consortium)** e participa de vários projetos de redação de especificações dos subcomitês como parte desse órgão.

Depois de descrever a conformidade OLCA da Autenticação da Adobe Pass e a participação de Adobe no OATC, também é importante observar que a Autenticação da Adobe Pass é, na verdade, &quot;agnóstica de protocolo&quot;.  Mas neste estágio da era TVE, a autenticação Adobe Pass é definitivamente orientada para os padrões OLCA.  Os padrões delineiam formas acordadas atualmente para os diferentes players de TVE (programadores, MVPDs e provedores de serviços) implementarem os recursos de TVE. Muitos desses recursos estão listados nas tabelas abaixo, com links para páginas relacionadas que fornecem detalhes e exemplos de como implementar os recursos.

As informações nas tabelas têm como objetivo orientar o processo de integração da Autenticação do Adobe Pass em direção a uma funcionalidade consistente em todas as integrações MVPD. A coluna priority classifica os recursos em A, B e C:

* R - São recursos &quot;obrigatórios&quot; para a implantação inicial de uma integração.
* B - São aprimoramentos importantes na integração inicial, a serem adicionados após a implantação inicial.
* C - São aprimoramentos adicionais na integração que podem ser implementados após os requisitos &quot;B&quot;.


## 1. Principais características funcionais {#core-func-features}


| Não. | Recurso | Descrição | Prioridade | Notas |
|------|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [Logon iniciado pelo programador](/help/authentication/authn-oauth2-protocol.md) | O visualizador seleciona o MVPD e inicia o fluxo de autenticação (AuthN) do site ou aplicativo da marca do Programador. | A+ |                                                                                                                                                         |
| 1,2 | [Autorização baseada em canal](/help/authentication/authz-usecase.md) | Uma vez autenticada, a autorização (AuthZ) pode ocorrer em segundo plano, com a passagem de apenas um identificador de canal de rede e um identificador de usuário para o MVPD. | A+ |                                                                                                                                                         |
| 1,3 | Persistência de UserID | O MVPD fornece uma UserID persistente e ofuscada. | A |                                                                                                                                                         |
| 1,4 | [Suporte para Logon Único](/help/authentication/sso-support.md) | O visualizador faz logon com o MVPD no site de uma marca e pode então ir para outra marca e não ser solicitado a fazer logon novamente. A sessão AuthN é compartilhada entre as marcas. O suporte a SSO é para sites e aplicativos móveis/de dispositivos.  Para MVPDs, é necessário retornar uma ID de usuário ou algum outro token de usuário que possa ser usado para AuthZ entre marcas. | A |                                                                                                                                                         |
| 1,5 | Experiência do usuário de logon otimizada para dispositivo (UX) | O MVPD oferece suporte ao redimensionamento da tela de logon para que se ajuste às dimensões do dispositivo que a exibição está usando. | A |                                                                                                                                                         |
| 1,6 | Logotipo de Seletor de MVPD Padrão | O MVPD fornece um URL para um logotipo padrão de dimensões apropriadas (112x33 pixels). | A | O logotipo deve ser hospedado pelo MVPD e armazenado em cache pela CDN. |
| 1,7 | [Escopo do Provedor de Serviços](/help/authentication/serv-provider-scoping.md) | O MVPD oferece suporte à transmissão do identificador da marca (valor do Solicitante) na solicitação AuthN. | A- | Isso habilita uma experiência de logon específica do Provedor de serviços. |
| 1,8 | [Autorização de vários canais](/help/authentication/mvpd-preflight-authz.md#preflight-multich-authz) | O MVPD oferece suporte a um Programador que envia vários canais em uma única solicitação de autorização. | B+ |                                                                                                                                                         |
| 1,9 | Autenticação baseada no iFrame ou &quot;JS Pop-up&quot; | O Programador é capaz de integrar o fluxo de logon em um iFrame ou uma experiência pop-up em vez de um redirecionamento HTTP. | B |                                                                                                                                                         |
| 1,10 | [Logout iniciado pelo programador](/help/authentication/usecase-mvpd-logout.md) | O visualizador tem uma sessão autenticada no Site ou Aplicativo do Programador e com o MVPD. O visualizador pode iniciar um logout federado do site do Programador e o logout também limpa a sessão no portal MVPD. | B | Garante que os computadores compartilhados estejam mais protegidos contra uso indevido da perspectiva do Programador. |
| 1,11 | [Mensagens de Erro de Autorização Personalizada](/help/authentication/error-reporting.md) | O MVPD passa sua própria cadeia de caracteres de erro personalizada, que é apropriada para o site ou aplicativo federado do Programador ser exibido ao usuário. | B | Habilita cenários de venda adicional |
| 1,12 | **Metadados do usuário na resposta de autenticação** | A resposta AuthN do MVPD pode incluir metadados do usuário que atuam como uma dica para personalização da experiência do usuário durante o fluxo de direito. Esse requisito ativa dicas de controle dos pais do MVPD para o Programador. | B- |                                                                                                                                                         |
| 1,13 | Autenticação iniciada por MVPD | O visualizador conclui uma sessão de Autenticação bem-sucedida no portal MVPD e, em seguida, navega até o site TVE do Programador. O usuário não é solicitado a fornecer o seletor de MVPD e é autenticado automaticamente. | B- |                                                                                                                                                         |
| 1,14 | Escopo da ID de usuário | A ID de usuário do MVPD deve ocorrer de duas formas: uma com escopo para programadores e outra com escopo de todo o Adobe para fraude.  Isso permite que o Adobe compartilhe a ID de usuário MVPD de escopo de programador sem mais criptografia/ofuscação. | C |                                                                                                                                                         |
| 1,15 | Autorização baseada em ativos | Uma vez que o AuthN é concluído, o AuthZ pode ocorrer em segundo plano, transmitindo dados estruturados que podem incluir rede, exibição, ativo, classificação de controle dos pais e muito mais, conforme necessário. Isso ativa os controles dos pais em cada chamada AuthZ do Programador para o MVPD. | C |                                                                                                                                                         |
| 1,16 | Logout iniciado pelo MVPD | O visualizador tem uma sessão autenticada no site ou aplicativo do Programador e com o MVPD. O visualizador pode iniciar um logout federado do site do MVPD que também limpa a sessão em todos os sites de Programadores federados. | C |                                                                                                                                                         |
| 1,17 | Obrigações de autorização | O MVPD fornece condições adicionais na resposta AuthZ, como registro ou uma OLCA (Maximum Parental Control Rate, Classificação Máxima de Controle dos Pais) atualizada. | C |                                                                                                                                                         |
| 1,18 | Contexto do endereço IP | O MVPD requer a transmissão explícita do endereço IP. Para AuthZ, em que as chamadas são do lado do servidor, isso fornece ao MVPD informações de rastreamento de fraude sobre de onde o usuário vem na chamada AuthZ. | C |                                                                                                                                                         |
| 1,19 | Valor de TTL (Time-to-live) da persistência de token definido dinamicamente | O MVPD pode definir o TTL do token de autenticação da Adobe Pass dinamicamente por meio de propriedades na resposta, de modo que o Adobe fique fora do loop para alterações de TTL. | C- |                                                                                                                                                         |
| 1,20 | Tipo de dispositivo | O MVPD oferece suporte à transmissão do tipo de dispositivo na solicitação AuthN ou AuthZ. Essa propriedade informa o MVPD sobre a natureza do dispositivo no qual o conteúdo será consumido, para que ele possa ajustar o TTL do token AuthN ou AuthZ para que esteja em conformidade com suas próprias considerações de segurança para o dispositivo. | C- | Isso é útil para a plataforma sem cliente, em que pode ser difícil expor cada tipo de dispositivo como uma configuração no lado da Autenticação do Adobe Pass. |



## 2. Características operacionais {#operational-features}

| Não | Recurso | Descrição | Prioridade | Notas |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | Tempo de atividade 24 horas por dia, 7 dias por semana | Um acordo MVPD para se esforçar 24 horas por dia, 7 dias por semana e avaliar isso ao longo do tempo. | A |       |
| 2,2 | Credenciais De Teste Para Cada Programador | O MVPD fornece contas de teste separadas para cada Programador. | A |       |
| 2,3 | Plano de cronograma de expiração e renovação de certificado | Um plano documentado pelo MVPD com um agendamento para garantir que os certificados SAML estejam atualizados. | A- |       |
| 2,4 | Latência Média Abaixo De 250 Ms/Resposta | Um acordo MVPD para se esforçar por baixa latência e para medir isso ao longo do tempo. | A- |       |
| 2,5 | Logotipo do Seletor de MVPD Próprio da Hospedagem | O MVPD precisa hospedar seu próprio logotipo e ele deve ser protegido pelo armazenamento em cache do CDN. | B |       |
| 2,6 | Plano de escalonamento de suporte | Um plano de escalonamento documentado pelo MVPD que é mantido atualizado e revisado pelo menos trimestralmente. | A |       |
| 2,7 | Plano de proteção contra interrupções | Um co-plano MVPD com Adobe sobre medidas de degradação que podem ser usadas geralmente, conforme necessário. | B |       |
| 2,8 | Manter pontos de extremidade separados de preparo e produção | Um teste de integração MVPD que não requer falsificação de arquivo de host para testar a integração de preparo. | B |       |
| 2,9 | Endpoint de controle de qualidade adicional | O MVPD mantém uma integração IdP de controle de qualidade adicional para o desenvolvimento conjunto de novos recursos.  Com suporte a isso, seria menos provável que precisássemos de solicitações especiais do UAT para testes de certificados da loja de aplicativos. | C |       |





## 3. Recursos da experiência de autenticação {#authn-exp-features}


| Não | Recurso | Descrição | Prioridade | Notas |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Conversão de Autenticação Acima da Expectativa Mínima | O MVPD garante taxa de conversão mínima para provar funcional (5%) e razoável (30%). | A |                                                                                                                                                           |
| 3,2 | Recuperação de senha em linha | O MVPD fornece um meio de recuperar senhas em linha para o fluxo de Autenticação federado. | A |                                                                                                                                                           |
| 3,3 | Registro de conta em linha | O MVPD fornece um meio de criar uma nova conta em linha para o fluxo de Autenticação federado. | A |                                                                                                                                                           |
| 3,4 | Ajuda/suporte em linha | O MVPD fornece um meio de fornecer ajuda durante o fluxo de Autenticação federado. | A |                                                                                                                                                           |
| 3,5 | Autenticação In-home baseada em modem | O MVPD autentica automaticamente um dispositivo quando ele está na rede local de um modelo registrado (somente MVPD do ISP). | B | Essa é uma prioridade mais baixa porque é uma otimização que muitos ainda não podem suportar e apresenta alguns desafios para a mitigação de fraudes e o controle dos pais |
Agora é possível importar o código da tabela do Markdown diretamente usando a caixa de diálogo File/Paste table data... .



## 4. Recursos do Analytics {#analytics-features}


| Não | Recurso | Descrição | Prioridade |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Alinhamento do funil de conversão de autenticação | O MVPD tem uma métrica para solicitações de AuthN que começa com a solicitação de AuthN do SAML - para se alinhar às métricas de solicitação de Autenticação e AuthN do Programador do Adobe Pass. | A |
| 4,2 | Usuários únicos | Usuários que foram autenticados com sucesso e deduplicados em um roll-up mensal entre dias. | A |
| 4,3 | Contabilização de Fuso Horário | Os relatórios incluem o fuso horário de quando o dia muda. | A |





## 5. Características de atenuação da fraude {#fraud-mitgn-features}


| Não | Recurso | Descrição | Prioridade | Notas |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Validação de assinante de vídeo na autenticação | O MVPD garante que o usuário seja um assinante de vídeo válido durante o fluxo de Autenticação. | A |                                                                                                |
| 5,2 | Limite de taxa para autenticação/autorização | O MVPD oferece suporte à limitação em seus serviços da Web para limitar o uso de uma conta de usuário específica, quando apropriado. | B |                                                                                                |
| 5,3 | ID de usuário persistente com escopo global para Adobe | O MVPD garante que o Adobe tenha uma ID de usuário que possa ser rastreada por programadores para fraude. Isso não precisa ser fornecido diretamente ao cliente. | B | Especificação do Protocolo de Autorização Multimídia Online (OMAP) e Monitoramento do Usuário Real (RUM). |
| 5,4 | Validação de Uso Concorrente | O MVPD tem um meio de rastrear e limitar o uso simultâneo da conta do assinante além de um limite de negócios. | B |                                                                                                |

## P1 Recursos funcionais específicos do proxy {#proxy-sp-func-features}

| Não | Descrição | Prioridade | Notas |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy responsável por manter a precisão da lista atualizada de subMVPDs | A |                                                   |
| P 1.2 | O proxy fornece o logotipo dimensionado apropriado para cada subMVPD | A | Alguns subMVPDs ativos não têm o tamanho de logotipo correto |
| P 1.3 | O proxy fornece a página de logon com a marca adequada para cada subMVPD | A |                                                   |
| P 1.4 | Proxy responsável por direcionar marcas específicas de provedor de serviços com lista subMVPD | B |                                                   |
| P 1,5 | O proxy especifica todas as propriedades subMVPD corretamente (tamanho do iFrame, TTL, logotipo etc.) | A |                                                   |
| P 1.6 | O proxy especifica entityID separada | B |                                                   |

## P2 Recursos operacionais de proxy {#proxy-op-features}

| Não | Descrição | Prioridade |
|-------|----------------------------------------------------------------------------------------|----------|
| P 2.1 | A chave de API é segura | A |
| P 2.2 | Credenciais de teste para o primeiro subMVPD usado na integração ao vivo para cada novo solicitante | A |
| P 2.3 | As listas subMVPD são precisas e completas por solicitante | A |
