---
title: Metadados personalizados
description: Metadados personalizados
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---



# Metadados personalizados {#cm}

>[!NOTE]
>
> Esta página está obsoleta, pois se aplica à versão anterior da API do que não é mais recomendada para novas integrações

O serviço permite que os clientes usem campos padrão e personalizados tanto em consultas quanto em tomadas de decisão. Os campos padrão estão sempre disponíveis para qualquer fluxo (pois são necessários para a criação de fluxos ou gerados pelo servidor):

* aplicativo
* mvpd
* accountId
* streamId
* streamStart
* iniciador


Os campos personalizados são quaisquer pares de chave/valor transmitidos na inicialização do fluxo, como parâmetros de formulário ou de sequência de consulta. Os campos padrão e personalizado podem ser usados nos seguintes cenários (para obter detalhes, consulte a documentação real dos recursos de API envolvidos abaixo):

* Solicitação de campos extras (por meio do parâmetro da cadeia de caracteres de consulta dos campos) ao buscar a lista de fluxos de uma conta (o recurso /streams)
* Analisar a atividade da conta especificando as dimensões pelas quais agrupar (o recurso /activity)
* Definir políticas do lado do servidor com base em valores de campo ou cardinalidades (os exemplos usam pseudo-SQL apenas para esclarecer):
* Configurar uma política para aplicar somente a valores de campo específicos (por exemplo, uma política dedicada do iOS: WHERE osType IS &#39;iOS&#39;)
* Limitar o número de valores distintos para um determinado campo (por exemplo, não mais do que X dispositivos distintos: HAVING DISTINCT COUNT(deviceId) >= 2)
* Limitar o número de fluxos ativos por valor de campo (por exemplo, não mais do que X fluxos ativos para um único tipo de dispositivo: GROUP BY deviceType HAVING COUNT(streamId) >= 3)


Com base nesses valores/chaves que estão sendo enviados, regras diferentes podem ser estabelecidas. Pode ser algo assim:

1. O cliente decide que gostaria de enviar o grupo de parâmetros, que terá como valores &quot;SPORTS&quot; e &quot;KIDS&quot;.
1. Em seguida, o aplicativo precisaria fazer isso:
   * Para canais esportivos, na inicialização do stream, o aplicativo seria enviado ***type=ESPORTES*** como parâmetro de consulta
   * Para canais com conteúdo relacionado a crianças, na inicialização do fluxo, o aplicativo seria enviado ***type=KIDS*** como parâmetro de consulta
1. Assim, uma política como essa poderia ser definida:
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. Isso basicamente significa que, quando um usuário está assistindo a esportes, ele não pode fazer isso em mais de um dispositivo. No entanto, quando o usuário assiste a conteúdo infantil, a visualização é permitida em no máximo três dispositivos.

