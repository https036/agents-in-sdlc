---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você ativa o modo de agente do GitHub Copilot, ele trabalha de forma autônoma: explora a base de código, propõe mudanças, edita arquivos e executa comandos. Como o trabalho ocorre em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição enquanto acontecem. Você pode revisar cada diff na conversa, aceitar ou rejeitar mudanças e orientar o agente com prompts adicionais sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design fez outro pedido: adicionar também um controle de modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de iniciar outra conversa, você orientará a sessão atual para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e eventuais comandos de terminal que ele pedir para executar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a mudança.
   - Selecione **Undo** para revertê-la e permitir outra tentativa do agente.
4. Você também pode abrir o indicador **Files changed** na parte superior da conversa para ver todos os arquivos alterados pelo agente.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo para executar um comando de terminal, o agente pausará e mostrará a solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde até que o agente conclua o modo de alto contraste. Ele apresentará uma mensagem final resumindo as mudanças feitas.

## Oriente o agente para adicionar o modo claro

Com o modo de alto contraste implementado, você ampliará a mesma conversa para incluir um controle de modo claro. Como continuará na sessão atual, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilos e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente analisará as mudanças já feitas e proporá as adições do modo claro.
3. Revise cada nova edição conforme ela aparecer, usando **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar os controles de uma maneira que você não quer —, pare-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Envie prompts de acompanhamento para pedir ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), corrigir a abordagem ("don't add a third toggle, integrate light mode into the existing settings panel") ou ampliar o escopo ("also add keyboard shortcuts for both toggles").

## Revise o conjunto de mudanças

Antes de fazer commit do trabalho, revise tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver mudanças em componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme que as práticas de acessibilidade do agente personalizado estão presentes: atributos ARIA, navegação por teclado, HTML semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das mudanças no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou mudanças e enviou prompts para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão durante o trabalho com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as mudanças produzidas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
