---
title: "Lição 2 - Adicione avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena mudança nos cartões de jogos, revise-a no navegador encaminhado e integre seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, chegou a hora de fazer sua primeira mudança no projeto. O escopo será pequeno: os dados dos jogos já incluem uma avaliação por estrelas, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a mudança e fará o merge do seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de recurso.
- pedir ao agente que faça uma pequena mudança no projeto.
- revisar a mudança com `/diff`.
- iniciar o aplicativo e conferir a mudança em um navegador encaminhado.
- abrir e integrar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Porém, os cartões da página inicial exibem apenas o título, a categoria, a editora e a descrição. Como primeiro exercício, você pedirá ao agente que mostre nos cartões a avaliação existente — uma mudança pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch Git ativos no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam como estado Git comum no disco.

Dentro de uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas enquanto o agente explora e edita arquivos e as mudanças que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a mudança

Vamos iniciar uma nova conversa para implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver em execução, inicie o Copilot com este comando:

   ```bash
   copilot --yolo
   ```

4. Use o comando de barra `/new` e pressione <kbd>Enter</kbd> para iniciar uma sessão.
5. Use este prompt para solicitar a mudança:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos que exibem os detalhes dos jogos e criará o código necessário. Você acaba de adicionar um recurso com o Copilot CLI.

## Revise o diff

Toda mudança gerada por IA deve ser revisada antes do merge, mesmo que seja pequena. Vamos examinar as mudanças diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes abrangem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Ao concluir a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como qualquer ferramenta de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar. Revise o comportamento esperado em vez de exigir uma implementação específica.

## Confira as mudanças

Não basta ler o código e presumir que ele funciona. Vamos pedir ao Copilot que inicie o site para conferirmos a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões exibem as avaliações de zero a cinco.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e integre seu primeiro pull request

Você criou o recurso. Agora, abra um pull request (PR) para integrar o código ao projeto.

1. Peça ao agente padrão que faça commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e exibirá a URL do PR.
3. Abra o PR segurando <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando tudo estiver pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize a branch local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança usando o GitHub Copilot CLI. Nesta lição, você:

- iniciou uma conversa focada do Copilot em uma branch de recurso.
- orientou o agente a fazer uma pequena mudança nos cartões de jogos.
- revisou a mudança com `/diff`.
- iniciou o aplicativo para conferir a avaliação por estrelas em um navegador encaminhado.
- abriu e integrou seu primeiro pull request.

Agora, comece pela [issue de filtragem e use os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
