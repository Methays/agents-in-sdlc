---
title: "Lição 8 - Criar e mesclar o pull request do recurso"
description: "Revise a filtragem, as instruções, a habilidade, o perfil de garantia de qualidade (QA) e os testes; depois crie um pull request e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações de instruções, a atualização da habilidade, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma ramificação. Agora, revise o conjunto e abra um pull request. Você mesclou manualmente o pull request (PR) de avaliação por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, o recurso, a atualização das instruções, a habilidade e o agente de QA seriam divididos em alguns pull requests. Para simplificar o workshop, todo o trabalho da filtragem e do fluxo de qualidade ficou em uma conversa e ramificação e será enviado em um único pull request.

Nesta lição, você vai:

- conhecer o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- inspecionar as mudanças completas e as evidências de verificação;
- criar o pull request da filtragem;
- ativar o Agent Merge somente depois da revisão e confirmar a mesclagem.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do pull request sem retirar do desenvolvedor a autorização para mesclar.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para concluir um pull request. Depois de ativado, o Copilot resolve o que estiver bloqueando o PR — como verificações de integração contínua (CI) com falha, comentários de revisão e necessidade de rebase — e habilita a mesclagem automática do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. Revise o trabalho e confirme que ele está pronto antes de ativar o Agent Merge.

## Use o Agent Merge para gerenciar o pull request

Com todo o código criado, revise o conjunto, abra o pull request e deixe o Agent Merge gerenciar as próximas etapas.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Na lista, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um pull request com `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do pull request e fará a mesclagem quando tudo for concluído com êxito.
7. Confirme se o Copilot exibiu uma mensagem semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de ramificações, filas de mesclagem, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou partes do processo de desenvolvimento: geração de código, testes e validação e, agora, o fluxo do pull request. Nesta lição, você:

- conheceu o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- inspecionou as mudanças completas e as evidências de verificação;
- criou o pull request da filtragem;
- ativou o Agent Merge somente depois da revisão e confirmou a mesclagem.

Em seguida, [explore outros comandos de barra do Copilot CLI][next-lesson] para gerenciar o contexto e os modelos, compartilhar sessões e delegar tarefas opcionalmente ao agente na nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
