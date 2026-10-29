---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao Copilot cloud agent que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o cloud agent trabalhe de forma assíncrona e autônoma, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao cloud agent, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas dos agentes com pull requests (PRs) abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do cloud agent.
- orientar uma sessão em andamento e solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria uma boa oportunidade para adicionar também um modo claro. Como o trabalho já envolve atualizar o estilo do site e adicionar a funcionalidade de alternância, parece lógico incluir esse recurso. Você orientará o agente para garantir que ele implemente também o modo claro.

## Revisar as tarefas do Copilot cloud agent

Vamos verificar o status atual das tarefas atribuídas ao Copilot cloud agent.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento. Revise as operações que o Copilot realizou, como:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel do pull request (PR), que aparece à direita. Nele, você pode ver o PR e os arquivos alterados para acompanhar o trabalho.

## Orientar o cloud agent

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à adição do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

    ![Sessão de acessibilidade no painel central](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Isso ficará evidente quando surgirem mensagens semelhantes aos exemplos.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do cloud agent na página de agentes com a caixa de diálogo de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot reconhece o prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Como isso levará alguns minutos, aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de monitoramento e orientação das tarefas do GitHub Copilot cloud agent. Nesse painel, você acompanha tarefas ativas e concluídas, revisa o trabalho realizado e redireciona tarefas em andamento para ajustar o escopo ou fornecer orientações.

Você explorou estes conceitos:

- explorar o painel central e a página de agentes para monitorar tarefas do cloud agent.
- redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
