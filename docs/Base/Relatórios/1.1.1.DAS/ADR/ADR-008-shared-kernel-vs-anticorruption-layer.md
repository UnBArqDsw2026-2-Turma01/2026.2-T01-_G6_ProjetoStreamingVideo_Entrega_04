# ADR-008 — Shared Kernel em vez de Anticorruption Layer entre contextos

| Campo | Valor |
| --- | --- |
| Status | Proposta, aguardando revisão cruzada |
| Data | 22/11/2026 |
| Autoria | Daniel de Oliveira Lira (SubEquipe_02), autor definido na [issue #19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19). **Rascunho preparado por Lucas Andrade Zanetti com apoio de IA generativa, a partir das evidências do código; Daniel precisa conferir e assumir o texto.** |
| Revisor | Pendente: SubEquipe_01 (revisão cruzada) |
| Escopo | Como os seis contextos compartilham vocabulário (tipos, ports e eventos) sem se importarem. Registra a decisão D08 da [ata de abertura](/Projeto/Atas/ata-G01-entrega-04.md), aplicada no [código #3](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/3) e [#4](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/4). |
| Seção do DAS | [1.1.1.2. Representação Arquitetural](/Base/Relatórios/1.1.1.DAS/1.1.1.2.RepresentacaoArquitetural.md) · [1.1.1.5. Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) · [1.1.2. Reutilização](/Base/Relatórios/1.1.2.Reutilizacao.md) |

## 1. Contexto

O [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md) proíbe um contexto de importar outro (RD3). Mas o CA-01 exige que eles conversem: a doação publica no chat, o chat consulta a moderação, o canal avisa o conteúdo de que a live acabou. Dois contextos precisam, portanto, concordar sobre **o que é** uma mensagem, uma live ou um veredito, sem que um conheça o código do outro.

Na Entrega 03 cada subequipe tinha o próprio modelo desses conceitos, em repositórios separados:

| Conceito | Como cada código da E03 o chamava |
| --- | --- |
| Mensagem de chat | `ChatMessage` (SG3) · `DoacaoNotificada` e `NotificadorChat` (SG2) |
| Resultado da moderação | `ModerationVerdict` e `ModerationDecision` (SG3) · `PolicyService` interno (SG1) |

A pergunta: **como os contextos compartilham esse vocabulário: por um núcleo comum, por tradução em cada fronteira, ou de outro modo?**

## 2. Evidências

Commit de referência no código: [`73534e9`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/73534e9b76685d348798fc3b38b85944dda7879b) (`main` em 22/11/2026).

| Id | O que mostra | Fonte |
| --- | --- | --- |
| K-01 | O grupo aprovou a integração por *shared kernel*, com contratos congelados em D-11 (D08), e Philipe a resumiu como “um contrato”. | [Ata G01, D08](/Projeto/Atas/ata-G01-entrega-04.md) |
| K-02 | O kernel contém só quatro coisas: `tipos`, `ports` (6 interfaces), `eventos` (+ `EventBus`) e `testing` (6 fakes e 7 suítes de contrato). Os arquivos dos seis ports somam 140 linhas. | [`packages/shared-kernel/src`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/73534e9b76685d348798fc3b38b85944dda7879b/packages/shared-kernel/src) |
| K-03 | O código da E03 já tinha acoplamentos cruzados: ciclo `chat` ↔ `moderation` na SG3 e frentes da SG2 (`f2-doacao` ↔ `f3-chat`) importando umas às outras. | [EV-02, itens A e C](/Base/Relatórios/1.1.1.DAS/assets/visao-logica/evidencias/ev-02-acoplamentos-e03.txt) |
| K-04 | Os fakes e as suítes de contrato (`casos<Port>`) são reaproveitados pelos serviços reais, por exemplo `RecargaService` e `ServicoDoacao`. | [`testing/suites`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/73534e9b76685d348798fc3b38b85944dda7879b/packages/shared-kernel/src/testing/suites) · [Reutilização](/Base/Relatórios/1.1.2.Reutilizacao.md) |
| K-05 | O kernel não importa nada do monorepo (RD4); a regra `shared-kernel-isolado` falha a CI se importar. | [`.dependency-cruiser.cjs`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/.dependency-cruiser.cjs) |
| K-06 | **O congelamento escorregou.** O marco M1 (contrato congelado) estava previsto para 12/11/2026 (provisório). O PR do contrato foi aberto em 11/11 e mesclado em **18/11**, e o PR dos fakes também em 18/11. | [PR #26](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/26) · [PR #27](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/27) · [Planejamento, seção 5](/Projeto/Planejamento.md) |
| K-07 | **O contrato mudou durante a revisão:** o `RecargaPort` passou a ser modelado em duas etapas (checkout pendente e confirmação) em 14/11, com o evento `RecargaConcluida`, e os fakes foram adaptados no mesmo dia. | commits [`d489cef`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/d489cefdf980b762a531e395520ffd471d72cf10) e [`3d43c7d`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/3d43c7d) |
| K-08 | Onde existe modelo **externo**, há tradução: um Adapter por processador de pagamento atrás de `GatewayPagamento`, com fallback entre eles (meta MA-03). | [Metas e Restrições, MA-03](/Base/Relatórios/1.1.1.DAS/1.1.1.3.MetasRestricoes.md) |
| K-09 | `StreamConfig` (produto do Builder, com regras de negócio R1–R5) **ficou fora** do kernel de propósito. | [ADR-009, alternativa A](/Base/Relatórios/1.1.1.DAS/ADR/ADR-009-transmissao-contexto-dois-donos.md) |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| A | **Importação direta entre contextos**, como na E03 | Nenhum código novo; cada subequipe usa o tipo do vizinho | É o que gerou o ciclo `chat` ↔ `moderation` (K-03). Viola RD3 e impede testar um contexto sem o outro. |
| **B** | **Shared Kernel pequeno**: tipos, ports, eventos, `EventBus` e kit de testes (fakes + suítes) | Um vocabulário só; cada contexto é testado contra fakes sem esperar o vizinho (K-04); o grafo de dependência tem um único destino (RD2) | O contrato vira ponto de coordenação (K-06, K-07). Quem muda um port precisa de aprovação das subequipes afetadas. |
| C | **Anticorruption Layer (ACL) em cada fronteira**: cada contexto mantém o próprio modelo e traduz o do vizinho | Cada contexto evolui o modelo sem negociar; é o padrão indicado quando o modelo vizinho é **externo ou legado** | Com 6 contextos, cada fronteira ganha um tradutor, em dois sentidos. Os modelos são nossos, na mesma linguagem e no mesmo processo, então a tradução seria só repetição. Teria de ser escrita e testada por três subequipes em 14 dias. |
| D | **Open Host Service e Published Language** (API HTTP/JSON entre processos) | Fronteira forte e substituível por outro processo | Contraria a restrição de processo único (RT-03) e o [ADR-007](/Base/Relatórios/1.1.1.DAS/ADR/ADR-007-eventbus-em-memoria-vs-chamada-direta.md). Exige servidor, serialização e porta por contexto. |
| E | **Customer–Supplier bilateral** (um contrato por par de subequipes) | Cada contrato é pequeno | Não há fonte única: o mesmo conceito (por exemplo `MensagemChat`) apareceria em vários contratos. Três subequipes teriam de renegociar a cada mudança. |
| F | **Kernel grande**, com os modelos de domínio completos (inclusive `StreamConfig`) | Menos tradução | O kernel passaria a carregar regra de negócio, o que o guia proíbe, e deixaria de ser “pequeno” (K-09). |

## 4. Decisão

Adotar **B**, com três restrições.

1. **O kernel só carrega o que atravessa uma fronteira:** tipos de id, DTOs dos ports, a união `EventoDominio`, o `EventBus` e o kit de testes. Regra de negócio fica no contexto (K-09).
2. **Mudança de contrato exige issue com rótulo `contrato`** e aprovação de uma pessoa de cada subequipe afetada (regra 4 do [guia](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/1)).
3. **A ACL fica onde há modelo externo:** nos gateways de pagamento (K-08), não entre contextos nossos.

## 5. Consequências

**Fica mais fácil:** desenvolver e testar um contexto contra fakes, sem esperar o vizinho; provar que o serviço real cumpre o port rodando a mesma suíte `casos<Port>`; dar um único destino de dependência aos contextos (RD2), o que a [fitness function](/Base/Relatórios/1.1.1.DAS/ADR/ADR-011-fitness-functions-regras-da-visao-logica.md) verifica.

**Fica mais difícil / custo:**

- **O congelamento é um processo, não um estado.** O contrato foi mesclado seis dias depois do M1 provisório e mudou durante a revisão (K-06, K-07). Quem implementava um port esperou o contrato ou trabalhou sobre uma versão provisória.
- **Um dono para o pacote** (`CODEOWNERS`: SubEquipe_02): toda correção de contrato passa por uma subequipe só, o que pode virar gargalo.
- **Fake não é prova.** Um fake que concorda com o port não demonstra que o serviço real concorda. Hoje, por exemplo, a composição provisória de `apps/web` ainda monta `FakeAutenticacao`, `FakeTransmissao`, `FakeDoacao`, `FakeChat` e `FakeModeracao` ([`ca01.ts`, linhas 279–284](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/73534e9b76685d348798fc3b38b85944dda7879b/apps/web/src/ca01.ts#L279-L284)). A evidência “CA-01 com módulos reais” só existe quando essa composição trocar os fakes pelos serviços (código #15 e #17).

**Riscos e limites declarados:**

- O kernel pode crescer por conveniência. O critério da decisão 1 precisa ser aplicado na revisão de cada PR do kernel; nenhuma ferramenta o verifica.
- Não foi medido quanto tempo cada subequipe perdeu com o contrato tardio; K-06 mostra apenas as datas.
- Se algum contexto passar a falar com um sistema externo além dos gateways, a decisão 3 precisa ser reavaliada para esse caso.

## 6. Referências

- EVANS, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Cap. 14, “Maintaining Model Integrity” (*Shared Kernel*, *Anticorruption Layer*, *Customer/Supplier*, *Open Host Service*, *Published Language*).
- Entrega 03: [Introdução](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Introducao.md) e os três códigos com modelos próprios; [ADR-001 a ADR-004](/Base/Relatórios/1.1.1.DAS/ADR/README.md).
- Entrega 02: modelagem estática de cada subequipe ([SG1](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.1.SubEquipe_01/1.1.1.1.Foco01.ModelagemEstatica.md), [SG2](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.2.SubEquipe_02/1.1.2.1.Foco01.ModelagemEstatica.md), [SG3](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/#/Base/Relatórios/1.1.3.SubEquipe_03/1.1.3.1.Foco01.ModelagemEstatica.md)), de onde saíram os conceitos que o kernel unificou.
- [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md), [ADR-007](/Base/Relatórios/1.1.1.DAS/ADR/ADR-007-eventbus-em-memoria-vs-chamada-direta.md), [ADR-009](/Base/Relatórios/1.1.1.DAS/ADR/ADR-009-transmissao-contexto-dois-donos.md).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 0.1 | 22/11/2026 | Rascunho da decisão, alternativas A–F e evidências K-01 a K-09, preparado com apoio de IA generativa para o autor definido na issue #19 | Lucas Andrade Zanetti (rascunho), Daniel de Oliveira Lira (autoria a confirmar) | Pendente (SubEquipe_01) |
