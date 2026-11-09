# Como contribuir

1. Pegue uma issue atribuída a você ([documentação](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues) ou [código](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues)). Cada issue diz o que fazer, quem revisa e quais páginas ou arquivos alterar.
2. Crie uma branch a partir da `main`: `docs/<assunto>` (ex.: `docs/visao-logica`).
3. Edite **apenas** a página da sua issue. Fontes UML (`.puml`) e imagens vão em `docs/Base/Relatórios/1.1.1.DAS/assets/<visao>/`.
4. Em toda página que alterar: atualize o **Histórico de Versões**, as **Referências** e a seção de **Rastreabilidade**. Diagrama sempre com fonte versionada + imagem exportada.
5. Acrescente sua linha em **Versionamentos** do [relatório 1.1.1](docs/Base/Relatórios/1.1.1.DAS.md) no mesmo PR.
6. Abra um PR com `Closes #N`, peça revisão a quem a issue indica (seções do DAS são revisadas por **outra** subequipe) e use Conventional Commits (`docs(visao-logica): add package diagram`).
7. Escreva o próprio relato de IA no Foco_02. Não preencha a linha de outra pessoa.

## Links no site

Links internos do Docsify partem da raiz de `docs/`, por exemplo `[Introdução](/Introducao.md)`. Não use o prefixo `/docs/`. Para entregas anteriores, use o endereço completo do GitHub Pages.

## Antes de pedir revisão

- Navegue localmente com `python3 -m http.server 8000 --directory docs`.
- Confira tabelas, imagens, diagramas Mermaid e links.
- Não troque “A preencher” por realização, avaliação ou autoria sem evidência.
- Página nova no menu? Atualize `docs/_sidebar.md`.

## Prazo

Commits e alterações no Pages depois do prazo **não são aceitos**. O horário de congelamento (D-1) fica registrado em ata.
