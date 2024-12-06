---
title: Configuração do ambiente e teste na pré-qualificação
description: Configuração do ambiente e teste na pré-qualificação
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Configuração do ambiente e dos testes na pré-classificação{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>O conteúdo neste página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe Systems. Nenhum uso não autorizado é permitido.

O objetivo desta nota técnica é ajudar nossos parceiros a configurar suas ambiente e start testar um novo build implantado no Adobe Systems ambiente de pré-qualificação.

Como há dois tipos build: ***produção*** e ***preparo***, neste documento vamos focalizar na configuração da produção com a menção de que todas as etapas são as mesmas para o preparo, apenas as URLs são diferentes.

As etapas 1 e 2 estão configurando a teste ambiente em uma das máquinas de teste, a etapa 3 é uma verificação do fluxo básico e as etapas 4 &amp;5 estão apresentando algumas diretrizes de teste.

>[!IMPORTANT]
>
> É muito importante executar as etapas 1 e 2 sempre que quiser alterar os ambiente de teste (alternando de teste para perfil de produção ou de outra forma de contornar)


## ETAPA 1. Resolução de Passagem de domínio para um IP {#resolving-pass-domain-to-an-ip}

* Para localizar um IP do balanceador de carga que possa ser usado para falsificação, execute o seguinte comando:

* **No Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

* **No Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>Os domínios excluídos da resposta, pois não são relevantes e podem ser diferentes de usuário para usuário.

>[!IMPORTANT]
>
> Esses endereços IP podem mudar no futuro e podem não ser os mesmos para usuários em diferentes regiões geográficas.


## ETAPA 2.  Falsificação do ambiente de pré-qualificação para produção {#spoofing-the-prequalification-environment}

* Editar o *arquivo c:\\windows\\System32\\drivers\\etc\\hosts* (no Windows) ou */etc/hosts* (em Macintosh/Linux/Android) e adicione o seguinte:

* Perfil de produção falsos
   * 52.13.71.11 entitlement.auth.adobe.com sp.auth.adobe.com api.auth.adobe.com

**Falsificação no Android:** Para fazer spoof no Android, é necessário usar um emulador do Android.

* Depois que a falsificação estiver em vigor, você poderá simplesmente usar as URLs normais para os perfis de produção e de preparo: (ou seja, `http://sp.auth-staging.adobe.com` e `http://entitlement.auth-staging.adobe.com` e, na verdade, você obterá o *ambiente de pré-qualificação/produção* da* nova compilação.


## ETAPA 3.  Verifique se você está apontando para o ambiente correto {#Verify-you-are-pointing-to-the-right-environment}

**Esta é uma etapa fácil:**

* carregamento [de ambiente](https://entitlement-prequal.auth.adobe.com/environment.html) qualificado e [direito](https://entitlement.auth.adobe.com/environment.html). Elas devem retornar a mesma resposta.


## ETAPA 4.  Realize um fluxo simples de autenticação/autorização usando o site do programador {#peform-a-simple-auth-flow}

* Esta etapa requer o endereço do site do programador e algumas credenciais válidas de MVPD (uma usuário que seja autenticada e autorizada).

## ETAPA 5.  Realizar teste de cenário usando os sites do programador {#perform-scenario-testing-using-programmer-website}

* Depois de concluir a configuração ambiente e garantir que o fluxo básico de autenticação-autorização esteja funcionando, você pode continuar com o teste de cenários mais complexos.


## ETAPA 6.  Realizar testes usando o site de teste da API {#perform-testing-using-api-testing-site}

* Se quiser aprofundar no teste da Autenticação do Adobe Pass, recomendamos que você use o [site de teste de API](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Você pode encontrar mais detalhes sobre o site de teste da API em [Como testar fluxos de Autenticação e Autorização usando o site de teste da API do Adobe](/help/authentication/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).
