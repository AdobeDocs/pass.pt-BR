---
title: Exportar informações para contas com alta pontuação de compartilhamento
description: Exportar informações para contas com alta pontuação de compartilhamento.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Exportar informações para contas com alta pontuação de compartilhamento {#export-account-info-high-score}

[!UICONTROL Account IQ] permite exportar detalhes de compartilhamento de conta para 1000 contas de assinantes principais com base em suas [compartilhando probabilidades](/help/accountiq/product-concepts.md#account-sharing-probability-def). É possível exportar as informações de compartilhamento de conta da conta atual [segmento](/help/accountiq/product-concepts.md#segment-def) e [intervalo de tempo especificado](/help/accountiq/product-concepts.md#time-interval-def) no [Relatórios de Contas Compartilhadas](/help/accountiq/shared-acc-reports.md) página.

Siga as etapas para exportar as informações de compartilhamento de conta de contas de assinantes para um segmento específico.

1. Faça logon com suas credenciais.
1. Navegue até a **Contas compartilhadas** em **Relatórios** seção.
1. Selecione o segmento e o intervalo de tempo necessários no painel Segmento e intervalo de tempo. Saiba mais [como selecionar um segmento e um intervalo](segments-timeinterval.md).

   Se necessário, consulte as instruções de [criação de um segmento](work-with-segments.md#create-new-segment) ou [editar um segmento](work-with-segments.md#edit-segment).

1. Selecionar **[!UICONTROL Export top 1000 accounts]** localizado no canto superior direito do painel segmento e intervalo de tempo.

   ![Exportar as 1000 contas principais](assets/export-top-1000-accounts.png)

   *Selecione a opção Exportar as 1000 contas principais*

O arquivo será baixado automaticamente para o computador local como um .csv.

Esse arquivo contém os dados para as 1.000 contas principais com base nas probabilidades de compartilhamento das contas do assinante no segmento atual em ordem decrescente.

Veja a seguir um exemplo do arquivo .csv exportado.

![dados exportados em arquivo .csv](assets/exported-csv.png)

*Dados exportados em arquivo .csv*

## Colunas no relatório exportado {#columns-in-export}

**Semana/Mês**

A semana ou mês selecionado no **[!UICONTROL Granularity and Time Interval]** no seletor de segmentos.

**MVPD**

Se você for um programador, a coluna mostrará o distribuidor com o qual a conta está inscrita.

>[!NOTE]
>
> A variável **MVPD** A coluna só está disponível para versões do TV Everywhere.

**ID do Assinante**

O identificador exclusivo da conta específica.

**Nº Mínimo de Dispositivos**

O número mínimo de dispositivos dos quais os usuários fazem streaming de conteúdo ativamente.

>[!NOTE]
>
>O número real de dispositivos cujo conteúdo é transmitido é maior que o número mínimo de dispositivos especificados para uma determinada conta.

**Nº Mínimo de Pessoas**

O número mínimo de indivíduos que transmitiram conteúdo ativamente usando esses dispositivos.

>[!NOTE]
>
>O número real de pessoas que transmitem conteúdo é maior que o número mínimo de pessoas atribuídas a uma conta específica.

**[!UICONTROL # IPs]**

O número de endereços IP a partir dos quais o conteúdo é transmitido.

**[!UICONTROL # Locations]**

O número de localizações (com base no código postal) a partir das quais o conteúdo é transmitido.

**[!UICONTROL # Cities]**

O número de cidades onde a atividade de streaming ocorreu.

**[!UICONTROL # States]**

O número de estados em que a atividade de streaming ocorreu.

**[!UICONTROL # Clusters]**

O número de [clusters](/help/accountiq/product-concepts.md#cluster-def) onde ocorreu a transmissão.

**[!UICONTROL Geographic span (miles)]**

A distância máxima entre os locais de streaming associados à conta.

**[!UICONTROL # AuthN OK]**

O número de logons que os usuários fazem durante o período especificado usando essa conta.

>[!NOTE]
>
> Alguns serviços D2C podem não ver **[!UICONTROL # AuthN OK]** dados, pois eles podem não ser incluídos nos dados de sua empresa.

**[!UICONTROL # AuthZ OK]**

O número de vezes que um MVPD autorizou um fluxo ou concedeu acesso ao conteúdo para essa conta.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** não está disponível para serviços D2C.

>[!NOTE]
>
>Para TV em todos os lugares, **[!UICONTROL # AuthZ OK]** está correlacionado com o número de **[Nº de Solicitações Play](/help/accountiq/product-concepts.md##play-requests-def)**. Sempre será menor que **[!UICONTROL # Play Requests]** porque o Adobe normalmente armazena em cache as autorizações de MVPDs por aproximadamente 24 horas.


**[!UICONTROL # Play Requests]**

O número real de fluxos ocorridos durante um período especificado.

>[!NOTE]
>
>A variável [Nº de Solicitações Play](/help/accountiq/product-concepts.md##play-requests-def) A coluna não está disponível na versão MVPD do TV Everywhere.

**[!UICONTROL # Channels]**

O número geral de canais que a conta assistiu por um período especificado.

>[!NOTE]
>
> Para serviços D2C **[!UICONTROL # Channels]** é equivalente ao número de **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Para a TV Everywhere, eles incluem os canais que podem não pertencer ao programador conectado. Esse número para a conta inclui seu canal e outros canais acessados durante o período especificado.


**Padrão de uso**

Os valores nessas colunas servem como identificadores correspondentes a um dos 14 padrões que usamos para categorizar todas as contas de usuário.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Padrões de uso</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Usuário normal</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Viajante ou viajante</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Família grande</td>
      </tr>
      <tr>
        <td>4</td>
        <td>Fechar família e amigos</td>
      </tr>
      </tr>
         <td>5 e 8</td>
         <td>Compartilhamento em grupo social</td>
      </tr>
      </tr>
         <td>6</td>
         <td>Grande grupo de amigos</td>
      </tr>
      </tr>
         <td>7</td>
         <td>Transmissão simultânea</td>
      </tr>
      </tr>
         <td>9</td>
         <td>Compartilhamento de comunidade</td>
      </tr>
      </tr>
         <td>10 e 11</td>
         <td>Comportamento incerto</td>
      </tr>
      </tr>
         <td>12</td>
         <td>Família pequena</td>
      </tr>
      </tr>
         <td>13</td>
         <td>Segunda página inicial </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Uso anormal</td>
      </tr>
    </tbody>
  </table>

*Identificadores de padrão de uso no mapeamento .csv exportado com padrões de uso*

**Probabilidade de Compartilhamento**

A probabilidade de uma conta específica estar compartilhando suas credenciais.

>[!NOTE]
>
> A média da probabilidade de compartilhamento de todas as contas no segmento selecionado é usada para calcular a [nível de compartilhamento](/help/accountiq/data-panels.md#sharing-level) do [pontuação média de compartilhamento](/help/accountiq/data-panels.md#aggregated-sharing).
