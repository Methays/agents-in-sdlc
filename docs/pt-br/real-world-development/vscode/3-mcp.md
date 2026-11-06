---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ele funciona no navegador. Configure o **servidor MCP do Playwright** para que o Copilot controle um navegador real, teste o recurso e, em seguida, publique sua ramificação e abra um pull request.

Neste exercício, você vai:

- entender o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot;
- adicionar o servidor MCP do Playwright ao workspace;
- pedir ao Copilot para testar manualmente a filtragem no navegador;
- publicar a ramificação e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente pode usar ferramentas externas por meio do Model Context Protocol (MCP), um padrão que permite ao agente se comunicar com serviços e ferramentas externas.

![Diagrama do funcionamento do modo de agente e da interação com contexto, um modelo de linguagem grande e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de inteligência artificial se comuniquem em tempo real com ferramentas e serviços externos. Assim, eles podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

Um servidor MCP faz a ponte entre o agente e as ferramentas ou serviços externos. Ele gerencia a comunicação com serviços existentes, como APIs, ou ferramentas locais, como pacotes npm. Cada servidor fornece um conjunto próprio de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright, incluindo navegação, preenchimento de formulários e seleção de botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e avalie as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações sensíveis.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Observe que o servidor `github` já está configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

A entrada `github` vem do modelo do projeto. Por isso, o Copilot pôde consultar o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não é necessário instalar nada localmente. O VS Code conecta-se por HTTP, e você autentica no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicione o servidor MCP do Playwright

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, deixando o arquivo assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o `playwright` é local: o VS Code inicia o processo com `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário em um codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright nos testes de ponta a ponta, portanto o navegador normalmente já está instalado. Se o Copilot informar que falta um navegador, peça para executar `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP sob demanda. Na primeira vez que o Copilot precisar das ferramentas, o VS Code iniciará o servidor e pedirá que você confirme se confia nele.

1. Em `.vscode/mcp.json`, selecione **Start**, que aparece acima da entrada `playwright`, para iniciar o servidor e confirmar a configuração.
2. Se o VS Code pedir que você confirme a confiança no servidor, revise a configuração e escolha confiar nele.

Iniciar o servidor manualmente é opcional. Se você não fizer isso, o VS Code o iniciará e pedirá confirmação quando o Copilot precisar das ferramentas na etapa seguinte.

## Teste o recurso de filtragem

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para exercitar a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O servidor MCP do Playwright abrirá um navegador, seguirá as etapas e apresentará um relatório. Compare o resultado com os critérios de aceitação da issue. Se algo estiver errado, faça perguntas ou peça para corrigir o código antes de abrir o pull request.

O servidor trabalha com a [árvore de acessibilidade][playwright-mcp-server] da página, não com capturas de tela. Assim, o agente usa elementos estruturados e rotulados, como botões, links e itens de lista, de forma semelhante às tecnologias assistivas. Um teste funcional também oferece uma verificação básica de acessibilidade.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento tiver sido parado, inicie-o antes de enviar o prompt. Na primeira utilização do servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um, peça para executar `npx playwright install chromium` e tente novamente.

## Publique a ramificação e crie um pull request

Depois de confirmar que o recurso funciona, publique a ramificação `filtering-vscode` para que a equipe possa revisá-la.

1. Abra o painel **Source Control** no codespace e revise as mudanças feitas pelo Copilot.
2. Prepare as mudanças selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as mudanças feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a ramificação ao repositório.

## Crie o pull request

Você pode criar um pull request pelo GitHub.com, pela interface ou pela interface de linha de comando do GitHub (CLI). Neste exercício, peça ao Copilot para encontrar a issue correspondente e associá-la ao pull request.

1. Abra o painel Copilot Chat e selecione **New Chat**.
2. No seletor de agentes, confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot para criar um pull request:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar-se; siga as instruções.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o pull request.
6. Abra o link criado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Neste exercício, você:

- conheceu o MCP e como servidores MCP ampliam as ferramentas do Copilot;
- adicionou o servidor MCP do Playwright ao workspace;
- usou o servidor para testar o recurso no navegador antes de publicá-lo;
- publicou a ramificação e abriu um pull request.

Você usou um servidor MCP para testar um recurso. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
