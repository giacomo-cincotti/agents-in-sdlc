---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo opções de modo de alto contraste e modo claro. Vamos explorar as alterações e, se necessário, dar feedback ao Copilot para melhorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a inclusão de IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e GitHub Copilot cloud agent

Como o Copilot cloud agent executa tarefas de forma assíncrona e sem supervisão, existem algumas restrições de segurança para manter o ambiente protegido:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O cloud agent é executado no GitHub Actions, que cria um ambiente separado e efêmero para o trabalho.
- Uma pessoa precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo GitHub Copilot cloud agent: a adição de documentação ao código. Você fará a revisão pela interface normal de PRs do GitHub.com.

> [!NOTE]
> Ao explorar o PR, você poderá ver um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do Copilot cloud agent][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request incluirá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários de documentação TSDoc. As alterações exatas variam.

    Ao revisar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentários. Eles foram definidos nos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode marcar o Copilot em comentários do pull request ou em comentários de linha de código, como faria com outra pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não há um texto prescritivo. Você pode pedir ao Copilot que:

- adicione cabeçalhos de comentários ao início de cada arquivo de código, com uma descrição breve da finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e marque **@copilot**, como faria com outra pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para realizar as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode converter o PR de rascunho em pronto para revisão e integrá-lo à branch principal.

![Converter PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Vamos voltar ao PR criado pelo Copilot para resolver a issue de exibição de jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode selecionar essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot (por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar com o Copilot cloud agent para solicitar atualizações. Por exemplo, peça que ele altere a quantidade de jogos relacionados exibidos ou adicione cabeçalhos de comentários e comentários TSDoc (lembre-se: a tarefa foi atribuída **antes** de você atualizar as instruções personalizadas). Você pode fazer essas solicitações adicionando um comentário na guia **Conversation**. O Copilot verá o comentário e iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel central no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request incluirá a marca **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, principalmente:
   - os componentes de interface para alternar entre os modos.
   - o uso do armazenamento local para manter as preferências.
   - as alterações de CSS ou estilos para os modos de alto contraste e claro.
   - os atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - qualquer código JavaScript/TypeScript que gerencie a alternância entre modos.
6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** selecione essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme se funcionam como esperado e se as preferências permanecem após atualizar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos de acordo com as práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, marque **@copilot** em um comentário para solicitar atualizações, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O cloud agent funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso de cloud agent. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas do repositório** para ver como influenciariam os resultados do cloud agent.
- **Atribuiu issues ao Copilot cloud agent** e observou como ele configura o ambiente, planeja e executa tarefas de forma assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de modo de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel central** para monitorar e orientar a sessão de acessibilidade durante a execução.
- **Revisou e refinou os pull requests do cloud agent**, marcando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso de cloud agent. Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para voltar ao cloud agent.

No seu próprio repositório, experimente estas ideias:

- atribua uma issue de refatoração ou cobertura de testes ao cloud agent.
- crie um agente personalizado para outro domínio.
- configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]
- [Configurar o firewall do Copilot cloud agent][agent-firewall]

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
