---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, como controles de alto contraste e modo claro. Vamos examinar as mudanças e, se necessário, enviar comentários para que o Copilot aprimore o trabalho.

### Cenário

Como já destacamos, os fundamentos do design de software e do DevOps não mudam com a IA generativa. Sempre é necessário revisar o código gerado e seguir o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de permitir que o restante da equipe revise os pull requests.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **apenas** à branch que usará para o código.
- O agente de nuvem executa as tarefas no GitHub Actions, em um ambiente separado e temporário.
- Uma pessoa precisa aprovar os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Você usará a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Talvez o PR mostre um aviso de que o Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá o sinalizador **[WIP]**. Aguarde até que o trabalho seja concluído. Isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as mudanças.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários TSDoc e outras documentações. As mudanças exatas variam.

   Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções personalizados examinados no início do Exercício 2.

6. Depois da revisão, volte à guia **Conversation** e percorra a página.
7. Procure um indicador informando que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão entrar na fila de verificações do PR. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça mudanças ao GitHub Copilot

O trabalho com o Copilot em um pull request não precisa ser unilateral. Você pode mencioná-lo em comentários do PR ou em comentários de linha no código, como faria com outras pessoas da equipe. O Copilot verá os comentários e iniciará outra sessão para resolvê-los. Como os resultados são não determinísticos, não há um texto específico obrigatório. Você pode pedir, por exemplo, que o Copilot:

- adicione cabeçalhos de comentário no início dos arquivos de código, explicando brevemente sua finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README descrevendo a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma mudança na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou outra solicitação relacionada à documentação.
2. Selecione **View Session** para acompanhar o trabalho. Observe que o Copilot inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR, se desejar.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as mudanças, um novo commit aparecerá no PR.
5. Selecione **Files changed** para revisar as alterações.

Continue fazendo iterações até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e integrá-lo à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, volte ao PR que o Copilot criou para mostrar jogos relacionados na página de detalhes.

1. Acesse o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações, volte à guia **Conversation** e percorra a página.
6. Procure o indicador de workflows aguardando aprovação.
7. Se houver workflows pendentes, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão entrar na fila de verificações do PR. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar para essa branch no Codespace e testar manualmente o recurso. Abra um terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode pedir atualizações ao agente de nuvem. Por exemplo, peça que ajuste a quantidade de jogos relacionados ou adicione cabeçalhos e comentários TSDoc. Lembre-se de que essa issue foi atribuída **antes** da atualização das instruções personalizadas. Para solicitar mudanças, adicione um comentário à guia **Conversation**; o Copilot o verá e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes no Exercício 4.

1. Acesse o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá o sinalizador **[WIP]**. Aguarde até que o trabalho seja concluído; isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código.
5. Examine a implementação, prestando atenção especial a:
   - controles de interface para alternar entre os modos.
   - armazenamento local para persistir as preferências.
   - mudanças de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância de modos.

6. Depois da revisão, volte à guia **Conversation** e percorra a página.
7. Procure o indicador de workflows aguardando aprovação.
8. Se houver workflows pendentes, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão entrar na fila de verificações do PR. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** mude para essa branch no Codespace para testar manualmente os recursos. Abra um terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador. Confirme que funcionam e que as preferências persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar os recursos de acordo com as práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para pedir mudanças, como fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando recebe tarefas reais do backlog enquanto você se dedica a outras atividades. Para praticar, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros às funções auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Durante as lições, você:

- **Examinou os arquivos de instruções personalizados do repositório** para observar depois o efeito deles no trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e o trabalho assíncrono.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel de controle** para acompanhar e orientar a sessão de acessibilidade durante o trabalho.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar aprendendo, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas mudanças revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e a ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outra área.
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
