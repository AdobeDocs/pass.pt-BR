---
title: Mecanismo de verificação de integridade do armazenamento iOS/tvOS
description: Mecanismo de verificação de integridade do iOS/tvOS
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 2%

---

# Mecanismo de verificação de integridade do iOS/tvOS (herdado) {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Introdução {#Intro}

A partir da versão 3.8.3 do iOS/tvOS AccessEnabler SDK, a opção de executar verificações de integridade de armazenamento está disponível na inicialização do AccessEnabler.

Para usar esse mecanismo, a api foi estendida com um método de inicialização adicional para a classe AccessEnabler.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Verificações de integridade {#Checks}

As verificações de integridade de armazenamento são úteis quando há suspeita de corrupção do armazenamento AccessEnabler (como quando uma condição de corrida ocorre durante uma operação de armazenamento de leitura/gravação).

As seguintes verificações estão disponíveis para serem executadas na inicialização do AccessEnabler:
- Operabilidade do armazenamento: verifica o sucesso em operações de leitura e gravação
- Stored values integration: verifica se todos os valores são válidos e estão no formato esperado

>[!IMPORTANT]
> 
>Se uma das verificações falhar, todos os valores no armazenamento serão apagados e o usuário será desconectado, o que poderá causar uma experiência de usuário ruim. Use as verificações de integridade de armazenamento somente quando necessário.


## Comportamento padrão {#Default}

As verificações de integridade de armazenamento são desativadas por padrão ao inicializar o AccessEnabler usando o método de inicialização padrão:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Para especificar explicitamente quais verificações de integridade de armazenamento devem ser executadas na inicialização do AccessEnabler, use o seguinte método de inicialização:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

A enumeração IntegrityCheckType é exposta ao aplicativo cliente e tem os seguintes valores:

| Valor | Verificações realizadas | Armazenamento limpo | Descrição | Caso de uso recomendado |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Nenhum | Nunca | Nenhuma verificação de integridade é realizada na inicialização do armazenamento | Quando os fluxos do SDK estão funcionando como esperado |
| INTEGRITY_CHECK_ALL | Operabilidade de armazenamento <br/> Validade dos valores armazenados | Na falha da verificação | Todas as verificações de integridade disponíveis são executadas na inicialização do armazenamento | Quando há suspeita de corrupção do armazenamento do SDK. <br/> Se alguma das verificações de integridade falhar, o usuário será desconectado |
| INTEGRITY_CHECK_CLEAR | Nenhum | Sempre | O armazenamento é limpo na inicialização do armazenamento | Quando os fluxos do SDK não podem ser concluídos conforme esperado |
