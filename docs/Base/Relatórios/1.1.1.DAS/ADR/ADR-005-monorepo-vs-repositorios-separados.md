# ADR-005 — Monorepo com npm workspaces em vez de três repositórios

| Campo | Valor |
| --- | --- |
| Status | Proposta, aguardando revisão cruzada |
| Data | 22/11/2026 |
| Autoria | Lucas Andrade Zanetti (SubEquipe_01), autor definido na [issue #19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19); redigido com apoio de IA generativa |
| Revisor | Pendente: SubEquipe_02 (revisão cruzada) |
| Escopo | Como o código das três subequipes da Entrega 03 passa a viver em um único repositório executável. Registra uma decisão já aplicada em 09/11/2026 ([PR #24 do código](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/24)). |
| Seção do DAS | [1.1.1.2. Representação Arquitetural](/Base/Relatórios/1.1.1.DAS/1.1.1.2.RepresentacaoArquitetural.md) · [1.1.1.3. Metas e Restrições](/Base/Relatórios/1.1.1.DAS/1.1.1.3.MetasRestricoes.md) (RT-02) · [1.1.1.8. Visão de Implementação](/Base/Relatórios/1.1.1.DAS/1.1.1.8.VisaoImplementacao.md) |

## 1. Contexto

A Entrega 03 terminou com **três repositórios independentes**, um por subequipe, cada um com o próprio padrão de testes. A Entrega 04 pede “algo rodando” (cenário **CA-01**), e o CA-01 atravessa os três: login (SG3) → live (SG1 + SG3) → recarga e doação (SG2) → chat moderado (SG3) → VOD (SG1).

| Origem na E03 | Padrões | Executor de testes |
| --- | --- | --- |
| `g6-streaming-entrega03-subgrupo01-codigo` | Builder, Factory Method | Vitest |
| `2026.2-T01-_G6_ProjetoStreamingVideo_SubEquipe02_Codigo` | Decorator, Adapter, Proxy, Facade, Mediator | Jest |
| `g6-streaming-entrega03-subgrupo03-codigo` | Observer, Chain of Responsibility, AutoMod | `node:test` |

A pergunta: **os três códigos continuam em três repositórios, ligados de algum modo, ou passam a morar em um só?**

## 2. Evidências

Commit de referência no código: [`73534e9`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/73534e9b76685d348798fc3b38b85944dda7879b) (`main` em 22/11/2026).

| Id | O que mostra | Fonte |
| --- | --- | --- |
| M-01 | O monorepo declara `workspaces: ["packages/*", "apps/*"]`; cada contexto é um pacote `@g6/*` e só `apps/*` os compõem. | [`package.json`, linhas 10–13](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/package.json#L10-L13) |
| M-02 | O scaffold entrou em um PR de 64 arquivos, aberto e mesclado em 09/11/2026, o dia da reunião de abertura. | [PR #24](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/24) · [issue #2](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/2) |
| M-03 | A origem de cada módulo fica registrada em um commit `chore(<pacote>): import from <repo>@<sha>`, separado da adaptação. Sete importações estão na `main`: `monetizacao/destaque` ([`1c296d5`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/1c296d5)), `monetizacao/recarga` ([`5d43dd0`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/5d43dd0)), `monetizacao/doacao` ([`2dac290`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/2dac290)), `transmissao/canal` ([`98eb0a5`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/98eb0a5)), `moderacao` ([`afb982f`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/afb982f)), `transmissao/configuracao` ([`c1507b5`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/c1507b5)) e `conteudo` ([`43e95e3`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/43e95e3)). `identidade` entrou por [`fb2c8bd`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/fb2c8bd). | `git log --grep "import from"` no código |
| M-04 | Cada pacote mantém o executor de testes da E03; o README lista Vitest, Jest e `node:test` lado a lado, e o Node ≥ 22 é exigido pelo código da SubEquipe_01. | [`README.md`, seção “Estrutura”](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/README.md) |
| M-05 | Uma só CI (typecheck, testes e regras de arquitetura) roda em todo PR, sobre todos os pacotes. | [`ci.yml`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/.github/workflows/ci.yml) |
| M-06 | A propriedade por caminho está no `CODEOWNERS` (um dono por pacote e por subpasta de `transmissao` e `monetizacao`). | [`CODEOWNERS`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/CODEOWNERS) |
| M-07 | O guia fixado prevê o custo do arquivo compartilhado: em conflito no `package-lock.json`, aceita-se o da `main` e roda-se `npm install`. | [guia #1, regra 6](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/1) |
| M-08 | O código da E03 já tinha acoplamentos entre módulos (ciclo `chat` ↔ `moderation` na SG3; frentes da SG2 importando umas às outras). Juntar tudo sem regra de dependência os traria para dentro do monorepo. | [EV-02](/Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt) |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| A | **Manter três repositórios** e integrar por `git submodule` ou dependência `git+https` | Preserva o histórico e a autoria de cada repositório da E03 | Rodar o CA-01 exige clonar três repositórios na versão certa. Mudança de contrato vira três PRs coordenados. Não há CI única, e nenhuma ferramenta enxerga as dependências entre pacotes. |
| B | **Três repositórios e pacotes publicados** em um registro (npm ou GitHub Packages) | Versionamento explícito e fronteira forte entre subequipes | Publicar a cada ajuste em 14 dias e manter credenciais de registro. O trabalho em paralelo ficaria esperando versões publicadas. É o oposto de “desenvolver contra fakes” (ADR-008). |
| **C** | **Um monorepo com npm workspaces**: `packages/*` (contextos) e `apps/*` (composição) | Um clone, um `npm install`, uma CI. `@g6/*` resolve por *symlink* de workspace, sem publicar nada. Regras por caminho (CODEOWNERS e dependency-cruiser, ADR-011) funcionam. | Perde o histórico git da E03 (a origem fica só na mensagem do commit de importação). Conflito no `package-lock.json` (M-07). |
| D | **Monorepo com orquestrador** (Nx, Turborepo ou pnpm) | Cache de build e execução de tarefas só nos pacotes afetados | Ferramenta nova para um grafo de 9 pacotes que roda em segundos e **não tem etapa de build** (o código roda com `tsx`). Custo de aprendizado sem ganho medido. *Não prototipada*: descartada por julgamento de escopo. |
| E | **Um único pacote**, com uma pasta por contexto e um só `package.json` | Mais simples ainda | A fronteira de contexto deixa de ser fronteira de pacote: nada impede `import` entre pastas, e dependências dos três códigos se misturam. Perde a leitura por pacote na [Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md). |

## 4. Decisão

Adotar **C**.

- **Estrutura:** `packages/<contexto>` para os seis contextos e o `shared-kernel`; `apps/cli-demo` e `apps/web` para a composição (RD1 da Visão Lógica).
- **Sem etapa de build:** os pacotes expõem o próprio TypeScript (`exports` → `src/index.ts`) e rodam com `tsx`, como nos três repositórios da E03. A verificação de tipos é `tsc --noEmit` em cada workspace.
- **Importação em dois commits:** primeiro a cópia literal (`chore(<pacote>): import from <repo>@<sha>`), depois a adaptação. A autoria do trabalho novo da E04 fica legível no quadro de [participações](/Base/1.2.Participacoes.md), e a origem de cada linha volta ao repositório da E03 pelo `sha`.
- **Executor de testes por pacote:** cada contexto mantém Vitest, Jest ou `node:test` (RT-07); o que une os três é a suíte de contrato do kernel.

## 5. Consequências

**Fica mais fácil:** executar o CA-01 com `npm install` e `npm run demo` ou `npm run web`; mudar um port e seus consumidores em um único PR; verificar regras de arquitetura por caminho (ADR-011); manter uma só CI.

**Fica mais difícil / custo:**

- **Histórico da E03 não entra no grafo git.** A rastreabilidade para as entregas anteriores depende da disciplina dos commits `import from` (M-03). Um commit de importação sem o `sha` de origem quebraria o elo.
- **Três executores no mesmo `npm test`** (M-04): instalar as três ferramentas e esperar a soma dos tempos. É o preço de não reescrever os testes da E03.
- **Arquivo compartilhado:** o `package-lock.json` é inevitavelmente compartilhado (M-07).
- **Propriedade por caminho só vale com revisão obrigatória.** O `CODEOWNERS` pede revisão de quem é dono, mas houve ao menos um merge sem a aprovação exigida, depois revertido ([`be46d45`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/be46d45)). A regra de proteção de branch precisa ser conferida no GitHub (ver pendências no [Planejamento](/Projeto/Planejamento.md)).

**Riscos e limites declarados:**

- Não foi medido o custo de instalação nem de CI do monorepo; a escolha se apoia na simplicidade, não em medição.
- Um monorepo não impede o acoplamento: ele só o torna visível. A proteção vem das regras RD1–RD7, verificadas por ferramenta (ADR-011), e do contrato (ADR-008).

## 6. Referências

- Entrega 03: três repositórios de código por subequipe e [ADR-001 a ADR-004](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/1.PadroesDeProjeto.md).
- Entrega 02: [diagrama de componentes da SubEquipe_01](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.1.SubEquipe_01/1.1.1.9.Extra.DiagramaComponentes.md), origem das interfaces entre componentes que viraram ports.
- [Ata da Reunião Geral 1](/Projeto/Atas/ata-G01-entrega-04.md), D09 (formatação dos repositórios) e D07 (TypeScript mantido).
- [Planejamento da Entrega 04](/Projeto/Planejamento.md), seção 2 (arquitetura-alvo).
- NPM. *Workspaces*. Documentação do npm CLI. Disponível em: <https://docs.npmjs.com/cli/using-npm/workspaces>.
- [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md), [ADR-008](/Base/Relatórios/1.1.1.DAS/ADR/ADR-008-shared-kernel-vs-anticorruption-layer.md), [ADR-011](/Base/Relatórios/1.1.1.DAS/ADR/ADR-011-fitness-functions-regras-da-visao-logica.md).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 22/11/2026 | Registro da decisão, alternativas A–E e evidências M-01 a M-08, redigido com apoio de IA generativa | Lucas Andrade Zanetti | Pendente (SubEquipe_02) |
