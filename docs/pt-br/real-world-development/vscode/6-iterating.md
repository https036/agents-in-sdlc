---
title: "Exercício 6 - Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário. Usou o modo de agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente o recurso no navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — controles de alto contraste e modo claro. Você também orientou a sessão em andamento para ampliar o trabalho. Agora, publique as mudanças locais e revise-as como sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. É importante ter um ciclo real de revisão para tudo o que o Copilot produz. Vamos enviar as mudanças de acessibilidade do Codespace ao repositório remoto, abrir um pull request e revisar o diff antes de envolver o restante da equipe.

## Publique os recursos de acessibilidade

Os controles de alto contraste e modo claro implementados com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no Codespace como mudanças já registradas em commits. Vamos enviá-las para uma branch do repositório remoto e abrir um pull request para que a equipe possa revisar.

1. Volte ao Codespace.
2. Abra a exibição **Source Control** no VS Code.
3. Confirme que as mudanças de acessibilidade estão registradas em commits. Se houver mudanças do Exercício 5 ainda sem commit, prepare-as e faça o commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** ou usando o menu **...** → **Push**.
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite a solicitação ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro, por exemplo `Add high-contrast and light-mode toggles`, e uma descrição curta que resuma o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Depois de abrir o PR, selecione a guia **Files changed** para revisar todo o trabalho. Preste atenção especial a:
   - componentes de interface dos controles de alternância dos modos.
   - uso do armazenamento local para persistir as preferências.
   - mudanças de CSS ou estilos dos modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância dos modos.

9. Volte à guia **Conversation**.
10. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows. Se tudo correr bem, eles serão aprovados.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário do PR e peça algo como "revise este PR em busca de outros problemas relacionados às WCAG" ou "sugira melhorias na navegação por teclado". O Copilot iniciará outra sessão para responder ao comentário.

## Exercício opcional - Continue explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade que se desenvolve com a prática. Experimente estas ideias em outras sessões no VS Code:

- adicione um formulário de interesse de apoiadores à página de detalhes do jogo.
- implemente paginação na página de lista de jogos.
- adicione validação de entrada e tratamento de erros às funções auxiliares de acesso a dados em `src/lib/`.
- amplie o escopo do agente de acessibilidade, por exemplo, auditando a ordem de foco do teclado no site inteiro.

## Resumo

Parabéns, você concluiu o percurso do VS Code. Durante este laboratório, você:

- **Usou o MCP do Playwright para testar manualmente o recurso.** Adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a filtragem antes de abrir um pull request.
- **Conduziu o modo de agente em mudanças coordenadas em toda a pilha.** Adicionou um recurso de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Selecionou o agente personalizado de acessibilidade na lista de agentes e acompanhou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Revisou as mudanças propostas no editor, aceitou o que queria e ampliou a sessão com uma solicitação para adicionar o modo claro.
- **Concluiu o ciclo com um pull request.** Publicou o trabalho local e o revisou de ponta a ponta, como sua equipe faria.

## Revisão e próximos passos

Este é o fim do percurso obrigatório do VS Code. Você pode parar aqui com o workshop concluído.

Para ampliar sua compreensão dos recursos de agente do Copilot, os outros percursos apresentam cenários relacionados em interfaces diferentes:

- 💻 **[Percurso da CLI](../../cli/)** — execute fluxos semelhantes no terminal com o Copilot CLI: modo de planejamento, skills de agente, agentes personalizados e comandos de barra, como `/share`, `/context` e `/delegate`.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — atribua issues ao agente de nuvem, acompanhe sessões na página de agentes e faça iterações assíncronas em pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma boa fonte de arquivos de instruções, agentes personalizados e skills que podem ser adaptados aos seus projetos.

Como extensão opcional, [Incorpore o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
