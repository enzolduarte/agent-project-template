# Base de projeto com Matt Pocock Skills

Base reutilizável para trabalhar com Codex e Claude Code usando as skills de engenharia do pacote `mattpocock-skills`.

## Configuração incluída

- `AGENTS.md` orienta o Codex.
- `CLAUDE.md` orienta o Claude Code.
- `docs/agents/issue-tracker.md` configura tarefas e especificações em Markdown local, dentro de `.scratch/`.
- `docs/agents/triage-labels.md` mantém as cinco labels padrão.
- `docs/agents/domain.md` define a leitura de glossários e decisões arquiteturais.

Os três documentos em `docs/agents/` são cópias integrais dos modelos originais do setup do Matt Pocock, versão 1.3.2. O modelo `issue-tracker-local.md` foi copiado com o nome `issue-tracker.md`.

A base usa contexto único, com `GLOSSARY.md` na raiz e decisões em `docs/adr/`. Esses arquivos surgem conforme o domínio é definido. Sua ausência inicial permite prosseguir normalmente.

## Aplicar a um projeto

1. Extraia a base na raiz do projeto.
2. Se já houver `AGENTS.md` ou `CLAUDE.md`, incorpore o bloco `Agent skills` ao arquivo existente, preservando as outras instruções. Se o bloco já existir, atualize-o.
3. Se já houver arquivos em `docs/agents/`, compare e incorpore as configurações conforme o projeto.
4. Se já houver um README, mantenha-o e salve este guia como `docs/guia-matt-pocock.md`.
5. Tenha o plugin `mattpocock-skills` instalado em cada ferramenta que utilizar. A base fornece a configuração do projeto; as skills vêm do plugin. A instalação no Claude precisa ser conferida nesse ambiente.
6. Abra uma conversa no Codex ou Claude Code com a pasta do projeto como diretório de trabalho.

```text
meu-projeto/
├── AGENTS.md
├── CLAUDE.md
├── README.md
├── docs/agents/
│   ├── issue-tracker.md
│   ├── triage-labels.md
│   └── domain.md
└── ...código do projeto
```

Os arquivos de entrada apontam para a mesma documentação. Ao mudar as convenções, mantenha suas referências coerentes.

## Começar uma funcionalidade

Selecione a skill correspondente na interface, quando disponível, e descreva o objetivo. Os nomes exibidos podem incluir o prefixo do plugin.

Exemplo de pedido inicial:

> Use a skill `grill-with-docs` do mattpocock-skills. Quero desenvolver [sua ideia]. Leia as configurações de `docs/agents/` e me ajude a definir o comportamento e as decisões necessárias.

Se precisar escolher o caminho adequado:

> Use a skill `ask-matt` para indicar o fluxo adequado a esta tarefa.

## Fluxo de desenvolvimento

| Etapa | Skill | Resultado |
| --- | --- | --- |
| Escolher o caminho | `ask-matt` | Seleção das skills adequadas à situação. |
| Refinar a ideia | `grill-with-docs` | Perguntas para esclarecer a ideia, com registro de termos e decisões. |
| Especificar | `to-spec` | Síntese da conversa em uma especificação, com alinhamento dos pontos de teste. |
| Dividir o trabalho | `to-tickets` | Proposta de entregas pequenas e completas, com dependências, para aprovação. |
| Executar um ticket | `implement` | Implementação com TDD quando possível, verificações, revisão e commit na branch atual. |
| Executar a especificação | `implement-spec` | Agentes por ticket, respeitando dependências e integrando o trabalho em uma branch. |
| Preparar um PR | `pr` | Descrição com resumo visual, evidências e impacto da mudança, quando houver PR. |
| Fazer retrospectiva | `retro` | Propostas de melhoria nas instruções, ferramentas e verificações do projeto. |

### Alteração pequena

Conversa e definição do comportamento → `implement`.

### Funcionalidade com várias sessões

`grill-with-docs` → `to-spec` → `to-tickets` → `implement` por ticket ou `implement-spec` para coordenar a especificação inteira.

O fluxo de implementação inclui TDD e revisão. `code-review` verifica tanto os padrões do repositório quanto o atendimento à especificação. Essas skills também podem ser usadas separadamente.

### Dúvida que exige experimentação

Quando a conversa não resolve uma dúvida sobre comportamento, lógica ou interface, o fluxo original prevê um protótipo em outro diretório. Use `handoff` para levar o contexto, `prototype` para experimentar e outro `handoff` para trazer as conclusões à conversa original.

## Organização das tarefas locais

Exemplo de uma funcionalidade de login:

```text
.scratch/
└── login/
    ├── spec.md
    └── issues/
        ├── 01-autenticacao-basica.md
        ├── 02-recuperacao-de-senha.md
        └── 03-limite-de-tentativas.md
```

Cada ticket ocupa um arquivo próprio e declara suas dependências. Execute primeiro os tickets que desbloqueiam os demais.

As labels de triagem são `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human` e `wontfix`. Consulte `docs/agents/triage-labels.md` para os significados. O fluxo Wayfinder tem convenções próprias de mapa, tipos, dependências e estados `claimed` e `resolved`, documentadas em `docs/agents/issue-tracker.md`.

## Pedidos para o dia a dia

Depois de discutir a funcionalidade:

> Use a skill `to-spec` para transformar nossa conversa na especificação da funcionalidade.

Depois de aprovar a especificação:

> Use a skill `to-tickets` para dividir `.scratch/login/spec.md` em tickets com dependências.

Para executar um ticket:

> Use a skill `implement` para executar `.scratch/login/issues/01-autenticacao-basica.md`.

Para coordenar a funcionalidade completa:

> Use a skill `implement-spec` para executar `.scratch/login/spec.md` e seus tickets, respeitando as dependências.

Para uma retrospectiva:

> Use a skill `retro` para analisar esta sessão e propor melhorias no ambiente de desenvolvimento.

## Contexto entre sessões e ferramentas

Mantenha a discussão, a especificação e a divisão em tickets na mesma conversa, preservando o contexto até terminar `to-tickets`.

Depois disso, cada execução com `implement` pode começar em uma conversa nova, recebendo o caminho do ticket e da especificação. Execute `retro` na sessão que deseja analisar, antes de encerrar seu contexto.

Para continuar em outra ferramenta, abra a mesma pasta de projeto e indique a especificação, o ticket e o estado atual do trabalho. Os arquivos persistidos transportam as decisões entre Codex e Claude.

## Ajustes futuros

Edite `docs/agents/*.md` para adaptar as convenções ao projeto. O setup pode ser executado novamente para mudar o sistema de tarefas ou recomeçar a configuração.

A retenção de três versões por projeto será definida posteriormente. Esta base não implementa limpeza automática.
