---
title: Endpoints de API
description: Lista completa das APIs de monitoramento de simultaneidade
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
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
