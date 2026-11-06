---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para atualizar os cards de jogos, revisar a mudança no navegador encaminhado e mesclá-la no primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou a interface de linha de comando (CLI) do GitHub Copilot, faça sua primeira mudança no projeto. Os dados dos jogos já incluem avaliações por estrelas, mas os cards da página inicial não as exibem. Peça ao agente para mostrar as avaliações, revise o resultado e mescle-o no primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma ramificação de recurso;
- pedir ao agente uma pequena mudança no projeto;
- revisar as alterações com `/diff`;
- executar o aplicativo e confirmar a mudança no navegador encaminhado;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial exibem somente título, categoria, editora e descrição. Como primeiro exercício, peça ao agente para mostrar nos cards a avaliação existente — uma mudança pequena e independente, ideal para iniciar sua sessão.

## Anatomia de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a ramificação atualmente abertas no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a discussão; os arquivos e a ramificação continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá seus prompts e as respostas do agente, a atividade das ferramentas enquanto o agente explora e edita arquivos e as mudanças que pode examinar com `/diff`.

## Inicie uma conversa e solicite a mudança

Inicie uma conversa nova para começar a implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Se necessário, inicie o Copilot:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Envie este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Assim, você terá adicionado um recurso com o Copilot CLI.

## Revise o diff

Revise todas as mudanças geradas por inteligência artificial antes de mesclá-las.

1. Digite `/diff` e examine cada arquivo alterado.
2. Confirme que o card mostra a avaliação numérica e exibe `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações e peça ao Copilot para corrigir as falhas.
5. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como toda ferramenta de inteligência artificial generativa, o Copilot é probabilístico, não determinístico. O código pode variar. Revise o comportamento, não espere uma implementação idêntica.

## Verifique as mudanças

Peça ao Copilot para iniciar o site e examine a interface atualizada no navegador encaminhado pelo Codespaces.

1. Envie este prompt:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça para parar o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Agora que o recurso está pronto, crie um pull request (PR) para mesclar o código no projeto.

1. Peça ao agente padrão para fazer commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar o pull request e exibirá a URL.
3. Abra o pull request mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado enquanto seleciona a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua cópia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Você entregou sua primeira mudança usando o GitHub Copilot CLI. Você:

- iniciou uma conversa focada do Copilot em uma ramificação de recurso;
- pediu ao agente uma pequena mudança no projeto;
- revisou as alterações com `/diff`;
- executou o aplicativo e confirmou a mudança no navegador encaminhado;
- abriu e mesclou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
