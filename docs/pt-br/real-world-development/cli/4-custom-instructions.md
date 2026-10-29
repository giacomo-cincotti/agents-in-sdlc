---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções associados a caminhos chegam ao agente.
- atualizar o arquivo de instruções para que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não são óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar com TSDoc/JSDoc sua finalidade, parâmetros e valores de retorno, incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser mantidas.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Elas permitem orientar o Copilot para sugerir código mais relevante. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter contexto relevante para a maioria das solicitações de chat ou CLI.
- `.github/instructions/*.instructions.md` contém diretrizes para linguagens, tipos de arquivos ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instruções varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções deste projeto

O projeto inicial já inclui alguns arquivos de instruções. Vamos explorá-los antes de fazer uma alteração para observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, e não no terminal, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros assuntos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo, que define um glob para determinar a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e apontar o Copilot para exemplos.

## Atualizar as instruções para corresponder às diretrizes da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma orientação. Vamos modificar `copilot-instructions.md` para garantir que o novo código TypeScript tenha comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar próxima da linha 35.
2. Adicione o item a seguir ao final da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot use as orientações atualizadas sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme se a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme se explicam corretamente o código.

## Resumo e próximos passos

Você explorou como o Copilot CLI usa os arquivos de instruções do repositório e aplicou um novo padrão ao recurso. Especificamente, você:

- explorou como as instruções do repositório e os arquivos associados a caminhos chegam ao agente.
- atualizou o arquivo de instruções para que os padrões de código sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você vai [personalizar e executar a skill reutilizável `quality-checks`][next-lesson] para garantir que os testes e o lint sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas para o Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — uma coleção de arquivos de instruções][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
