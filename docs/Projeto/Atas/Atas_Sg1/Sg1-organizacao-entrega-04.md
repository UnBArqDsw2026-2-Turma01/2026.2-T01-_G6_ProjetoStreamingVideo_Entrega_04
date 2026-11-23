# Organização da Entrega 04 — SubEquipe_01

> **Foco da subequipe**: estrutura do DAS, Visão Lógica e transmissão/conteúdo.
>
> **Forma de organização**: a SubEquipe_01 não realizou reuniões formais. A coordenação e o acompanhamento das tarefas ocorreram presencialmente e por mensagens.

## 1. Escopo e entregáveis

| Entregável | Responsável principal | Participação da subequipe |
| --- | --- | --- |
| Introdução e Representação Arquitetural | Lucas Andrade Zanetti | Estrutura geral do DAS e descrição da arquitetura |
| Metas, Restrições e Visão de Casos de Uso | Pedro Druck Montalvão Reis | Contexto, requisitos e cenários do sistema |
| Visão Lógica | Matheus Lemes Amaral | Organização dos pacotes, dependências e regras arquiteturais |
| Tamanho e Desempenho | Heitor Macedo Ricardo | Análise de desempenho do CA-01 |
| Transmissão e configuração da live | Heitor Macedo Ricardo | Builder/Director e integração do `TransmissaoService` |
| Conteúdo e geração de VOD | Matheus Lemes Amaral | Factory Method e criação do conteúdo derivado da live |
| Monorepo e integração | Lucas Andrade Zanetti | Organização dos workspaces e integração dos contextos |
| ADRs e rastreabilidade | Pedro Druck Montalvão Reis e Lucas Andrade Zanetti | Registro das decisões arquiteturais e seus elos com as entregas anteriores |

## 2. Comunicação e acompanhamento

| Forma | Uso |
| --- | --- |
| Conversa presencial | Alinhamento rápido de decisões, divisão das tarefas e resolução de dúvidas |
| Mensagens | Coordenação assíncrona, confirmação de versões, encaminhamento de PRs e revisão cruzada |
| Issues e Pull Requests | Registro verificável de responsabilidade, implementação e aceitação |

Não existem atas ou gravações desta subequipe, pois o trabalho foi conduzido sem reuniões formais.

## 3. Divisão por integrante

| Integrante | Responsabilidade no DAS | Responsabilidade no código | Extra |
| --- | --- | --- | --- |
| Lucas Andrade Zanetti | Introdução, Representação Arquitetural e consolidação da Visão Lógica | Organização do monorepo | CI e matriz de rastreabilidade |
| Matheus Lemes Amaral | Visão Lógica | Conteúdo e geração de VOD | Fitness functions |
| Pedro Druck Montalvão Reis | Metas e Restrições, Casos de Uso e coordenação dos ADRs | Revisão cruzada de transmissão e conteúdo | ADRs da arquitetura |
| Heitor Macedo Ricardo | Tamanho e Desempenho | Transmissão, configuração da live e CLI do cenário CA-01 | Benchmark integrado |

## 4. Critérios de integração

- Cada contribuição é registrada em issue ou Pull Request.
- Mudanças de contrato exigem confirmação entre as subequipes impactadas.
- PRs de documentação e código passam por revisão cruzada.
- Evidências citadas no DAS devem apontar para artefatos reais do monorepo ou commits fixos.
- Uma decisão arquitetural relevante deve ser registrada em ADR.

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 23/11/2026 | Organização da SubEquipe_01 em formato equivalente ao documento da SubEquipe_02, sem atas por não haver reuniões formais | SubEquipe_01 | — |
