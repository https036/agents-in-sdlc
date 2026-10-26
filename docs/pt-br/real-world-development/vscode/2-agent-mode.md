---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente exigem mudanças em vários arquivos e tarefas como executar testes. O fluxo costuma envolver localizar os arquivos necessários, fazer as mudanças, executar testes, depurar, descobrir o que faltou e atualizar novamente. E assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente trabalha com mais autonomia no IDE. Ele explora a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora tenham nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena mudanças.
- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- revisar as mudanças e os testes gerados pelo modo de agente antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você criou uma função auxiliar para editoras. Agora, concluirá as mudanças restantes na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer mudanças, explore o site Tailspin Toys para conhecer as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. As pessoas podem consultar jogos e ver seus detalhes. O site é um único aplicativo Astro que gera páginas HTML estáticas durante o build. As páginas consultam diretamente um banco de dados SQLite local pelo frontmatter e pelo Drizzle ORM — não há API de back-end separada nem framework de interface no cliente. As funções reutilizáveis de acesso a dados ficam em `src/lib/`; a interatividade é adicionada com um pequeno `<script>` Astro específico, usando APIs padrão do DOM.

### Inicie o site

A tarefa `npm run dev` inicia o único servidor de desenvolvimento Astro:

1. Volte ao Codespace. Depois do exercício anterior, você deve estar na branch `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um aviso semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir `http://localhost:4321` no navegador, feche-a selecionando **x**.

5. Segure <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço `http://localhost:4321` no terminal para abrir o site.

> [!NOTE]
> No Codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor Web do Codespace.

### Explore o site

Com o site em execução, explore as funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes dos exercícios abaixo, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; as etapas indicam o que importa.

A primeira versão do site funciona, mas queremos adicionar recursos. Vamos começar examinando o backlog. Ao criar o projeto a partir do modelo, você recebeu automaticamente um conjunto de issues. Peça ao GitHub Copilot que o mostre.

1. Na lista de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa mudanças em vários arquivos, executa comandos de terminal e invoca ferramentas.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo específico. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos para que as sugestões fossem relativamente consistentes. Como os LLMs são probabilísticos, as sugestões podem ser diferentes das apresentadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

3. Envie este prompt ao Copilot:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com a utilidade provável para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, examine o arquivo de instruções que fornecerá contexto. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de tipos de arquivo, como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem apontar para outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos à localização do arquivo de instruções. Isso permite reutilizar conteúdo, dividir instruções complexas em partes menores e fornecer exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, o aplicativo precisa de pelo menos três mudanças:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Os testes precisam ser executados e aprovados antes da integração. O modo de agente do Copilot pode executar essas tarefas.

1. Continue a conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como o arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue correspondente do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados ao recurso, incluindo funções auxiliares da camada de dados, interface e testes. Depois, ele modificará os arquivos e executará os testes.

   ![Captura de tela do Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, conforme a complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as mudanças; não precisa selecionar esses botões enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. Isso é normal: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore o novo recurso.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu como:

- o modo de agente implementa recursos na interface e na camada de dados.
- o modo de agente explora o projeto, identifica arquivos relevantes e coordena mudanças.
- revisar as mudanças e os testes gerados pelo modo de agente antes de integrá-los à base de código.

Agora, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício de exploração opcional - Implemente paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu neste exercício para pedir ao Copilot que adicione paginação ao site. Considere:

- seguir as práticas existentes, inclusive usar os arquivos de instruções disponíveis.
- decidir como a paginação funcionará: por exemplo, se a pessoa usuária poderá selecionar o tamanho da página ou se ele será fixo.
- fornecer ao Copilot orientações suficientes para implementar o comportamento desejado.
- fazer iterações com o GitHub Copilot, pedindo mudanças e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
