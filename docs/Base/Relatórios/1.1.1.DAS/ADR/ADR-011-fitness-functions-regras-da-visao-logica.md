# ADR-011 — Regras da Visão Lógica verificadas por fitness function (dependency-cruiser) na CI

| Campo | Valor |
| --- | --- |
| Status | 🟢 Revisado |
| Data | 22/11/2026 |
| Autoria | SubEquipe_01: Matheus Lemes Amaral (extra E2, [código #18](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/18)) e Lucas Andrade Zanetti (extra E3, [código #19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/19)). **Rascunho preparado por Lucas com apoio de IA generativa; Matheus precisa conferir.** |
| Revisor | SubEquipe_02 e SubEquipe_03 — revisão concluída |
| Escopo | Como as regras de dependência RD1–RD7 da Visão Lógica deixam de ser só texto e passam a falhar a CI quando violadas. |
| Seção do DAS | [1.1.1.5. Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) · [1.1.1.8. Visão de Implementação](/Base/Relatórios/1.1.1.DAS/1.1.1.8.VisaoImplementacao.md) · [1.1.1.3. Metas e Restrições](/Base/Relatórios/1.1.1.DAS/1.1.1.3.MetasRestricoes.md) (MA-07) |

## 1. Contexto

O [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md) define sete regras de dependência (RD1–RD7), e o [ADR-005](/Base/Relatórios/1.1.1.DAS/ADR/ADR-005-monorepo-vs-repositorios-separados.md) coloca tudo no mesmo repositório, onde nada impede um `import` indevido. Na E03 isso já aconteceu: o ciclo `chat` ↔ `moderation` e as frentes da SG2 que se importavam ([EV-02](/Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt)). Uma Visão Lógica que ninguém verifica envelhece sozinha.

A pergunta: **como garantir que o código continua obedecendo às regras sem depender da atenção do revisor?**

## 2. Evidências

Commit de referência no código: [`73534e9`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/73534e9b76685d348798fc3b38b85944dda7879b) (`main` em 22/11/2026).

| Id | O que mostra | Fonte |
| --- | --- | --- |
| FF-01 | O PR das fitness functions (16 arquivos, +1076 linhas) foi mesclado em 20/11/2026; a issue #18 segue **aberta** porque o PR não usa `Closes #18`. | [PR #33](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/33) · [issue #18](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/18) |
| FF-02 | A CI roda `npm run verificar --prefix scripts/arquitetura` depois de typecheck e testes, em todo PR e em `push` na `main`. | [`ci.yml`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/.github/workflows/ci.yml) · [PR #37](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/37) |
| FF-03 | Há três regras, todas com severidade `error`: `contexto-so-importa-shared-kernel` (proibições RD3 e RD5), `shared-kernel-isolado` (RD4) e `g6-nao-resolvido`. RD1 e RD2 são permissões, sem o que proibir. | [`.dependency-cruiser.cjs`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/.dependency-cruiser.cjs) |
| FF-04 | A regra `g6-nao-resolvido` existe porque sem `npm ci` na raiz um `@g6/*` não resolve e escaparia das outras duas, dando falso verde. | comentário da regra no mesmo arquivo |
| FF-05 | Há um teste negativo: `npm run violacao` roda a verificação sobre uma pasta de violações propositais e **deve falhar com 6 erros**. | [`scripts/arquitetura/README.md`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/scripts/arquitetura/README.md) |
| FF-06 | **RD6 e RD7 não têm regra.** Nenhuma das três regras cobre `configuracao` ↔ `canal` nem o uso de `@g6/shared-kernel/testing` em código de produção. | `.dependency-cruiser.cjs` (só os três nomes acima) · achado 5 da [revisão da coordenação](/Base/Relatórios/1.1.1.DAS/ADR/README.md) |
| FF-07 | O script `npm run grafo` verifica as regras e, se passarem, gera o grafo de dependências SVG usado na Visão de Implementação. | [`scripts/arquitetura`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/73534e9b76685d348798fc3b38b85944dda7879b/scripts/arquitetura) |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| A | **Só revisão humana** (CODEOWNERS e template de PR) | Sem ferramenta | Depende de quem revisa lembrar RD1–RD7 em cada PR. Houve merges sem a revisão exigida (ver ADR-005). |
| B | **ESLint com restrição de imports** | Já comum em projetos TypeScript | Não gera o grafo de dependências para a Visão de Implementação (FF-07). *Não prototipada*; descartada por julgamento. |
| **C** | **dependency-cruiser** com regras por caminho, verificadas na CI | Regras declarativas, falha o PR, gera o grafo (FF-07), aceita teste negativo (FF-05) | Dependência extra em `scripts/arquitetura` (com `package-lock.json` próprio). Só vê imports estáticos. |
| D | **Referências de projeto do TypeScript** (`references`) | Fronteira imposta pelo compilador | Exige configurar e manter um `tsconfig` por pacote e muda o fluxo `tsx` sem build (ADR-005). *Não prototipada*. |
| E | **Biblioteca de testes de arquitetura** escrita como testes | Regra dentro da suíte de testes | Sem grafo, e mais um executor entre os três que o monorepo já tem. *Não prototipada*. |

## 4. Decisão

Adotar **C**, rodando o passo *Architecture* em toda PR, depois de typecheck e testes.

- Uma violação de regra falha a CI. Que isso bloqueie o merge depende da proteção de branch do GitHub, ainda não conferida (ver riscos).
- O teste negativo (`npm run violacao`) existe para provar que as regras **pegam** violações; o resultado esperado é falhar com 6 erros.
- RD6 e RD7 ficam **por revisão** até haver regra (FF-06). A revisão da coordenação pede uma regra por caminho para `src/configuracao` ↔ `src/canal` e outra para `testing` em `src/`; ainda não há issue aberta no código para isso.

## 5. Consequências

**Fica mais fácil:** descobrir um `import` proibido no PR e não na integração; manter a Visão Lógica e o grafo da Visão de Implementação coerentes com o código; defender a Visão Lógica diante da professora com um comando.

**Fica mais difícil / custo:**

- Instalar duas árvores de dependências na CI (`npm ci` na raiz e em `scripts/arquitetura`).
- A ferramenta enxerga só `import` estático: carregamento dinâmico (`import()` com variável, `require` computado) escapa.
- **A cobertura é parcial** (FF-06): as proibições RD3, RD4 e RD5 estão automatizadas; RD6 (a regra interna do `transmissao`, ADR-009) e RD7 dependem de revisão.

**Riscos e limites declarados:**

- Fitness function verde não prova que a arquitetura é boa; prova só que as regras automatizadas (RD3 a RD5) não foram violadas.
- A proteção só vale se a CI for exigida pela proteção de branch do GitHub. Isso não foi verificado nesta revisão e precisa ser conferido no repositório de código.
- `npm run verificar` não faz parte do `npm run check` da raiz; quem roda só `check` localmente não vê a violação antes do PR.

## 6. Referências

- FORD, Neal; PARSONS, Rebecca; KUA, Patrick. *Building Evolutionary Architectures*. O’Reilly, 2017. (fitness functions)
- Entrega 03: [ADR-001 a ADR-004](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/1.PadroesDeProjeto.md) e os acoplamentos herdados ([EV-02](/Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt)).
- Entrega 02: [diagrama de componentes da SubEquipe_01](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.1.SubEquipe_01/1.1.1.9.Extra.DiagramaComponentes.md).
- [ADR-005](/Base/Relatórios/1.1.1.DAS/ADR/ADR-005-monorepo-vs-repositorios-separados.md), [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md), [ADR-009](/Base/Relatórios/1.1.1.DAS/ADR/ADR-009-transmissao-contexto-dois-donos.md).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 0.1 | 22/11/2026 | Rascunho da decisão, alternativas A–E e evidências FF-01 a FF-07, preparado com apoio de IA generativa | Lucas Andrade Zanetti (rascunho), Matheus Lemes Amaral (autoria a confirmar) | 🟢 Revisado |
