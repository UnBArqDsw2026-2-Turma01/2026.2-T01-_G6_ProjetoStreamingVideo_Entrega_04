# ADRs da Entrega 04 (extra E1)

> **Coordenação:** Pedro Druck Montalvão Reis · **Issue:** [#19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19) · **Status:** 🟢 ADR-005 a ADR-011 revisados

Continuação da série da Entrega 03. Cada ADR registra contexto, alternativas, decisão e consequências (NYGARD, 2011), e é escrito por quem tomou a decisão; a coordenação confere o formato e as evidências contra o código antes da revisão cruzada.

## Índice

| ADR | Decisão | Autor | Seções do DAS que dependem dele | Status |
| --- | --- | --- | --- | --- |
| [ADR-005](/Base/Relatórios/1.1.1.DAS/ADR/ADR-005-monorepo-vs-repositorios-separados.md) | Monorepo vs. repositórios separados | Lucas Andrade Zanetti | [Metas e Restrições](/Base/Relatórios/1.1.1.DAS/1.1.1.3.MetasRestricoes.md) (RT-02), [Representação Arquitetural](/Base/Relatórios/1.1.1.DAS/1.1.1.2.RepresentacaoArquitetural.md), [Visão de Implementação](/Base/Relatórios/1.1.1.DAS/1.1.1.8.VisaoImplementacao.md) | 🟢 revisado |
| [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md) | Camadas + contextos delimitados | Matheus Lemes Amaral, com a SubEquipe_01 | [Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) | 🟢 revisado |
| [ADR-007](/Base/Relatórios/1.1.1.DAS/ADR/ADR-007-eventbus-em-memoria-vs-chamada-direta.md) | EventBus em memória vs. chamada direta entre contextos | Eduardo Lôbo Moreira | [Visão de Processos](/Base/Relatórios/1.1.1.DAS/1.1.1.6.VisaoProcessos.md), [Metas e Restrições](/Base/Relatórios/1.1.1.DAS/1.1.1.3.MetasRestricoes.md) (MA-06, RT-03) | 🟢 revisado |
| [ADR-008](/Base/Relatórios/1.1.1.DAS/ADR/ADR-008-shared-kernel-vs-anticorruption-layer.md) | Shared Kernel vs. Anticorruption Layer | Daniel de Oliveira Lira | [Representação Arquitetural](/Base/Relatórios/1.1.1.DAS/1.1.1.2.RepresentacaoArquitetural.md), [Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md), [Reutilização](/Base/Relatórios/1.1.2.Reutilizacao.md), [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md) (consequências) | 🟢 revisado |
| [ADR-009](/Base/Relatórios/1.1.1.DAS/ADR/ADR-009-transmissao-contexto-dois-donos.md) | `transmissao` como um contexto com dois donos (configuração e canal) | SubEquipe_01 | [Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) | 🟢 revisado |
| [ADR-010](/Base/Relatórios/1.1.1.DAS/ADR/ADR-010-foco-das-vs-reutilizacao.md) | Foco DAS + IA Generativa em vez de Reutilização | Lucas Andrade Zanetti | [DAS](/Base/Relatórios/1.1.1.DAS.md), [Reutilização](/Base/Relatórios/1.1.2.Reutilizacao.md), [Planejamento](/Projeto/Planejamento.md) | 🟢 revisado |
| [ADR-011](/Base/Relatórios/1.1.1.DAS/ADR/ADR-011-fitness-functions-regras-da-visao-logica.md) | Regras da Visão Lógica verificadas por fitness function na CI | Matheus Lemes Amaral e Lucas Andrade Zanetti | [Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md), [Visão de Implementação](/Base/Relatórios/1.1.1.DAS/1.1.1.8.VisaoImplementacao.md), [Metas e Restrições](/Base/Relatórios/1.1.1.DAS/1.1.1.3.MetasRestricoes.md) (MA-07) | 🟢 revisado |

**ADRs da Entrega 03**, que as seções do DAS continuam citando:

| ADR | Decisão | Subequipe |
| --- | --- | --- |
| [ADR-001](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.1.SubEquipe_01/ADR/ADR-001-builder-stream-config.md) | Builder para a configuração da live (`StreamConfig`) | SG1 |
| [ADR-002](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.1.SubEquipe_01/ADR/ADR-002-factory-method-user-content.md) | Factory Method para o conteúdo do usuário | SG1 |
| [ADR-003](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.3.SubEquipe_03/ADR/ADR-003-observer-concorrente-notificacoes-twitch.md) | Observer concorrente nas notificações do canal | SG3 |
| [ADR-004](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.3.SubEquipe_03/ADR/ADR-004-chain-of-responsibility-autenticacao.md) | Chain of Responsibility na autenticação | SG3 |

## Rastreabilidade com as entregas anteriores

Cada ADR parte de algo que as Entregas 01 a 03 deixaram e termina em um artefato da Entrega 04. Onde não há elo, a célula diz “—” e o ADR explica o motivo, sem forçar a ligação.

| ADR | Entrega 01 (requisitos e NFR) | Entrega 02 (modelagem) | Entrega 03 (padrões e código) | Entrega 04 (artefato) |
| --- | --- | --- | --- | --- |
| ADR-005 | — (vem da organização do trabalho; ata D09) | [Diagrama de componentes SG1](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.1.SubEquipe_01/1.1.1.9.Extra.DiagramaComponentes.md) | Três repositórios por subequipe; [ADR-001 a ADR-004](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/1.PadroesDeProjeto.md) | [PR #24](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/24) · RT-02 · [1.1.1.8](/Base/Relatórios/1.1.1.DAS/1.1.1.8.VisaoImplementacao.md) |
| ADR-006 | [SIGs de SG1, SG2 e SG3](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_01/#/Projeto/EscopoProduto.md) (metas MA-01 a MA-07) | Componentes e interfaces das três subequipes | Padrões GoF distribuídos em três códigos | RD1–RD7 · [1.1.1.5](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) |
| ADR-007 | [O18 Pub/Sub (fanout)](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_01/#/Base/Relatorios/1.1.1.SubEquipe_01/2.NFRFramework.md) | `EventBus` com `StreamOnline`/`StreamOffline` ([componentes SG1](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.1.SubEquipe_01/1.1.1.9.Extra.DiagramaComponentes.md)) | [ADR-003 Observer concorrente](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.3.SubEquipe_03/ADR/ADR-003-observer-concorrente-notificacoes-twitch.md) | `EventBusEmMemoria` · [1.1.1.6](/Base/Relatórios/1.1.1.DAS/1.1.1.6.VisaoProcessos.md) · MA-06 |
| ADR-008 | — (vem da organização do trabalho; ata D08) | Modelagem estática de [SG1](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.1.SubEquipe_01/1.1.1.1.Foco01.ModelagemEstatica.md), [SG2](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.2.SubEquipe_02/1.1.2.1.Foco01.ModelagemEstatica.md) e [SG3](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.3.SubEquipe_03/1.1.3.1.Foco01.ModelagemEstatica.md) | Modelos próprios e acoplamentos cruzados ([EV-02](/Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt)) | `@g6/shared-kernel` · [1.1.2](/Base/Relatórios/1.1.2.Reutilizacao.md) |
| ADR-009 | [BPMN “Início e Configuração da Transmissão”](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_01/#/Base/Relatorios/1.1.1.SubEquipe_01/4.BPMN.md) | Fase de configuração das sequências A01 e A05 ([dinâmica SG1](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.1.SubEquipe_01/1.1.1.2.Foco02.ModelagemDinamica.md)) | [ADR-001 Builder](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.1.SubEquipe_01/ADR/ADR-001-builder-stream-config.md) e [ADR-003 Observer](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.3.SubEquipe_03/ADR/ADR-003-observer-concorrente-notificacoes-twitch.md) | `@g6/transmissao` · RD6 |
| ADR-010 | Escopo do produto e ênfases ([E01](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_01/#/Projeto/EscopoProduto.md)) | Modelos reaproveitados nas visões | Três códigos que viram o CA-01 | Ata D01 · [1.1.2](/Base/Relatórios/1.1.2.Reutilizacao.md) |
| ADR-011 | — (nota 3 da [matriz](/Base/Relatórios/1.1.1.DAS/1.1.1.12.Rastreabilidade.md)) | — | Acoplamentos herdados ([EV-02](/Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt)) | [PR #33](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/33) · [PR #37](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/37) · MA-07 |

## Revisão da coordenação (21/11/2026)

Critério de aceite da [#19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19): cada ADR com contexto, alternativas, decisão e consequências. Além do formato, cada evidência citada foi conferida no repositório de código, no commit [`76bec6c`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/76bec6c8763ddfd1aa3804ec2b6e3c59d034b366) (`main` em 21/11/2026).

| ADR | Contexto | Alternativas | Decisão | Consequências | Evidências conferidas no código |
| --- | --- | --- | --- | --- | --- |
| ADR-006 | ✅ três forças, com EV-02 | ✅ cinco (A–E), com prós e contras | ✅ tabela de camadas e D1–D5 | ✅ ganhos, custos, riscos | ✅ C-01 a C-03 continuam válidos (`MensagemChat` e `VereditoModeracao` seguem no kernel); ⚠️ um risco ficou desatualizado (achado 1) |
| ADR-007 | ✅ quatro necessidades do CA-01 | ✅ cinco (A–E) | ✅ critério EventBus × port × Observer | ✅ inclui perda de entrega garantida | ✅ EB-01 a EB-05 conferem com o código (fila FIFO e `AggregateError` em `event-bus-em-memoria.ts`); ⚠️ dois trechos descrevem código que ainda não está na `main` (achados 3 e 4) |
| ADR-009 | ✅ as duas metades e o port único | ✅ quatro (A–D) | ✅ três pastas e RD6 | ✅ inclui dois executores de teste | ✅ T-01 e T-02 conferem com `CODEOWNERS` e `package.json`; ⚠️ um risco já foi resolvido (achado 2) e uma consequência continua aberta (achado 5) |

**Achados e ajustes pedidos aos autores.**

| # | ADR | Achado | Evidência | Ajuste sugerido |
| --- | --- | --- | --- | --- |
| 1 | ADR-006 | O risco “a regra ainda não é imposta por ferramenta” ficou **desatualizado**: a extra E2 e a CI já estão na `main` e verificam RD1–RD5 em cada PR | PRs [#33](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/33) e [#37](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/37) do código | Trocar o risco pelo que continua verdade (achado 5) e citar os dois PRs |
| 2 | ADR-009 | O risco do mapeamento preset → Director (T-04) **foi resolvido**: o mapeamento está implementado e os parâmetros de cada receita são testados (720p/30 fps/`LOW`, 2160p/60 fps/`STANDARD`, 480p/30 fps/`STANDARD`) | [`presets.ts`, linhas 18–20](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/76bec6c8763ddfd1aa3804ec2b6e3c59d034b366/packages/transmissao/src/configuracao/presets.ts#L18-L20) · [`stream-config-director.test.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/76bec6c8763ddfd1aa3804ec2b6e3c59d034b366/packages/transmissao/test/configuracao/stream-config-director.test.ts) (PR [#35](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/35)) | Marcar o risco como resolvido; o que ainda falta conferir é se o canal aceita a `StreamConfig` pronta, o que depende do PR #29 |
| 3 | ADR-007 | O contexto cita o `apps/web` atualizando clientes por SSE ao receber `LiveEncerrada`, mas o `apps/web` na `main` ainda é esqueleto; a página com SSE está no PR [#36](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/36), aberto | [`apps/web/src/servidor.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/76bec6c8763ddfd1aa3804ec2b6e3c59d034b366/apps/web/src/servidor.ts) | Indicar que o assinante SSE é previsto (PR #36), ou citar o assinante que já existe na `main`: o `ConteudoDeLive`, do `conteudo` |
| 4 | ADR-007 | A tabela de critérios cita o `StreamChannel` despachando em lotes com `Promise.allSettled`; esse código está no PR [#29](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/29), não no commit `76bec6c` usado como base das evidências | [`transmissao/src/canal/index.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/76bec6c8763ddfd1aa3804ec2b6e3c59d034b366/packages/transmissao/src/canal/index.ts) (vazio na `main`) | Apontar o PR #29 como fonte desse trecho, para que todas as afirmações tenham evidência fixada |
| 5 | ADR-009 (e Visão Lógica) | A consequência “a E2 precisa de uma regra específica para `configuracao` ↔ `canal`” **continua aberta**: o dependency-cruiser tem só três regras (`contexto-so-importa-shared-kernel`, `shared-kernel-isolado`, `g6-nao-resolvido`), e nenhuma cobre RD6 nem RD7 | [`.dependency-cruiser.cjs`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/76bec6c8763ddfd1aa3804ec2b6e3c59d034b366/.dependency-cruiser.cjs) | Registrar no ADR-009 que RD6 vale por revisão até haver a regra; abrir issue no código para as regras de RD6 e RD7 |

**Reconferência em 22/11/2026**, contra a `main` do código em [`73534e9`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/73534e9b76685d348798fc3b38b85944dda7879b):

| # | Situação | Observação |
| --- | --- | --- |
| 1 | ✅ aplicado no [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md) | Risco trocado pelo que continua verdade: só RD3 a RD5 são automatizadas |
| 2 | ✅ aplicado no [ADR-009](/Base/Relatórios/1.1.1.DAS/ADR/ADR-009-transmissao-contexto-dois-donos.md) | T-04 resolvido; fica aberto o aceite da `StreamConfig` pelo canal (código #8, PR #44 em revisão) |
| 3 | ✅ resolvido pelo código | O assinante SSE existe na `main` (`apps/web/src/rotas.ts`, PR #36 mesclado) |
| 4 | ✅ resolvido pelo código | O despacho em lotes com `Promise.allSettled` está na `main` (`canal/strategy/AsynchronousConcurrentDispatchStrategy.ts`, PR #29 mesclado); o autor do [ADR-007](/Base/Relatórios/1.1.1.DAS/ADR/ADR-007-eventbus-em-memoria-vs-chamada-direta.md) pode agora fixar essa evidência em `73534e9` |
| 5 | ⚠️ continua aberto | `.dependency-cruiser.cjs` segue com três regras; RD6 e RD7 sem regra e sem issue aberta ([ADR-011](/Base/Relatórios/1.1.1.DAS/ADR/ADR-011-fitness-functions-regras-da-visao-logica.md), FF-06) |

**Situação da série.** Os ADR-005 a ADR-011 foram revisados e marcados como verdes no índice desta entrega.

## Como escrever um ADR desta série

- **Arquivo:** `ADR-00N-titulo-curto.md`, nesta pasta, e uma entrada no `_sidebar.md`.
- **Cabeçalho:** tabela com Status, Data, Autoria, Revisor, Escopo e Seção do DAS, como nos ADR-006, ADR-007 e ADR-009.
- **Seções:** 1. Contexto (termina com a pergunta que o ADR responde) · 2. Evidências (cada uma com id e link para um commit fixo) · 3. Alternativas (com a favor e contra) · 4. Decisão · 5. Consequências (o que fica mais fácil, o que fica mais difícil, riscos declarados) · 6. Referências (inclui o elo com as Entregas 01 a 03) · Histórico de Versões.
- **Rastreabilidade:** toda linha nova entra também na tabela “Rastreabilidade com as entregas anteriores” acima.
- **Status:** *Proposta* até a revisão cruzada aprovar; *Aceita* depois; *Substituída por ADR-00X* se outra decisão a revogar.

## Referências

- NYGARD, Michael. [Documenting Architecture Decisions](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions). Cognitect, 2011.

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 0.1 | 09/11/2026 | Estrutura inicial da página, conforme o planejamento da Entrega 04 | Lucas Andrade Zanetti | — |
| 0.2 | 15/11/2026 | ADR-006 escrito e ADR-009 incluído, ambos decorrentes da Visão Lógica ([#5](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/5)) | Lucas Andrade Zanetti, Matheus Lemes Amaral, Pedro Druck Montalvão Reis, Heitor Macedo Ricardo | Revisado |
| 0.3 | 19/11/2026 | Inclusão do ADR-007 sobre EventBus em memória vs. chamada direta entre contextos ([#19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19)) | Eduardo Lôbo Moreira | Revisado |
| 0.4 | 21/11/2026 | Índice com as seções que dependem de cada ADR e os ADRs da E03; revisão da coordenação dos ADR-006, ADR-007 e ADR-009 contra o código (`76bec6c`), com cinco achados; guia de escrita da série; redigido com apoio de IA generativa ([#19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19)) | Pedro Druck Montalvão Reis | Revisado |
| 0.5 | 22/11/2026 | ADR-005 e ADR-008 escritos (rascunhos), ADR-010 e ADR-011 incluídos; tabela de rastreabilidade com as Entregas 01 a 03; reconferência dos achados da coordenação contra `73534e9`; ajustes nos ADR-006 e ADR-009; redigido com apoio de IA generativa | Lucas Andrade Zanetti | Revisado |
