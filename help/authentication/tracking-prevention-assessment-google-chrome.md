---
title: Avaliação de prevenção de rastreamento no Google Chrome
description: Avaliação de prevenção de rastreamento no Google Chrome
source-git-commit: 579ce868b6ee94e1854bbc51145fc7840268db26
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# Avaliação da prevenção de rastreamento - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral

Este documento agrega recursos úteis e avalia as próximas alterações planejadas pelo Google Chrome como parte de sua iniciativa para eliminar os cookies de terceiros.

A avaliação é realizada para aplicativos da TV Everywhere (TVE) em execução no navegador Google Chrome e que estão usando o SDK JavaScript v4 do Adobe Pass Access Enabler para integrar-se aos serviços de back-end de autenticação da Adobe Pass.

## Recursos públicos

Veja abaixo uma lista de recursos agregados do site do desenvolvedor do Google e também do blog oficial que recomendamos que nossos clientes consultem:

* [A próxima etapa para a eliminação gradual de cookies de terceiros no Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Documentação do desenvolvedor para sandbox de privacidade](https://developers.google.com/privacy-sandbox)
* [Preparar para restrições de cookies de terceiros](https://developers.google.com/privacy-sandbox/3pcd)
* [Preparar-se para a eliminação de cookies de terceiros](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Preparação para o fim de cookies de terceiros](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Cookies de terceiros restritos por padrão a 1% dos usuários do Chrome](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Linha do tempo

Como um breve resumo, o Google Chrome iniciou os testes [Proteção contra Rastreamento](https://privacysandbox.com/), um novo recurso que limita o rastreamento entre sites que afeta todos os cookies de terceiros.

Inicialmente, esta medida teve início no início de 2024 e afetou cerca de 1 % dos seus utilizadores e o seu plano (provisório) consiste em prorrogar esta medida até 100 % dos utilizadores, a partir do terceiro trimestre de 2024.

## Avaliação

A Google publicou um documento agregando seu manual recomendado para se preparar para o fim da fase de cookies de terceiros no seguinte link: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Seguimos esse manual para a avaliação de aplicativos da TV Everywhere (TVE) em execução no navegador Google Chrome e que estão usando o SDK JavaScript v4 do Adobe Pass Access Enabler para integração com os serviços de back-end de autenticação da Adobe Pass.

### Conclusões

Com base em nossos testes, simulando as atualizações futuras do Google Chrome, os principais fluxos de negócios da TVE **continuará a funcionar conforme esperado**.

No entanto, é crucial reconhecer a estratégia mais ampla da Google, que envolve não apenas a descontinuação de cookies de terceiros, mas também o particionamento de armazenamento de terceiros.

Consequentemente, os usuários do Chrome enfrentarão interrupções com os recursos de Logon único (SSO), Logout único (SLO) e autenticação passiva, exigindo ações de logon/logout separadas para cada aplicativo TVE que usam (alinhado com a experiência atual no Safari).

## Chamada para autoavaliação

Recomendamos que nossos clientes conduzam avaliações semelhantes de forma proativa para identificar possíveis problemas com bastante antecedência e se familiarizarem com a experiência do usuário revisada do Google Chrome.

Essa avaliação deve abranger serviços primários e de terceiros, especialmente no que diz respeito à integração do SDK v4 do JavaScript do Adobe Pass Access Enabler.

Caso encontre dificuldades relacionadas aos fluxos comerciais da TVE, incluindo autenticação, pré-autorização, autorização, metadados do usuário ou logout, convidamos você a registrar um relatório por meio de um ticket do Zendesk com nossa equipe de atendimento ao cliente.

Para obter ajuda para desenvolver seu plano de autoavaliação, consulte as seções abaixo.

### Auditoria de uso de cookies

A partir do Chrome 118, a variável [Problemas do DevTools](https://developer.chrome.com/docs/devtools/issues/) A guia destaca os cookies possivelmente afetados com a seguinte mensagem: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Os cookies marcados para uso de terceiros podem ser identificados por seus `SameSite=None` valor do atributo.

Siga este link para ler mais: https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Ensaio de ruptura

Para testar a quebra, inicie o Chrome usando o `--test-third-party-cookie-phaseout` sinalizador de linha de comando ou do Chrome 118 habilitar `#test-third-party-cookie-phaseout` in `chrome://flags/`.

Isso configurará o Google Chrome para bloquear cookies de terceiros e garantir que a funcionalidade futura esteja ativa, a fim de simular melhor o estado após o encerramento.

Vale a pena aprofundar as especificações técnicas para os seguintes sinalizadores do Chrome:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Siga este link para ler mais: https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Outros navegadores

### Firefox

O Firefox tinha uma implantação do mecanismo chamado: `Enhanced Tracking Protection` alguns anos atrás.

Veja abaixo alguns recursos úteis do Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

O Safari teve uma implantação de seu mecanismo chamado: `Intelligent Tracking Prevention` alguns anos atrás.

Veja abaixo alguns recursos úteis do Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Veja abaixo alguns recursos úteis do Adobe Pass:

* [Avaliação da prevenção de rastreamento - Apple Safari](tracking-prevention-assessment-apple-safari.md)
