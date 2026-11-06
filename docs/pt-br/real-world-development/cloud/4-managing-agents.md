---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente na nuvem do Copilot que executasse três tarefas para melhorar a experiência e adicionar recursos. Embora o agente trabalhe de forma autônoma e assíncrona, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] do GitHub.com. Nesse painel central, você pode ver tarefas com pull requests (PRs) abertos, examinar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente na nuvem;
- orientar uma sessão em andamento para solicitar outro recurso.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe decidiu adicionar também um modo claro. Como o agente já está trabalhando nos estilos e nos controles, oriente a sessão para incluir os dois modos.

## Revise as tarefas do agente na nuvem

Examine o estado das tarefas atribuídas ao agente na nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas, incluindo:
   - documentar o código;
   - gerar interfaces de programação de aplicações (APIs) para modificar jogos;
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, que podem incluir:
   - obter o código do repositório;
   - preparar o ambiente de trabalho;
   - configurar servidores do Model Context Protocol (MCP);
   - concluir várias etapas da tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem usada.

4. Observe também o painel de pull requests à direita, que mostra o pull request e os arquivos alterados.

## Oriente o agente na nuvem

Agora, peça ao agente para incluir o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à implementação do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no painel central](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a configuração seja concluída e o agente comece a trabalhar.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente na nuvem com a caixa de diálogo de orientação destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer a solicitação e incorporá-la ao fluxo de trabalho.

## Aguarde o trabalho do Copilot

O Copilot incorporará a nova solicitação depois de concluir a etapa em andamento. Isso pode levar alguns minutos; aproveite o tempo para rever o que já aprendeu.

## Resumo e próximos passos

Neste exercício, você:

- explorou a página de agentes para acompanhar as tarefas do agente na nuvem;
- orientou uma sessão em andamento para solicitar outro recurso.

Enquanto o Copilot conclui os recursos de acessibilidade, [revise e faça iterações nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
