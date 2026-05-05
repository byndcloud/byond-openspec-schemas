# byond-openspec-schemas

Coleção de schemas customizados para [OpenSpec](https://github.com/Fission-AI/OpenSpec), mantida pela Beyond Co.

OpenSpec é um framework para documentar e implementar mudanças de software de forma rastreável: você descreve o problema, gera os artefatos, revisa e só então implementa. Este repositório guarda os schemas que padronizam esse fluxo nos projetos da Beyond.

## Schemas disponíveis

| Schema             | Para quê                                                                                                | Artefatos                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `full-cycle-sdlc`  | Ciclo completo SDLC para mudanças críticas (regra de negócio, faturamento, permissões, refactors grandes). | `prd → sdd + bdd → tdd → code → mutation → review`     |

> O schema padrão do OpenSpec (`spec-driven`, com `proposal/design/tasks`) já vem instalado pela CLI e cobre a maioria das mudanças do dia a dia. Use os schemas deste repositório quando precisar de um fluxo mais rigoroso.

## Pré-requisitos

- Node.js **20.19+**
- OpenSpec CLI instalada:

  ```bash
  npm install -g @fission-ai/openspec@latest
  openspec --version
  ```

- Um projeto que já tenha `openspec init` rodado (existe a pasta `openspec/`).

## Instalação rápida

Na raiz do seu projeto:

```bash
npx degit byndcloud/byond-openspec-schemas/schemas/full-cycle-sdlc openspec/schemas/full-cycle-sdlc
```

Isso copia o schema para `openspec/schemas/full-cycle-sdlc/` no seu projeto.

Depois ative o schema no `openspec/config.yaml`:

```yaml
schema: full-cycle-sdlc
```

Pronto. A próxima `/opsx-propose` (ou `openspec new change`) vai gerar PRD, SDD, BDD, TDD, tasks, mutation e review na ordem correta.

## Métodos alternativos de instalação

### Via git clone

```bash
git clone https://github.com/byndcloud/byond-openspec-schemas /tmp/byond-openspec-schemas
cp -r /tmp/byond-openspec-schemas/schemas/full-cycle-sdlc openspec/schemas/full-cycle-sdlc
rm -rf /tmp/byond-openspec-schemas
```

### Via download direto

Baixe o ZIP do GitHub, extraia, e copie a pasta `schemas/full-cycle-sdlc/` para `openspec/schemas/full-cycle-sdlc/` no seu projeto.

## Como funciona o `full-cycle-sdlc`

```
┌────────┐
│  PRD   │  Problema, objetivo, não-escopo, critérios de aceite
└───┬────┘
    │
    ├─────────────┬──────────────┐
    ▼             ▼              │
┌────────┐   ┌────────┐          │
│  SDD   │   │  BDD   │  Em paralelo (ambos derivam só do PRD)
└───┬────┘   └───┬────┘          │
    │            │               │
    └─────┬──────┘               │
          ▼                      │
      ┌────────┐                 │
      │  TDD   │  Plano de testes ANTES do código
      └───┬────┘                 │
          ▼                      │
      ┌────────┐                 │
      │  Code  │  Implementação a partir do TDD
      └───┬────┘                 │
          ▼                      │
      ┌──────────┐               │
      │ Mutation │  Validação da força dos testes
      └────┬─────┘               │
           ▼                     │
      ┌────────┐                 │
      │ Review │  Decisão final  │
      └────────┘                 │
```

| Artefato       | Arquivo         | O que define                                                              |
| -------------- | --------------- | ------------------------------------------------------------------------- |
| **PRD**        | `prd.md`        | Problema, objetivo, atores, escopo, critérios de aceite, perguntas abertas.  |
| **SDD**        | `sdd.md`        | Arquitetura, contratos, dados, segurança, rollout, riscos.                |
| **BDD**        | `bdd.md`        | Cenários `Given/When/Then` derivados do PRD, prontos para QA e produto.   |
| **TDD**        | `tdd.md`        | Plano de testes — unit, integração, banco, edge, E2E, mutation, manual.   |
| **Code**       | `tasks.md`      | Checklist de implementação. Primeiras tarefas obrigatoriamente são testes. |
| **Mutation**   | `mutation.md`   | Resultado do mutation testing: score, mutantes, exceções.                 |
| **Review**     | `review.md`     | Decisão final do judge: pass, conditional-pass ou fail.                   |

A ordem é forçada pelo schema: você não pode gerar `tdd.md` sem ter `sdd.md` e `bdd.md`, nem rodar `apply` sem `tasks.md`.

## Adaptação ao seu stack

Os templates assumem uma stack específica (a stack mais comum dos produtos Beyond):

- **Frontend:** React + TypeScript + Vite, TanStack Query, React Hook Form + Zod, Tailwind/shadcn-ui
- **Banco:** Supabase / Postgres com RLS, RPCs e migrations versionadas
- **Testes:** Vitest, Playwright, pgTAP, Stryker

Se o seu projeto usa stack diferente, ajuste os arquivos `templates/*.md` após a instalação:

- `tdd.md`: troque referências a `pgTAP`, `npm run test:db`, `npx vitest run` pela ferramenta equivalente.
- `code.md`: ajuste comandos `npm run db:types`, `npm run lint` etc.
- `review.md`: substitua a checklist de regras Cursor (`code-review.mdc`, `frontend-patterns.mdc`...) pelas convenções do seu projeto.
- `mutation.md`: troque exemplos `npm run test:mutation:*` pelo seu setup.

Os esqueletos `prd.md`, `sdd.md` e `bdd.md` são quase 100% genéricos e raramente precisam de ajuste.

## Configuração recomendada do `openspec/config.yaml`

Veja [`examples/full-cycle-sdlc/config.yaml.example`](./examples/full-cycle-sdlc/config.yaml.example) para um arquivo pronto que você pode copiar e adaptar.

Para colocar a configuração em vigor:

```bash
cp examples/full-cycle-sdlc/config.yaml.example openspec/config.yaml
# e ajuste o bloco `context:` para refletir a stack do seu projeto
```

## Estrutura do repositório

```
byond-openspec-schemas/
├── README.md
├── CONTRIBUTING.md
├── LICENSE
├── .cursor/
│   └── skills/
│       └── openspec-create-schema/
│           ├── SKILL.md
│           └── reference.md
├── schemas/
│   └── full-cycle-sdlc/
│       ├── schema.yaml
│       └── templates/
│           ├── prd.md
│           ├── sdd.md
│           ├── bdd.md
│           ├── tdd.md
│           ├── code.md
│           ├── mutation.md
│           └── review.md
└── examples/
    └── full-cycle-sdlc/
        └── config.yaml.example
```

## Contribuindo um novo schema

Para o passo a passo completo, leia [`CONTRIBUTING.md`](./CONTRIBUTING.md). Resumo dos dois caminhos:

### Caminho A — via Cursor Skill (recomendado)

Este repositório inclui a skill [`openspec-create-schema`](./.cursor/skills/openspec-create-schema/SKILL.md), que coleta nome, descrição, artefatos e dependências, e gera todos os arquivos para você. Clone o repo, crie uma branch e peça à IA no Cursor:

```text
Use a skill openspec-create-schema para criar um schema chamado <nome>
```

A skill cria os arquivos. Você revisa, valida, commita e abre o PR.

### Caminho B — manual

Crie `schemas/<nome>/schema.yaml`, `schemas/<nome>/templates/*.md`, `examples/<nome>/config.yaml.example` e atualize a tabela "Schemas disponíveis" deste README. Veja o [`CONTRIBUTING.md`](./CONTRIBUTING.md) para detalhes e [`reference.md`](./.cursor/skills/openspec-create-schema/reference.md) da skill para a semântica completa de `schema.yaml`.

### Usar a skill fora do clone

Se você quer a skill disponível em qualquer projeto seu (não só dentro do clone), instale-a no projeto via `degit`:

```bash
npx degit byndcloud/byond-openspec-schemas/.cursor/skills/openspec-create-schema .cursor/skills/openspec-create-schema
```

A skill assume que está rodando a partir do clone do `byond-openspec-schemas`, então use-a quando estiver trabalhando para contribuir um schema novo.

### Validar localmente

```bash
# dentro de um projeto OpenSpec já inicializado
cp -r schemas/<nome> openspec/schemas/<nome>
# ative no openspec/config.yaml
openspec status
```

## Recursos

- [OpenSpec — repositório oficial](https://github.com/Fission-AI/OpenSpec)
- [OpenSpec — documentação](https://thedocs.io/openspec/)
- [Custom schemas — guia oficial](https://github.com/Fission-AI/OpenSpec/blob/main/docs/customization.md)

## Licença

[MIT](./LICENSE) — Beyond Co (byndcloud).
