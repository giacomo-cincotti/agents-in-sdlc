---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revisá-la no navegador pela porta encaminhada do Codespaces e integrá-la como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a mostram. Você pedirá ao agente que a exiba, revisará a alteração e fará o merge como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa direcionada com o Copilot em uma branch de recurso.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração no navegador pela porta encaminhada do Codespaces.
- abrir e fazer merge do seu primeiro pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Porém, os cards da página inicial mostram apenas o título, a categoria, a distribuidora e a descrição. Como aquecimento, peça ao agente para exibir em cada card a avaliação existente: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar um worktree dedicado. Você pode voltar a conversas salvas mais tarde, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade de ferramentas do agente enquanto ele explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a alteração

Vamos iniciar uma nova conversa para implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal ainda não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revisar o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos explorar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme se o card do jogo exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verificar as alterações

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot para iniciar o site para que possamos examinar a interface atualizada no navegador pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cards dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que interrompa o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir e fazer merge do primeiro pull request

Você criou o recurso! Agora é hora de abrir um pull request (PR) para integrar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e exibirá a URL dele.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Especificamente, você:

- iniciou uma conversa direcionada com o Copilot em uma branch de recurso.
- orientou o agente a fazer uma pequena alteração nos cards dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo para confirmar a avaliação por estrelas no navegador pela porta encaminhada do Codespaces.
- abriu e integrou seu primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
