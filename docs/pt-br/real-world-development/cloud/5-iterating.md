---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Neste laboratório, você pediu ao GitHub Copilot para documentar o código, implementar jogos relacionados e criar recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Agora, examine as mudanças e, se necessário, dê feedback ao Copilot.

### Cenário

Os fundamentos de desenvolvimento de software e DevOps não mudam com o uso de inteligência artificial generativa. Revise o código gerado e siga o processo normal de DevOps. Antes de habilitar a revisão da equipe, examine as propostas do Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade.

## Segurança e agente na nuvem do GitHub Copilot

Como o agente na nuvem trabalha de forma assíncrona e sem supervisão, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação somente à ramificação que usará.
- O agente na nuvem é executado no GitHub Actions em um ambiente efêmero separado.
- Um ser humano precisa aprovar fluxos de trabalho do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores do Model Context Protocol (MCP).

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente na nuvem: o que adiciona documentação ao código.

> [!NOTE]
> O pull request pode exibir um aviso de que o Copilot foi bloqueado por um firewall. Isso é esperado, pois o acesso a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente na nuvem][agent-firewall].

1. Acesse seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o pull request estiver pronto, selecione **Files changed** e revise as mudanças.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado e os comentários de documentação de TypeScript (TSDoc). Observe também as convenções de TypeScript e os cabeçalhos, derivados dos arquivos de instruções revisados no início do Exercício 2.
6. Volte à guia **Conversation** e role para baixo.
7. Verifique se há um indicador de que fluxos de trabalho aguardam aprovação.
8. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Aguarde os fluxos de trabalho aparecerem na seção de verificações do pull request e confirme se as verificações do aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.

## Peça mudanças ao GitHub Copilot

Você pode mencionar o Copilot em comentários do pull request ou em comentários de linha do código. Ele usará o feedback para iniciar outra sessão e atualizar o trabalho. Como os resultados não são determinísticos, não há um prompt obrigatório. Você pode pedir, por exemplo:

- cabeçalhos breves nos arquivos de código descrevendo sua finalidade;
- comentários TSDoc em arquivos TypeScript e Astro;
- um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário sobre uma mudança na documentação gerada e mencione **@copilot** como faria com outra pessoa. Use uma das ideias acima ou outra solicitação.
2. Selecione **View Session** para acompanhar a nova sessão do Copilot.
3. Para voltar ao pull request, selecione **Back to pull request**.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as mudanças, confirme que um novo commit foi adicionado.
5. Selecione **Files changed** e revise as mudanças.

Continue fazendo iterações até ficar satisfeito. Depois, marque o PR como pronto para revisão e mescle-o na ramificação principal.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, revise o pull request que atende à issue sobre mostrar jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o pull request com título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código gerado.
5. Quando estiver satisfeito, volte à guia **Conversation** e role para baixo.
6. Verifique se há fluxos de trabalho aguardando aprovação.
7. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Aguarde as verificações do aplicativo Astro e confirme se foram aprovadas.
9. **Opcional:** Para testar manualmente o recurso, abra o terminal no Codespace e troque `<branch-name>` pelo nome da ramificação criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

10. Inicie o aplicativo e teste o recurso de jogos relacionados.

O Copilot criou o recurso de jogos relacionados. Você pode pedir mudanças adicionais comentando na guia **Conversation**, como ajustar a quantidade de jogos ou adicionar cabeçalhos e comentários TSDoc. Lembre-se: essa issue foi atribuída antes de atualizar as instruções personalizadas.

## Revise os recursos de acessibilidade

Por fim, revise o pull request criado pelo agente personalizado de acessibilidade. Ele deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado na sessão do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o pull request com título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Aguarde a conclusão.

4. Selecione **Files changed** e revise o código.
5. Examine especialmente:
   - os componentes dos controles para alternar os modos;
   - o armazenamento local usado para persistir as preferências;
   - as mudanças de folhas de estilo em cascata (CSS) ou de estilo dos modos de alto contraste e claro;
   - os atributos de acessibilidade, como rótulos de Aplicações de Internet Ricas e Acessíveis (ARIA) e navegação por teclado;
   - o código JavaScript ou TypeScript que gerencia os modos.

6. Quando estiver satisfeito, volte à guia **Conversation** e role para baixo.
7. Verifique se há fluxos de trabalho aguardando aprovação.
8. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Aguarde as verificações do aplicativo Astro e confirme se foram aprovadas.
10. **Opcional:** Para testar manualmente os recursos de acessibilidade, abra o terminal no Codespace e troque `<branch-name>` pelo nome da ramificação criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador. Confirme se as preferências continuam ativas depois de recarregar a página.

O agente personalizado de acessibilidade orientou a implementação segundo práticas de acessibilidade. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar mudanças.

## Exercício opcional: continue delegando

O agente na nuvem é útil quando você pode delegar itens reais do backlog e trabalhar em outra coisa. Para praticar, crie issues no repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na lista de jogos;
- adicionar validação de entrada e tratamento de erros às funções auxiliares de acesso a dados.

## Resumo

Você concluiu o ambiente de agente na nuvem. Ao longo das lições, você:

- **Examinou os arquivos de instruções personalizados incluídos no repositório** para ver como influenciam o trabalho do agente na nuvem.
- **Atribuiu issues ao agente na nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncronos.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e ajustou os pull requests do agente na nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o ambiente de agente na nuvem. Para continuar, explore outros ambientes:

- **[Ambiente do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- **[Ambiente de CLI](../../cli/)** — entregue duas mudanças revisadas pelo terminal com Plan e Autopilot, habilidades de agente, agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para o agente na nuvem.

No seu repositório, experimente atribuir ao agente na nuvem uma issue de refatoração ou cobertura de testes, criar um agente personalizado para outro domínio ou configurar `copilot-setup-steps.yml` para outra pilha.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para configurar o agente na nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente na nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
