# Registro de Prompts de IA Generativa (extra E5)

> **Organização:** Philipe Amâncio Reis Caetano · **Issue:** [#21](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/21) · **Status:** 🟡 em preenchimento

Registro feito **durante** o trabalho, por cada integrante. É a fonte dos relatos do [Foco_02](/Base/Relatórios/1.1.1.DAS.md?id=foco_02-ia-generativa).

| Data | Integrante | Tarefa | Ferramenta | Prompt (resumo) | O que foi aceito, rejeitado ou corrigido, e por quê | Link |
| --- | --- | --- | --- | --- | --- | --- |
| 16/11/2026 | Eduardo Lôbo Moreira | Adaptação de `StreamChannel` para publicar `LiveIniciada`/`LiveEncerrada` no `EventBus` e criação de testes de eventos (`#7`) | Assistente de código (LLM) | Portar `StreamChannel` da E03 para `packages/transmissao/src/canal`, publicar eventos no `EventBus` usando `StreamConfigCanal` e criar `test/canal/eventos.test.ts` | **Aceito:** estrutura do adaptador de eventos e testes de publicação. **Corrigido após revisão:** a IA gerou import relativo interno (`../../../shared-kernel/src/eventbus/index.js`, violando a Regra 2 do guia) e valores fictícios de configuração (`'1080p'` e `'NORMAL'`), corrigidos no commit `567f094` para importar pela raiz `@g6/shared-kernel` e usar `R1080P` e `STANDARD`. | [PR #29](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/pull/29) · [`567f094`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/commit/567f094956db761224f143903c46342a35f3582c) |

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 0.1 | 09/11/2026 | Estrutura inicial da página, conforme o planejamento da Entrega 04 | Lucas Andrade Zanetti | — |
| 0.2 | 19/11/2026 | Primeiro registro individual de uso de IA generativa (Eduardo Lôbo Moreira, adaptação de `transmissao/canal` no PR #29) | Eduardo Lôbo Moreira | — |
