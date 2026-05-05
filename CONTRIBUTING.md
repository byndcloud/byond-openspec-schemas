# Contribuindo

Obrigado por considerar contribuir com `byond-openspec-schemas`. Este repositório guarda os schemas customizados de [OpenSpec](https://github.com/Fission-AI/OpenSpec) usados nos projetos da Beyond.

Você pode contribuir com:

- Um **novo schema** (workflow OpenSpec) — descrito neste documento.
- Uma **correção** em schema existente (template, instrução, dependência) — abra um PR direto.
- Um **exemplo de configuração** novo em `examples/<schema>/`.

Para discussões maiores ou propostas de schemas controversos, abra uma issue antes de gastar tempo no PR.

## Pré-requisitos

- Node.js **20.19+**
- OpenSpec CLI: `npm install -g @fission-ai/openspec@latest`
- Git
- (Opcional, para o caminho com skill) Um editor com suporte a [Cursor Agent Skills](https://docs.cursor.com).

## Como contribuir um novo schema

Existem dois caminhos. Escolha o que fizer mais sentido para você.

### Caminho A — via Cursor Skill (recomendado)

Este repositório inclui a skill [`openspec-create-schema`](./.cursor/skills/openspec-create-schema/SKILL.md), que automatiza a criação dos arquivos.

1. Clone o repositório e crie uma branch:

   ```bash
   git clone https://github.com/byndcloud/byond-openspec-schemas
   cd byond-openspec-schemas
   git checkout -b add-<nome-do-schema>-schema
   ```

2. Abra o repositório no Cursor. A skill já fica disponível porque vive em `.cursor/skills/`.

3. No chat, peça à IA:

   ```text
   Use a skill openspec-create-schema para criar um schema chamado <nome>.
   ```

4. A skill vai pedir:
   - **nome** (kebab-case);
   - **descrição** (uma frase);
   - **lista de artefatos** em ordem de build;
   - **dependências** entre artefatos;
   - **artefato de implementação** (geralmente `tasks` ou `code`).

5. A skill cria:
   - `schemas/<nome>/schema.yaml`
   - `schemas/<nome>/templates/<artefato>.md` (um por artefato)
   - `examples/<nome>/config.yaml.example`
   - atualiza a tabela "Schemas disponíveis" no README.

6. Revise os arquivos, valide localmente (ver seção [Validar localmente](#validar-localmente)), commite e abra o PR.

> A skill **não** commita, faz push ou abre PR. Você fica no controle.

### Caminho B — manual

Faça o que a skill faria, na mão.

1. Clone o repositório e crie uma branch:

   ```bash
   git clone https://github.com/byndcloud/byond-openspec-schemas
   cd byond-openspec-schemas
   git checkout -b add-<nome-do-schema>-schema
   ```

2. Crie a estrutura:

   ```
   schemas/<nome>/
   ├── schema.yaml
   └── templates/
       ├── <artefato-1>.md
       ├── <artefato-2>.md
       └── ...
   examples/
   └── <nome>/
       └── config.yaml.example
   ```

3. Use os arquivos de [`schemas/full-cycle-sdlc/`](./schemas/full-cycle-sdlc/) como referência:
   - `schema.yaml` mostra como declarar `artifacts`, `requires` e `apply`.
   - `templates/*.md` mostram o padrão "headings + Judge Gate" de cada template.

4. Atualize o `README.md`: adicione uma linha na tabela "Schemas disponíveis".

5. Valide localmente.

6. Commite e abra o PR.

Para o detalhamento dos campos de `schema.yaml`, padrões de dependência, regras de templates e erros comuns, veja [`docs/schema-reference.md`](./docs/schema-reference.md).

## Como atualizar um schema existente

Quando você precisa **modificar** um schema que já está no repo (adicionar artefato, mudar dependências, atualizar template, bumpar version), também há dois caminhos.

### Caminho A — via Cursor Skill (recomendado)

Use a skill [`openspec-update-schema`](./.cursor/skills/openspec-update-schema/SKILL.md):

```text
Use a skill openspec-update-schema para adicionar o artefato `security` ao schema full-cycle-sdlc
```

A skill identifica o tipo de mudança (add/remove/rename artifact, change dependencies, update template, etc.), aplica as edições, valida o grafo de dependências e bumpa `version` automaticamente quando a mudança é breaking. Igual à `create-schema`, **não commita nem abre PR**.

### Caminho B — manual

1. Edite `schemas/<nome>/schema.yaml`.
2. Edite/adicione/remova templates em `schemas/<nome>/templates/`.
3. Bumpe `version: <int>` se a mudança for breaking (ver [Bumping de versão](#bumping-de-versão)).
4. Atualize a linha do schema no `README.md` se a descrição ou lista de artefatos mudou.
5. Valide localmente, commite e abra PR.

Veja também [`docs/schema-reference.md`](./docs/schema-reference.md) — seção "Common mistakes" lista os erros mais comuns ao editar um schema.

## Validar localmente

Antes de abrir o PR, valide o schema em um projeto OpenSpec real:

```bash
# em um projeto qualquer com openspec init já rodado:
cp -r schemas/<nome> /caminho/para/projeto/openspec/schemas/<nome>
# editar /caminho/para/projeto/openspec/config.yaml e setar:
#   schema: <nome>

cd /caminho/para/projeto
openspec status
```

Erros estruturais (referências quebradas, ciclos em `requires`, `apply.requires` apontando para artefato inexistente) aparecem aqui.

Opcionalmente, gere uma mudança de teste:

```bash
openspec new change "test-<nome>"
openspec status --change "test-<nome>"
```

Confirme que os artefatos esperados aparecem na ordem correta. Apague a mudança de teste depois.

## Convenções

| Item | Convenção |
| --- | --- |
| Nome do schema | `kebab-case`, descritivo (`event-driven`, não `ed`) |
| Nome do artefato (`id`) | `kebab-case` curto (`prd`, `tdd`, `events`) |
| Arquivo gerado | `<artifact-id>.md` |
| Arquivo de template | `<artifact-id>.md` |
| Idioma dos templates | EN para reuso amplo; PT-BR só se o schema for explicitamente Beyond-only |
| Idioma do README e CONTRIBUTING | PT-BR |
| Mensagem de commit | [Conventional Commits](https://www.conventionalcommits.org/) (`feat: add event-driven schema`) |
| Branch | `add-<nome>-schema`, `fix-<schema>-<artefato>`, `docs-<o-que>` |

## Boas práticas para templates

- Cada template deve terminar em `## Judge Gate` listando o que precisa estar verdadeiro antes de considerar o artefato pronto.
- Templates devem ser headings-only quando possível. Deixe a IA preencher o corpo, não escreva placeholders verbosos.
- Evite linguagem stack-specific (Supabase, pgTAP, Vitest, etc.) em templates de schemas que pretendem ser genéricos. Coloque essas decisões no `context:` do `config.yaml.example`.
- Use 2-4 regras por artefato no bloco `rules:` do exemplo. Mais que isso vira ruído.

## Bumping de versão

Schemas têm um campo `version: <int>` em `schema.yaml`. Bump quando:

- Adicionar/remover/renomear um artefato existente.
- Mudar `apply.requires` de forma que projetos que já usam o schema parem de validar.
- Mudar `requires` de um artefato.

Não precisa bumpar para:

- Ajustes em templates.
- Mudanças de wording em `instruction:`.
- Adição de exemplos.

## Pull Request

Ao abrir o PR:

1. Use um título descritivo: `feat: add <nome> schema` ou `docs: improve <schema> README`.
2. Na descrição, inclua:
   - **Objetivo** do schema (quando vale a pena usar).
   - **Artefatos** e o ciclo (ex.: `events → specs → design → tasks`).
   - **Stack assumida**, se houver.
   - **Validação**: confirme que rodou `openspec status` em um projeto real.
3. Marque alguém da Beyond para review.

Se o schema é experimental ou ainda em discussão, abra como **Draft PR**.

## Código de conduta

Trate o repositório como qualquer outro repositório público da Beyond. Comunique-se com clareza e respeito. Se discordar de uma decisão, abra uma issue ou comente no PR antes de reverter alguém.

## Dúvidas

Abra uma issue ou pergunte no canal interno da Beyond sobre OpenSpec.
