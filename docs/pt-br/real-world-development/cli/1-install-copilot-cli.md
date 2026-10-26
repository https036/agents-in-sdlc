---
title: "Lição 1 - Instale o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça a ferramenta e encontre a issue de filtragem no backlog."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um assistente de programação baseado em agentes que funciona no terminal. Ele permite explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas — tudo pela linha de comando. Você pode delegar tarefas, solicitar mudanças e manter o foco. O primeiro passo é instalar a ferramenta, usando recursos que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI com npm.
- autenticar com sua conta do GitHub.
- confiar no repositório do workshop e iniciar uma conversa.
- encontrar a issue de filtragem usando o servidor GitHub MCP integrado.

## Cenário

Sua equipe está começando a usar agentes de IA para avançar em um backlog crescente. O Copilot CLI leva essa capacidade ao terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você instalará e autenticará a ferramenta e se preparará para usá-la no restante do workshop.

## Instale o Copilot CLI

Você pode instalar o Copilot CLI com [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará o npm.

1. Volte ao Codespace e abra um terminal.
2. Confira se o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou posterior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Confira a instalação:

   ```bash
   copilot --version
   ```

   O número da versão será exibido.

## Autentique-se no GitHub

Na primeira vez que iniciar o Copilot CLI, ele solicitará que você autentique sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções de autenticação com código de dispositivo e autorize o Copilot CLI.
3. O Copilot CLI exibirá este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme que o caminho corresponde ao repositório Tailspin Toys e selecione **Yes, and remember this folder for future sessions**.

> [!NOTE]
> No Codespace, você talvez já esteja autenticado por meio da sessão do GitHub. Se o Copilot CLI iniciar sem solicitar autenticação, está tudo certo.

## Conheça a ferramenta

Os comandos digitados no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI inicia, você pode conversar com o agente em linguagem natural e controlar a conversa com comandos de barra.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na sua versão e pressione <kbd>Esc</kbd> para fechar a tela de ajuda.
3. Faça uma pergunta simples para confirmar que tudo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme que o servidor GitHub MCP integrado está disponível.
6. Peça ao Copilot que encontre a issue de filtragem:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abra a URL e leia a issue. Você a usará depois de fazer uma pequena alteração inicial.

> [!TIP]
> Uma sessão comum do Copilot CLI usa a branch ativa no terminal e não cria uma worktree automaticamente. Você criará uma branch de recurso antes de cada mudança.

## Use o atalho do workshop

Normalmente, antes de usar ferramentas fora das permissões já concedidas, o Copilot CLI solicita sua aprovação. Neste workshop, você o reiniciará com `--yolo`, um atalho aprovado pelo usuário que elimina essas solicitações no Codespace para que você possa se concentrar nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma máquina virtual, e nunca o defina como alias padrão para o desenvolvimento diário. Consulte [Permitir e negar o uso de ferramentas][allow-all-warning] para saber mais.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas GitHub MCP de leitura e gravação usadas nas próximas lições para trabalhar com issues e pull requests. O Codespace limita o acesso ao seu computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as mudanças antes de publicá-las ou fazer merge.

1. Saia do Copilot CLI com `/exit`.
2. Inicie-o novamente na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar que a conversa funciona e saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais tarde, depois de alterar as instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa e branch do recurso.

## Resumo e próximos passos

Nesta lição, você:

- instalou o GitHub Copilot CLI com npm.
- autenticou sua conta do GitHub.
- confiou no repositório do workshop e iniciou uma conversa.
- encontrou a issue de filtragem usando o servidor GitHub MCP integrado.

Agora, [faça sua primeira mudança focada][next-lesson] e use o Copilot CLI para exibir avaliações por estrelas nos cartões de jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
