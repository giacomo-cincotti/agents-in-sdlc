---
title: "Exercício 1 - Instruções personalizadas"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos de instruções associados a caminhos.
- gerar a primeira fatia de dados para filtragem (um auxiliar de distribuidoras) com as instruções *atuais* em vigor.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão.
- fazer commit das instruções atualizadas e da fatia de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de comportamento após a atualização das instruções, não obter um resultado idêntico em todos os detalhes.

## Arquivos de instruções

### Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Elas permitem orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de código, bibliotecas e tipos de comentários preferidos. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter contexto aplicável à maioria das interações de chat ou CLI com o Copilot, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que você pode criar para tarefas ou tipos de arquivos específicos. Use-os para dar orientações sobre linguagens, como TypeScript ou Astro, ou tarefas, como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código nem sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o Copilot cloud agent usam os arquivos de instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a arquivos de instruções em outros padrões, como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está além do escopo deste workshop. Os exemplos do projeto mostram uma abordagem representativa. Em alto nível:

- Mantenha `copilot-instructions.md` focado nas orientações para o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e padrões gerais de código.
- Use arquivos `*.instructions.md` para fornecer orientações específicas para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Escreva em linguagem natural. Mantenha as orientações claras e inclua exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefas, como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface apresenta esse recurso de uma maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição sobre a interface que você está usando explicará o recurso quando for pertinente.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui alguns arquivos de instruções. Vamos explorá-los antes de fazer uma alteração para observar o impacto.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros assuntos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo, que define um glob (relativo à raiz do repositório) para determinar a quais arquivos as instruções se aplicam. Neste caso, correspondem a arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários para este projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao Codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício — cada etapa indicará o que usar quando necessário.

Para observar o impacto das instruções personalizadas, envie primeiro um prompt com o arquivo de instruções atual. Depois, atualize as instruções e envie o mesmo prompt novamente para comparar os resultados.

1. Feche as guias de editor abertas de exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o auxiliar deve ficar.
3. Selecione **Agent** na lista de agentes da visualização Chat para que o Copilot possa aplicar alterações nos arquivos.

   ![Captura de tela do seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, normalmente ao arquivo do auxiliar e aos testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de distribuidoras. Essa abordagem vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentários no arquivo.

> [!CAUTION]
> Como as respostas do Copilot são probabilísticas, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a melhoria da *consistência* depois da atualização das instruções.

## Adicionar um novo padrão para o repositório

Como já mencionado, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Reabra `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar próxima da linha 27. Observe como ela documenta os padrões de código, mas ainda não tem regras para documentação no código — por isso, o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown abaixo dos padrões existentes para instruir o Copilot a adicionar cabeçalhos de comentários aos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais, ou como arquivos `*.instructions.md` para linguagens, tipos de arquivos ou tarefas específicas. O arquivo do repositório é adequado para padrões gerais, como a regra sobre comentários de documentação que você acabou de adicionar.

## Reenviar o prompt e observar a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são ativos do repositório, portanto são gerenciados pelo mesmo processo de controle de versão que os demais arquivos. Vamos fazer commit das alterações e enviar a branch.

1. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. No terminal, confirme se as alterações nas instruções e no auxiliar estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Depois de enviar a branch, crie um pull request e associe-o à issue do backlog sobre a atualização das instruções. Em vez de fazer isso manualmente, você pode pedir ao Copilot que encontre a issue, crie o PR para encerrá-la e faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O modelo do projeto já conecta as ferramentas do GitHub ao espaço de trabalho, portanto não é necessário configurar nada. Na primeira vez que o Copilot usar uma ferramenta, o VS Code talvez peça que você entre no GitHub. Siga as instruções. Você aprenderá sobre essa conexão (Model Context Protocol) em um exercício futuro.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme se **Agent** está selecionado na lista de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual. O PR deve descrever tanto a atualização das instruções quanto a nova base do auxiliar de distribuidoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot pedir permissão para executar ações no GitHub, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois que o PR for criado, peça ao Copilot que faça o merge e retorne sua branch local para `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot pedir permissão para executar ações do GitHub e comandos do shell, revise o comando e selecione **Allow** se estiver de acordo.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot usa os arquivos de instruções deste projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa continuar o trabalho.

Em seguida, você usará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
