# byond-openspec-schemas

Coleção de schemas customizados para [OpenSpec](https://github.com/Fission-AI/OpenSpec), mantida pela Beyond Co.

OpenSpec é um framework para documentar e implementar mudanças de software de forma rastreável: você descreve o problema, gera os artefatos, revisa e só então implementa. Este repositório guarda os schemas que padronizam esse fluxo nos projetos da Beyond.

## Schemas disponíveis

| Schema             | Para quê                                                                                                | Artefatos                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `full-cycle-sdlc`  | Ciclo completo SDLC para mudanças críticas (regra de negócio, faturamento, permissões, refactors grandes). | `prd → specs → sdd + bdd → tdd → code → mutation → review` |

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

### Schema

Na raiz do seu projeto:

```bash
npx degit byndcloud/byond-openspec-schemas/schemas/full-cycle-sdlc openspec/schemas/full-cycle-sdlc
```

Isso copia o schema para `openspec/schemas/full-cycle-sdlc/` no seu projeto.

Depois ative o schema no `openspec/config.yaml`:

```yaml
schema: full-cycle-sdlc
```

Pronto. A próxima `/opsx-propose` (ou `openspec new change`) vai gerar PRD, Specs, SDD, BDD, TDD, tasks, mutation e review na ordem correta.

### Comandos `/opsx-*` (Cursor)

Os comandos que controlam o ciclo completo (`/opsx-propose`, `/opsx-apply`, `/opsx-explore`, `/opsx-archive`) também ficam neste repositório e podem ser instalados via `degit`:

```bash
npx degit byndcloud/byond-openspec-schemas/.cursor/commands .cursor/commands
```

> **Nota:** isso sobrescreve qualquer `.cursor/commands/` existente. Se você já tem comandos customizados, copie os arquivos `opsx-*.md` individualmente:
>
> ```bash
> npx degit byndcloud/byond-openspec-schemas/.cursor/commands/opsx-propose.md .cursor/commands/opsx-propose.md
> npx degit byndcloud/byond-openspec-schemas/.cursor/commands/opsx-apply.md .cursor/commands/opsx-apply.md
> npx degit byndcloud/byond-openspec-schemas/.cursor/commands/opsx-explore.md .cursor/commands/opsx-explore.md
> npx degit byndcloud/byond-openspec-schemas/.cursor/commands/opsx-archive.md .cursor/commands/opsx-archive.md
> ```

| Comando | Para quê |
| --- | --- |
| `/opsx-propose` | Criar uma change e gerar todos os artefatos em um passo |
| `/opsx-apply` | Implementar as tasks de uma change |
| `/opsx-explore` | Modo de pensamento — explorar ideias antes de propor |
| `/opsx-archive` | Arquivar uma change concluída via CLI (sincroniza delta specs na canônica) |

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
    ▼
┌────────┐
│ Specs  │  Delta spec por capability (contra a canônica)
└───┬────┘
    │
    ├─────────────┬──────────────┐
    ▼             ▼              │
┌────────┐   ┌────────┐          │
│  SDD   │   │  BDD   │  Em paralelo (ambos derivam dos Specs)
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

| Artefato       | Arquivo                                | O que define                                                                          |
| -------------- | -------------------------------------- | ------------------------------------------------------------------------------------- |
| **PRD**        | `prd.md`                               | Problema, objetivo, atores, escopo, critérios de aceite, perguntas abertas, classificação da mudança e política de E2E baseline. |
| **Specs**      | `specs/<capability>/spec.md`           | Delta spec contra a canônica em `openspec/specs/<capability>/spec.md`, com ADDED/MODIFIED/REMOVED/RENAMED Requirements. |
| **SDD**        | `sdd.md`                               | Arquitetura, contratos, dados, segurança, rollout, riscos.                            |
| **BDD**        | `bdd.md`                               | Cenários `Given/When/Then` derivados do PRD e dos Specs, prontos para QA e produto.   |
| **TDD**        | `tdd.md`                               | Plano de testes — unit, integração, banco, edge, E2E, mutation, manual.               |
| **Code**       | `tasks.md`                             | Checklist de implementação. Primeiras tarefas obrigatoriamente são testes (e baseline E2E quando bloqueante). |
| **Mutation**   | `mutation.md`                          | Resultado do mutation testing: score, mutantes, exceções.                             |
| **Review**     | `review.md`                            | Decisão final do judge: pass, conditional-pass ou fail.                               |

A ordem é forçada pelo schema: você não pode gerar `tdd.md` sem ter `sdd.md` e `bdd.md`, nem rodar `apply` sem `tasks.md`.

## Adaptação ao seu stack

Os templates assumem uma stack específica (a stack mais comum dos produtos Beyond):

- **Frontend:** React + TypeScript + Vite, TanStack Query, React Hook Form + Zod, Tailwind/shadcn-ui
- **Banco:** Supabase / Postgres com RLS, RPCs e migrations versionadas
- **Testes:** Vitest, Playwright, pgTAP, Stryker

Se o seu projeto usa stack diferente, ajuste os arquivos `templates/*.md` após a instalação:

- `sdd.md`: remova ou substitua as seções `Database / Supabase Design` e `Edge Function Design` pelo equivalente da sua stack.
- `tdd.md`: troque referências a `pgTAP`, `npm run test:db`, `npx vitest run` pela ferramenta equivalente.
- `code.md`: ajuste comandos `npm run db:types`, `npm run lint`, etc.
- `review.md`: substitua a checklist de regras Cursor (`code-review.mdc`, `frontend-patterns.mdc`...) pelas convenções do seu projeto.
- `mutation.md`: troque exemplos `npm run test:mutation:*` pelo seu setup.

Os esqueletos `prd.md`, `spec.md` e `bdd.md` são quase 100% genéricos e raramente precisam de ajuste.

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
├── docs/
│   └── schema-reference.md          ← semântica completa de schema.yaml
├── .cursor/
│   ├── commands/
│   │   ├── opsx-propose.md          ← criar change + artefatos
│   │   ├── opsx-apply.md            ← implementar tasks
│   │   ├── opsx-explore.md          ← modo de exploração
│   │   └── opsx-archive.md          ← arquivar via CLI (fix: usa `openspec archive`, não `mv`)
│   └── skills/
│       ├── openspec-create-schema/
│       │   └── SKILL.md             ← criar um schema novo
│       └── openspec-update-schema/
│           └── SKILL.md             ← atualizar um schema existente
├── schemas/
│   └── full-cycle-sdlc/
│       ├── schema.yaml
│       └── templates/
│           ├── prd.md
│           ├── spec.md
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

## Contribuindo

O repositório expõe duas Cursor Skills para acelerar contribuições:

| Skill | Para quê | Documento |
| --- | --- | --- |
| `openspec-create-schema` | Criar um schema novo do zero. | [SKILL.md](./.cursor/skills/openspec-create-schema/SKILL.md) |
| `openspec-update-schema` | Modificar um schema existente (artefatos, dependências, templates, version bump). | [SKILL.md](./.cursor/skills/openspec-update-schema/SKILL.md) |

Resumo do uso:

```text
Use a skill openspec-create-schema para criar um schema chamado <nome>
Use a skill openspec-update-schema para adicionar `security` ao schema full-cycle-sdlc
```

Ambas as skills criam/editam arquivos. **Você** revisa, valida, commita e abre o PR.

Para o passo a passo completo (incluindo o caminho manual sem skills, convenções de naming, validação e regras de bump de versão), leia [`CONTRIBUTING.md`](./CONTRIBUTING.md).

Para a semântica completa de `schema.yaml` — campos, dependências, padrões de templates e erros comuns — veja [`docs/schema-reference.md`](./docs/schema-reference.md).

### Usar as skills fora do clone

Se você quer as skills disponíveis em qualquer projeto seu (não só dentro do clone do `byond-openspec-schemas`), instale via `degit`:

```bash
npx degit byndcloud/byond-openspec-schemas/.cursor/skills/openspec-create-schema .cursor/skills/openspec-create-schema
npx degit byndcloud/byond-openspec-schemas/.cursor/skills/openspec-update-schema .cursor/skills/openspec-update-schema
```

As skills assumem que estão rodando a partir do clone do `byond-openspec-schemas`, então elas se aplicam quando você está trabalhando para contribuir.

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
