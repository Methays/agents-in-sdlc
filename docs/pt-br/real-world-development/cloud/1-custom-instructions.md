---
title: "Exercício 1 - Instruções personalizadas (agente na nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente na nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Os [arquivos de instruções][instruction-files] fornecem orientação para que o Copilot entenda não só o que fazer, mas também como fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções do repositório e instruções aplicadas a caminhos específicos;
- adicionar um novo padrão geral a `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos ambientes do VS Code e da interface de linha de comando (CLI), aqui você não fará uma comparação antes/depois com um prompt. O agente na nuvem trabalha de forma assíncrona em issues do GitHub, por isso é mais difícil demonstrar a influência das instruções em tempo real. Você verá esse efeito mais adiante, ao revisar os pull requests que o agente criar.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem estas diretrizes de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação de TypeScript (TSDoc).
- Cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Use arquivos de instruções para orientar o Copilot a seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, como convenções de codificação, bibliotecas e comentários desejados. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos:

- `.github/copilot-instructions.md` é enviado ao Copilot em todas as solicitações do repositório. Inclua contexto geral, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e outras diretrizes globais.
- `.github/instructions/*.instructions.md` fornece orientações para linguagens, tipos de arquivo ou tarefas específicas, como criar componentes de interface ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para gerar código no Copilot Chat, não para completar código nem para sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente na nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também aceita arquivos de instruções em outros formatos, como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto mostram uma abordagem possível:

- Mantenha `copilot-instructions.md` focado em contexto do projeto, estrutura e padrões globais.
- Use `*.instructions.md` para tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Escreva diretrizes claras em linguagem natural e inclua exemplos.

Não há uma única maneira de criar arquivos de instruções. Experimente para descobrir o que funciona no projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot devem ter instruções abrangentes. Neste projeto, há arquivos para tarefas como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de um jeito diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para encontrar modelos, explore [awesome-copilot][awesome-copilot], uma coleção de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

Leia os arquivos de instruções incluídos no repositório pela interface Web do GitHub ou em um codespace.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, examine as instruções aninhadas sobre fluxos de trabalho do GitHub Actions. Elas se aplicam a todas as interações com o Copilot.
3. Abra `.github/instructions` e examine os arquivos para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe `applyTo`, que define um glob relativo à raiz do repositório e corresponde a arquivos TypeScript de teste, por exemplo, `**/*.test.ts`.
5. Observe as instruções específicas para testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos do projeto. Esses links permitem dividir orientações grandes em arquivos reutilizáveis. Os caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` descreve os padrões, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicione um padrão ao repositório

Adicione uma regra geral para que o agente na nuvem use comentários TSDoc e um cabeçalho em cada arquivo.

1. No terminal do codespace, crie e selecione uma ramificação:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

O agente na nuvem lerá as instruções da ramificação de destino da issue. Na próxima lição, ele partirá de `main`; portanto, as instruções precisam ser mescladas nessa ramificação.

2. Abra `.github/copilot-instructions.md`.
3. Localize **Code formatting requirements**, perto da linha 27. Observe que a seção ainda não exige documentação no código.
4. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Instruções gerais ficam em `.github/copilot-instructions.md`; instruções para linguagens, tipos de arquivo ou tarefas específicas ficam em `*.instructions.md`. O arquivo do repositório é o lugar certo para padrões gerais.

## Faça commit, envie e mescle a atualização das instruções

O agente na nuvem lê as instruções da ramificação de destino. Como a próxima issue será atribuída a partir de `main`, mescle as mudanças nessa ramificação.

1. Prepare e faça commit da atualização:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a ramificação:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e mescle-o. O agente na nuvem lerá as instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main`, ignore a etapa da ramificação e faça o commit diretamente nessa ramificação. A ramificação do exercício representa o fluxo usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções e adicionou um padrão geral a `.github/copilot-instructions.md`. Você verá a influência desse padrão nos pull requests que o agente na nuvem criar.

Em seguida, [atribua sua primeira issue ao agente na nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
