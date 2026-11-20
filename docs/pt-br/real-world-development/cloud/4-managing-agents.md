---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas distintas ao agente de nuvem do Copilot para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas de agentes que têm pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você:

- explorará a página de agentes para monitorar tarefas do agente de nuvem.
- orientará uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria uma boa oportunidade para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar um seletor, fazia sentido incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente tanto o modo claro quanto o alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualização da documentação do código.
   - geração de APIs para modificar jogos.
   - adição de um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações já executadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem adotada pelo Copilot.

4. Observe também o painel de pull request à direita, onde você pode ver o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia de acordo com o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Esse processo também levará alguns minutos. Aproveite a pausa para refletir sobre o que aprendeu e explorou até agora.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, o centro para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho em andamento e até redirecionar uma tarefa para ajustar o escopo ou fornecer orientações adicionais.

Você explorou os seguintes conceitos:

- explorou o centro de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
