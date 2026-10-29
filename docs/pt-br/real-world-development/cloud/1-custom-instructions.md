---
title: "Exercício 1 - Instruções personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Copilot cloud agent →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica — ou há informações de contexto que o Copilot deve conhecer —, garanta que essas informações estejam disponíveis. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não apenas *o que* você quer, mas também *como* quer que isso seja feito.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos de instruções associados a caminhos.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não fará uma comparação de prompt *antes e depois* neste exercício. O Copilot cloud agent trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar a influência das instruções lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente na nuvem.

## Arquivos de instruções

### Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de código, bibliotecas e tipos de comentários preferidos. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

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

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. Experimente para descobrir o que funciona no seu ambiente e nos seus cenários.

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

Reserve um momento para ler os arquivos de instruções incluídos neste repositório: um arquivo principal `copilot-instructions.md` e uma coleção de arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros assuntos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo, que define um glob (relativo à raiz do repositório) para determinar a quais arquivos as instruções se aplicam. Neste caso, correspondem a arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários para este projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicionar um novo padrão para o repositório

Agora você fará a única edição deste exercício: adicionará um padrão para todo o projeto, exigindo que a documentação no código seja feita com comentários TSDoc e um cabeçalho de comentário no arquivo. O cloud agent usará essas orientações nas issues atribuídas mais adiante neste percurso.

Antes de editar, crie uma branch (o cloud agent lerá as instruções da branch de destino da issue, mas você fará o commit das alterações de forma organizada):

1. No terminal do Codespace, crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Fazer commit, push e merge da atualização de instruções

O cloud agent lê os arquivos de instruções da branch de destino da issue. Na próxima lição, você atribuirá uma issue ao Copilot, que criará uma branch a partir de `main`. Portanto, as atualizações de instruções precisam chegar a `main` para que o agente possa usá-las.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o cloud agent lerá essas instruções quando trabalhar nas issues que você atribuir a ele na próxima lição.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante o workshop, ignore a etapa da branch e faça commit diretamente em `main`. A etapa da branch demonstra como você trabalharia em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa os arquivos de instruções deste projeto e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o cloud agent gerar nas próximas atividades.

Em seguida, você vai [atribuir sua primeira issue ao Copilot cloud agent][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
