# ADR-007 — EventBus em memória vs. chamada direta entre contextos

| Campo | Valor |
| --- | --- |
| Status | 🟢 Revisado |
| Data | 19/11/2026 |
| Autoria | Eduardo Lôbo Moreira (SubEquipe_03) |
| Revisor | Pedro Druck Montalvão Reis (coordenação da [issue #19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19)) e revisão cruzada da SubEquipe_01 |
| Escopo | Comunicação entre contextos delimitados por eventos de domínio em processo único Node.js (`@g6/shared-kernel` e consumidores em `packages/*` e `apps/*`) |
| Seção do DAS | [1.1.1.6. Visão de Processos](/Base/Relatórios/1.1.1.DAS/1.1.1.6.VisaoProcessos.md) · [1.1.1.5. Visão Lógica](/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md) |

> Este ADR segue o formato da série da Entrega 03 e da Entrega 04: contexto, evidências, alternativas, decisão e consequências. As referências de código apontam para o estado da `main` do repositório de código no commit [`26c3290`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/tree/26c3290b6ff56ad2289044c8f8c24578b2fede58).

## 1. Contexto

A decomposição em seis contextos delimitados no monorepo ([ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md)) proíbe que um pacote da camada 2 importe outro diretamente (regra RD3). Ao mesmo tempo, o cenário arquitetural **CA-01 — Live com doação destacada** exige que fatos ocorridos em um contexto disparem reações em outros módulos sem acoplá-los:

1. **Encerramento da live → geração de VOD e atualização de interface.** Quando `@g6/transmissao` encerra uma transmissão (`StreamChannel.endLive()`), `@g6/conteudo` precisa gerar o VOD e `apps/web` precisa atualizar clientes conectados via SSE, sem que a transmissão conheça `conteudo` ou a camada web.
2. **Eventos financeiros e de chat com múltiplos interessados.** Quando `@g6/monetizacao` confirma uma recarga ou doação, ou `@g6/chat` publica uma mensagem aprovada, a camada de composição (`apps/cli-demo` e `apps/web`) observa os fatos para logging, SSE e métricas sem alterar o código do produtor.
3. **Ambiente acadêmico de execução local.** Todo o sistema roda em um único processo Node.js (`>= 22`) acionado com um comando (`npm run demo` / `npm run web`), sem banco de dados nem infraestrutura externa obrigatória.
4. **Isolamento de falhas e previsibilidade nos testes.** Um assinante que lança exceção não pode impedir os outros assinantes de receberem o evento, nem eventos publicados em cascata dentro de um tratador podem causar recursão descontrolada.

A pergunta deste ADR é: **como contextos delimitados notificam fatos já acontecidos para outros módulos sem dependerem diretamente uns dos outros?**

## 2. Evidências

| Id | O que mostra | Fonte |
| --- | --- | --- |
| EB-01 | Interface `EventBus` com `publicar(evento)` e `assinar(tipo, tratador): Cancelamento`, provida por `@g6/shared-kernel` com instância única criada em `apps/*`. | [`eventos/event-bus.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/eventos/event-bus.ts#L1-L14) |
| EB-02 | União discriminada `EventoDominio` restringindo os eventos aceitos a `LiveIniciada`, `LiveEncerrada`, `RecargaConcluida`, `DoacaoConfirmada` e `MensagemPublicada`, todos com `em: Date`. | [`eventos/evento-dominio.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/eventos/evento-dominio.ts#L7-L40) |
| EB-03 | `EventBusEmMemoria` entrega de forma síncrona sobre cópia (`[...lista]`), protege reentrância com `fila` FIFO (`emProcessamento`), isola falhas entre tratadores agregando erros em `AggregateError` e garante cancelamento idempotente. | [`eventbus/event-bus-em-memoria.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/eventbus/event-bus-em-memoria.ts#L12-L58) |
| EB-04 | Suíte de contrato `casosEventBus` valida entrega na ordem de assinatura, filtragem por `tipo`, cancelamento idempotente, reentrância FIFO e isolamento de falhas com `AggregateError`. | [`testing/suites/event-bus.suite.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/testing/suites/event-bus.suite.ts#L5-L89) |
| EB-05 | Os ports definem explicitamente quando há chamada direta por interface vs. publicação de evento (`TransmissaoPort` publica `LiveIniciada`/`LiveEncerrada`; `DoacaoPort` chama `ChatPort` e publica `DoacaoConfirmada`). | [`ports/transmissao.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/ports/transmissao.ts#L8-L9) · [`ports/doacao.ts`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/ports/doacao.ts#L6-L14) |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| A | **Chamada direta entre contextos** (`@g6/transmissao` chama `ConteudoService.criarVod()`, `DoacaoService` chama implementação concreta de chat) | Fluxo de execução linear no stack trace; menos abstração e zero infraestrutura de fila | Viola RD3 ([ADR-006](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md)): todo novo consumidor de `LiveEncerrada` (ex.: SSE em `apps/web`) obriga editar o código do produtor; acopla prazos de subequipes diferentes e impede testar um pacote isolado sem dublar cada consumidor. |
| B | **`EventEmitter` nativo do Node.js** usado diretamente como barramento | Zero código novo de barramento no kernel; padrão conhecido da biblioteca padrão | `EventEmitter` usa nomes de evento como `string`/`symbol` sem tipagem estrita da união discriminada `EventoDominio` em compilação; por padrão interrompe no primeiro listener que lança exceção síncrona; e eventos disparados dentro de listeners executam em recursão imediata, sem proteção FIFO contra reentrância. |
| C | **Barramento em memória com despacho assíncrono** (`setImmediate` / `queueMicrotask` a cada `publicar()`) | Evita que um tratador lento bloqueie o retorno imediato do publicador | Torna a ordem entre reação de domínio (ex.: criação de VOD ao receber `LiveEncerrada`) e asserts de teste dependente de *ticks* do Event Loop, exigindo `sleep`/flushes artificiais nos testes unitários e de contrato (`casosEventBus`) sem ganho real em processo único. |
| D | **Broker externo** (Redis Pub/Sub, RabbitMQ ou Kafka) | Desacoplamento entre processos, persistência e *Guaranteed Delivery* (HOHPE; WOOLF, 2003) fora da memória | Exige serviço externo rodando (Docker/daemon), serialização de rede e configuração de ambiente para rodar um protótipo acadêmico cujo objetivo é compor o CA-01 com um comando em processo único. |
| **E** | **`EventBusEmMemoria` síncrono e tipado no `@g6/shared-kernel`**, combinado com **Ports** para comandos/consultas que exigem resposta imediata | Mantém produtores desacoplados dos consumidores (RD3); valida payloads de `EventoDominio` em tempo de compilação; garante ordem determinística por fila FIFO contra reentrância; isola falhas com `AggregateError`; dispensa broker externo e facilita testes determinísticos | Não persiste eventos se o processo cair; um tratador lento segura os demais na mesma thread; exige disciplina para não transformar o barramento em substituto de chamada síncrona quando o produtor precisa de resposta (`Resultado`). |

## 4. Decisão

Adotar **E**: usar o **`EventBusEmMemoria`** (implementando `EventBus` em `@g6/shared-kernel`, padrão *Publish-Subscribe Channel* de HOHPE; WOOLF, 2003) para propagar **fatos de domínio já ocorridos** entre contextos, mantendo **Ports** (`ChatPort`, `ModeracaoPort`, `AutenticacaoPort`, `RecargaPort`, `TransmissaoPort`, `DoacaoPort`) apenas quando o chamador precisa de resposta ou confirmação direta.

### Critério de fronteira: quando usar EventBus vs. chamada via Port

| Tipo de interação | Mecanismo adotado | Exemplo no CA-01 |
| --- | --- | --- |
| **Comando ou consulta com resposta imediata** | Chamada via **Port** (`Promise<Resultado<T>>`) | `ChatService` consulta `ModeracaoPort.moderar()`; `DoacaoService` chama `ChatPort.publicarDestacada()` para saber `destaqueEntregue` (D05). |
| **Notificação de fato ocorrido (1-para-N)** | Publicação no **`EventBus`** (`EventoDominio`) | `StreamChannel` publica `LiveIniciada` / `LiveEncerrada` (`conteudo` reage criando VOD e `apps/web` encaminha via SSE); `DoacaoService` publica `DoacaoConfirmada`; `ChatService` publica `MensagemPublicada`. |
| **Notificação interna a um único contexto** | Padrão **Observer** local | `StreamChannel` notifica observadores internos em lotes assíncronos (`Promise.allSettled`) antes de publicar `LiveIniciada`/`LiveEncerrada` no `EventBus`. |

### Regras operacionais do `EventBusEmMemoria`

1. **Instância única na composição:** `apps/cli-demo` e `apps/web` instanciam `EventBusEmMemoria` e o injetam nos serviços dos contextos; pacotes em `packages/*` dependem apenas da interface `EventBus` e dos tipos de `@g6/shared-kernel` (EB-01).
2. **Fila FIFO contra reentrância:** se um tratador publicar um novo evento durante o processamento de outro, o evento entra em `this.fila` e só é despachado após concluir o evento corrente (EB-03, EB-04).
3. **Isolamento de falhas:** exceções de tratadores não interrompem os demais inscritos e são agregadas ao final do ciclo em `AggregateError` (EB-03, EB-04).
4. **Cancelamento seguro:** `assinar()` retorna uma função `Cancelamento` idempotente, segura para encerramento de conexões SSE e limpeza de testes (EB-03).

## 5. Consequências

**Fica mais fácil:**

- **Integrar subequipes sem acoplamento de código:** `@g6/transmissao` publica `LiveEncerrada` e não precisa saber se `@g6/conteudo` ou `apps/web` já existem no momento em que é desenvolvido e testado.
- **Testar de forma determinística:** a entrega síncrona com fila FIFO permite que os testes de contrato (`casosEventBus`) e os testes dos módulos (`@g6/transmissao`, `@g6/conteudo`, `@g6/monetizacao`, `@g6/chat`) verifiquem eventos publicados sem timers nem mocks de rede.
- **Garantir contrato em compilação:** a união discriminada `EventoDominio` impede publicar ou assinar eventos com nomes ou payloads fora do contrato acordado.

**Fica mais difícil / custo:**

- **Sem entrega garantida nem persistência:** como todo estado fica em memória, se o processo Node.js cair com itens em `this.fila`, os eventos pendentes são perdidos (não implementa *Guaranteed Delivery* de HOHPE; WOOLF, 2003).
- **Execução na mesma thread (Event Loop):** como o despacho é síncrono, um tratador pesado bloqueia os demais tratadores e o Event Loop enquanto executa (risco detalhado na [Visão de Processos](/Base/Relatórios/1.1.1.DAS/1.1.1.6.VisaoProcessos.md?id=decisões-e-senso-crítico)).
- **Rastreio indireto do fluxo:** em comparação com chamadas diretas, descobrir quem reage a um evento exige buscar chamadas a `eventBus.assinar('<Tipo>')` na composição (`apps/*`) ou nos módulos assinantes.
- **Evolução para múltiplos processos exige troca de adaptador:** se o sistema deixar de ser um monólito em memória e passar a rodar em serviços separados, será necessário introduzir um adaptador assíncrono de `EventBus` apoiado em broker externo (alternativa D) e tornar os consumidores idempotentes.

## 6. Referências

- EVANS, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Cap. 14 (*Shared Kernel*).
- HOHPE, Gregor; WOOLF, Bobby. *Enterprise Integration Patterns: Designing, Building, and Deploying Messaging Solutions*. Addison-Wesley, 2003. (*Publish-Subscribe Channel*, *Guaranteed Delivery*).
- Node.js Documentation. *The Node.js Event Loop, Timers, and `process.nextTick()`* e *Don't Block the Event Loop*. Disponível em: https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick.
- Documentação interna: [ADR-006 — Camadas + contextos delimitados](/Base/Relatórios/1.1.1.DAS/ADR/ADR-006-camadas-contextos-delimitados.md) e [1.1.1.6. Visão de Processos](/Base/Relatórios/1.1.1.DAS/1.1.1.6.VisaoProcessos.md).
- Código-fonte de referência: [`EventBus`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/eventos/event-bus.ts), [`EventBusEmMemoria`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/eventbus/event-bus-em-memoria.ts) e [`casosEventBus`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/blob/26c3290b6ff56ad2289044c8f8c24578b2fede58/packages/shared-kernel/src/testing/suites/event-bus.suite.ts).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 19/11/2026 | Redação do ADR-007 (alternativas A–E, evidências EB-01 a EB-05, critério EventBus vs. Port e consequências), com apoio de IA generativa ([#19](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/19)) | Eduardo Lôbo Moreira | 🟢 Revisado |
