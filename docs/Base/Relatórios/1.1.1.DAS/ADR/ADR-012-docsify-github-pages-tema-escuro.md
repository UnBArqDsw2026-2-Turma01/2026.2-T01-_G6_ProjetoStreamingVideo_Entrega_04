# ADR-012 — Docsify no GitHub Pages, com tema claro e escuro

| Campo | Valor |
| --- | --- |
| Status | Aceita e aplicada; proposta de revisão cruzada pendente |
| Data | 22/11/2026 |
| Autoria | Lucas Andrade Zanetti (SubEquipe_01); redigido com apoio de IA generativa |
| Revisor | Pendente: SubEquipe_03 |
| Escopo | Plataforma que publica a documentação da Entrega 04, o fluxo de publicação e o tema visual. Não trata da documentação das entregas anteriores além da continuidade de plataforma. |
| Seção do DAS | [1.1.1.2. Representação Arquitetural](/Base/Relatórios/1.1.1.DAS/1.1.1.2.RepresentacaoArquitetural.md) (como o entregável é apresentado) · [Checklist](/Checklist.md) (cópia offline) |

## 1. Contexto

O entregável da disciplina é “relatório documentado no GitPages”, apresentado à professora a partir do site. As Entregas 01, 02 e 03 foram publicadas como sites **Docsify** em GitHub Pages, com `_sidebar.md` e rotas por `#/`. A Entrega 04 acrescenta mais conteúdo (DAS com 12 seções, sete ADRs novos, diagramas em `.puml`/`.svg`/`.png`, tabelas largas) e será apresentada ao vivo, possivelmente em sala escura ou projetor.

A pergunta: **continuar no Docsify, trocar de gerador ou publicar só os arquivos Markdown, e como tornar a leitura confortável (navegação e tema)?**

## 2. Evidências

| Id | O que mostra | Fonte |
| --- | --- | --- |
| D-01 | As entregas anteriores usam Docsify com `_sidebar.md` e rotas por `#/` (os arquivos `.md` são servidos diretamente). | [E01](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_01/_sidebar.md) · [E02](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_02/_sidebar.md) · [E03](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/_sidebar.md) |
| D-02 | A publicação é um workflow do GitHub Actions que envia a pasta `docs/` ao Pages a cada `push` na `main`, sem etapa de build. | [`pages.yml`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/blob/main/.github/workflows/pages.yml) · commit [`bbdd04b`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/commit/bbdd04b) |
| D-03 | O Docsify e seus plugins (busca, copiar código, paginação, Mermaid) vêm de CDN; sem internet a página não carrega. | [`docs/index.html`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/blob/main/docs/index.html) · README, seção “Há algo a ser executado?” |
| D-04 | Em Docsify, imagens e links escritos em **HTML** (`<img src>`, `<a href>`) não são reescritos pelo Docsify: resolvem contra a raiz do site, não contra a página. Foi esse o motivo de as visões 1.1.1.7 e 1.1.1.8 terem imagens quebradas, corrigido em 22/11/2026. | revisão de links desta data (ver [Planejamento](/Projeto/Planejamento.md)) |
| D-05 | O leitor pode escolher o tema; sem escolha, vale a preferência do sistema (`prefers-color-scheme`). O botão guarda a escolha em `localStorage` e funciona sem ele. | [`docs/index.html`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/blob/main/docs/index.html) · [`docs/assets/styles.css`](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/blob/main/docs/assets/styles.css) |
| D-06 | Os pares de cor principais do tema escuro têm razão de contraste entre 6,1:1 e 15,6:1 (texto/fundo 15,6:1; link/fundo 8,4:1; botão 6,1:1), calculados pela fórmula de luminância relativa da WCAG 2.x. | cálculo feito em 22/11/2026; mínimo AA é 4,5:1 |

## 3. Alternativas consideradas

| # | Alternativa | A favor | Contra |
| --- | --- | --- | --- |
| **A** | **Continuar no Docsify** no GitHub Pages, sem build | Mesma plataforma das E01–E03; links entre entregas continuam válidos; publicar é `push` na `main`; o conteúdo é Markdown puro, legível também no GitHub | Depende de CDN (D-03); a busca indexa só o que o leitor já abriu; links HTML exigem cuidado (D-04) |
| B | **MkDocs** (ou Material for MkDocs) | Geração estática, busca completa, tema escuro nativo | Troca de plataforma na última semana; exige etapa de build e `mkdocs.yml`; as rotas `#/` das E01–E03 deixariam de coincidir. *Não prototipado.* |
| C | **Só Markdown no GitHub**, sem site | Zero manutenção | A diretriz pede o relatório no GitPages; navegação lateral, busca e Mermaid ficam por conta do GitHub. |
| D | **Docusaurus** | Ecossistema rico, versionamento de docs | Build Node/React e dependências a mais para um site de cerca de 40 arquivos Markdown. *Não prototipado.* |

## 4. Decisão

Adotar **A** e acrescentar quatro melhorias, todas em `docs/` e sem dependência nova:

1. **Navegação por grupos** no `_sidebar.md`: Contexto e gestão · Arquitetura (DAS, em Fundamentos, Visões 4+1 e Atributos) · Decisões (ADRs) · Reutilização e IA · Participação e extras · Código e entregas anteriores.
2. **Tema claro e escuro** por tokens de cor em CSS (`:root` e `:root[data-theme="dark"]`), com botão de alternância, escolha salva e preferência do sistema como padrão.
3. **Diagramas continuam com fundo claro** no tema escuro: os `.svg`/`.png` do PlantUML são exportados com fundo claro e ficam em cartão claro, em vez de serem invertidos por filtro (que alteraria as cores semânticas dos diagramas).
4. **Mermaid acompanha o tema** (`dark` ou `default`); como o diagrama é desenhado uma vez, a alternância recarrega a rota **somente** em páginas que têm Mermaid.

## 5. Consequências

**Fica mais fácil:** ler no escuro; achar uma seção pela navegação por grupo; manter os links das entregas anteriores; publicar sem build.

**Fica mais difícil / custo:**

- **Internet obrigatória** na apresentação (D-03). Mitigação já prevista no [Checklist](/Checklist.md): cópia offline do site antes do congelamento.
- Páginas com Mermaid recarregam ao trocar o tema; o leitor perde a posição de rolagem nessas páginas.
- Dois temas dobram a superfície de revisão visual: qualquer cor nova precisa de um valor em cada tema.

**Riscos e limites declarados:**

- O tema escuro foi conferido por cálculo de contraste (D-06) e por inspeção no navegador; **não** houve teste com leitor de tela nem com todos os navegadores.
- Os diagramas PlantUML claros dentro de uma página escura criam contraste forte; é uma troca consciente por fidelidade de cor.
- O Docsify é carregado de CDN com versão fixa (`docsify@4.13.1`, `mermaid@10.9.1`); `docsify-copy-code@3` e `docsify-pagination@2` aceitam versões menores novas, o que pode mudar o comportamento sem aviso.

## 6. Referências

- Documentação do Docsify. Disponível em: <https://docsify.js.org/>.
- GITHUB. *Using custom workflows with GitHub Pages*. Disponível em: <https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages>.
- W3C. *Web Content Accessibility Guidelines (WCAG) 2.2*, critério 1.4.3 (contraste mínimo). Disponível em: <https://www.w3.org/TR/WCAG22/>.
- Entrega 03: [site publicado](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/) e [ADR-001 a ADR-004](https://unbarqdsw2026-2-turma01.github.io/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_03/#/Base/1.PadroesDeProjeto.md).
- [Ata da Reunião Geral 1](/Projeto/Atas/ata-G01-entrega-04.md), D06 (apresentação ao vivo).

## Histórico de Versões

| Versão | Data | Descrição | Autor(es) | Revisor(es) |
| --- | --- | --- | --- | --- |
| 1.0 | 22/11/2026 | Registro da escolha de plataforma, alternativas A–D, tema claro/escuro e reorganização da navegação, redigido com apoio de IA generativa | Lucas Andrade Zanetti | Pendente (SubEquipe_03) |
