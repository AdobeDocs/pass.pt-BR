---
title: Autenticação do Adobe Pass e o novo modelo de permissões "Marshmallow" do Android 6
description: Autenticação do Adobe Pass e o novo modelo de permissões "Marshmallow" do Android 6
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# (Herdado) Autenticação do Adobe Pass e o novo modelo de permissões &quot;Marshmallow&quot; do Android 6 {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

A nova versão do Android 6 Marshmallow apresenta algumas atualizações no modelo de permissões, que pode afetar o comportamento de aplicativos que usam a versão 1.8 e mais antiga do Adobe Pass Authentication SDK.

Como um novo recurso, o novo sistema operacional Android oferece [controle granular sobre as permissões que os aplicativos exigem no momento da instalação e no tempo de execução](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>As alterações descritas abaixo **afetarão somente os aplicativos desenvolvidos especificamente para o Android 6.0** (targetSdkVersion=23). Eles não afetam aplicativos mais antigos já instalados no dispositivo do usuário ao atualizar para o Android 6.0.


Especificamente, para aplicativos desenvolvidos no Android Studio usando o [nível de API 23](http://developer.android.com/sdk/api_diff/23/changes.html) e que usam o Adobe Pass Authentication SDK, o desenvolvedor precisará gravar um código personalizado (consulte o trecho de código abaixo) [para acionar a caixa de diálogo de permissão/negação](https://developer.android.com/training/permissions/requesting.html).

A seguir está o trecho de código usado para solicitar acesso de gravação ao armazenamento externo do dispositivo:

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**Do ponto de vista dos usuários**, após a instalação, os usuários são recebidos por uma janela solicitando que confirmem permissões de leitura/gravação para arquivos (veja a figura 2 abaixo). Isso leva a um dos dois resultados a seguir:

1. Se o usuário **confirmar** as permissões, o fluxo de autenticação regular será mantido e os tokens serão armazenados no armazenamento global. Os usuários permanecerão autenticados no aplicativo e em vários aplicativos usando a Autenticação do Adobe Pass enquanto os tokens forem válidos.
1. Se o usuário **negar** as permissões, as ações de gravação no armazenamento falharão e os usuários só serão autenticados até que saiam do aplicativo. Observe que alguns aplicativos são reinicializados ao alternar entre o primeiro e o segundo plano, para que os usuários sejam desconectados ao executar essa ação. Os tokens NÃO são armazenados e os usuários precisarão se autenticar sempre que usarem o aplicativo.


>[!TIP]
>
>Um recurso que apresenta resiliência de armazenamento está sendo desenvolvido atualmente para o SDK de autenticação da Adobe Pass 1.9. A nova SDK está programada para a versão **na última semana de outubro de**. O aplicativo sofrerá fallback para gravar no armazenamento de sandbox do aplicativo sempre que o armazenamento geral não puder ser usado. Isso abrange o caso em que, para aplicativos desenvolvidos no nível 23 da API, os usuários NÃO aceitam permissões de leitura/gravação no armazenamento global. Os tokens são armazenados individualmente por aplicativo, o que significa que o Logon único entre aplicativos que usam a Autenticação do Adobe Pass será desativado.


![](../../../assets/android-permissions-request.png)

*Figura: a caixa de diálogo de solicitação de permissão para aplicativos gravados com API de direcionamento nível 23*

>[!IMPORTANT]
>
> O Adobe aconselha **seus parceiros a desenvolver aplicativos usando o nível de API 22 (targetSdkVersion=22) ou mais antigo para garantir a melhor experiência possível do usuário no processo de autenticação**.
