---
title: Compreensão dos ambientes de Adobe
description: Compreensão dos ambientes de Adobe
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Compreensão dos ambientes de Adobe {#understanding-the-adobe-environments}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A documentação oficial que descreve os ambientes de Adobe está disponível [Configurando seu ambiente e testando em Pré-Igual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

Os ambientes de Adobe, resumidos em poucas palavras:

O Adobe tem dois ambientes: **Pré-qualificação** e **Versão**.

* No ambiente de pré-qualificação, estamos preparando a nova build a ser lançada.

* A build de versão atual está no ambiente de versão.

Cada ambiente tem dois perfis: **preparo** e **produção**.

* O perfil de preparação se conecta ao servidor de preparação MVPDs
* O perfil de produção se conecta ao perfil de produção MVPDs.

O motivo para ter os dois perfis é que no perfil temporário, estamos preparando novos parceiros para entrar em funcionamento e eles gostariam de testar o sistema com a próxima build (Pré-qualificação) ou com a versão (mais estável).

Se um parceiro quiser testar a nova versão, há algumas etapas adicionais que precisam ser realizadas. Consulte, [Configuração do ambiente e teste em Pré-Igual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

Seguindo as etapas acima, é garantido que a próxima versão será testada no ambiente de Pré-qualificação.
