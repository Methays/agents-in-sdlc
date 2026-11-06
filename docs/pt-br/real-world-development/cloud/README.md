---
slug: pt-br/real-world-development/cloud
title: "Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui uma tarefa no GitHub, e o agente na nuvem a executa em segundo plano: explora o repositório, faz mudanças e abre um pull request enquanto você trabalha em outras atividades.

Nos exercícios, você adicionará instruções personalizadas para orientar o agente na nuvem e, em seguida, atribuirá uma issue do GitHub para que ele implemente o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, acompanhará e direcionará sessões no painel de agentes e, por fim, revisará os pull requests e fará iterações no resultado.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione as instruções personalizadas que o agente na nuvem seguirá |
| [2. Agente na nuvem][ex2] | Agente assíncrono | Atribua issues ao agente na nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Acompanhe e oriente sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revise pull requests, faça iterações no trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes do workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente na nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem usar o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui agente, Model Context Protocol (MCP), revisão de código e Copilot CLI (interface de linha de comando), recursos usados neste workshop. Com ele, você pode concluir todos os ambientes.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente na nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise**, mas não no Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitar o recurso para a organização.

> [!NOTE]
> Este ambiente não aborda MCP. Para aprender a usar servidores MCP com o Copilot, consulte o [ambiente de CLI][cli] ou o [ambiente do VS Code][vscode].

## Comece

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
