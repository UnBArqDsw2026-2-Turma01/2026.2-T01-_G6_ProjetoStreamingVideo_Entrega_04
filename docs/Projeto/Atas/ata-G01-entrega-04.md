# Ata — Reunião Geral 1 (Entrega 04)

## Identificação

- **Reunião:** Reunião Geral 1 — planejamento da Entrega 04.
- **Data e horário:** 9 de novembro de 2026, às 19h, conforme informado por Lucas Andrade Zanetti (a transcrição em si não traz data nem horário). A fala final está em 22min40s da gravação.
- **Local ou plataforma:** Reunião on-line; gravação no Microsoft Stream/SharePoint.
- **Participantes com fala identificada na transcrição:** Lucas Andrade Zanetti, Davi Severiano Freitas, Philipe Amâncio Reis Caetano, Pedro Henrique Freire Rodrigues, Pedro Druck Montalvão Reis e Mateus Rodrigues Barreto. As falas de Pedro Druck e de Mateus Rodrigues Barreto saíram ininteligíveis na transcrição automática, então só a presença deles é registrada.
- **Sem fala registrada** (podem ter estado presentes sem falar): Matheus Lemes Amaral, Heitor Macedo Ricardo, Daniel de Oliveira Lira, Hugo Freitas Silva e Eduardo Lôbo Moreira. A transcrição identifica quem falou, mas não confirma a lista completa de presentes.
- **Responsável pelo registro:** não definido na reunião. Esta ata foi redigida a partir da transcrição VTT, com apoio de IA generativa, e **aguarda revisão** de quem participou (ver *Revisão do registro*).
- **Gravação:** [Microsoft Stream](https://unbbr.sharepoint.com/:v:/s/ProjetoStreamingVideoArquiteturaeDesenhodeSoftware/IQDXj5ZKpIxORbxK2ETXbhnNATq_9_XvhISiYE1SKWwbB4k?e=22qgkL&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D).
- **Transcrição-fonte:** transcrição automática (VTT) da gravação, mantida com o autor do registro e **não versionada** neste repositório. Os horários citados abaixo (`mm:ss`) se referem a ela.

> **Qualidade da transcrição.** A transcrição automática tem trechos em inglês sem sentido e nomes trocados. Só entraram aqui decisões e falas cujo texto estava claro; o que não pôde ser confirmado está marcado como tal.

## Pauta

- Data de entrega e de apresentação.
- Foco da Entrega 04: Documento de Arquitetura (DAS) ou Reutilização de Software.
- Divisão do trabalho entre as três subequipes e organização dos repositórios.
- Cenário arquitetural que servirá de evidência de “algo rodando”.
- Pontos de integração (shared kernel), iniciativas extras, riscos e regras de trabalho.

## Discussões e decisões

| Id | Decisão ou resultado | Justificativa e fontes | Participantes identificados |
| --- | --- | --- | --- |
| **D01** | **Foco da entrega: DAS + IA Generativa.** Aprovado. | Lucas propôs o DAS por ele se ligar ao que já foi feito: rastreabilidade requisito (E01) → modelo (E02) → padrão (E03) → pacote/componente (E04). A Reutilização exigiria componentes React, e o código do grupo é TypeScript/Node, o que obrigaria a refazer muita coisa. A evidência executável será um fluxo ponta a ponta. Fonte: 00:13–08:52 (apresentação do plano); aprovação em 21:40–22:15. | Lucas propôs. Davi declarou-se a favor (21:40). Lucas registrou que a SubEquipe_03 também concordava (21:46) e concluiu “todos de acordo” (22:15). |
| **D02** | **Divisão por subequipe aprovada com ressalvas.** SubEquipe_01: Visão Lógica (obrigatória) e estrutura do DAS; código de transmissão (Builder), conteúdo (Factory Method) e roteiro do CA-01 no terminal. SubEquipe_02: Visão de Implementação e Implantação; código do shared kernel (primeiro), monetização e página web. SubEquipe_03: Visão de Processos e Qualidade; código de identidade, chat e moderação. A divisão dentro de cada subequipe é livre. | Lucas defendeu uma visão por subequipe porque uma única visão feita pelas 11 pessoas viraria confusão, e cada subequipe pode apoiar-se nos documentos das entregas anteriores e em IA para entender as partes que não implementou. **Ressalvas de Davi:** (a) cada subequipe conhece só parte do sistema; (b) alternativa de reduzir o número de visões e fazer as três subequipes trabalharem em cada uma, que Lucas rejeitou. Davi observou ainda que a entrega mínima é a Visão Lógica mais uma visão; Lucas respondeu que, com três subequipes, vale dividir mais, e que mais visões rendem mais pontos de iniciativa extra. Fonte: 09:14–11:26 e 17:09–20:10. | Lucas, Davi, Philipe (disse que a questão se resolve na elaboração do documento, 17:47). Davi: “Ok, vamos ver” (20:00). Pedro Henrique: “Bem tranquilo” (20:07). |
| **D03** | **Datas: sem confirmação documental.** O grupo seguirá, de forma **provisória**, com D = 23/11/2026 como data-limite. | Lucas indicou 23/11 como a entrega e o grupo como o primeiro a apresentar, e lembrou que a entrega seria dia 27 (“se eu não me engano”). Davi afirmou que a entrega ocorre sempre no primeiro dia das apresentações. Ninguém confirmou no Aprender3. Adotou-se a data **mais cedo** das duas por segurança. Fonte: 00:13–01:51. | Lucas, Davi. |
| **D04** | **Horário de congelamento em D-1: pendente.** Não foi discutido. | O plano prevê congelar o repositório em D-1; o horário exato não foi tratado na reunião. | — |
| **D05** | **Próxima reunião geral: pendente de data.** Lucas propôs comunicar por mensagem, issues e comentários, com reuniões por subequipe e uma reunião final; não houve objeção registrada. | Lucas está com pouco tempo para reuniões e disse que consegue responder mensagens ao longo do dia. Davi observou que o trabalho exige muita integração entre as subequipes e que talvez fossem necessárias mais reuniões que o habitual. Fonte: 20:37–21:40. | Lucas, Davi. |
| **D06** | **Apresentação ao vivo; a parte de vídeo do plano deve ser desconsiderada.** No fim, o grupo mostra o funcionamento (CA-01). | Lucas explicou que o plano presumia vídeo, mas que será a apresentação de sempre; comentou que, embora a professora fale em 10 minutos, as apresentações costumam durar bem mais. Pediu que o vídeo do plano não fosse considerado. Fonte: 01:51–08:52 (explicação do plano) e 21:50 (pedido para desconsiderar o vídeo). | Lucas. Sem objeção registrada. |
| **D07** | **Linguagem: continuar com TypeScript**, reaproveitando os módulos já prontos da Entrega 03. | Davi perguntou e Lucas confirmou. Fonte: 22:16–22:30. | Davi, Lucas. |
| **D08** | **Integração por shared kernel**, com contratos congelados em D-11. | Lucas explicou o conceito (termo de *Domain-Driven Design*: parte pequena do modelo que contextos combinam usar em comum) e disse já ter aplicado a ideia em outro projeto. Philipe resumiu como “um contrato”. Davi pediu para detalhar melhor depois (14:37). Fonte: 12:39–14:37 (explicação), 14:50 (Philipe) e 14:59–17:09 (contratos congelados em D-11). | Lucas, Philipe, Davi. |
| **D09** | **Repositórios e issues.** Lucas formata o repositório de documentação e o de código ao fim da reunião e cria as issues de código e do DAS, com rastreabilidade interna. | Objetivo: evitar conflito na divisão, já que o trabalho de todas as subequipes se junta ao final. Davi apontou que a SubEquipe_02 também trabalhou no chat na E03; Lucas respondeu que a divisão desses pontos precisa ser detalhada para não gerar conflito. Fonte: 09:14–09:53 e 20:10–20:37. | Lucas, Davi. |

## Encaminhamentos

| Atividade | Responsável | Prazo | Artefato ou issue relacionada |
| --- | --- | --- | --- |
| Formatar os repositórios de documentação e de código e criar as issues | Lucas Andrade Zanetti | Ao fim da reunião | [Documentação #1](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/1) · [Código #2](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/2) |
| Enviar o planejamento ao grupo, avisando que a parte de vídeo deve ser desconsiderada | Lucas Andrade Zanetti | Não definido | [Planejamento](/Projeto/Planejamento.md) |
| Confirmar no Aprender3 a data de entrega e a de apresentação | Equipe; responsável não definido | Antes do M1 | D03 |
| Perguntar à professora se o DAS é entregue por subequipe ou pelo grupo todo | Equipe; interlocutor não definido | Não definido | Dúvida levantada por Davi (09:53) |
| Detalhar a divisão do chat entre SubEquipe_02 e SubEquipe_03 e o contrato entre elas | Lucas Andrade Zanetti, nas issues de código | Antes do M1 | [Código #3](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/3) · [#11](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/11) · [#13](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04_Codigo/issues/13) |
| Enviar dúvidas e sugestões de mudança no planejamento | Todos | Não definido | [Planejamento](/Projeto/Planejamento.md) |
| Definir o horário de congelamento em D-1 e a data da próxima reunião geral | Equipe | Antes do M2 | D04 · D05 |

## Pontos em aberto identificados no registro

Estes pontos **não foram discutidos na reunião**; foram notados ao redigir esta ata e precisam de decisão do grupo.

1. **Vídeo de demonstração.** O template do relatório (seção “Vídeo de Demonstração” da [página 1.1.1](/Base/Relatórios/1.1.1.DAS.md)) e a [issue #14](https://github.com/UnBArqDsw2026-2-Turma01/2026.2-T01-_G6_ProjetoStreamingVideo_Entrega_04/issues/14) preveem um vídeo. A D06 manda desconsiderar o vídeo do plano. É preciso decidir se a seção do template fica com uma gravação curta (a evidência do CA-01 também precisa estar no Pages, já que a diretriz recomenda apresentar por ele) ou com outro registro do que está rodando.
2. **Prazo do contrato.** Com D = 23/11, o marco M1 (contrato do shared kernel congelado) cai em **12/11/2026** (prazo provisório).

## Revisão do registro

| Data | Autor do registro | Revisor | Alteração ou comprobatório |
| --- | --- | --- | --- |
| 10/11/2026 | Redação a partir da transcrição VTT, com apoio de IA generativa | Pendente | Cada decisão aponta o trecho da transcrição; gravação vinculada acima. Data e horário da reunião informados por Lucas Andrade Zanetti. |
