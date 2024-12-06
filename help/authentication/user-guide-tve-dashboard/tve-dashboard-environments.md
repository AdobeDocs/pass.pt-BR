---
title: Ambientes do painel TVE
description: Entenda o uso e o funcionamento de diferentes ambientes no Painel de TVE.
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Ambientes {#environments}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

O painel TVE fornece diferentes ambientes personalizados para atender a objetivos específicos na autenticação do Adobe Pass. Há dois ambientes principais:

* **Pré-Igual**: o ambiente de pré-qualificação serve como um campo de teste para preparar e testar novas compilações antes da implantação em produção.

* **Versão**: o ambiente de versão hospeda as compilações finalizadas e testadas para produção.

Em cada ambiente, há dois perfis diferentes:

* **Estágios**: o perfil de preparo se conecta ao servidor de preparo do MVPD para testes e validação de integrações antes de entrar em funcionamento.

* **Produção**: o perfil de produção se conecta ao perfil de produção do MVPD para atividades de produção reais.

## Casos de uso

Os ambientes no painel TVE atendem a vários casos de uso durante todo o ciclo de vida do aplicativo. Esses ambientes permitem:

### Estágios desiguais

* Valide novos recursos não lançados do servidor de Autenticação do Adobe Pass usando pontos de extremidade de preparo do MVPD.
* Usado principalmente pela equipe de produtos de autenticação da Adobe Pass para adicionar e validar novas integrações MVPD.

### Produção Pré-Igual

* Valide novos recursos ou configurações não lançados do servidor de Autenticação da Adobe Pass usando os endpoints de produção do MVPD.
* Valide novas versões de aplicativos para cada canal usando os endpoints de produção do MVPD.
* Valide cada alteração de configuração antes de enviá-la para produção.

### Estágios de lançamento

* Valide novas versões de aplicativos para cada canal usando os pontos de extremidade de preparo do MVPD.
* Realizar testes de desempenho ou capacidade nesse ambiente.

### Liberar produção

* Representa o ambiente ativo com a build de versão mais recente do Adobe Pass disponível para todos os usuários finais.
* Mantém a estabilidade do código e da configuração e reflete imediatamente as alterações de configuração no aplicativo do usuário final.

## Alternar ambientes {#switch-environments}

Siga as etapas para alternar entre os ambientes do Painel TVE de autenticação da Adobe Pass.

1. Faça logon com as credenciais do programador.

1. Selecione o ambiente de preparo ou produção necessário no menu suspenso **Ambiente**, na parte superior do painel esquerdo.

   ![Lista suspensa de ambientes do Painel do TVE](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Menu suspenso do ambiente do Painel TVE de Autenticação do Adobe Pass*

>[!NOTE]
>
> As configurações podem variar em cada ambiente com base em suas configurações.
