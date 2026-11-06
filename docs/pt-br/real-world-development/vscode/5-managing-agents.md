---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe mudanças, edita arquivos e executa comandos. Como o trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições. Você pode revisar cada diff, aceitar ou rejeitar mudanças individuais e orientar a conversa com novos prompts.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições propostas;
- orientar o agente durante a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design pediu também um modo claro para usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa, oriente a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A exibição Copilot Chat mostra as etapas do agente, incluindo os arquivos examinados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas, leituras e edições de arquivos e os comandos do terminal solicitados.
3. Quando uma edição for proposta, use o diff:
   - Selecione **Keep** para aceitar a mudança.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Abra **Files changed** na parte superior do chat para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando do terminal, o agente aguardará e exibirá **Allow**. Aprove somente comandos com os quais você se sinta confortável.

5. Aguarde o agente terminar o modo de alto contraste. Ele exibirá uma mensagem final resumindo as mudanças.

## Oriente o agente para adicionar o modo claro

Continue a mesma conversa para adicionar um controle do modo claro. O agente manterá o contexto dos componentes, da abordagem de estilo e do padrão de armazenamento local usados anteriormente.

1. No campo de entrada do Copilot Chat, envie este prompt:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as mudanças anteriores e proporá as alterações para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de modo, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma instrução de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts de acompanhamento para pedir ajustes (por exemplo, "simplifique as folhas de estilo em cascata (CSS) — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo (por exemplo, "não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo (por exemplo, "adicione atalhos de teclado para os dois controles").

## Revise o conjunto de mudanças

Antes de fazer commit, revise o trabalho do agente.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade do agente estão presentes: atributos de Aplicações de Internet Ricas e Acessíveis (ARIA), navegação por teclado, linguagem de marcação de hipertexto (HTML) semântica e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das mudanças pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento, revisando cada edição, aceitando ou rejeitando mudanças e enviando prompts de acompanhamento sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real;
- aceitou ou rejeitou edições individuais;
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], publique seu trabalho de acessibilidade em um pull request e revise as mudanças que o agente personalizado local produziu durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
