---
title: SSO no iOS ao usar o Adobe Pass Authentication Access Enabler
description: SSO no iOS ao usar o Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%

---

# SSO (herdado) no iOS ao usar o Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

## Visão geral

O Logon único (SSO) entre aplicativos alimentados pela Autenticação da Adobe Pass funciona de maneiras diferentes, dependendo do sistema operacional subjacente.

Este documento aborda o **SSO no iOS**, ao usar o **Ativador de Acesso** da Autenticação do Adobe Pass.

O **Ativador de Acesso** **1.10** é a versão mais recente do SDK nativo do iOS de Autenticação da Adobe Pass. A Adobe recomenda mudar para essa versão em vez de ficar com uma versão mais antiga. Se você estiver usando uma versão mais antiga do Access Enabler, poderá baixar a versão mais recente [aqui](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

O SSO no iOS é ditado pelas seguintes condições:

- Os aplicativos devem usar o mesmo **armazenamento de token** (no formato de uma área de trabalho personalizada criada pelo Ativador de Acesso).
- Os aplicativos devem gerar a mesma **ID de Dispositivo** (o Access Enabler calcula a ID de Dispositivo com base no endereço do MAC ou IDFV, dependendo da versão do sistema operacional).

## Comportamento

O comportamento do SSO é o seguinte:

- **iOS 6 e inferior**: o SSO funciona automaticamente entre aplicativos desenvolvidos pela mesma equipe ou por equipes diferentes. A ID do dispositivo é calculada com base no endereço do MAC (o mesmo valor é produzido em todos os aplicativos) e a área de armazenamento é comum a todos os aplicativos (a área de trabalho personalizada pode ser compartilhada entre aplicativos no iOS 6 e versões anteriores).
   - **Importante:** observe que a versão 1.9.4 do iOS SDK [aumentou o destino mínimo de implantação do iOS para o iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 e superior**: o SSO funcionará nas seguintes condições:

1. Os aplicativos são publicados usando o mesmo perfil de distribuição do Apple ou perfis que pertencem à mesma equipe. Essa é a única maneira de os aplicativos compartilharem áreas de trabalho personalizadas no iOS 7 e superior. Em todos os outros cenários, a área de trabalho é colocada em sandbox por aplicativo. De [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] e +\[`UIPasteboard pasteboardWithUniqueName`\] agora são exclusivos para o nome fornecido, permitindo que somente os aplicativos no mesmo grupo de aplicativos acessem a área de trabalho. Se o desenvolvedor tentar criar uma área de transferência com um nome que já existe e não faz parte do mesmo conjunto de aplicativos, ele obterá sua própria área de transferência exclusiva e privada. Observe que isso não afeta as áreas de trabalho fornecidas pelo sistema, o geral e a localização.

1. Os aplicativos têm o mesmo prefixo de ID do pacote (todos os componentes, exceto o último). Somente os aplicativos que compartilham o mesmo prefixo de ID do pacote calcularão o mesmo IDFV. De [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): no IOS 7, todos os componentes do pacote, exceto o último componente, são usados para gerar a ID do fornecedor. Se a ID do pacote tiver apenas um único componente, a ID do pacote inteiro será usada.

Agora vamos nos concentrar no cenário do **&#39;iOS 7 e superior&#39;**, já que ele é o mais frequente para usuários reais:

Ambas as condições (compartilhar um perfil da mesma equipe de desenvolvimento e ter um prefixo de identificador de pacote comum) são condições obrigatórias para o SSO.

Estas são as combinações possíveis e seus resultados apresentados:

- **Perfis da mesma equipe e do mesmo prefixo de ID do Pacote**: os aplicativos compartilharão o mesmo armazenamento da área de trabalho e terão a mesma ID do Dispositivo (IDFV). Um usuário precisará se autenticar apenas uma vez (no primeiro aplicativo usado) e o estado de autenticação será compartilhado em todos os outros aplicativos. Exemplo de fluxo:

1. O usuário abre o aplicativo A (com a ID do Pacote *com.x.y.AppA*) e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B (com a ID do Pacote *com.x.y.AppB*) e é autenticado automaticamente ao compartilhar os dados de qualificação do aplicativo
A (da etapa 2)
1. O usuário abre o aplicativo A e ainda está autenticado (da etapa 2)



- **Perfis da mesma equipe, mas com diferentes prefixos de ID de Pacote**: os aplicativos compartilharão o mesmo armazenamento da área de trabalho, mas terão diferentes IDs de Dispositivo (IDFVs). Um usuário precisará ser autenticado uma vez para cada aplicativo. Exemplo de fluxo:

1. O usuário abre o aplicativo A (com a ID do Pacote *com.x.y.AppA*) e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B (com a ID do Pacote *com.z.AppB*) e o Ativador de Acesso detecta o token obtido pelo primeiro aplicativo (porque o armazenamento é compartilhado), mas não tentará usá-lo via SSO devido às diferentes IDs do Dispositivo
1. O usuário realiza autenticação no aplicativo B
1. O usuário abre o aplicativo A e ainda está autenticado (da etapa 2)



- **Perfis de equipes diferentes (a ID do Pacote é irrelevante neste cenário)**: os aplicativos terão armazenamentos diferentes na área de trabalho e o SSO será desabilitado entre eles. Um usuário precisará se autenticar uma vez por aplicativo e as sessões de autenticação permanecerão persistentes ao alternar entre aplicativos. Exemplo de fluxo:


1. O usuário abre o aplicativo A e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B e não é autenticado
1. O usuário realiza autenticação no aplicativo B
1. O usuário abre o aplicativo A e é autenticado (da etapa 2)
1. O usuário abre o aplicativo B e é autenticado (da etapa 4)

**Observação:** observe que as condições acima para SSO são aplicáveis ao instalar aplicativos por meio do **Apple App Store**. Se os aplicativos forem implantados em um simulador (onde a assinatura do aplicativo não se aplica), instalados com o Xcode ou distribuídos por um perfil Ad Hoc, você poderá obter resultados diferentes.

**Importante:** Observação (**sobre o AccessEnabler v1.8**): o segundo cenário descrito acima (perfis da mesma equipe, mas com diferentes prefixos de ID de Pacote) criará uma experiência de usuário muito desagradável para os usuários do **AccessEnabler v1.8** em aplicativos desenvolvidos pela mesma equipe (empresa de mídia). O usuário será automaticamente desconectado durante a transição entre aplicativos da mesma empresa de mídia. Portanto, os desenvolvedores de aplicativos devem tomar cuidado ao decidir a ID do pacote e o perfil de distribuição. O cenário exato nesse caso é apresentado abaixo:

Os aplicativos compartilharão o mesmo armazenamento da área de transferência, mas terão IDs de dispositivo diferentes (IDFVs). Um usuário precisará se autenticar uma vez por cada aplicativo, **mas as sessões de autenticação serão apagadas ao alternar entre aplicativos**. Exemplo de fluxo:

1. O usuário abre o aplicativo A (com a ID do Pacote *com.x.y.AppA*) e não está autenticado
1. O usuário faz autenticação no aplicativo A
1. O usuário abre o aplicativo B (com a ID de Pacote *com.z.AppB*) e os dados de direito criados pelo aplicativo A são automaticamente apagados pelo Ativador de Acesso (mecanismo de segurança que detecta um conflito entre a ID de Dispositivo calculada no momento no aplicativo B e a que está armazenada nos tokens de direito - criados pelo aplicativo A)
1. O usuário realiza autenticação no aplicativo B
1. O usuário abre o aplicativo A e os dados de direito criados pelo aplicativo B são automaticamente apagados pelo Ativador de acesso (mecanismo de segurança que detecta um conflito entre a ID de dispositivo atualmente calculada no aplicativo A e a que está armazenada nos tokens de direito - criado pelo aplicativo B)
