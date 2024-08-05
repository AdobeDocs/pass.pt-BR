---
title: Cabeçalho - X-Device-Info
description: REST API V2 - Cabeçalho - X-Device-Info
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 3%

---


# Cabeçalho - X-Device-Info {#header-x-device-info}

## Visão geral {#overview}

O cabeçalho de solicitação <b>X-Device-Info</b> contém as informações do cliente (dispositivo, conexão e aplicativo) relacionadas ao dispositivo de streaming real.

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;informações_do_dispositivo&gt;</td>
   </tr>
   <tr>
      <td>Tipo de cabeçalho</td>
      <td>Cabeçalho da solicitação</td>
   </tr>
   <tr>
      <td>Padrão</td>
      <td>Não</td>
   </tr>
</table>

## Diretivas {#directives}

<b>&lt;informações_do_dispositivo></b>

O valor `Base64-encoded` do elemento JSON que contém pelo menos os atributos marcados como exigidos pela tabela a seguir.

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Chave</th>
        <th style="background-color: #EFF2F7;">Descrição</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Presença</th>
        <th style="background-color: #EFF2F7;">Valores possíveis</th>
    </tr>
    <tr>
        <td>primaryHardwareType</td>
        <td>O tipo de hardware principal do dispositivo.</td>
        <td></td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Câmera</li>
                <li>DataCollectionTerminal</li>
                <li>Computador desktop</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Óculos</li>
                <li>MediaPlayer</li>
                <li>Celular</li>
                <li>TerminalDePagamento</li>
                <li>PluginModem</li>
                <li>DefinirCaixaSuperior</li>
                <li>TV</li>
                <li>Tablet</li>
                <li>Ponto de acesso sem fio</li>
                <li>Relógio de pulso</li>
                <li>Desconhecido</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>modelo</td>
        <td>O nome do modelo do dispositivo.</td>
        <td><i>obrigatório</i></td>
        <td>Por exemplo, iPhone, SM-G930V, Apple TV etc.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>A versão do dispositivo.</td>
        <td></td>
        <td>Por exemplo, 2.0.1, etc.</td>
    </tr>
    <tr>
        <td>fabricante</td>
        <td>A empresa/organização de fabricação do dispositivo.</td>
        <td></td>
        <td>Por exemplo, Samsung, LG, ZTE, Huawei, Motorola, Apple, etc.</td>
    </tr>
    <tr>
        <td>fornecedor</td>
        <td>A empresa/organização de venda do dispositivo.</td>
        <td></td>
        <td>Por exemplo, Apple, Samsung, LG, Google, etc.</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>O nome do sistema operacional do dispositivo.</td>
        <td><i>obrigatório</i></td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Android</li>
                <li>SO CHROME</li>
                <li>Linux</li>
                <li>Mac OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osFamily</td>
        <td>O nome do grupo do Sistema Operacional (SO) do dispositivo.</td>
        <td></td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVendor</td>
        <td>O fornecedor do sistema operacional do dispositivo.</td>
        <td></td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Projeto Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVersion</td>
        <td>A versão do sistema operacional do dispositivo.</td>
        <td></td>
        <td>Por exemplo, 10.2, 9.0.1, etc.</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>O nome do navegador.</td>
        <td></td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Navegador Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Navegador Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVendor</td>
        <td>A empresa/organização de construção do navegador.</td>
        <td></td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVersion</td>
        <td>A versão do navegador do dispositivo.</td>
        <td></td>
        <td>ex: 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>O agente do usuário do dispositivo.</td>
        <td></td>
        <td>por exemplo, Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, como Gecko) Versão/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>A largura da tela física do dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>A altura da tela física do dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>A densidade de pixels da tela física do dispositivo.</td>
        <td></td>
        <td>por exemplo, 294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>A dimensão diagonal da tela física do dispositivo em polegadas.</td>
        <td></td>
        <td>Por exemplo, 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>O IP do dispositivo usado para enviar solicitações HTTP.</td>
        <td></td>
        <td>Por exemplo, 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>A porta do dispositivo usada para enviar solicitações HTTP.</td>
        <td></td>
        <td>por exemplo, 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>O tipo de conexão de rede.</td>
        <td></td>
        <td>por exemplo, WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>O status de segurança da conexão de rede.</td>
        <td></td>
        <td>
            Os valores são restritos:
            <ul>
                <li>true - no caso de uma rede segura</li>
                <li>false - no caso de um ponto de acesso público</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>O identificador exclusivo do aplicativo.</td>
        <td></td>
        <td>por exemplo, CNN</td>
    </tr>
</table>


## Exemplos {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```
