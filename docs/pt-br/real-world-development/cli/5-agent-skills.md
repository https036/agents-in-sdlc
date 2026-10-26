---
title: "Lição 5 - Personalize e use uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar envolve mais do que apenas escrever código. Já verificamos manualmente se o código funciona e usamos arquivos de instruções para garantir que siga nossos padrões. Mas e os testes, o lint e as outras etapas da integração contínua (CI)?

Para esse tipo de tarefa, **skills de agente** são ideais. As skills ajudam o Copilot a entender como executar operações desse tipo.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para executar testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega quando necessário. Cada skill é uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. Esse arquivo contém um frontmatter com o nome e a descrição, uma visão geral do que a skill faz e orientações sobre quando usá-la. A pasta também pode incluir subpastas com scripts e outros recursos que a skill utiliza.

> [!NOTE]
> Uma skill não precisa ter outros arquivos ou pastas. A skill `quality-checks` da Tailspin Toys contém somente `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto, como recursos do repositório que a equipe compartilha e reutiliza, ou na pasta de skills do usuário, em `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificações de tipos: `quality-checks`.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando deve usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e o lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Execute a skill antes de fazer mudanças

As skills podem ser chamadas diretamente pelo Copilot CLI ou por linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa sobre filtragem no modo Interactive.
2. Use este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalize o relatório

Queremos um relatório melhor, que informe o que foi executado, se houve sucesso e o que as ferramentas relataram. Vamos atualizar a skill para gerar esse relatório.

1. Volte para `.github/skills/quality-checks/SKILL.md`.
2. Adicione esta seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

Agora que fizemos a mudança, vamos conferir o resultado. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa mudança acompanhará a filtragem no PR do recurso. Na próxima lição, você pedirá ao Copilot que interaja diretamente com o site [usando o servidor MCP do Playwright][next-lesson].

## Outros exemplos de skills

Estes exemplos da comunidade são referências, não atividades adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e um script de exportação: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
