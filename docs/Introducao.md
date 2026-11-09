# Introdução — Entrega 04

**Grupo 06 · Turma 01 · FGA0208 · Universidade de Brasília · 2026.2**

## Contexto

O projeto investiga uma plataforma de streaming de vídeo com ênfase em transmissões ao vivo e conteúdo gerado pelos usuários. A [Entrega 02](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/) modelou os domínios e as interações; a [Entrega 03](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/) aplicou padrões GoF em três códigos TypeScript, um por subequipe.

## Objetivo desta etapa

A Entrega 04 é opcional e pode acrescentar até 1 ponto à média ponderada. O grupo escolheu o foco **DAS + IA Generativa**: descrever a arquitetura da plataforma em visões, apoiada por evidência executável, e registrar o ponto de vista de cada integrante sobre o uso de IA Generativa.

A evidência executável é o cenário **CA-01 — Live com doação destacada**, que atravessa os padrões das três subequipes:

1. login (Chain of Responsibility);
2. abertura da live por preset (Builder → Observer);
3. recarga de moeda virtual (Decorator · Adapter · Proxy);
4. doação com mensagem (Facade + Mediator);
5. mensagem destacada no chat moderado (Decorator → Observer);
6. VOD da live encerrada (Factory Method).

## Equipe por subequipe

A composição é a mesma das Entregas 02 e 03.

### 🔵 SubEquipe_01 — Estrutura do DAS e Visão Lógica

Seções 1–5 e 10 · transmissão (Builder), conteúdo (Factory Method), CLI do CA-01 · [épico da subequipe](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/25)

| Matrícula | Integrante | GitHub |
| --- | --- | --- |
| 24/1039645 | Lucas Andrade Zanetti | [@Bappoz](https://github.com/Bappoz) |
| 24/1031852 | Matheus Lemes Amaral | [@1emes](https://github.com/1emes) |
| 24/1040332 | Pedro Druck Montalvão Reis | [@pedruck](https://github.com/pedruck) |
| 24/1039073 | Heitor Macedo Ricardo | [@HeitorM50](https://github.com/HeitorM50) |

### 🟢 SubEquipe_02 — Visões de Implementação e Implantação

Seções 7–9 · shared-kernel, monetização, página web · [épico da subequipe](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/26)

| Matrícula | Integrante | GitHub |
| --- | --- | --- |
| 24/1011018 | Davi Severiano Freitas | [@Davi-UnB](https://github.com/Davi-UnB) |
| 24/1025505 | Daniel de Oliveira Lira | [@Daniellira540](https://github.com/Daniellira540) |
| 24/1011466 | Mateus Rodrigues Barreto | [@Mateus0xC](https://github.com/Mateus0xC) |
| 23/1026545 | Pedro Henrique Freire Rodrigues | [@Pedro-Henrique3](https://github.com/Pedro-Henrique3) |

### 🟠 SubEquipe_03 — Visão de Processos e Qualidade

Seções 6 e 11 · identidade, transmissão/canal, chat, moderação · [épico da subequipe](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/27)

| Matrícula | Integrante | GitHub |
| --- | --- | --- |
| 24/1041302 | Hugo Freitas Silva | [@HugoFreitass](https://github.com/HugoFreitass) |
| 24/1040350 | Philipe Amâncio Reis Caetano | [@Phill-Chill](https://github.com/Phill-Chill) |
| 24/1011027 | Eduardo Lôbo Moreira | [@EduLoboM](https://github.com/EduLoboM) |

## Como o trabalho está organizado

- [Planejamento](/Projeto/Planejamento.md): divisão, cronograma por marcos, pontos de integração e extras.
- Issues e milestones: [documentação](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues) · [código](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues).
- [Checklist](/Checklist.md) e [atas](/Projeto/Atas/README.md).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 0.1 | 09/11/2026 | Estrutura inicial da página, conforme o planejamento da Entrega 04 | Lucas Andrade Zanetti | — |
