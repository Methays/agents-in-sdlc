---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

WCAG é a sigla em inglês para as Diretrizes de Acessibilidade para Conteúdo Web.

[Agentes personalizados][custom-agents-concept] no GitHub Copilot são assistentes de inteligência artificial especializados em tarefas ou áreas específicas do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório podem fornecer instruções, práticas recomendadas, padrões de código e conhecimento do domínio. As equipes podem codificar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem as diretrizes [WCAG][wcag], agentes de segurança ou agentes de testes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e habilidades de agente

Agentes personalizados e [habilidades de agente][agent-skills-concept] têm algumas semelhanças: ambos usam principalmente arquivos Markdown para orientar tarefas. Uma distinção útil é que o **agente personalizado** é quem executa o trabalho, enquanto as **habilidades** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar habilidades e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar habilidades, por exemplo, para gerenciar pull requests ou executar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em outros usos de inteligência artificial, teste e faça iterações para encontrar o que funciona em seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a pessoas com diferentes capacidades e preferências visuais. Comentários de usuários indicaram que o tema escuro atual é difícil de ler devido ao contraste insuficiente entre texto e fundo. A equipe de design pediu um modo de alto contraste que os usuários possam ativar e desativar. Use um agente personalizado para implementar o recurso com rapidez.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar esse agente no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente de acessibilidade já foi criado. Examine o arquivo para entender como ele orientará o Copilot.

1. Volte ao codespace, abra um terminal e crie uma ramificação nova a partir de `main` para manter o trabalho de acessibilidade separado do pull request de filtragem do Exercício 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Examine as seções seguintes, que destacam responsabilidades principais na geração de código acessível, práticas recomendadas, exemplos da linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript, além de erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code exibe no seletor de agentes do Copilot Chat os agentes definidos em `.github/agents`. Selecione um agente para aplicar suas instruções e ferramentas à conversa.

> [!TIP]
> **Abra o Copilot Chat**
>
> Volte ao codespace, abra o Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar.

1. No seletor de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior da exibição Chat. Por padrão, o agente ativo é **default**.
3. Selecione **Accessibility agent** na lista.
4. Envie ao agente de acessibilidade este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará o código, proporá mudanças e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho e um diff para revisão.
6. Observe o indicador **Files changed**, que atualiza a lista de arquivos alterados.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot altera arquivos do projeto, como componentes Astro, CSS e testes relacionados. Você acompanhará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Você explorou [agentes personalizados][custom-agents], assistentes especializados que permitem codificar conhecimentos e padrões da equipe em orientações reutilizáveis. Neste exercício, você:

- revisou como agentes personalizados são definidos;
- usou um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as mudanças enquanto são feitas e solicite um controle para o modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
