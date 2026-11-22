<header class="hero">
  <span class="eyebrow">Entrega 04 · Arquitetura e Desenho de Software</span>
  <h1>Plataforma de Streaming de Vídeo</h1>
  <p>
    A documentação do Grupo 06 reúne o Documento de Arquitetura de Software,
    evidências executáveis e pontos de vista sobre IA Generativa em uma jornada
    clara de leitura e apresentação.
  </p>
  <div class="hero-actions">
    <a class="button" href="/Base/Relatórios/1.1.1.DAS.md">Ler o DAS</a>
    <a class="button-secondary" href="https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo">
      Explorar o código
    </a>
  </div>
</header>

<div class="metadata-strip" aria-label="Informações da disciplina">
  <div><strong>Disciplina</strong>FGA0208</div>
  <div><strong>Turma</strong>01 · Grupo 06</div>
  <div><strong>Semestre</strong>2026.2</div>
  <div><strong>Entregável</strong>DAS + IA Generativa</div>
</div>

## Comece pela sua necessidade

<div class="resource-grid">
  <article class="resource-card">
    <h3>Entender o projeto</h3>
    <p>Conheça o contexto da plataforma, os objetivos desta entrega e a divisão das subequipes.</p>
    <a href="/Introducao.md">Ler a introdução →</a>
  </article>
  <article class="resource-card">
    <h3>Explorar a arquitetura</h3>
    <p>Navegue pelas visões do DAS, decisões arquiteturais, dados, implantação e qualidade.</p>
    <a href="/Base/1.DAS_Reutilizacao.md">Abrir o guia do DAS →</a>
  </article>
  <article class="resource-card">
    <h3>Executar o cenário</h3>
    <p>Veja como o CA-01 integra transmissão, doação, chat, moderação e conteúdo em um monorepo.</p>
    <a href="https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo">Acessar o código →</a>
  </article>
</div>

## Roteiro recomendado

<div class="journey" aria-label="Roteiro de leitura">
  <article class="resource-card">
    <h3>1 · Contexto</h3>
    <p>Comece pela introdução e pelo planejamento para compreender escopo, responsáveis e cronograma.</p>
    <a href="/Introducao.md">Introdução</a>
  </article>
  <article class="resource-card">
    <h3>2 · Arquitetura</h3>
    <p>Leia as visões 4+1, os ADRs e a matriz de rastreabilidade para seguir os elos do sistema.</p>
    <a href="/Base/Relatórios/1.1.1.DAS.md">Documento DAS</a>
  </article>
  <article class="resource-card">
    <h3>3 · Evidência</h3>
    <p>Confira participações, decisões, prompts e o checklist que materializa a entrega.</p>
    <a href="/Checklist.md">Checklist da entrega</a>
  </article>
</div>

## Evidência arquitetural em execução

O cenário **CA-01 — Live com doação destacada** atravessa os padrões das três subequipes:

1. autenticação com Chain of Responsibility;
2. abertura da live com Builder e Observer;
3. recarga com Decorator, Adapter e Proxy;
4. doação com Facade e Mediator;
5. destaque no chat com Decorator e Observer;
6. criação do VOD com Factory Method.

<div class="action-links">
  <a class="button" href="/Base/Relatórios/1.1.1.DAS/1.1.1.5.VisaoLogica.md">Visão lógica</a>
  <a class="button-secondary" href="/Base/Relatórios/1.1.1.DAS/1.1.1.6.VisaoProcessos.md">Visão de processos</a>
  <a class="button-secondary" href="/Base/Relatórios/1.1.1.DAS/ADR/README.md">Decisões ADR</a>
</div>

## Equipe

O trabalho está organizado em três subequipes, responsáveis pelas visões, pelo código integrado e pelas decisões de arquitetura.

| Subequipe | Foco |
| --- | --- |
| SubEquipe_01 | Estrutura do DAS, visão lógica e transmissão/conteúdo |
| SubEquipe_02 | Visões de implementação, implantação e dados |
| SubEquipe_03 | Processos, qualidade, chat e moderação |

## Entregas anteriores

<div class="action-links">
  <a class="button-secondary" href="https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/">Entrega 02</a>
  <a class="button-secondary" href="https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/">Entrega 03</a>
</div>
