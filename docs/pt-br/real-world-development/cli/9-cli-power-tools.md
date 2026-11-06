---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como outras ferramentas de linha de comando, o GitHub Copilot na interface de linha de comando (CLI) oferece vários comandos de barra. Eles dão acesso a recursos avançados, informações internas e opções de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Agora, explore alguns outros.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar o gerenciamento do contexto;
- usar `/model` para examinar os modelos disponíveis;
- aprender como `/share` exporta ou compartilha uma sessão;
- explorar comandos opcionais de trabalho paralelo, worktrees e delegação ao agente na nuvem.

## Cenário

Você concluiu o fluxo principal do CLI. Agora, explore o gerenciamento de contexto, a seleção de modelos, o compartilhamento de sessões e a delegação opcional ao [agente na nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Em tarefas maiores, você pode atingir o limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário; os comandos de barra permitem inspecionar ou compactar o contexto manualmente.

1. Volte ao Codespace e, se necessário, inicie o Copilot CLI na raiz do repositório.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. Se a conversa já for pequena, talvez a diferença não seja grande.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` para escolher o momento. Use `/clear` ou `/new` ao começar uma tarefa não relacionada e quiser uma conversa nova.

## Escolha um modelo

Modelos diferentes têm pontos fortes diferentes. O Copilot CLI permite listar e selecionar o modelo.

1. Digite:

   ```plaintext
   /model
   ```

2. Examine os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todos a usar melhor as ferramentas de inteligência artificial. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um gist do GitHub.

1. Digite `/help` e examine as opções de `/share` na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino apropriado, como `/share file` para exportá-la como arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretenda compartilhar.

## Opcional: expanda o trabalho ou delegue

O fluxo principal do workshop está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` divide tarefas independentes entre subagentes e pode executá-las em paralelo.
- `/worktree` cria um worktree do Git isolado para outra tarefa.
- `/delegate` envia uma tarefa ao agente na nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Antes de usá-los, comece uma tarefa nova e bem delimitada. Para saber mais sobre trabalho assíncrono, continue no [workshop do agente na nuvem][cloud-workshop].

## Resumo e próximos passos

Você usou comandos de barra para configurar o CLI, compartilhar sessões e inspecionar o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar o gerenciamento do contexto;
- usou `/model` para examinar os modelos disponíveis;
- aprendeu como `/share` exporta ou compartilha uma sessão;
- explorou comandos opcionais de trabalho paralelo, worktrees e delegação ao agente na nuvem.

Há mais comandos de barra para explorar. Conclua o fluxo [revisando o que aprendeu][next-lesson] e os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente na nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
