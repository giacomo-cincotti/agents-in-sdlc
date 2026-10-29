---
title: "Lição 10 - Conclusão e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo no Tailspin Toys. Você:

- preparou o Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já criada.
- adicionou avaliações por estrelas, revisou o resultado no navegador pela porta encaminhada do Codespaces e fez manualmente o merge do primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, desenvolveu no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e usou-a para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge para o PR do recurso.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch baseada na `main` atualizada:

1. **Avaliações por estrelas:** exiba o valor existente de `starRating` e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as ao recurso, personalize o relatório da skill `quality-checks`, crie um perfil de QA e inclua os testes relacionados.

Do planejamento da filtragem à abertura do PR, você usou a mesma conversa e a mesma branch. Reunimos o trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção manual no navegador e exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados com a revisão dos requisitos e da cobertura dos testes antes do PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclua que não são necessários testes novos também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para avaliar uma abordagem antes de começar, **Interactive** para acompanhar alterações focadas e **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para edições rotineiras e um modelo mais avançado para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Explicar com clareza *o que* você quer desenvolver, *por que* e *como* influencia significativamente o resultado.

## Mais para explorar

Você concluiu o fluxo de trabalho principal. Confira outros recursos do CLI:

- `/review` para pedir que o agente de revisão de código analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa.
- `/delegate` para enviar uma tarefa ao Copilot cloud agent.

## Próximos passos

A melhor maneira de aprender a usar uma ferramenta é continuar usando-a. Use-a em código de produção, projetos pessoais e naquele pequeno aplicativo que você gostaria de criar há anos. Compartilhe o que aprendeu com sua equipe e aprenda com os outros. E, como sempre, explore a documentação.

Para ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para conhecer outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do Copilot cloud agent][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
