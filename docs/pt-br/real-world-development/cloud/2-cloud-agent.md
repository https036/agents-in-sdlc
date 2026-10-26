---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas de dívida técnica. Pode haver problemas de segurança sem solução, código legado que precisa de atualizações ou solicitações de recursos esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi desenvolvido para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um PR de rascunho para uma pessoa desenvolvedora revisar. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- executar operações de forma segura.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte de sua dívida técnica. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em estado inadequado: na prática, não há documentação. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar outros títulos da mesma categoria para que as pessoas continuem explorando. A equipe ainda não precisa de uma implementação pronta para produção; quer algo que possa usar para testar a experiência. Esse trabalho é um impedimento, mas há issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são boas candidatas para o agente de nuvem do Copilot. O agente pode trabalhar nelas de forma assíncrona enquanto você cuida de outras tarefas. Depois, você revisa o trabalho e confirma se está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão durante o trabalho ou depois da conclusão. Quando a solução estiver pronta para revisão, o agente mencionará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: ainda é necessário pensar no problema. Mesmo tarefas aparentemente simples ficam complexas quando examinadas em detalhes. Por isso, [defina cuidadosamente o escopo das tarefas atribuídas ao agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e ajuste a abordagem. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo. Antes de atribuir uma tarefa ao agente de nuvem, leia os arquivos de instruções incluídos no repositório sem modificá-los, para que depois possa observar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace ativo:

- `.github/copilot-instructions.md`: revise a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe se o pull request resultante inclui comentários TSDoc, segue as convenções de TypeScript e contém cabeçalhos nos arquivos, conforme orientado por esses arquivos de instruções. Você revisará isso novamente em um exercício futuro.

## Configure o ambiente de desenvolvimento do agente de nuvem

O desenvolvimento de código normalmente exige um ambiente específico e scripts de configuração que garantam que tudo esteja pronto. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa [GitHub Actions][github-actions] como ambiente. Você pode personalizá-lo criando um [workflow de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes de o agente começar. Isso permite instalar as ferramentas e dependências necessárias. O projeto foi pré-configurado para este laboratório com Node.js, dependências, navegadores para testes de ponta a ponta e um banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo para instalar dependências, você precisará da permissão `contents: read`. Caso contrário, o Copilot fará isso automaticamente após a conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para disponibilizar dados para builds e testes.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho, chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona, sem precisar esperar que o Copilot o execute.

## Adicione documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica atrasa o trabalho e dificulta a manutenção da base de código e a integração de novas pessoas à equipe. O Copilot é útil para criar documentação, então esta é uma boa issue para atribuir ao agente de nuvem. Ele trabalhará em segundo plano, e você revisará o resultado em um exercício futuro.

1. Em uma nova guia, acesse o repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer, aguarde um ou dois minutos e atualize o navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer no PR. O Copilot está criando o ambiente, executando o workflow descrito anteriormente, analisando o projeto e definindo a abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Percorra a linha do tempo do PR. Você verá uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema: explora o código e o estado do projeto, pausa para planejar e tomar decisões e, depois, cria o código.

O processo pode levar alguns minutos. Uma das principais vantagens do agente de nuvem é trabalhar de forma assíncrona, enquanto você se dedica a outras tarefas. Vamos aproveitar isso: atribua outra tarefa ao agente e, enquanto ele trabalha, crie recursos para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é dividir o trabalho: você pode se concentrar em uma tarefa enquanto ele cuida de outra. A seção de jogos relacionados talvez não demore muito, mas esse tempo pode ser usado em outras atividades. Vamos atribuí-la ao agente.

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem funciona de maneira semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o trabalho quando estiver concluído. Enquanto isso, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao Copilot para execução assíncrona e usá-lo para resolver dívida técnica, criar recursos ou migrar código entre frameworks.

Você explorou:

- personalizar o ambiente de geração de código.
- executar operações de forma segura.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas apresentadas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
