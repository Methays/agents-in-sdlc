---
title: "Lição 10 - Recapitulação e próximos passos"
description: "Recapitule o fluxo do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou a interface de linha de comando (CLI) do GitHub Copilot em um fluxo contínuo de desenvolvimento do Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem;
- adicionou avaliações por estrelas, revisou o resultado no navegador encaminhado e mesclou manualmente seu primeiro pull request (PR);
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a habilidade existente `quality-checks` e a usou para executar testes unitários, lint e verificações de tipos;
- adicionou o servidor MCP do Playwright e o usou para explorar a filtragem em um navegador;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da habilidade e evidências do navegador;
- revisou a mudança completa da filtragem e autorizou o Agent Merge para o pull request;
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem dois marcos de pull request, cada um em sua própria ramificação derivada de `main` atualizado:

1. **Avaliações por estrelas:** exibir nos cards de jogos o valor existente de `starRating` e um estado explícito para jogos sem avaliação.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Desde o planejamento da filtragem até a abertura do pull request, você usou a mesma conversa e ramificação. O trabalho foi combinado em um único pull request para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código com testes automatizados, uma inspeção própria no navegador e a exploração do navegador pelo Copilot usando MCP. A habilidade `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados e revisou os requisitos e a cobertura antes da abertura do pull request.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclua que não são necessários testes novos também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de qualquer mudança.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho. Neste workshop, você atualizou instruções, personalizou uma habilidade, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, habilidades descrevem tarefas repetíveis, agentes personalizados definem papéis especializados e servidores MCP conectam ferramentas externas. Revise as mudanças reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para definir uma abordagem antes de começar, **Interactive** para acompanhar mudanças focadas e **Autopilot** em tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e outro mais capaz para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* influencia significativamente o resultado.

## Explore mais

Você concluiu o fluxo principal. Alguns outros recursos do CLI que pode experimentar:

- `/review` para pedir ao agente de revisão de código que analise as mudanças;
- `/rubber-duck` para discutir um problema e obter outra perspectiva;
- `/fleet` para coordenar tarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa;
- `/delegate` para enviar uma tarefa ao agente na nuvem do Copilot.

## Próximas etapas

Continue usando a ferramenta em código de produção, projetos pessoais ou um aplicativo que gostaria de criar. Compartilhe o que aprendeu com a equipe e explore a documentação.

Para ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente na nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
