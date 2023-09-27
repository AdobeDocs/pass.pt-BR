---
title: Visão geral do Account IQ
description: O Account IQ ajuda os MVPDs e os programadores a entender os riscos para suas operações de receita e negócios e determinar as ações mais eficazes a serem tomadas para atenuar os impactos da fraude de credenciais.
exl-id: c0d85fc8-b5ab-4284-802e-82f52eff401f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---

# [!UICONTROL Account IQ] visão geral {#account-iq-overview}

O compartilhamento de credenciais por assinantes do serviço de transmissão é um problema importante e crescente para o setor. Além disso, entender, identificar e agir sobre o compartilhamento de credenciais é um processo complexo. Há uma complexidade envolvida na compreensão do comportamento de uso do assinante e no desenvolvimento de uma visão holística de sua atividade, por exemplo, diferenciando o compartilhamento entre membros dentro da mesma família e fora dela. Devido a esse desafio, os provedores de serviços de streaming podem ter inibições de agir contra o compartilhamento de credenciais.


<div class "preview">
Geralmente, os provedores de serviços de transmissão de vídeo entendem o risco e o custo de compartilhar com seus negócios, mas têm soluções limitadas, como bloquear os compartilhadores ou fazer ofertas. No entanto, recomenda-se uma abordagem informada e direcionada, que permita que os serviços entendam o compartilhamento com precisão e adotem estratégias que recompensem o bom comportamento de visualização e, simultaneamente, direcionem o crescimento dos negócios. </span>

![Diagrama de fluxo do Account IQ](assets/aiq-intro.png)

*Figura: Fluxo de informações do Account IQ*

Adobe Pass [!UICONTROL Account IQ] permite que os serviços de streaming de vídeo entendam os padrões de uso do assinante e identifiquem o compartilhamento de credenciais. Analisando profundamente a longa trilha de dados deixada para trás por cada assinante usando o modelo de aprendizado de máquina multicamada proprietário do Adobe, os serviços de transmissão podem entender o comportamento de uso e identificar o compartilhamento de credenciais com um grau maior de certeza. Além disso, permite que a ação seja tomada por meio de integrações com outros sistemas, como limitar fluxos simultâneos ou personalizar ofertas, e valida o impacto dessas ações — incentivando o comportamento de visualização legítimo ou aumentando os assinantes e a receita.

[!UICONTROL Account IQ] O fornece as ferramentas e os recursos para medir, gerenciar e monetizar o compartilhamento de credenciais. Relatórios, análises e painéis permitem a exploração de dados para identificar padrões. A ação direta é suportada por meio de exportações e integrações com o Adobe e sistemas de terceiros, como gestão de campanha, limitação de moeda ou registro de assinantes. E ferramentas de rastreamento dedicadas medem o sucesso dessas ações para que possam ser atualizadas ou expandidas.

[!UICONTROL Account IQ] as ferramentas e os recursos do aplicativo são explicados nas seguintes seções:

* [Painel](/help/accountiq/dashboard.md)
* [Relatórios de uso geral](/help/accountiq/general-usage-reports.md)
* [Relatórios de Contas compartilhadas](/help/accountiq/shared-acc-reports.md)
* [Padrões de uso](/help/accountiq/usage-patterns.md)
* [Operações](/help/accountiq/operations.md)

Vamos dar um mergulho profundo nos gráficos e relatórios em cada uma dessas seções.

>[!MORELIKETHIS]
>
>* [Como começar a usar o Account IQ](/help/accountiq/get-started.md)
>* [Painel](/help/accountiq/dashboard.md)
>* [Relatórios de uso geral](/help/accountiq/general-usage-reports.md)
>* [Relatórios de contas compartilhadas](/help/accountiq/shared-acc-reports.md)
>* [Padrões de uso](/help/accountiq/usage-patterns.md)
>* [Glossário de termos do produto](/help/accountiq/product-concepts.md)
>* [Informe oficial do Account IQ](https://www.adobe.com/content/dam/dx/us/en/products/primetime/resources/primetime-account-iq-whitepaper.pdf)

<!-- Credential sharing is rampant and prevalent among subscribers in the video streaming industry. To add to it, understanding, identifying, and acting on password sharing is a complex process. There is complexity involved in understanding the subscriber usage behavior and developing a holistic view of viewer activity—for example, distinguishing sharing among members within the same household and outside. Due to this challenge, streaming service providers have inhibitions in acting against password sharing.

Generally, video streaming service providers consider password sharing as fatal for business and act strongly against it, by blocking the sharers. However, it is advised to follow a holistic approach that enables them to understand sharing accurately and adopt strategies to reward good viewing behavior and target business growth simultaneously.

![Account IQ flow diagram](assets/aiq-intro.png)

*Figure: Account IQ information flow*

Adobe Pass Account IQ enables video streaming services understand the subscriber usage patterns and identify password sharing by analyzing usage behavior. Moreover, it validates the impact of applying actions to encourage legitimate viewing behavior while maximizing business ROI, eventually growing subscribers and revenue.

By deeply analyzing the long, winding trail of data left behind by each subscriber using Adobe's proprietary multi-layer machine learning model, customers can understand usage behavior and identify password sharing with a greater degree of certainty, use the insights to validate the impact of applying actions to encourage legitimate viewing behavior while maximizing business growth, eventually act on password sharing using validated tactics to improve viewer experience, growing subscribers and revenue (for e.g. converting sharers to paid subscribers, managing ad loads based on sharing behavior, rewarding good behavior with better viewer experience).

Account IQ is helps you understand usage patterns and identify password sharing by leveraging the Adobe Pass Authentication  solution that processes a huge volume of TV Everywhere transactions. A proprietary multi-layer machine learning model trained by this real-world TVE data accurately characterizes usage patterns and helps video streaming services understand usage patterns and identify password sharing at an individual account level. Based on Adobe's customer experience management solutions, Account IQ enables video streaming services to effectively use their audience data to create actionable sharing profiles as well powers integrations with other Adobe Digital Experience and 3rd party solutions—for example, Adobe Pass Concurrency Monitoring or Adobe Analytics—to enable understanding usage patterns, identify and act upon password sharing.


<!-- The widespread availability of video content and streaming services bring with it problem of account sharing; eventually leading to the loss of revenue by content providers. Account IQ helps TV Everywhere and VOD (video on demand) providers understand the risks to their revenue and business operations, and determine the most effective actions to take to mitigate the impacts of credential fraud. It helps these media companies (MVPDs, Programmers, and VOD providers) manage and uncover the instances of password sharing with a high level of confidence, enabling them deliver better business outcomes and provide better viewing experiences for subscribers.

To help media companies better understand the password sharing within their businesses, Adobe Pass Account IQ determines **Password Sharing Risk Index** that rates every subscriber on their likelihood of sharing account credentials for subscription passwords, from very low to very high. Based on these calculations and the resulting indices, analytics are performed and visuals are generated for better understanding and interpretation of the account sharing behavior. Account IQ is a hosted web application, which you can access using your browser.

Account IQ assigns sharing scores to different subscriber accounts, so that the content providers (media companies, programmers, MVPDs, and VOD providers) can take informed decisions about subscriber accounts and check the illicit sharing.

Passwords are the main methods for viewers to authenticate, and there is a misconception that credential sharing is allowed. This idea makes illicit password sharing a common practice; necessitating the need for media companies to educate their viewers about permissible sharing and prevent illicit sharing.-->
