---
title: "Lição 6 - Validar funcionalidades com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar o recurso de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já vimos, desenvolver software não se resume a escrever código. Também é preciso trabalhar com dados, serviços externos e automações adicionais disponíveis para o Copilot. Os servidores Model Context Protocol (MCP) ampliam as ferramentas e os serviços integrados à interface de linha de comando (CLI) do Copilot.

Nesta lição, você vai:

- entender o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível;
- pedir ao agente para controlar um navegador e explorar a filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar mudanças na interface exige interagir com ela. Com o servidor MCP do Playwright, o Copilot pode usar o site como uma pessoa usuária e automatizar a validação.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial se comuniquem com ferramentas e serviços externos. Isso possibilita acessar informações atualizadas e executar ações em seu nome.

Um servidor MCP faz a ponte entre o agente e as ferramentas e os serviços externos e fornece um conjunto de recursos e ferramentas. Alguns servidores conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios, issues e pull requests.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece automação de navegador com Playwright.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta e as contribuições.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência. Antes de usar um, revise o código-fonte, verifique o publicador e avalie as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore as etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo de adição de servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e depois <kbd>Tab</kbd>.
5. Na caixa de diálogo **Command**, digite:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo.

## Peça ao Copilot para explorar o recurso com o Playwright

Você já confirmou manualmente que o recurso funciona. Agora peça ao Copilot para fazer o mesmo usando o servidor MCP do Playwright.

1. Envie este prompt:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente, não é necessário dizer ao Copilot para usar o servidor MCP: ele costuma identificar a ferramenta. Nomeá-la no prompt pode tornar os resultados mais consistentes.

2. Acompanhe as etapas que o Copilot executa no navegador.
3. Leia o relatório e confirme se o comportamento está correto.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor MCP do Playwright para explorar o recurso em um navegador real a partir do Copilot CLI. Nesta lição, você:

- aprendeu o que é o MCP e como o Copilot CLI o usa;
- adicionou o servidor MCP do Playwright caso ele ainda não estivesse disponível;
- pediu ao agente para controlar um navegador e explorar a filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que combine a habilidade e as ferramentas do navegador.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
