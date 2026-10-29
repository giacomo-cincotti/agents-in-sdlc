---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando o GitHub Copilot está no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que ocorrem. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar um modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz outra solicitação: também quer uma opção de modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização do Copilot Chat mostra cada etapa do agente: os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição aparece em linha na conversa, com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização do Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar as chamadas de ferramentas. Você verá leituras e edições de arquivos, além dos comandos de terminal cuja execução ele solicita.
3. Quando o agente propuser uma edição, o diff aparecerá em linha:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para reverter a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente pausará o trabalho e exibirá uma solicitação **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde até que o agente termine de implementar o modo de alto contraste. Você deverá ver uma mensagem final que resume as alterações realizadas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar um controle do modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilos e o padrão usado para o armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição assim que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar os controles de modo de uma maneira que você não prefere —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer a solicitação.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts de acompanhamento para pedir ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), corrigir o rumo ("don't add a third toggle, integrate light mode into the existing settings panel") ou ampliar o escopo ("also add keyboard shortcuts for both toggles").

## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá encontrar atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se a implementação reflete as práticas de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência com o armazenamento local.
4. Quando estiver tudo pronto, prepare e faça commit das alterações pela visualização Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento. Revisou cada edição conforme ela surgia, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- monitorou o trabalho de um agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
