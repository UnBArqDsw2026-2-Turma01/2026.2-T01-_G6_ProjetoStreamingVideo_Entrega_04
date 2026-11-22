# ADR-006 — Camadas + contextos delimitados

| Campo | Valor |
| --- | --- |
| Status | Proposta, aguardando revisão cruzada |
| Data | 15/11/2026 |
| Autoria | Matheus Lemes Amaral (responsável), com Lucas Andrade Zanetti, Pedro Druck Montalvão Reis e Heitor Macedo Ricardo (SubEquipe_01) |
| Revisor | Pendente: SubEquipe_02, na revisão cruzada da [Visão Lógica (#5)](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/5) |
| Escopo | Decomposição lógica do sistema e regras de dependência entre pacotes. A verificação automática das regras é a [extra E2 (código #18)](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/18). |
| Seção do DAS | [1.1.1.5. Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) |

> Este ADR segue o formato da série da Entrega 03 (ADR-001 a ADR-004): contexto, evidências, alternativas, decisão e consequências. Cada afirmação factual aponta para uma evidência (`EV-01`, `EV-02`) ou para um arquivo do código em um commit fixo. O que não tem evidência está marcado como *decisão de projeto*.

## 1. Contexto

A Entrega 04 junta, em um único repositório, os códigos que as três subequipes escreveram **separadamente** na Entrega 03, e precisa executar com eles o cenário CA-01 (live com doação destacada). Três forças puxam a decomposição lógica:

1. **Trabalho em paralelo.** Onze pessoas em três subequipes, cada uma dona de uma parte do código (ata da [Reunião Geral 1](/Projeto/Atas/ata-G01-entrega-04.md), D02). Se um pacote só puder ser testado quando o vizinho existir, o cronograma de dez dias trava.
2. **Acoplamentos herdados.** O código da E03 já tinha dependências cruzadas entre módulos que, na E04, ficam em pacotes de subequipes diferentes (<a href="Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt" target="_blank" rel="noopener">EV-02</a>):
   - na SubEquipe_03, `chat` importa `moderation` e `moderation` importa `chat` (ciclo, EV-02 A);
   - na SubEquipe_02, a exibição no chat (`f3-chat/Chat.ts`) importa a doação (`f2-doacao`), e a doação importa a recarga (EV-02 C).
3. **Regras de qualidade já levantadas.** A mais citada é “**falha no chat não desfaz a doação**” (E03, Ata S2_02, D05). Ela só se sustenta se a doação não depender da implementação do chat, mas de um contrato que possa falhar sem levar a doação junto.

A pergunta deste ADR: **em que pacotes o sistema se divide e quem pode depender de quem?**

## 2. Evidências

| Id | O que mostra | Fonte |
| --- | --- | --- |
| EV-01 | Na `main` (`55b1ed8`) e na branch do contrato (`3d43c7d`), nenhum contexto declara ou importa outro; o kernel não declara dependência `@g6`; só `apps/*` importam contextos. | <a href="Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-01-dependencias-monorepo.txt" target="_blank" rel="noopener">ev-01-dependencias-monorepo.txt</a> |
| EV-02 | Acoplamentos entre módulos nos três repositórios da E03, com os imports exatos e o commit de cada repositório. | <a href="Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt" target="_blank" rel="noopener">ev-02-acoplamentos-e03.txt</a> |
| C-01 | Cada port do contrato declara **quem provê** e **quem consome** no TSDoc (ex.: `ChatPort` é provido por `@g6/chat` e consumido por `@g6/monetizacao`). | [`ports/chat.ts`, linhas 13–19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/b75c711d4fd3d114762ef5353009663838038fc3/packages/shared-kernel/src/ports/chat.ts#L13-L19) (PR #26) |
| C-02 | O `DoacaoPort` torna a regra D05 parte do contrato: chat que falha devolve `ok` com `destaqueEntregue: false`. | [`ports/doacao.ts`, linhas 6–14](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/b75c711d4fd3d114762ef5353009663838038fc3/packages/shared-kernel/src/ports/doacao.ts#L6-L14) (PR #26) |
| C-03 | `MensagemChat` e `VereditoModeracao` estão no kernel, e não mais em `chat` ou `moderation`. | [`tipos/chat.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/b75c711d4fd3d114762ef5353009663838038fc3/packages/shared-kernel/src/tipos/chat.ts#L5) · [`tipos/moderacao.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/b75c711d4fd3d114762ef5353009663838038fc3/packages/shared-kernel/src/tipos/moderacao.ts#L5) (PR #26) |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| A | **Camadas técnicas horizontais** (`dominio/`, `aplicacao/`, `infra/`) atravessando todo o sistema | Padrão conhecido (BASS; CLEMENTS; KAZMAN, 2021); separa regra de negócio de detalhe técnico | Cada pacote mistura código das três subequipes: toda tarefa toca arquivos de outra subequipe. O código da E03 teria de ser **redistribuído** por camada, o que impede a importação literal (regra 7 do guia do código). |
| B | **Contextos que importam uns aos outros** (ex.: `@g6/monetizacao` importa `@g6/chat`) | Menos código: sem ports, sem fakes | Reproduz os acoplamentos de EV-02 entre subequipes diferentes, inclusive o ciclo chat ↔ moderação. A doação só roda se o chat existir, e uma exceção do chat atravessa a doação, o que fere a D05. |
| C | **Um pacote só, com uma pasta por subequipe** (organização da E03) | Zero configuração de workspaces | Nenhuma fronteira verificável: nada impede `import '../chat/...'`. A regra de dependência existiria só no papel. |
| D | **Arquitetura hexagonal completa em cada contexto** (domínio, aplicação e adaptadores por pacote) | Isolamento máximo; cada contexto testável sem infraestrutura | Reescreve a estrutura interna de todo o código da E03 para um sistema em memória, sem banco nem rede. O custo não se paga nesta entrega. |
| **E** | **Três camadas (apps · contextos · shared-kernel) × seis contextos**, dependência só para baixo, comunicação entre contextos **apenas** por ports e eventos definidos no kernel | Fronteira por pacote npm (verificável); cada subequipe trabalha no próprio pacote; os fakes do kernel permitem testar sem o vizinho; a D05 vira contrato | Mais artefatos (ports, DTOs, fakes, suítes de contrato); o kernel acopla as subequipes (ver consequências) |

## 4. Decisão

Adotar **E**. A Visão Lógica fica organizada em três camadas e seis contextos:

| Camada | Pacotes | Pode depender de |
| --- | --- | --- |
| 1 · Apresentação e composição | `@g6/cli-demo`, `@g6/web` | camadas 2 e 3 |
| 2 · Contextos delimitados | `@g6/identidade`, `@g6/transmissao`, `@g6/conteudo`, `@g6/monetizacao`, `@g6/chat`, `@g6/moderacao` | só a camada 3 |
| 3 · Shared kernel | `@g6/shared-kernel` (tipos, ports, eventos, EventBus, kit de testes) | nada do monorepo |

As regras de dependência RD1–RD7, numeradas e ligadas ao diagrama de pacotes, estão na [Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md?id=regras-de-dependência). Elas são a base da fitness function da extra E2.

### Decisões de projeto derivadas

| # | Decisão | Motivo |
| --- | --- | --- |
| D1 | `monetizacao` é **um** contexto com três pastas (`recarga`, `doacao`, `destaque`), e não três contextos | As três frentes da E03 já dependem umas das outras (`Doacao` usa `REPASSE_POR_MOEDA` da recarga; a faixa de destaque é da doação, EV-02 C). Separá-las exigiria levar tipos internos da monetização para o kernel, que deve ser pequeno. |
| D2 | O **chat de exibição** deixa a monetização e recebe a `MensagemDestacada` já resolvida (cor, tempo de fixação) | Na E03, `f3-chat/Chat.ts` importava `FaixaDestaque` da doação (EV-02 C). Agora a regra de valor fica na monetização (`destaque`), e o chat (`@g6/chat`, SG3) só exibe. |
| D3 | `MensagemChat` e `VereditoModeracao` vão para o kernel | Quebra o ciclo chat ↔ moderação da E03 (EV-02 A): o chat depende de `ModeracaoPort`, e a moderação não conhece o chat (C-03). |
| D4 | A moderação de **conteúdo** (VOD) continua como `PolicyService`, interna a `conteudo` | O `ModeracaoPort` modera mensagens de chat. O `PolicyService` da E03 (EV-02 D) tem outra entrada (id de conteúdo) e outro consumidor; generalizá-lo agora aumentaria o kernel sem um segundo uso no CA-01. *Decisão de projeto*, registrada no PR #26 do código. |
| D5 | `transmissao` é um contexto com dois donos | Registrado à parte, no [ADR-009](/Base/Relatórios/1.1.1.DAS/ADR/ADR-009-transmissao-contexto-dois-donos.md). |

## 5. Consequências

**Fica mais fácil:** desenvolver e testar cada contexto contra os fakes do kernel, sem esperar o vizinho; revisar uma mudança olhando um único pacote; explicar a D05 apontando um tipo (`destaqueEntregue`) em vez de um comentário; verificar a arquitetura por ferramenta, porque as fronteiras são pacotes npm (EV-01).

**Fica mais difícil / custo:**

- O kernel **acopla as subequipes**: mudar um port afeta quem o provê e quem o consome. Por isso o contrato é congelado no M1 e muda só por issue com label `contrato`. O trade-off com uma camada anticorrupção é tema do ADR-008 (SubEquipe_02).
- Cada port pede DTO, fake e suíte de contrato. É mais código do que uma chamada direta (alternativa B).
- Um teste que envolve dois contextos reais só pode existir em `apps/*` (ex.: o teste de ponta a ponta do CA-01).

**Riscos e limites declarados:**

- **A regra é imposta por ferramenta só em parte.** A extra E2 ([PR #33](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/33)) e a CI ([PR #37](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/37)) verificam as proibições RD3, RD4 e RD5 em cada PR. RD6 e RD7 não têm regra e dependem de revisão (ver [ADR-011](/Base/Relatórios/1.1.1.DAS/ADR/ADR-011-fitness-functions-regras-da-visao-logica.md)). *Atualizado em 22/11/2026, conforme o achado 1 da revisão da coordenação.*
- **Os observadores de moderação de vídeo do `StreamChannel`** da E03 (`AutoModObserver`, `SafetyVisionModerationObserver` etc.) entram com o canal em `transmissao` (código #7). Se algum deles precisar da moderação real, viola RD3. A saída compatível com este ADR é ligá-los em `apps/*` ou mantê-los no canal; a decisão fica com a SubEquipe_03 na #7.
- Seis contextos para um sistema em memória é granularidade de **organização do trabalho**, não de escala. Este ADR não afirma que os contextos poderiam virar serviços independentes; isso exigiria outra análise (ver ADR-005, sobre o monorepo).

## 6. Referências

- BASS, Len; CLEMENTS, Paul; KAZMAN, Rick. *Software Architecture in Practice*. 4. ed. Addison-Wesley, 2021. (estilo em camadas)
- EVANS, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Cap. 14, “Maintaining Model Integrity” (*Bounded Context*, *Shared Kernel*).
- MARTIN, Robert C. *Clean Architecture: A Craftsman's Guide to Software Structure and Design*. Prentice Hall, 2017. (princípio da dependência acíclica e regra da dependência)
- NYGARD, Michael. [Documenting Architecture Decisions](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions). Cognitect, 2011.
- Repositório de código: [guia de desenvolvimento (CONTRIBUTING.md)](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/55b1ed863866dcf3a41587f3642ef06efcb0f3be/CONTRIBUTING.md) e [contrato do shared-kernel (PR #26)](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/26).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 15/11/2026 | Registro da decisão, alternativas A–E, decisões derivadas D1–D5 e evidências EV-01, EV-02 e C-01 a C-03, redigido com apoio de IA generativa | Lucas Andrade Zanetti, Matheus Lemes Amaral, Pedro Druck Montalvão Reis, Heitor Macedo Ricardo | Pendente (SubEquipe_02) |
