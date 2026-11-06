---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Se uma tarefa precisa ser feita de determinada maneira ou o Copilot precisa conhecer informações adicionais, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] orientam como o código deve ser gerado.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções do repositório e instruções aplicadas a caminhos específicos;
- gerar a primeira parte dos dados para filtragem (uma função auxiliar de editoras) com as instruções atuais;
- adicionar um padrão geral do repositório a `.github/copilot-instructions.md`;
- executar o mesmo prompt novamente e observar o efeito do novo padrão no código;
- enviar a atualização das instruções e a parte da filtragem para `main`, para que o Copilot use as instruções atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a tendência após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação de TypeScript (TSDoc).
- Cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Use arquivos de instruções para fornecer ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda seu estilo e seus requisitos de codificação. Você pode definir convenções, bibliotecas e comentários preferidos. As instruções podem se aplicar a todo o repositório ou a tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um arquivo único enviado ao Copilot em todas as solicitações feitas no repositório. Inclua nele contexto geral do projeto, como a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras diretrizes globais.
- `.github/instructions/*.instructions.md` contém diretrizes para linguagens, tipos de arquivo ou tarefas específicas, como criar componentes de interface ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para gerar código no Copilot Chat, não para completar código nem para sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente na nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também aceita arquivos de instruções em outros formatos, como AGENTS.md e CLAUDE.md. Consulte o [suporte a instruções personalizadas][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Os arquivos de exemplo do projeto mostram uma abordagem possível:

- Mantenha `copilot-instructions.md` focado em diretrizes gerais do projeto, como sua descrição, estrutura e padrões globais de codificação.
- Use `*.instructions.md` para tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Escreva em linguagem natural, com diretrizes claras e exemplos do que fazer e evitar.

Não há uma única maneira correta de criar arquivos de instruções ou usar inteligência artificial. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot devem ter um conjunto sólido de arquivos de instruções. Neste projeto, há arquivos para tarefas como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a criar esses arquivos. Cada ambiente oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). O exercício correspondente explicará quando usar esses recursos.
>
> Para encontrar modelos e exemplos, explore [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

Examine o arquivo principal `copilot-instructions.md` e os arquivos `*.instructions.md` para entender o contexto que o Copilot recebe.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as diretrizes aninhadas de fluxos de trabalho do GitHub Actions. Elas se aplicam às interações com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início, que define um glob relativo à raiz do repositório. Por exemplo, ele corresponde a arquivos de teste TypeScript como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e arquivos do projeto. Assim, conjuntos maiores de diretrizes podem ser divididos em arquivos reutilizáveis com referências a exemplos. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` descreve os padrões de codificação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Criar uma ramificação para as mudanças

Crie uma ramificação para trabalhar nas instruções e na função auxiliar.

1. Volte ao codespace do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma ramificação:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar.

Primeiro, envie um prompt com as instruções atuais. Depois, atualize as instruções e envie o mesmo prompt para comparar o resultado.

1. Feche as guias do editor abertas nos exercícios anteriores para limitar o contexto do Copilot.
2. Abra `src/lib/publishers.ts` para indicar onde a função auxiliar deve ficar.
3. Na exibição Chat, selecione **Agent** no seletor de agentes para permitir que o Copilot altere arquivos.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot examina o projeto e aplica as mudanças, que podem incluir a função auxiliar e os testes.
6. Observe que a função proposta é tipada, recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras. Essas convenções vêm de `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele talvez adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a melhora de consistência depois da atualização das instruções.

## Adicione um padrão ao repositório

Atualize os padrões gerais de codificação para melhorar as sugestões do Copilot.

1. Reabra `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, perto da linha 27. Ela ainda não exige documentação no código, razão pela qual a função auxiliar pode não ter comentários.
3. Adicione estas linhas de Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como visto na lição anterior, as instruções gerais ficam em `.github/copilot-instructions.md`; instruções para linguagens, tipos de arquivo ou tarefas específicas ficam em `*.instructions.md`. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários que você acabou de adicionar.

## Execute o prompt novamente e observe a diferença

Envie o mesmo prompt em uma conversa nova para que o Copilot carregue as instruções atualizadas e compare o código proposto.

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` no editor para indicar o arquivo relevante.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto começa com um comentário semelhante a este:

   ```typescript
   /**
    * Funções auxiliares de acesso aos dados de editoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - O cliente de banco de dados do Drizzle.
    * @returns Uma promessa resolvida com um array de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código que servirá de base para o próximo exercício.

## Faça commit das instruções e envie a ramificação

Gerencie os arquivos de instruções com o mesmo fluxo de controle de versão usado para os demais arquivos.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme se as instruções e as mudanças da função auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e dos arquivos alterados pelo Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se o git status mostrar outras alterações de apoio (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a ramificação ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Com a ramificação publicada, crie um pull request associado à issue sobre os padrões de documentação. O Copilot pode encontrar a issue, criar o pull request e mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub. Na primeira utilização, o VS Code talvez peça que você entre no GitHub; siga as instruções. Em um exercício posterior, você verá como essa conexão funciona com o Model Context Protocol (MCP).

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot para encontrar a issue sobre a atualização dos arquivos de instruções e criar, a partir da ramificação atual, um pull request que descreva as instruções e a base da função auxiliar de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Aguarde enquanto o Copilot localiza a issue e cria o pull request.
5. Quando solicitado a permitir ações do GitHub, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois que o pull request for criado, peça ao Copilot para mesclá-lo em `main` e atualizar sua cópia local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir ações do GitHub e comandos do shell, revise o comando e selecione **Allow** se estiver de acordo.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes;
- adicionar um padrão geral a `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o efeito do novo padrão;
- fazer commit das instruções e da função auxiliar de base na ramificação `main` para que o próximo exercício use esse trabalho.

Em seguida, use essas instruções no [modo de agente][next-lesson] para adicionar um recurso ao código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
