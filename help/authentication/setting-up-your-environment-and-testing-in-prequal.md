---
title: Configuração do ambiente e teste na pré-qualificação
description: Configuração do ambiente e teste na pré-qualificação
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Configuração do ambiente e dos testes na pré-qualidade{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API requer uma licença atual da Adobe. Nenhum uso não autorizado é permitido.

O objetivo desta nota técnica é ajudar nossos parceiros a configurar o ambiente e a começar a testar um novo build implantado no ambiente de pré-qualificação da Adobe.

Como há duas opções de criação: ***produção*** e ***teste***, neste documento vamos focar na configuração de produção com a menção de que todas as etapas são as mesmas para o teste, somente as URLs são diferentes.

As etapas 1 e 2 estão configurando o ambiente de teste em uma das máquinas de teste, a etapa 3 é uma verificação do fluxo básico e as etapas 4 &amp;5 apresentam algumas diretrizes de teste.

>[!IMPORTANT]
>
> É muito importante executar as etapas 1 e 2 sempre que você desejar alterar o ambiente de teste (alternando do palco para o perfil de produção ou o contrário)


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
>Os domínios excluídos da resposta por não serem relevantes e podem diferir de usuário para usuário.

>[!IMPORTANT]
>
> Esses endereços IP podem mudar no futuro e podem não ser os mesmos para usuários em regiões geográficas diferentes.


## ETAPA 2.  Falsificar o ambiente de pré-qualificação para ser produção {#spoofing-the-prequalification-environment}

* Edite o *arquivo c:\\windows\System32\\drivers\\etc\hosts* (no Windows) ou */etc/hosts* (no Macintosh/Linux/Android) e adicione o seguinte:

* Perfil de produção spoof
   * 52.13.71.11 http://entitlement.auth.adobe.com, http://sp.auth.adobe.com http://api.auth.adobe.com

**Falsificação no Android:** Para fazer spoof no Android, é necessário usar um emulador do Android.

* Depois que a falsificação estiver em vigor, você poderá simplesmente usar as URLs normais para os perfis de produção e de preparo: (ou seja, `http://sp.auth-staging.adobe.com` e `http://entitlement.auth-staging.adobe.com` e, na verdade, você obterá o *ambiente de pré-qualificação/produção* da* nova compilação.


## ETAPA 3.  Verifique se você está apontando para o ambiente correto {#Verify-you-are-pointing-to-the-right-environment}

**Esta é uma etapa fácil:**

* carregar [ambiente e [direito](https://entitlement.auth.adobe.com/environment.html) pré-qualificados](https://entitlement-prequal.auth.adobe.com/environment.html). Eles devem retornar a mesma resposta.


## PASSO 4.  Execute um fluxo simples de autenticação/autorização usando o site do programador {#peform-a-simple-auth-flow}

* Esta etapa requer o endereço de site do programador e algumas credenciais válidas de MVPD (um usuário que é autenticado e autorizado).

## PASSO 5.  Realizar testes de cenário usando os sites do programador {#perform-scenario-testing-using-programmer-website}

* Depois de concluir a configuração do ambiente e garantir que o fluxo básico de autenticação de autorização esteja funcionando, você pode continuar com o teste de cenários mais complexos.


## ETAPA 6.  Realizar testes usando o site de teste da API {#perform-testing-using-api-testing-site}

* Se quiser aprofundar no teste da Autenticação do Adobe Pass, recomendamos que você use o [site de teste de API](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Você pode encontrar mais detalhes sobre o site de teste da API em [Como testar fluxos de Autenticação e Autorização usando o site de teste da API do Adobe](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).
