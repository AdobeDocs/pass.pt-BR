---
title: Notas de versão do Adobe Pass Concurrency Monitoring 2.6.0
description: Notas de versão do Adobe Pass Concurrency Monitoring 2.6.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Notas de versão do Adobe Pass Concurrency Monitoring 2.6.0 {#cm-260}


Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:



## Data de lançamento: 10/11/2016



## Novos recursos

Essa versão adiciona a capacidade de encerrar fluxos existentes para permitir que o fluxo atual seja iniciado (também conhecido como kill stream).



**Término remoto**

* Em uma resposta de Conflito 409, cada sessão listada no campo &quot;conflitos&quot; da dica terá um atributo termincode.
* O usuário pode ser avisado com a lista de sessões conflitantes e ter permissão para escolher as que deseja eliminar
* As sessões remotas só podem ser eliminadas transmitindo um cabeçalho de solicitação X-Terminate (com os códigos de encerramento selecionados como valores) em uma tentativa de inicialização de sessão.
* Um novo tipo de &quot;aviso&quot; foi definido para a resposta 410 Ida para indicar a sessão que eliminou a atual.


Consulte a documentação atualizada para obter mais detalhes.



>[!NOTE]
>
>A definição de sessão usada para listar sessões ativas foi atualizada para incluir o nome do aplicativo e o nome do dispositivo (em vez da ID do aplicativo).




## Correções de erros {#bug-fixes}

Os cabeçalhos duplicados foram removidos na resposta do servidor (a correção envolve os cabeçalhos CORS e o cabeçalho Data).




## Problemas conhecidos {#known-issues}

N/D
