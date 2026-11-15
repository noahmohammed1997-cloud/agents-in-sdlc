---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real no seu computador, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições. Você pode revisar cada diff no editor, aceitar ou rejeitar alterações e orientar a conversa com prompts de acompanhamento sem sair do ambiente de desenvolvimento integrado (IDE).

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design solicita também um modo claro para quem considera o tema padrão escuro demais. Em vez de iniciar outra conversa, você direcionará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A exibição Copilot Chat mostra as etapas executadas pelo agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas na conversa. Você verá leituras e edições de arquivos e comandos de terminal que ele solicita executar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Abra o indicador **Files changed** no início da conversa para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente aguardará sua resposta em um prompt **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde o agente terminar a implementação do modo de alto contraste. Ele deverá apresentar uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar um controle de modo claro. Como você continuará na mesma sessão, o agente manterá o contexto dos componentes alterados, dos estilos usados e do padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as alterações existentes e proporá as adições para o modo claro.
3. Revise cada nova alteração quando aparecer e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de modo de uma forma que você não queira, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes, corrigir o rumo ou ampliar o escopo.

## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise tudo que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e nos testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme se a implementação usa padrões de acessibilidade, como atributos Accessible Rich Internet Applications (ARIA), navegação por teclado, linguagem de marcação de hipertexto (HTML) semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e direcionar uma sessão em andamento, revisar cada edição, aceitar ou rejeitar alterações e ampliar o trabalho com prompts de acompanhamento sem perder o contexto.

Nesta lição, você explorou:

- monitorar o trabalho do agente em tempo real.
- aceitar e rejeitar edições individuais.
- direcionar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: agentes personalizados][previous-lesson] | [Próxima lição: iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
