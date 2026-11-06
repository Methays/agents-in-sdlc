---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para combinar uma abordagem, Autopilot para implementar a filtragem de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos com um recurso pequeno, mas mudanças maiores exigem um processo mais completo. A interface de linha de comando (CLI) do GitHub Copilot se integra ao fluxo de trabalho da organização. Nesta lição, você iniciará pelo uso de uma issue para definir e criar um recurso, validar o código e o comportamento e preparar a mesclagem.

> [!NOTE]
> Você continuará o fluxo de trabalho do recurso usando a mesma conversa e ramificação. Em geral, tarefas diferentes podem usar ramificações ou pull requests (PRs) distintos; neste workshop, o fluxo foi simplificado para enfatizar os conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub;
- definir requisitos no modo Plan;
- implementar o recurso com o modo Autopilot;
- revisar o código;
- validar manualmente o recurso em um navegador encaminhado.

Ao longo do trabalho, você também atualizará as instruções do repositório, personalizará a habilidade `quality-checks`, adicionará validação do Model Context Protocol (MCP), criará um agente de garantia de qualidade (QA) e abrirá o pull request do recurso.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar jogos por categoria e editora. A issue do backlog descreve o recurso, mas decisões como combinar categorias precisam ser definidas antes de escrever código. Use o modo Plan para combinar essas decisões e autorize uma implementação delimitada com Autopilot.

## Contexto

Os agentes de programação não mudam os fundamentos do desenvolvimento. Um fluxo comum é:

1. Começar com uma issue que descreva o trabalho.
2. Planejar o que será criado.
3. Implementar e revisar o código.
4. Executar testes.
5. Validar o novo recurso manualmente.
6. Criar um pull request (PR).
7. Depois da revisão e das verificações de integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas muitos fluxos seguem essas etapas.

Esse processo ajuda a garantir que o código gerado por inteligência artificial atenda aos requisitos e passe pelo mesmo nível de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar:

- **Interactive**: você e o agente trabalham em conjunto; o agente sugere mudanças e aguarda sua orientação.
- **Plan**: o agente cria um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e use Autopilot para implementá-lo.

## Comece pela issue

Antes de começar a filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o.
2. Confirme que o pull request de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma ramificação para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para selecionar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

O prompt começará com `#7` ou um número semelhante. O `#` adiciona o contexto de uma issue ou de um pull request do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem antes de implementar um recurso ou executar uma tarefa.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para selecionar Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue, envie:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano explorando o projeto e determinando a melhor abordagem.

3. Responda às perguntas do Copilot sobre o comportamento da filtragem de acordo com suas preferências. Não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Examine o plano.
6. Peça revisões se alguma parte não corresponder às decisões tomadas.

## Aprove o Autopilot

Depois de revisar o plano, autorize a implementação.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot executar o plano, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; revise o plano antes. Se o Copilot indicar dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o e execute o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e examine a implementação e os testes da filtragem.
3. Compare o resultado com a issue e as decisões de planejamento.
4. Pressione <kbd>Esc</kbd> para sair da exibição de diff.
5. Examine as verificações do projeto e peça ao Copilot para corrigir falhas.

## Explore o novo recurso

Inicie o aplicativo e abra o site pela porta encaminhada do Codespaces.

1. Envie ao Copilot:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros de categoria e de editora e as combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às decisões.
5. Volte ao Codespace e peça ao Copilot para parar o servidor iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub;
- definiu os requisitos no modo Plan;
- implementou o recurso com o modo Autopilot;
- revisou o código;
- validou manualmente o recurso no navegador encaminhado.

Em seguida, explore como o código é gerado e como seguir práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
