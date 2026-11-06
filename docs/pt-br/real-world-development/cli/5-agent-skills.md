---
title: "Lição 5 - Personalizar e usar uma habilidade quality-checks"
description: "Explore a habilidade quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código é apenas uma parte do desenvolvimento. Até aqui, verificamos manualmente se o código funciona e usamos arquivos de instruções para garantir que ele siga os padrões da equipe. Mas e os testes, o lint e as outras partes da integração contínua (CI)?

Para esses tipos de tarefa, as **habilidades de agente** são ideais. Elas ajudam o Copilot a entender como executar operações como essas.

Nesta lição, você:

- explorará a habilidade `quality-checks` existente.
- personalizará o formato dos resultados.
- recarregará e executará a habilidade.

## Cenário

O Tailspin Toys usa a habilidade `quality-checks` para testes de unidade, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As habilidades de agente agrupam instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que um agente carrega sob demanda. Em sua forma básica, uma habilidade é uma pasta com o nome da habilidade e um arquivo Markdown chamado `SKILL.md`. O arquivo Markdown contém frontmatter com um nome e uma descrição que definem a habilidade, uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode conter subpastas com scripts e outros recursos que a habilidade utiliza.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A habilidade `quality-checks` do Tailspin Toys contém somente `SKILL.md`, pois usa os comandos já existentes no projeto.

As habilidades podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta de habilidades do usuário, em `~/.copilot/skills`.

## Explorar a habilidade

Explore a habilidade `quality-checks` que a equipe do Tailspin Toys criou para executar testes de unidade, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` no início do arquivo. A descrição ajuda o Copilot a saber quando chamar a habilidade.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e o lint.
4. Observe que a habilidade ainda não tem uma seção **Results output formatting**.

## Executar a habilidade antes de fazer uma alteração

Peça ao Copilot para executar as três verificações da habilidade antes de mudar seu relatório.

1. Volte à conversa de filtragem no modo Interactive.
2. Use o seguinte prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório no final.

## Personalizar o relatório

Agora, você atualizará a habilidade para produzir um relatório melhor, que informe o que foi executado, se houve sucesso e o que as ferramentas realmente relataram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seguinte seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo é salvo automaticamente.

## Executar a habilidade atualizada

O Copilot CLI pode recarregar as habilidades editadas sem reiniciar a conversa. Execute novamente a mesma solicitação para comparar os relatórios.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Nesta lição, você:

- explorou a habilidade `quality-checks` para testes de unidade, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a habilidade.

Essa alteração acompanhará o trabalho de filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor Model Context Protocol (MCP) do Playwright][next-lesson].

## Mais exemplos de habilidades

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
