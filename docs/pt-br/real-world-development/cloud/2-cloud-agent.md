---
title: "Exercício 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos esquecidas no backlog por falta de tempo para implementá-las. O cloud agent do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera um PR de rascunho para revisão de uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em objetivos maiores.

Neste exercício, você vai explorar:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os prestadores de serviço contratados inicialmente para criar o site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Ainda não é necessário ter uma implementação refinada; basta algo que permita testar a experiência de usuário durante a aceitação. Esse trabalho é um impedimento, mas há issues com prioridade maior neste momento.

Esses são exemplos de tarefas que podem ser adiadas rapidamente e que são ideais para o Copilot cloud agent. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades. Depois, você volta para revisar o trabalho e confirmar se o resultado está adequado.

## Conhecer o GitHub Copilot cloud agent

O [GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Assim como ao trabalhar com uma pessoa da equipe, há várias maneiras de atribuir trabalho, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois que for concluído. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot — você não pode deixar de pensar. Até operações aparentemente simples podem se tornar complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o Copilot cloud agent][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão de IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu a atividade prática de instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao Copilot cloud agent, leia os arquivos de instruções já incluídos no repositório para perceber o efeito deles mais adiante.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um Codespace que já esteja em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas para convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que limita essas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao cloud agent, observe no pull request resultante os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentários. Esses elementos vêm dos arquivos de instruções que você verá novamente ao revisar PRs em um exercício futuro.

## Configurar o ambiente de desenvolvimento do Copilot cloud agent

Criar código, independentemente de quem o desenvolve, geralmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O cloud agent usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes de o agente começar. Assim, ele tem acesso às ferramentas e dependências necessárias. O workflow já foi configurado para facilitar o fluxo do laboratório e permitir que você aprenda. Ele fornece ao Copilot Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração de frontend/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer workflow do GitHub, mas há alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o acionador `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o workflow funciona sem esperar o Copilot executá-lo.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Essa dívida técnica costuma não ser resolvida, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novas pessoas à equipe. Felizmente, o Copilot é eficaz na criação de documentação, por isso esta é uma issue ideal para atribuir ao Copilot cloud agent. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se não aparecer um novo PR, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Role a sessão em tempo real e observe como o Copilot resolve o problema. Ele explora o código e entende o estado do projeto, pausa para pensar e decidir a abordagem adequada e, em seguida, cria o código.

O processo provavelmente levará alguns minutos. Um dos objetivos principais do Copilot cloud agent é permitir que tarefas sejam executadas de forma assíncrona, liberando você para trabalhar em outras atividades. Vamos aproveitar esse recurso: atribua outra tarefa ao cloud agent e, em seguida, escreva código para adicionar funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do GitHub Copilot cloud agent é a possibilidade de dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente se dedica a outra. Mesmo que a seção de jogos relacionados para a equipe de design não leve muito tempo, você poderia usar esse período para outras atividades. Vamos atribuir essa tarefa ao Copilot cloud agent.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando diligentemente na segunda solicitação. O Copilot cloud agent funciona de maneira semelhante a uma pessoa engenheira de software, portanto você não precisa monitorá-lo continuamente; basta revisar o trabalho quando for concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você conheceu o [GitHub Copilot cloud agent][copilot-agents]. Com o cloud agent, você pode atribuir issues ao Copilot para execução assíncrona. Use-o para resolver dívidas técnicas, criar recursos ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o cloud agent trabalha em segundo plano, vamos criar e usar agentes personalizados. O [Copilot cloud agent também pode usar servidores MCP][cloud-agent-mcp] e disponibiliza as instruções personalizadas exploradas nos módulos anteriores.

## Recursos

- [Sobre o Copilot cloud agent][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
