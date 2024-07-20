---
title: Requisitos mínimos do sistema
description: Requisitos mínimos do sistema
exl-id: 57b21e2a-abd7-4b4b-85f1-25584a850e40
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Requisitos mínimos do sistema {#minimum-system-requirements}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


## Visão geral {#overview}

Este documento apresenta os requisitos atuais de software e hardware para implementar as integrações da Autenticação Adobe Pass nas plataformas compatíveis. Todos os navegadores e sistemas operacionais da Web/dispositivos móveis compatíveis listados abaixo se beneficiarão do suporte total da equipe de autenticação da Adobe Pass, vinculados aos SLAs acordados.

Enquanto a equipe de autenticação da Adobe Pass incentivamos o uso das versões estáveis mais recentes dos navegadores e sistemas operacionais; também reconhecemos a existência de plataformas e navegadores incompatíveis/antigos que estão em uso no momento. Esses dispositivos desatualizados ainda podem funcionar sem problemas, no entanto, estarão mais propensos a erros.

A abordagem inicial para atenuar quaisquer problemas que apareçam nessas plataformas desatualizadas deve ser a atualização para as versões mais recentes; pode ser a versão do SO, a versão do navegador ou a versão do aplicativo instalado.

Quaisquer problemas que aparecerem nessas plataformas serão tratados como uma base de melhor esforço pela equipe de autenticação da Adobe Pass.

A Adobe Pass incentiva nossos clientes e parceiros a considerarem a atualização para as versões mais recentes para se beneficiarem do suporte completo do Adobe em quaisquer problemas em potencial, além de melhorias de desempenho, eficiência e segurança.


## Requisitos de sistema operacional e navegador {#browser-OS-system-requirements}


| Navegador da Web/móvel (†) | Versões suportadas |
|---|---|
| Google Chrome | **70** ou posterior |
| Mozilla Firefox | **57** ou posterior |
| Apple Safari | **14** ou posterior |
| Microsoft Edge | **100** ou posterior |

(†) O Adobe recomenda não usar o modo privado ou anônimo.

| Sistema operacional | Versões suportadas |
|---|---|
| *Android* | **7.0** (Nougat) ou posterior |
| *iOS* | **14** ou posterior |
| *iPadOS* | **14** ou posterior |
| *tvOS* | **14** ou posterior |
| *Acionar SO* | **5 (Android 5.1)** ou posterior |
| *SO Mac* | **10.13** ou posterior |
| *Microsoft Windows* | **10** ou posterior |




>[!NOTE]
>
>Cookies de terceiros - Os fluxos de direito de autenticação da Adobe Pass podem falhar quando os cookies de terceiros são desativados.  Esse problema entra em ação somente quando as configurações do navegador são modificadas. Para todos os navegadores compatíveis, a Autenticação do Adobe Pass deve funcionar com as configurações padrão.


## Requisitos de dispositivo para implementações sem cliente (REST) {#general_clientless_reqs}


Qualquer dispositivo que consumirá os serviços de Autenticação da Adobe Pass por meio de implementações sem cliente **deve ser capaz de**:

* Forneça uma ID de dispositivo exclusiva com hash. Se o dispositivo não fornecer uma ID de dispositivo com hash exclusiva, ele deverá ser capaz de manter uma ID exclusiva fornecida pela Autenticação Adobe Pass. O dispositivo deve ser capaz de manter o identificador exclusivo permanentemente em seu armazenamento local e fornecer o identificador exclusivo como o identificador do dispositivo ao fazer chamadas para as APIs de autenticação da Adobe Pass.
* Gerar assinaturas digitais usando o algoritmo HMAC-SHA1
* Definir cabeçalhos HTTP arbitrários
* Consumir serviços Web RESTful
* Analisar formatos de dados XML e JSON
* Enviar tráfego usando HTTPS
* Lidar com códigos de erro HTTP
