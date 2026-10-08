---
title: Endpoints de API
description: Lista completa das APIs de monitoramento de simultaneidade
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# Endpoints de API

## Gerenciamento de sessão principal

| Endpoint | Método | Descrição |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | Criar uma nova sessão de streaming |
| `/sessions/{idp}/{subject}/{session}` | POST | Enviar pulsação para manter a sessão ativa |
| `/sessions/{idp}/{subject}/{session}` | DELETE | Encerrar uma sessão |
| `/runningStreams/{idp}/{subject}` | GET | Obter todas as sessões ativas para um assunto |

## Gerenciamento de metadados

| Endpoint | Método | Descrição |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | Obter campos de metadados necessários para o aplicativo |
