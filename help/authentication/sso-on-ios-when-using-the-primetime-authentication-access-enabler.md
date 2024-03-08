---
title: SSO no iOS ao usar o Adobe Pass Authentication Access Enabler
description: SSO no iOS ao usar o Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# SSO no iOS ao usar o Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

## Visão geral

O Logon único (SSO) entre aplicativos alimentados pela Autenticação da Adobe Pass funciona de maneiras diferentes, dependendo do sistema operacional subjacente.

Este documento aborda **SSO no iOS**, ao usar a Autenticação do Adobe Pass **Ativador de acesso**.

**Ativador de acesso** **1.10** é a versão mais recente do SDK nativo do iOS de autenticação da Adobe Pass. A Adobe recomenda que você mude para essa versão em vez de ficar com uma versão mais antiga. Se você estiver usando uma versão mais antiga do Access Enabler, poderá baixar a versão mais recente [aqui](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

O SSO no iOS é ditado pelas seguintes condições:

- Os aplicativos devem usar o mesmo **armazenamento de token** (no formato de uma área de trabalho personalizada criada pelo Access Enabler).
- Os aplicativos devem gerar o mesmo **ID do dispositivo** (O Access Enabler calcula a ID do dispositivo com base no endereço do MAC ou IDFV, dependendo da versão do sistema operacional).

## Comportamento

O comportamento do SSO é o seguinte:

- **iOS 6 e inferior**: o SSO funciona automaticamente entre aplicativos desenvolvidos pela mesma equipe ou por equipes diferentes. A ID do dispositivo é calculada com base no endereço do MAC (o mesmo valor é produzido em todos os aplicativos) e a área de armazenamento é comum a todos os aplicativos (a área de trabalho personalizada pode ser compartilhada entre aplicativos no iOS 6 e versões anteriores).
   - **Importante:** Observe que a versão 1.9.4 do SDK do iOS [o objetivo mínimo de implantação do iOS foi aumentado para iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 e superior**: O SSO funcionará nas seguintes condições:

1. Os aplicativos são publicados usando o mesmo perfil de distribuição do Apple ou perfis que pertencem à mesma equipe. Essa é a única maneira de os aplicativos compartilharem áreas de trabalho personalizadas no iOS 7 e superior. Em todos os outros cenários, a área de trabalho é colocada em sandbox por aplicativo. De [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] e +\[`UIPasteboard pasteboardWithUniqueName`\] agora é exclusivo no nome fornecido para permitir que apenas os aplicativos no mesmo grupo de aplicativos acessem a área de trabalho. Se o desenvolvedor tentar criar uma área de transferência com um nome que já existe e não faz parte do mesmo conjunto de aplicativos, ele obterá sua própria área de transferência exclusiva e privada. Observe que isso não afeta as áreas de trabalho fornecidas pelo sistema, o geral e a localização.

1. Os aplicativos têm o mesmo prefixo de ID do pacote (todos os componentes, exceto o último). Somente os aplicativos que compartilham o mesmo prefixo de ID do pacote calcularão o mesmo IDFV. De [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): no IOS 7, todos os componentes do pacote, exceto o último componente, são usados para gerar a ID do fornecedor. Se a ID do pacote tiver apenas um único componente, a ID do pacote inteiro será usada.

Agora vamos nos concentrar no **&#39;iOS 7 e superior&#39;** como é o mais frequente para usuários reais:

Ambas as condições (compartilhar um perfil da mesma equipe de desenvolvimento e ter um prefixo de identificador de pacote comum) são condições obrigatórias para o SSO.

Estas são as combinações possíveis e seus resultados apresentados:

- **Perfis da mesma equipe e do mesmo prefixo de ID do pacote**: os aplicativos compartilharão o mesmo armazenamento da área de trabalho e terão a mesma ID do dispositivo (IDFV). Um usuário precisará se autenticar apenas uma vez (no primeiro aplicativo usado) e o estado de autenticação será compartilhado em todos os outros aplicativos. Exemplo de fluxo:

1. O usuário abre o aplicativo A (com ID do pacote) *com.x.y.AppA*) e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B (com ID do pacote) *com.x.y.AppB*) e é automaticamente autenticado ao compartilhar os dados de direito do aplicativo A (da etapa 2)
1. O usuário abre o aplicativo A e ainda está autenticado (da etapa 2)



- **Perfis da mesma equipe, mas com diferentes prefixos de ID do pacote**: os aplicativos compartilharão o mesmo armazenamento da área de transferência, mas terão IDs de dispositivo diferentes (IDFVs). Um usuário precisará ser autenticado uma vez para cada aplicativo. Exemplo de fluxo:

1. O usuário abre o aplicativo A (com ID do pacote) *com.x.y.AppA*) e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B (com ID do pacote) *com.z.AppB*) e o Ativador de acesso detecta o token obtido pelo primeiro aplicativo (porque o armazenamento é compartilhado), mas não tentará usá-lo via SSO devido às diferentes IDs do dispositivo
1. O usuário realiza autenticação no aplicativo B
1. O usuário abre o aplicativo A e ainda está autenticado (da etapa 2)



- **Perfis de equipes diferentes (a ID do pacote é irrelevante neste cenário)**: os aplicativos terão armazenamentos diferentes na área de trabalho e o SSO será desativado entre eles. Um usuário precisará se autenticar uma vez por aplicativo e as sessões de autenticação permanecerão persistentes ao alternar entre aplicativos. Exemplo de fluxo:


1. O usuário abre o aplicativo A e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B e não é autenticado
1. O usuário realiza autenticação no aplicativo B
1. O usuário abre o aplicativo A e é autenticado (da etapa 2)
1. O usuário abre o aplicativo B e é autenticado (da etapa 4)

**Nota:** Observe que as condições acima para o SSO são aplicáveis ao instalar aplicativos por meio do **Apple App Store**. Se os aplicativos forem implantados em um simulador (onde a assinatura do aplicativo não se aplica), instalados com o Xcode ou distribuídos por um perfil Ad Hoc, você poderá obter resultados diferentes.

**Importante:** Nota (**em relação ao AccessEnabler v1.8**): o segundo cenário descrito acima (perfis da mesma equipe, mas com diferentes prefixos de ID do pacote) criará uma experiência do usuário muito desagradável para os usuários do **AccessEnabler v1.8** em aplicativos desenvolvidos pela mesma equipe (empresa de mídia). O usuário será automaticamente desconectado durante a transição entre aplicativos da mesma empresa de mídia. Portanto, os desenvolvedores de aplicativos devem tomar cuidado ao decidir a ID do pacote e o perfil de distribuição. O cenário exato nesse caso é apresentado abaixo:

Os aplicativos compartilharão o mesmo armazenamento da área de transferência, mas terão IDs de dispositivo diferentes (IDFVs). Um usuário precisará ser autenticado uma vez por aplicativo, **mas as sessões de autenticação serão apagadas ao alternar entre aplicativos**. Exemplo de fluxo:

1. O usuário abre o aplicativo A (com ID do pacote) *com.x.y.AppA*) e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B (com ID do pacote) *com.z.AppB*) e os dados de direito criados pelo aplicativo A são automaticamente apagados pelo Ativador de acesso (mecanismo de segurança que detecta um conflito entre a ID de dispositivo atualmente calculada no aplicativo B e a que está armazenada nos tokens de direito - criados pelo aplicativo A)
1. O usuário realiza autenticação no aplicativo B
1. O usuário abre o aplicativo A e os dados de direito criados pelo aplicativo B são automaticamente apagados pelo Ativador de acesso (mecanismo de segurança que detecta um conflito entre a ID de dispositivo atualmente calculada no aplicativo A e a que está armazenada nos tokens de direito - criado pelo aplicativo B)
