---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Muitas organizações têm dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou recursos adiados no backlog por falta de tempo. O agente na nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir, ele abre um pull request de rascunho para revisão de um desenvolvedor. Assim, é possível delegar tarefas repetitivas e concentrar-se em atividades de maior impacto.

Neste exercício, você vai explorar:

- como personalizar o ambiente de geração de código;
- como garantir que as operações sejam feitas com segurança;
- a importância de issues bem delimitadas;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer reduzir a dívida técnica. O código inicial não tem documentação, então a equipe quer comentários de documentação de TypeScript (TSDoc) em todas as funções exportadas. Além disso, a equipe de design quer mostrar jogos relacionados na página de detalhes de cada jogo: outros títulos da mesma categoria que ajudem as pessoas a continuar navegando. Ainda não é necessário um acabamento visual; o recurso servirá para testes de aceitação da experiência. Como essa tarefa é um bloqueio, mas outras issues têm prioridade maior, o agente na nuvem pode trabalhar nela de forma assíncrona enquanto você executa outras tarefas.

## Conheça o agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano, de forma semelhante a um desenvolvedor. Você pode iniciar uma sessão, por exemplo, [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). O Copilot cria um pull request de rascunho para acompanhar o progresso, prepara o ambiente e inicia a tarefa. Você pode examinar a sessão durante ou depois da execução. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não elimina a necessidade de pensar. Até tarefas aparentemente simples podem ter complexidades. [Defina o escopo cuidadosamente][cloud-agent-best-practices] e trate o agente como um parceiro de programação com inteligência artificial: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com o uso de inteligência artificial generativa.

## Instruções personalizadas neste repositório

Antes de atribuir trabalho, examine as instruções já incluídas no repositório, para que você possa identificar seus efeitos depois.

1. Abra `.github/copilot-instructions.md` na interface Web do GitHub ou em um codespace. Revise **Code standards**, principalmente as convenções de TypeScript e os comentários TSDoc.
2. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation`, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos nos arquivos da alteração. Essas orientações vêm dos arquivos de instruções.

## Configurar o ambiente de desenvolvimento do agente na nuvem

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início da tarefa. O fluxo deste laboratório já foi configurado para instalar Node.js e as dependências do projeto, instalar navegadores para testes de ponta a ponta e preparar o banco de dados SQLite migrado e preenchido do único aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração da interface e do aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com outros fluxos de trabalho do GitHub, mas contém alguns pontos importantes:

- Há um único trabalho chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot iniciar o pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório para verificar sua execução, sem aguardar uma sessão do Copilot.

## Adicione documentação ao código

A falta de documentação é um tipo comum de dívida técnica. Atribua ao agente na nuvem uma issue para adicionar comentários TSDoc às funções exportadas.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. No campo **Title**, digite `Code lacks documentation`.
5. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. Selecione **Create**.
7. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

8. Selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)
9. Selecione a guia **Pull Requests** e abra o pull request criado, com título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde e atualize a página.
10. Aguarde o Copilot criar uma lista de tarefas. Isso pode levar alguns minutos enquanto ele configura o ambiente, analisa o projeto e define uma abordagem.
11. Revise a lista de tarefas e a linha do tempo do pull request. Observe a atualização que indica que o Copilot começou a trabalhar.
12. Selecione **View session** para acompanhar a sessão.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

13. Acompanhe a sessão, observando como o Copilot explora o código, avalia a situação, planeja e cria código.

O trabalho pode levar alguns minutos. Como o agente na nuvem trabalha de forma assíncrona, você pode atribuir outra tarefa enquanto ele trabalha.

## Adicione uma seção de jogos relacionados

Enquanto o agente trabalha na documentação, atribua outra issue para mostrar jogos relacionados na página de detalhes.

1. Volte ao repositório no github.com.
2. Selecione **Issues**, **New issue** e **Blank issue**.
3. No campo **Title**, digite `Show related games on the game details page`.
4. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. Selecione **Create**.
6. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

7. Selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

Em breve, um emoji de olhos 👀 aparecerá no primeiro comentário da issue, indicando que o Copilot está trabalhando. Como o agente na nuvem atua de forma semelhante a um desenvolvedor, você pode voltar para revisar o trabalho quando ele terminar. Enquanto isso, crie e use agentes personalizados.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

## Resumo e próximos passos

Você explorou o [agente na nuvem do GitHub Copilot][copilot-agents] e atribuiu issues para que ele trabalhe de forma assíncrona. O agente pode tratar dívida técnica, criar recursos ou ajudar a migrar código entre estruturas.

Neste exercício, você:

- personalizou o ambiente para gerar código;
- considerou como executar operações com segurança;
- explorou a importância de delimitar as issues;
- atribuiu issues ao Copilot.

Agora, crie e use agentes personalizados. O agente na nuvem também pode usar [servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e as instruções personalizadas vistas anteriormente.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
