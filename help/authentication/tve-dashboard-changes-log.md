---
title: Log de alterações
description: Saber como um administrador pode monitorar as alterações de configuração no Painel TVE.
source-git-commit: 06c2e1e54515a2ec47722ba1f360467dadd1f73b
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# Log de alterações {#changes-log}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A variável **Registro de alterações** do Painel TVE permite visualizar as alterações de configuração enviadas para o ambiente de autenticação do Adobe Pass por meio do Painel TVE. Você também pode comparar duas alterações de configuração diferentes.

A variável **Registro de alterações** no painel esquerdo exibe uma lista de todas as alterações de configuração feitas por meio de uma conta específica do Painel TVE. Esta lista de alterações contém os seguintes detalhes:

* **Alterar descrição**: uma breve descrição sobre o escopo da alteração de configuração.
* **Enviado por**: uma ID de email do usuário responsável por fazer a alteração.
* **Data de push**: a data da alteração da configuração.
* **Status de push**: indica se a operação de push foi bem-sucedida, pendente ou falhou.

## Comparar alterações {#compare-changes}

Para comparar alterações, siga estas etapas:

1. Selecione duas alterações de configuração na lista que você deseja comparar.

   ![Comparar alterações de configuração](assets/select-changes.png)

   *Comparar alterações de configuração*

1. Selecionar **Comparar** no canto superior direito da tela.

   A variável **Alterações de configuração** exibe o tipo de entidade, a ID da entidade, a propriedade e o status da operação de alteração para cada alteração.

1. Passe o mouse sobre a alteração de configuração que deseja visualizar.
1. Selecionar **Exibir** para acessar os valores alterados.

   ![Exibir alterações de configuração](assets/view-changes.png)

   *Exibir alterações de configuração*

Veja a seguir um exemplo de uma alteração feita na configuração selecionada. Você pode visualizar a diferença entre os valores antigos e novos dentro da alteração.

![Valor antigo e novo](assets/change.png)

*Valor antigo e novo*


