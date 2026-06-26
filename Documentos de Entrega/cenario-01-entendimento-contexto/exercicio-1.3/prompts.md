# Conversa — Projeto NovaTech: PRD Assistente de Atendimento com RAG

**Data:** Junho/2026  
**Participantes:** Usuário (Product Specialist) + Claude (Assistente)  
**Tema:** Criação, revisão e refinamento de Especificação de Requisitos (PRD) para assistente de atendimento com RAG

---

## Índice

1. [Turno 1 — Geração do PRD v1.0](#turno-1)
2. [Turno 2 — Revisão sênior do PRD v1.0](#turno-2)
3. [Turno 3 — Refinamento do PRD para v2.0](#turno-3)
4. [Turno 4 — Extração desta conversa](#turno-4)
5. [Inventário de arquivos](#inventário-de-arquivos)

---

## Turno 1 — Geração do PRD v1.0 {#turno-1}

### Documentos fornecidos pelo usuário

| Arquivo | Descrição |
|---------|-----------|
| `anexo-a-documentacao-simulada-novatech.md` | Documentação simulada da NovaTech: POL-001 (Política de Devolução v3.1), PROC-042 v1.0 e v2.0 (Frete Especial), SLA-2024 (Tabela de SLA), FAQ-Atendimento (documento informal) |
| `anexo-b-chunks-referencia-rag.md` | Chunks de referência do pipeline de RAG — resultado simulado do Azure AI Search por similaridade semântica, com mapa de cobertura e armadilhas para exercícios de avaliação |
| `jornada-atendente-assistente-ia.md` | Fluxo principal, fallbacks, ciclo de feedback e guardrails do assistente — documento base de operação |

### Prompt do usuário

```
Você é Product Specialist no projeto NovaTech.

Seu objetivo é produzir uma Especificação de Requisitos do Produto para um assistente de
atendimento com RAG, em linguagem de negócio (não técnica), orientada à operação real do
atendimento.

- Contexto do cenário: A NovaTech é uma empresa de logística com grande volume de
  documentação espalhada em SharePoint, wiki interna e planilhas. O time de atendimento hoje
  consulta em média 4 fontes por chamado e gasta cerca de 12 minutos buscando respostas. O
  objetivo do assistente é reduzir esse tempo para menos de 2 minutos, sempre respondendo com
  base documental e indicação de fonte.

- Dados do discovery: Os atendentes hoje abrem em média 4 fontes diferentes por chamado.
  As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%), política de
  devolução (20%) e outros (20%). Em 15% dos casos, o atendente não encontra resposta e escala
  para o supervisor.

- Considere o documento 'anexo-a-documentacao-simulada-novatech.md' como exemplo concreto das
  contradições e gaps que a spec precisa endereçar e demais documentos operacionais anexados.

- Considere o seguinte conceito de pipeline de RAG: "Documentos são divididos em pedaços
  (chunks), transformados em representações numéricas (embeddings), armazenados num banco
  vetorial, e recuperados por similaridade quando o usuário faz uma pergunta. O LLM então gera
  uma resposta usando os chunks recuperados como contexto."

- Regras obrigatórias:
    - Não usar conhecimento externo aos documentos anexados.
    - Em caso de conflito entre documentos, aplicar a seguinte ordem de precedência:
        - Política corporativa vigente
        - Procedimento mais recente aprovado
        - FAQ operacional
        - Conteúdos sem data/sem aprovação explícita
    - Todo requisito deve ser claro para público não técnico e testável por QA.
    - Destacar explicitamente riscos de curadoria de conteúdo (obsolescência, duplicidade,
      conflito de versão, ausência de dono).

- Escopo mínimo de requisitos:
    1. Fontes a indexar e fontes a excluir/marcar como obsoletas.
    2. Tratamento de documentos contraditórios.
    3. Comportamento para perguntas sem resposta na base.
    4. SLA de atualização da base após publicação de novos documentos.
    5. Rastreabilidade da resposta (fonte, versão, trecho, data).

- Requisitos de qualidade
    - Não use conhecimento externo.
    - Seja específico e operacional.
    - Priorize linguagem clara para negócio e delivery.
    - Os requisitos deve ser compreensível por pessoas não técnicas.
    - Os requisitos demonstram entendimento de que a qualidade do RAG depende da curadoria
      dos dados, não só da tecnologia.
    - O tratamento de contradições e ausência de resposta mostra maturidade de produto.
    - Os requisitos são testáveis (o QA conseguiria verificar cada um).

- Formato obrigatório de saída:
    1. Resumo executivo (máx. 10 linhas).
    2. Escopo e não escopo.
    3. Requisitos funcionais (tabela): ID, Requisito, Justificativa de negócio,
       Critério de aceitação (testável), Evidência documental esperada.
    4. Requisitos não funcionais (tabela): ID, Requisito, Métrica/SLA, Como validar.
    5. Regras de decisão em conflito documental.
    6. Política de ausência de resposta e escalonamento.
    7. Matriz de rastreabilidade (requisito x fonte).
    8. Gaps e pendências (perguntas que precisam de decisão de negócio).
    9. Cenários de teste BDD (Given/When/Then) para os requisitos críticos.
```

### Saída gerada pelo assistente

**Arquivo gerado:** `PRD-NovaTech-Assistente-RAG.docx`

Documento Word completo com as seguintes seções:

| Seção | Conteúdo |
|-------|----------|
| 1. Resumo Executivo | Contexto (12 min/consulta, 4 fontes, 15% escalonamento), objetivo, principais riscos de curadoria |
| 2. Escopo e Não Escopo | Fontes a indexar (POL-001, PROC-042-v2, SLA-2024), exclusões (FAQ como primário, frete < 500kg, seguro de carga) |
| 3. Requisitos Funcionais | 10 RFs com ID, requisito, justificativa, critério de aceitação testável e evidência documental |
| 4. Requisitos Não Funcionais | 8 RNFs com métrica/SLA e forma de validação (tempo de resposta, cobertura, alucinação, disponibilidade etc.) |
| 5. Regras de Decisão em Conflito | Ordem de precedência (4 níveis), comportamento esperado, tabela com 5 conflitos concretos identificados |
| 6. Política de Ausência de Resposta | 5 situações de fallback com gatilho, mensagem e encaminhamento; tabela de encaminhamentos de alto risco |
| 7. Matriz de Rastreabilidade | Cruzamento de todos os requisitos com suas fontes documentais e nível de precedência |
| 8. Gaps e Pendências | 8 gaps (G-01 a G-08) com impacto, área responsável e prazo sugerido |
| 9. Cenários BDD | 8 cenários Given/When/Then para requisitos críticos (carga perigosa, conflito de versão, SLA, tier inexistente etc.) |
| Apêndice | 6 riscos de curadoria de conteúdo com exemplos, consequências e mitigações |

---

## Turno 2 — Revisão sênior do PRD v1.0 {#turno-2}

### Documentos fornecidos pelo usuário

| Arquivo | Descrição |
|---------|-----------|
| `PRD-NovaTech-Assistente-RAG_v1.docx` | PRD v1.0 gerado no turno anterior, submetido para revisão |

### Prompt do usuário

```
Atue como revisor sênior de produto e qualidade de requisitos.
Analise o documento de Especificação de Requisitos do Produto (PRD) anexado e identifique
gaps, ambiguidades e inconsistências.

- Regras da análise:
    - Avalie clareza, testabilidade, completude, consistência, rastreabilidade e viabilidade
      operacional.
    - Considere ambiguidades de linguagem, ausência de critérios de aceite, conflitos entre
      seções, requisitos não mensuráveis e lacunas de decisão.
    - Não reescreva o PRD completo.
    - Não proponha solução detalhada; foque em feedback diagnóstico.
    - Não use conhecimento externo ao documento analisado.

- Formato obrigatório de saída:
    - Item: título curto do problema
    - Evidência: trecho resumido do documento que motivou o ponto
    - Impacto: risco para negócio, operação, QA ou entrega
    - Pergunta de validação: pergunta objetiva para destravar a decisão

Restrições:
    - Forneça apenas o feedback.
    - Seja direto, sem introdução e sem conclusão.
    - Liste de 8 a 15 achados, priorizados por severidade.
```

### Saída gerada pelo assistente

**Arquivo gerado:** `Feedback.md` *(entregue como resposta inline; o usuário o usou como entrada no turno seguinte)*

13 achados priorizados por severidade:

| # | Severidade | Título do Achado |
|---|-----------|-----------------|
| 1 | CRÍTICO | RF-02 contradiz seção 5.1 e RF-04 ao impor versão sem resolução formal do conflito |
| 2 | CRÍTICO | "Nível de confiança" é conceito central sem definição operacional |
| 3 | CRÍTICO | RF-04 e RF-08 exigem detecção de conflito sem especificar o mecanismo |
| 4 | ALTO | Comportamento para perguntas multi-domínio não está especificado |
| 5 | ALTO | SLA de atualização da base (RNF-04) não define o que conta como "publicação válida" |
| 6 | ALTO | Matriz de rastreabilidade referencia documento externo sem incorporação formal |
| 7 | ALTO | G-07 é gap de go-live sem critério de decisão, mas RF-02 pressupõe a resposta |
| 8 | ALTO | Critério de "confiança baixa" como gatilho de fallback sem threshold definido |
| 9 | MÉDIO | BDD-06 e RF-04/RNF-07 exigem comportamentos opostos para a mesma pergunta |
| 10 | MÉDIO | RNF-01 mede primeiro token, mas seção 2.1 exige resposta completa com 5 elementos |
| 11 | MÉDIO | RF-09 define feedback "ao final" mas seção 6.1 descreve acionamento imediato |
| 12 | MÉDIO | Seção 2.2 exclui FAQ como fonte, mas seção 5.1 o posiciona na hierarquia (implica indexação) |
| 13 | BAIXO | RNF-02 define 50 perguntas-teste sem responsável, critério de representatividade ou aprovação |

---

## Turno 3 — Refinamento do PRD para v2.0 {#turno-3}

### Documentos fornecidos pelo usuário

| Arquivo | Descrição |
|---------|-----------|
| `PRD-NovaTech-Assistente-RAG-v1.docx` | PRD v1.0 — documento base a ser refinado |
| `Feedback.md` | 13 achados da revisão sênior — entradas para as alterações |

### Prompt do usuário

```
Você é Product Specialist e editor técnico de requisitos.

- Tarefa:
    - Refinar o documento de especificação (PRD) com base no feedback recebido, preservando o
      escopo original e aumentando clareza, testabilidade e consistência.
    - Aplique apenas mudanças justificadas pelo feedback, sem conhecimento externo, mantendo o
      escopo original.
    - Elimine ambiguidades e torne os requisitos testáveis.

- Entradas:
    - Documento atual do PRD: PRD-NovaTech-Assistente-RAG-v1
    - Feedback da revisão: Feedback.md

- Entregue:
    - PRD refinado completo.
    - Tabela de alterações aplicadas (original, refinado, motivo, item de feedback).
    - Pendências em aberto com pergunta objetiva para destravar decisão.
```

### Saída gerada pelo assistente

**Arquivo gerado:** `PRD-NovaTech-Assistente-RAG-v2.docx`

Documento Word completo v2.0 com 18 alterações aplicadas e as seguintes mudanças estruturais:

#### Seções novas ou reestruturadas

| Seção | Alteração |
|-------|-----------|
| Capa | Histórico de versões adicionado (v1.0 → v2.0) |
| **2.3 (NOVA)** | Definição operacional de nível de confiança: alto ≥ 0,80; médio 0,60–0,79; baixo < 0,60 — com tabela de critérios, condições e comportamentos |
| 2.1 (coleta de feedback) | Dois mecanismos explicitados: imediato (botão "Discordo") e encerramento (formulário estruturado) |
| 2.2 (não escopo) | FAQ alinhado com seção 5.1: indexado com metadado `FAQ-informal`, nunca como fonte normativa primária |
| **5.3 (NOVA)** | Mecanismo determinístico de detecção de conflito por metadado (documento-id, versão, status, grupo-de-conflito) |
| 5.4 (renumerada) | Casos concretos de conflito (antes era 5.3) |
| **RF-11 (NOVO)** | Comportamento para perguntas multi-domínio: composição por seção, restrição categórica primeiro, fallback parcial |
| RF-02 (revisado) | Condicional a G-01: preferência por v2 apenas após ratificação formal; fallback de conflito enquanto pendente |
| RF-04 (revisado) | Taxa mínima de detecção: ≥ 90% (10 perguntas-teste, ≥ 9 alertas) |
| RF-09 (revisado) | Dois mecanismos independentes de feedback especificados |
| RNF-01 (revisado) | SLA medido até resposta completa, não primeiro token |
| RNF-02 (revisado) | Responsável (PM), aprovador (Gerente de Atendimento), distribuição obrigatória (35/25/20/20%) |
| RNF-04 (revisado) | "Publicação válida" definida: área correta + metadados obrigatórios + aprovação do responsável |
| RNF-07 (revisado) | Taxa mínima ≥ 90%; referência à seção 5.3 |
| Seção 7 (matriz) | `jornada-atendente-assistente-ia.md` incorporado formalmente como Anexo A |
| **G-09 (NOVO)** | Calibração dos thresholds de confiança ao final do piloto — bloqueante de go-live |
| **BDD-06a/06b (dividido)** | BDD-06 separado em estado G-01 pendente (conflito ativo) e G-01 resolvido (v2 vigente) |
| **BDD-09 (NOVO)** | Cenário multi-domínio: devolução + carga perigosa + frete especial |
| **Seção 10 (NOVA)** | Tabela de alterações v1.0 → v2.0 com 18 linhas: seção, texto original, texto revisado, motivo, achado |

#### Pendências em aberto (gaps)

| Gap | Bloqueia go-live? | Área Responsável |
|-----|:-----------------:|-----------------|
| G-01 | **SIM** | Diretoria Comercial + Operações |
| G-02 | **SIM** | Diretoria de Operações + Jurídico |
| G-03 | **SIM** | Comercial |
| G-04 | **SIM** | Comercial + Operações |
| G-07 | **SIM** | Operações + Comercial |
| G-09 | **SIM** | Product Manager + Gerente de Atendimento |
| G-05 | não | Gestão de Riscos |
| G-06 | não | Compliance |
| G-08 | não | Gerência de Atendimento |

---

## Turno 4 — Extração desta conversa {#turno-4}

### Prompt do usuário

```
Extraia a conversa desse chat em um arquivo markdown, com as entradas, prompts e documentos
que forneci e como saída, nome de cada arquivo gerado
```

### Saída gerada pelo assistente

**Arquivo gerado:** `conversa-novatech-rag.md` *(este arquivo)*

---

## Inventário de arquivos {#inventário-de-arquivos}

### Entradas (fornecidas pelo usuário)

| Arquivo | Turno | Tipo | Descrição |
|---------|-------|------|-----------|
| `anexo-a-documentacao-simulada-novatech.md` | 1 | Documentação | Base de conhecimento simulada da NovaTech (5 documentos: POL-001, PROC-042 v1 e v2, SLA-2024, FAQ) |
| `anexo-b-chunks-referencia-rag.md` | 1 | Referência RAG | Chunks simulados do pipeline de RAG com mapa de cobertura e armadilhas de avaliação |
| `jornada-atendente-assistente-ia.md` | 1 | Operacional | Fluxo do assistente, fallbacks, feedback e guardrails — posteriormente incorporado como Anexo A do PRD |
| `PRD-NovaTech-Assistente-RAG_v1.docx` | 2 | PRD para revisão | Versão 1.0 do PRD submetida para análise de gaps e inconsistências |
| `PRD-NovaTech-Assistente-RAG-v1.docx` | 3 | PRD para refinamento | Versão 1.0 do PRD usada como base para geração da v2.0 |
| `Feedback.md` | 3 | Feedback | 13 achados da revisão sênior usados como entrada para o refinamento |

### Saídas (geradas pelo assistente)

| Arquivo | Turno | Descrição |
|---------|-------|-----------|
| `PRD-NovaTech-Assistente-RAG.docx` | 1 | PRD v1.0 — especificação inicial completa com 9 seções e apêndice |
| `Feedback.md` | 2 | 13 achados priorizados por severidade (CRÍTICO / ALTO / MÉDIO / BAIXO) |
| `PRD-NovaTech-Assistente-RAG-v2.docx` | 3 | PRD v2.0 — versão refinada com 18 alterações, RF-11 novo, seções 2.3 e 5.3 novas, BDD-06 dividido, BDD-09 novo, G-09 novo, seção 10 (tabela de alterações) |
| `conversa-novatech-rag.md` | 4 | Este arquivo — extração completa da conversa |
