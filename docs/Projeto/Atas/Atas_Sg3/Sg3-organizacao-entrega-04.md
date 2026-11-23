# Organização da Entrega 04 — SubEquipe_03

> **Foco da subequipe**: identidade, chat em tempo real, moderação e canal de transmissão no código; Visão de Processos, Qualidade e elos da SG3 na Matriz de Rastreabilidade no DAS.
>
> **Fonte deste documento**: a SubEquipe_03 não tem atas versionadas nesta entrega. A organização abaixo foi reconstruída a partir das issues, dos pull requests e das revisões registrados nos dois repositórios, conferidos em 23/11/2026.

## 1. Escopo e entregáveis

| Entregável | Responsável principal | Issue | Pull Requests |
| --- | --- | --- | --- |
| Visão de Processos (1.1.1.6) | Eduardo Lôbo Moreira | [#6][i6] | [#32][d32] |
| ADR-007 — `EventBus` em memória vs. chamada direta | Eduardo Lôbo Moreira | [#19][i19] | [#33][d33] |
| Diagrama de sequência doação → chat (Figura 3 da Visão de Processos) | Philipe Amâncio Reis Caetano | [#12][i12] | [#66][d66] |
| Qualidade (1.1.1.11) | Hugo Freitas Silva (atualização com Eduardo Lôbo Moreira) | [#11][i11] | [#39][d39] e [#47][d47] (versão original); [#69][d69] (recuperação e atualização) |
| Linhas da SubEquipe_03 na Matriz de Rastreabilidade (1.1.1.12) | Hugo Freitas Silva e Eduardo Lôbo Moreira | [#20][i20] | [#40][d40] e [#48][d48] (versão original); [#68][d68] |
| Registro de prompts de IA (extra E5, organização) | Philipe Amâncio Reis Caetano | [#21][i21] | — |
| `@g6/identidade`: Chain of Responsibility da E03 atrás da `AutenticacaoPort` | Hugo Freitas Silva | [código #5][c5] | [#40][p40] (revertido) e [#43][p43] |
| Login real e *step-up* na página do CA-01 (`apps/web/src/auth`) | Hugo Freitas Silva | [código #17][c17] | [#41][p41] |
| `@g6/chat`: `ChatService` realizando o `ChatPort` | Philipe Amâncio Reis Caetano | [código #13][c13] | [#38][p38] |
| `@g6/transmissao/src/canal`: `StreamChannel` publicando os eventos da live | Eduardo Lôbo Moreira | [código #7][c7] | [#29][p29] |
| `@g6/moderacao`: `ModeracaoService` e AutoMod por Strategy | Eduardo Lôbo Moreira | [código #14][c14] | [#30][p30] |
| Benchmark do CA-01 integrado (extra E8) | Eduardo Lôbo Moreira (com Heitor Macedo Ricardo) | [código #21][c21] | [#39][p39] |
| Cenários de qualidade executáveis QA-01 a QA-07 (extra E10) | Hugo Freitas Silva | [código #23][c23] | [#42][p42] |

## 2. Divisão por integrante

| Integrante | Responsabilidade no DAS | Responsabilidade no código | Extra |
| --- | --- | --- | --- |
| Hugo Freitas Silva | Qualidade (1.1.1.11) e linhas da SG3 na Matriz de Rastreabilidade | `@g6/identidade` e login web (`apps/web/src/auth`) | Cenários de qualidade com testes (E10) |
| Philipe Amâncio Reis Caetano | Diagrama de sequência doação → chat | `@g6/chat` | Organização do registro de prompts (E5) |
| Eduardo Lôbo Moreira | Visão de Processos (1.1.1.6), ADR-007 e consolidação das linhas da SG3 na matriz | `@g6/transmissao/src/canal` e `@g6/moderacao` | Benchmark integrado (E8) |

## 3. Revisão e integração

Revisões e merges registrados nos PRs da subequipe:

| Pull Request | Autor | Revisão | Merge |
| --- | --- | --- | --- |
| Código [#29][p29] — canal | Eduardo | Heitor (pediu alterações) e Hugo (aprovou) | Hugo |
| Código [#30][p30] — moderação | Eduardo | Hugo (aprovou) | Hugo |
| Código [#38][p38] — chat | Philipe | Hugo (pediu alterações) e Eduardo (aprovou) | Lucas |
| Código [#39][p39] — benchmark | Eduardo | Hugo (aprovou) | Hugo |
| Código [#43][p43] — identidade | Hugo | Eduardo (aprovou) | Eduardo |
| Código [#35][p35] — Builder e Director (SubEquipe_01) | Heitor | Eduardo (aprovou) | Heitor |
| Documentação [#38][d38] — Tamanho e Desempenho | Eduardo | Hugo (aprovou) | Hugo |

A revisão cruzada das seções atribuídas à SubEquipe_03 foi feita por Eduardo, com comentários nas issues [#3][i3] (Representação Arquitetural), [#4][i4] (Visão de Casos de Uso) e [#7][i7] (Visão de Implantação).

## 4. Ocorrência registrada

Em 22/11/2026, os PRs [#39][d39], [#40][d40] e [#41][d41] da documentação e o [#40][p40] do código foram mesclados sem revisão e, em seguida, revertidos. O conteúdo foi reapresentado para revisão: no código, pelo [#43][p43], aprovado e mesclado por Eduardo; na documentação, pelos [#47][d47], [#48][d48] e [#49][d49], fechados pelo *force-push* da `main` no mesmo dia. A Qualidade foi recuperada no [#69][d69], e as linhas da SG3 na matriz foram consolidadas no [#68][d68].

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 23/11/2026 | Organização da SubEquipe_03 reconstruída a partir das issues, dos PRs e das revisões dos dois repositórios, sem atas por não haver registro de reuniões | Hugo Freitas Silva | 🟢 SubEquipe_03 |

[i3]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/3
[i4]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/4
[i6]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/6
[i7]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/7
[i11]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/11
[i12]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/12
[i19]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19
[i20]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/20
[i21]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/21
[d32]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/32
[d33]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/33
[d38]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/38
[d39]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/39
[d40]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/40
[d41]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/41
[d47]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/47
[d48]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/48
[d49]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/49
[d66]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/66
[d68]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/68
[d69]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/pull/69
[c5]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/5
[c7]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/7
[c13]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/13
[c14]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/14
[c17]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/17
[c21]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/21
[c23]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/23
[p29]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/29
[p30]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/30
[p35]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/35
[p38]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/38
[p39]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/39
[p40]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/40
[p41]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/41
[p42]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/42
[p43]: https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/43
