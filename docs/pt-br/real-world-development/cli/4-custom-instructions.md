---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com inteligência artificial generativa. Os [arquivos de instruções][instruction-files] descrevem não só o código desejado, mas também como ele deve ser estruturado. Agora que você criou a filtragem, examine as instruções usadas pelo Copilot CLI (interface de linha de comando), adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e as instruções aplicadas a caminhos específicos chegam ao agente;
- atualizar o arquivo de instruções para seguir os padrões de codificação;
- observar o efeito dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes de desenvolvimento:

- Comentários devem explicar a intenção e decisões não óbvias, não repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar propósito, parâmetros e valores de retorno com comentários de documentação de TypeScript (TSDoc) e de JavaScript (JSDoc), incluindo um argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar os contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Use arquivos de instruções para fornecer ao Copilot o contexto necessário para seguir essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a entender seu estilo de codificação e os requisitos. Elas podem especificar convenções, bibliotecas e os comentários desejados, para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos:

- `.github/copilot-instructions.md` é enviado ao Copilot em todas as solicitações do repositório e deve conter contexto geral relevante para a maioria delas.
- `.github/instructions/*.instructions.md` fornece orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> Os formatos e o suporte a instruções variam conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui arquivos de instruções. Examine-os antes de alterá-los para observar seu efeito.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Examine a descrição breve do projeto e as diretrizes. Essas instruções se aplicam a todas as interações com o Copilot no repositório.
4. Abra `.github/instructions` e examine os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob para determinar os arquivos correspondentes.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e arquivos do projeto. Assim, conjuntos maiores podem ser divididos em arquivos reutilizáveis.

## Atualize as instruções para corresponder às diretrizes da equipe

Adicione uma regra de documentação em código ao arquivo principal de instruções.

1. Em `.github/copilot-instructions.md`, localize **Code formatting guidance**, perto da linha 35.
2. Adicione este item como o último marcador da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Aplique a orientação atualizada

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, feche e retome a conversa de filtragem para carregar as mudanças sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit`. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na ramificação `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a ramificação `game-filters-cli` está aberta.

3. Peça ao Copilot para aplicar a orientação atualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e examine os arquivos TypeScript alterados. Observe os comentários TSDoc recém-gerados e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI usa arquivos de instruções e aplicou um novo padrão ao recurso. Nesta lição, você:

- explorou como as instruções do repositório e as instruções aplicadas a caminhos específicos chegam ao agente;
- atualizou o arquivo de instruções para seguir os padrões de codificação;
- observou o efeito dos arquivos de instruções no código.

Em seguida, [personalize e execute a habilidade reutilizável `quality-checks`][next-lesson] para executar lint e testes de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
