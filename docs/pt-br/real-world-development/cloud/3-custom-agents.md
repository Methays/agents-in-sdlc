---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

WCAG é a sigla em inglês para as Diretrizes de Acessibilidade para Conteúdo Web.

[Agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de inteligência artificial especializados em tarefas ou áreas do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório podem fornecer instruções, práticas recomendadas, padrões de código e conhecimento do domínio. As equipes podem codificar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], de segurança ou de testes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e habilidades de agente

Agentes personalizados e [habilidades de agente][agent-skills-concept] se sobrepõem em alguns aspectos: ambos são definidos principalmente em arquivos Markdown e orientam o trabalho de uma inteligência artificial. Uma distinção útil é que o **agente personalizado** é quem executa o trabalho, enquanto as **habilidades** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar habilidades e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar habilidades de fluxo de pull requests ou de execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em outros usos de inteligência artificial, teste e faça iterações para descobrir o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todos, independentemente da capacidade ou das preferências visuais. Comentários de usuários indicaram que o tema escuro atual é difícil de ler devido ao contraste insuficiente entre texto e fundo. A equipe de design pediu um modo de alto contraste que os usuários possam ativar e desativar. Como a acessibilidade é importante, use um agente personalizado para implementar o recurso com rapidez.

Neste exercício, você vai:

- entender como agentes personalizados são definidos;
- atribuir uma tarefa a um agente personalizado.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Examine-o para entender como orientará o Copilot.

1. Volte ao codespace e abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções que descrevem responsabilidades para gerar código acessível, práticas recomendadas, exemplos da linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript, além de erros comuns.

## Crie e atribua uma issue

A página de agentes é o local central para trabalhar com agentes no ambiente. Você pode atribuir tarefas ao agente na nuvem do Copilot, acompanhar tarefas e orientar sessões. Atribua ao agente personalizado a implementação de um modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
9. Na lista de agentes personalizados, selecione **Accessibility agent**.

   ![Atribuição do agente na nuvem com o agente de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Você explorou [agentes personalizados][custom-agents], assistentes especializados em tarefas e áreas específicas que permitem codificar conhecimentos e padrões da equipe em orientações reutilizáveis. Neste exercício, você:

- examinou como agentes personalizados são definidos;
- atribuiu uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [acompanhe e oriente a sessão do agente][next-lesson] na página de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente na nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
