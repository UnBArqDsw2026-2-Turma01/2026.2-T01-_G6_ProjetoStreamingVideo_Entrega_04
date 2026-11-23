# ADR-009 — `transmissao` como um contexto com dois donos

| Campo | Valor |
| --- | --- |
| Status | 🟢 Revisado |
| Data | 15/11/2026 |
| Autoria | SubEquipe_01: Lucas Andrade Zanetti, Matheus Lemes Amaral, Pedro Druck Montalvão Reis e Heitor Macedo Ricardo |
| Revisor | SubEquipe_02 e SubEquipe_03 — revisão concluída |
| Escopo | Fronteira interna do contexto `@g6/transmissao`. Registra uma decisão já aplicada no [guia do código](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/55b1ed863866dcf3a41587f3642ef06efcb0f3be/CONTRIBUTING.md), no `CODEOWNERS` e nas issues #6, #7 e #8 do código, que ainda não tinha justificativa escrita. |
| Seção do DAS | [1.1.1.5. Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) |

## 1. Contexto

O ciclo de vida da live tem duas metades, implementadas por subequipes diferentes na Entrega 03:

| Metade | Padrão (E03) | Código de origem | Subequipe |
| --- | --- | --- | --- |
| Montar a configuração da live a partir de um preset | Builder + Director (`StreamConfig`, `StreamConfigDirector`) | `g6-streaming-entrega03-subgrupo01-codigo/src/criacionais/builder` | SubEquipe_01 ([ADR-001](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.1.SubEquipe_01/ADR/ADR-001-builder-stream-config.md)) |
| Colocar a live no ar e notificar quem acompanha o canal | Observer + Strategy de despacho (`StreamChannel`) | `g6-streaming-entrega03-subgrupo03-codigo/src/patterns/observer` | SubEquipe_03 ([ADR-003](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.3.SubEquipe_03/ADR/ADR-003-observer-concorrente-notificacoes-twitch.md)) |

No CA-01 as duas metades formam **uma** operação: `TransmissaoPort.abrir(criador, preset)` monta a configuração e abre a live. Além disso, o contrato faz da transmissão a **única fonte** da relação “esta live é de tal criador”, que a doação consulta para saber quem recebe o repasse ([`ports/transmissao.ts`, linhas 6–12](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/b75c711d4fd3d114762ef5353009663838038fc3/packages/shared-kernel/src/ports/transmissao.ts#L6-L12)).

A pergunta: **configuração e canal formam um contexto ou dois?**

## 2. Evidências

| Id | O que mostra | Fonte |
| --- | --- | --- |
| T-01 | Donos por pasta: `configuracao` e `servico` com @HeitorM50 (SG1), `canal` com @EduLoboM (SG3). | [`CODEOWNERS`, linhas 8–14](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/55b1ed863866dcf3a41587f3642ef06efcb0f3be/CODEOWNERS#L8-L14) |
| T-02 | O pacote roda os dois executores de teste herdados da E03: Vitest (SG1) e `node:test` em `test/canal` (SG3). | [`packages/transmissao/package.json`, linhas 11–13](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/55b1ed863866dcf3a41587f3642ef06efcb0f3be/packages/transmissao/package.json#L11-L13) |
| T-03 | Critérios de aceite que separam as pastas: a #6 proíbe importar `src/canal`; a #7 recebe uma `StreamConfig` pronta, sem montar configuração; a #8 é o único lugar que liga as duas. | issues [#6](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/6), [#7](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/7) e [#8](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/8) do código |
| T-04 | Os três presets do contrato (`baixa-latencia`, `alta-qualidade`, `movel`) correspondem aos três métodos do Director da E03 (`lowLatency`, `highQuality`, `mobile`). | [`tipos/transmissao.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/b75c711d4fd3d114762ef5353009663838038fc3/packages/shared-kernel/src/tipos/transmissao.ts) · [`stream-config-director.ts`, linhas 62–80](https://github.com/UnBArqDsw2026-2-Turma01/g6-streaming-entrega03-subgrupo01-codigo/blob/be019ec93914c106459cf73e910e6baf0efc7175/src/criacionais/builder/stream-config-director.ts#L62-L80) |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| A | **Dois contextos** (`@g6/configuracao-live` e `@g6/canal`), integrados por um port | Um dono por pacote, sem pasta compartilhada | `StreamConfig` (produto do Builder, com regras R1–R5 do ADR-001) teria de ir para o kernel para atravessar a fronteira. O kernel deixaria de ser pequeno e passaria a ter regra de negócio, o que o guia proíbe. `TransmissaoPort` ficaria sem um provedor único. |
| B | **Configuração dentro do canal**, com a SubEquipe_03 dona de tudo | Um dono só; nenhuma regra interna | O Builder da SubEquipe_01 perde o dono e a rastreabilidade com a E03 (ADR-001). A SubEquipe_01 passaria a alterar código de outra subequipe. |
| C | **Configuração no `conteudo`** | `conteudo` já é da SubEquipe_01 | Mistura duas responsabilidades sem relação (configurar live e catalogar VOD) e obriga `conteudo` a conhecer o canal, o que fere RD3. |
| **D** | **Um contexto, `@g6/transmissao`, com três pastas**: `configuracao` (SG1), `canal` (SG3) e `servico` (SG1), em que só `servico` importa as outras duas | `StreamConfig` fica dentro do contexto; um único provedor do `TransmissaoPort`; cada subequipe altera só a própria pasta (T-01) | Regra extra, interna ao contexto (RD6), que o `package.json` não expressa; dois executores de teste no mesmo pacote (T-02) |

## 4. Decisão

Adotar **D**.

- `configuracao` e `canal` **não se importam** em nenhuma direção (RD6, na [Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md?id=regras-de-dependência)).
- `servico/TransmissaoService` (código #8) é o único ponto que conhece as duas: traduz o `PresetTransmissao` em uma receita do Director, entrega a `StreamConfig` ao `StreamChannel` e realiza o `TransmissaoPort`.
- O canal publica `LiveIniciada` e `LiveEncerrada` no `EventBus`, e é por eles que `conteudo` cria o VOD sem conhecer a transmissão.
- O mapeamento preset → Director é `baixa-latencia → lowLatency`, `alta-qualidade → highQuality` e `movel → mobile` (T-04), implementado na issue #6 ([`presets.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/packages/transmissao/src/configuracao/presets.ts), PR [#35](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/35)).

## 5. Consequências

**Fica mais fácil:** manter o Builder (SG1) e o Observer (SG3) com a mesma estrutura da E03, já que cada um entra na própria pasta; ter um único provedor do `TransmissaoPort`; manter o kernel sem `StreamConfig`.

**Fica mais difícil / custo:**

- RD6 é uma regra **interna** a um pacote. As ferramentas de dependência entre pacotes não a enxergam sem uma regra por caminho; a extra E2 precisa de uma regra específica para `src/configuracao` ↔ `src/canal`. *Em 22/11/2026 ela ainda não existe: RD6 vale por revisão (ver [ADR-011](/Base/Relatórios/1.1.1.DAS/ADR/ADR-011-fitness-functions-regras-da-visao-logica.md), FF-06).*
- O `npm test` do pacote chama dois executores (T-02). Um erro em um deles pode parecer falha do pacote inteiro para a outra subequipe.
- O `TransmissaoService` é **código novo de integração** entre duas subequipes. Por isso a revisão da #8 é cruzada: implementa Heitor (SG1), revisa Eduardo (SG3).

**Riscos e limites declarados:**

- ~~O mapeamento de T-04 é inferido pelo nome dos presets.~~ **Resolvido em 22/11/2026:** o mapeamento está implementado e os parâmetros de cada receita são testados ([`stream-config-director.test.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/packages/transmissao/test/configuracao/stream-config-director.test.ts)). Continua em aberto conferir se o canal aceita a `StreamConfig` pronta pelo `TransmissaoService`, o que depende da [#8](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/8) (PR [#44](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/44), em revisão).
- Os observadores de moderação de vídeo que o `StreamChannel` da E03 notificava ficam no `canal`. Se algum precisar da moderação real, a ligação acontece em `apps/*` (ADR-006, riscos).

## 6. Referências

- EVANS, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Cap. 14 (*Bounded Context*).
- GAMMA, E.; HELM, R.; JOHNSON, R.; VLISSIDES, J. *Padrões de Projeto: soluções reutilizáveis de software orientado a objetos*. Porto Alegre: Bookman, 2000. (Builder, Observer, Strategy)
- Entrega 03: [ADR-001 — Builder para `StreamConfig`](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.1.SubEquipe_01/ADR/ADR-001-builder-stream-config.md) e [ADR-003 — Observer concorrente](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/Relatórios/1.1.3.SubEquipe_03/ADR/ADR-003-observer-concorrente-notificacoes-twitch.md).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 15/11/2026 | Registro da decisão, alternativas A–D e evidências T-01 a T-04, redigido com apoio de IA generativa | Lucas Andrade Zanetti, Matheus Lemes Amaral, Pedro Druck Montalvão Reis, Heitor Macedo Ricardo | 🟢 Revisado |
