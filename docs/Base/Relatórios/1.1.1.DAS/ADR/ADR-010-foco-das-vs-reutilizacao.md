# ADR-010 — Foco da Entrega 04: DAS + IA Generativa em vez de Reutilização

| Campo | Valor |
| --- | --- |
| Status | 🟢 Revisado |
| Data | Decisão em 09/11/2026; ADR escrito em 22/11/2026 |
| Autoria | Lucas Andrade Zanetti (SubEquipe_01), que propôs o foco na reunião; redigido com apoio de IA generativa |
| Revisor | SubEquipe_02 — revisão concluída |
| Escopo | Qual dos dois focos propostos pela disciplina o grupo entrega, e o que essa escolha obriga e deixa de fora. |
| Seção do DAS | [1.1.1. DAS](/Base/Relatórios/1.1.1.DAS.md) · [1.1.2. Reutilização](/Base/Relatórios/1.1.2.Reutilizacao.md) · [Planejamento, seção 1](/Projeto/Planejamento.md?id=_1-por-que-das-e-não-reutilização) |

## 1. Contexto

A Entrega 04 é **opcional** e pode acrescentar até 1 ponto à média ponderada ([Introdução](/Introducao.md)). O template oferece dois focos: **DAS + IA Generativa** ou **Reutilização de Software + IA Generativa**. A entrega mínima do DAS é a Visão Lógica mais uma visão, com evidência de algo rodando, mais o ponto de vista de cada integrante sobre IA Generativa.

O grupo chegou à E04 com três acervos: requisitos e NFRs (E01), modelos UML por subequipe (E02) e três códigos TypeScript com padrões GoF (E03).

A pergunta: **qual foco aproveita melhor esse acervo e cabe em duas semanas de trabalho de onze pessoas?**

## 2. Evidências

| Id | O que mostra | Fonte |
| --- | --- | --- |
| F-01 | A decisão D01: Lucas propôs o DAS pela ligação com o que já existia (E01 → E02 → E03 → E04); Davi e a SubEquipe_03 concordaram. | [Ata G01, D01](/Projeto/Atas/ata-G01-entrega-04.md), 00:13–08:52 e 21:40–22:15 |
| F-02 | Segundo o planejamento, o exemplo de referência da Reutilização é baseado em React, e o código do grupo é TypeScript/Node sem React. A ata registra o mesmo argumento. | [Planejamento, seção 1](/Projeto/Planejamento.md) · [Ata G01, D01](/Projeto/Atas/ata-G01-entrega-04.md) |
| F-03 | A evidência executável pedida pelo DAS virou o cenário CA-01, que passa pelos padrões das três subequipes. | [Planejamento, seção 2](/Projeto/Planejamento.md) · [Visão de Casos de Uso](/Base/Relatórios/1.1.1.DAS/1.1.1.4.VisaoCasosDeUso.md) |
| F-04 | Há reutilização real mesmo fora do foco: sete módulos da E03 importados por commits `import from` e o `@g6/shared-kernel` reaproveitado pelos seis contextos. | [ADR-005, M-03](/Base/Relatórios/1.1.1.DAS/ADR/ADR-005-monorepo-vs-repositorios-separados.md) · [1.1.2](/Base/Relatórios/1.1.2.Reutilizacao.md) |
| F-05 | A divisão por visão (uma visão por subequipe) foi aprovada **com ressalvas** de Davi: cada subequipe conhece só parte do sistema. | [Ata G01, D02](/Projeto/Atas/ata-G01-entrega-04.md) |
| F-06 | O relato de Davi no Foco_02 registra que as tarefas e os prazos gerados com IA para o grupo geral **não foram cumpridos**, e que a IA sugeriu tarefas extras fora do escopo (diagramas C4). | [Foco_02 no relatório DAS](/Base/Relatórios/1.1.1.DAS.md) |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| **A** | **DAS + IA Generativa** | Organiza o acervo das três entregas em visões. Rastreabilidade direta requisito → modelo → padrão → pacote (F-01, F-03). A evidência executável reaproveita o código da E03. | Risco de o DAS ficar só documental; mitigado pelo CA-01 e pelas regras verificadas por ferramenta ([ADR-011](/Base/Relatórios/1.1.1.DAS/ADR/ADR-011-fitness-functions-regras-da-visao-logica.md)). Exige coordenar três subequipes em torno de seções interdependentes (F-05). |
| B | **Reutilização + IA Generativa** | Foco menor em documento; centrado em componentes | O exemplo de referência não se aplica ao código do grupo (F-02). Criar componentes reutilizáveis praticamente do zero, com pouca ligação com E01–E03. |
| C | **Os dois focos** | Máximo de cobertura | Duas entregas completas em duas semanas, sem folga. A diretriz diz que a ideia é qualidade, não quantidade. |
| D | **Não entregar a E04** | Sem custo | Abre mão do ponto adicional (a entrega é opcional) e do uso da integração que o grupo já havia planejado. |

## 4. Decisão

Adotar **A**. A página 1.1.2 permanece no relatório (o template não permite omitir seções), com a justificativa desta escolha e as evidências de reutilização (F-04).

O grupo cobre a Visão Lógica (obrigatória), Implementação, Processos e Implantação, além de Dados, Tamanho e Desempenho e Qualidade, e entrega o CA-01 como evidência.

## 5. Consequências

**Fica mais fácil:** reaproveitar o código e os ADRs da E03 sem reescrever; ligar cada seção do DAS a um artefato anterior (a [matriz de rastreabilidade](/Base/Relatórios/1.1.1.DAS/1.1.1.12.Rastreabilidade.md) e o [índice de ADRs](/Base/Relatórios/1.1.1.DAS/ADR/README.md) existem por causa disso).

**Fica mais difícil / custo:**

- A entrega depende de **integração**: a Visão Lógica só vale se o código obedecer às regras, e o CA-01 só vale se rodar com os módulos reais. Em 22/11/2026 isso ainda não é verdade para `apps/web` e `apps/cli-demo` (ver [ADR-008, consequências](/Base/Relatórios/1.1.1.DAS/ADR/ADR-008-shared-kernel-vs-anticorruption-layer.md)).
- Seções escritas por quem não implementou a parte exigem revisão cruzada séria (F-05); parte delas ainda está sem revisão (ver o [status das seções](/Base/Relatórios/1.1.1.DAS.md)).
- A camada web não ganha sistema de design nem biblioteca de componentes (limitação já registrada em 1.1.2).

**Riscos e limites declarados:**

- O planejamento foi feito com apoio de IA e, segundo o relato de Davi (F-06), seus prazos não foram cumpridos. Parte das extras da faixa B (Docker Compose, TypeDoc) não tem comprobatório na `main` do código.
- A data de entrega (D03) e o horário de congelamento (D04) **não foram confirmados em ata**; o cronograma usa D = 23/11/2026 de forma provisória.

## 6. Referências

- [Ata da Reunião Geral 1](/Projeto/Atas/ata-G01-entrega-04.md), D01–D03.
- [Planejamento da Entrega 04](/Projeto/Planejamento.md), seções 1, 6 e 7.
- Entregas anteriores: [E01](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_01/) (requisitos e NFR), [E02](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/) (modelagem), [E03](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/) (padrões GoF).
- [ADR-005](/Base/Relatórios/1.1.1.DAS/ADR/ADR-005-monorepo-vs-repositorios-separados.md), [ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 22/11/2026 | Registro escrito da decisão D01, alternativas A–D e evidências F-01 a F-06, redigido com apoio de IA generativa | Lucas Andrade Zanetti | 🟢 Revisado |
