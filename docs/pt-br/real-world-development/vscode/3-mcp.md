---
title: "Exercício 3 - Teste seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Depois, publique a branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis ao Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que o use para testar manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando consegue acessar recursos fora do editor. O Model Context Protocol (MCP) permite isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama dos componentes internos do modo de agente e de suas interações com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome usando ferramentas.

O acesso ocorre por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos, como APIs existentes ou ferramentas locais em pacotes npm. Cada servidor oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios, atualizá-los e gerenciar issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright, como navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição para esse ecossistema.

> [!CAUTION]
> Em relação à segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores em que confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

A entrada `github` faz parte do modelo do projeto e permitiu que o Copilot consultasse o backlog de issues no exercício anterior. Ela usa o [servidor remoto GitHub MCP][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code se conecta por HTTP e você autentica no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicione o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador que ele pode controlar para testar o recurso.

1. Em `.vscode/mcp.json`, adicione a entrada `playwright` junto à entrada `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o `playwright` é **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` executa o navegador sem uma janela visível, o que é necessário no Codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e autorize o servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code inicia o servidor e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar se a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e autorize-o para que possa iniciar.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e pedirá autorização quando o Copilot precisar das ferramentas.

## Teste o recurso de filtragem

O servidor MCP do Playwright oferece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para conferir o trabalho, o agente pode abrir uma página, aplicar filtros, ler os resultados e resumir o que observou. É uma maneira rápida de verificar o comportamento sem sair da conversa.

O servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente analisa elementos estruturados e identificados — botões, links e itens de lista — de forma semelhante à tecnologia assistiva. Uma verificação funcional rápida também oferece uma avaliação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot iniciará um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará os resultados. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas adicionais ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> Para este teste, o aplicativo precisa estar em execução em `http://localhost:4321`. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente de novo.

## Publique a branch e crie um pull request

Depois de confirmar que o recurso funciona, abra um pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. No Codespace, abra o painel **Source Control** e revise as mudanças feitas pelo Copilot.
2. Prepare as mudanças selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as mudanças](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Você pode criar um pull request em github.com ou pela interface de linha de comando do GitHub (CLI). Como já está trabalhando com o GitHub Copilot, deixe que ele crie o PR. O Copilot pode localizar a issue correspondente e associá-la ao PR.

1. No painel Copilot Chat, selecione **New Chat** para iniciar uma nova sessão.
2. Na lista de agentes, confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute ações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar; siga as instruções.
5. Observe o Copilot pesquisar as issues, encontrar a correspondente e criar o PR.
6. Selecione o link gerado pelo Copilot para revisar o PR, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou-o para testar manualmente a filtragem no navegador antes de publicar o recurso.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para testar um recurso, mas o MCP oferece outras maneiras de ampliar o Copilot. Na próxima lição, [crie um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
