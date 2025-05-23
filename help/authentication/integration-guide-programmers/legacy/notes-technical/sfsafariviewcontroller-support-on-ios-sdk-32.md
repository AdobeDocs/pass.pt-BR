---
title: Suporte a SFSafariViewController no iOS SDK 3.2+
description: Suporte a SFSafariViewController no iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# (Herdado) Suporte a SFSafariViewController no iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>


**Devido a requisitos de segurança, algumas páginas de logon do MVPD DEVEM ser apresentadas em um SFSafariViewController, em vez de exibições da Web.**

Alguns MVPD exigem que suas páginas de logon sejam apresentadas em um controle de navegador seguro, como SFSafariViewController. Eles estão bloqueando ativamente as visualizações da Web, portanto, para poder se autenticar com eles, precisamos usar o SVC.

## Compatibilidade {#compatiblity}

A partir do iOS SDK versão 3.1, o AccessEnabler SDK exibe automaticamente a página de logon de um MVPD específico em um SFSafariViewController, com base na configuração do servidor.

A versão 3.1 do SDK apresenta automaticamente o SFSafariViewController do controlador de visualização raiz do aplicativo. Embora isso simplifique o gerenciamento da página de logon para implementadores, há casos em que não é possível apresentar o SFSafariViewController a partir do controlador de exibição raiz, devido à implementação específica do aplicativo (como um controlador modal já visível).

Para tais casos, a versão 3.2 introduz a capacidade para o programador gerenciar manualmente o SVC.

## Gerenciamento manual de SVC {#manual-svc-management}

Para gerenciar manualmente o SVC, o implementador deve executar as seguintes etapas:


1. chame **setOptions([&quot;handleSVC&quot;:true])** após a inicialização de AccessEnabler (verifique se essa chamada foi executada antes do início da autenticação). Isso habilitará o gerenciamento &quot;manual&quot; do SVC. O SDK não apresentará automaticamente o SVC, mas, quando necessário, o     chame **navigate(toUrl:*{url}* useSVC:true)**.

1. implemente o retorno de chamada opcional **`navigateToUrl:useSVC:`** dentro da implementação, você deve criar uma instância svc usando a instância SFSafariViewController usando a url fornecida e apresentá-la na tela:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Notas:***

   - *Você pode personalizar o SFSafariViewController da maneira que desejar. Por exemplo, no iOS 11+, é possível alterar o rótulo &quot;Concluído&quot; para &quot;Cancelar&quot;.*
   - *para poder descartar o svc, você precisa de uma referência a ele. Não o crie no escopo de **navigateToUrl:useSVC***
   - *usar seu próprio controlador de exibição para &quot;myController&quot;*


1. Na implementação delegada do seu aplicativo de **application(\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, adicione o código para fechar o svc. Você já deve ter algum código lá que chame **accessEnabler.handleExternalURL()**. Logo abaixo, adicione:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Novamente, svc é uma referência ao SFSafariViewController criado na etapa 2.


1. Implemente **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** de **SFSafariViewControllerDelegate** para detectar quando o usuário cancelou o svc usando o botão &quot;Concluído&quot;. Nesta função, para informar ao SDK que a autenticação foi cancelada, é necessário chamar:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
