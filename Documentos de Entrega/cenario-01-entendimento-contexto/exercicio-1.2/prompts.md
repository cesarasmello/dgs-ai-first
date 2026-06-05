# Histórico do Chat — Tarefa 1 

**Data:** 01–02/06/2026  
**Contexto:** Exercício 1.1 — Análise documental com Progressive Disclosure e geração de jornada do atendente com assistente de IA (RAG)

---

## Turno 1 — Entradas e prompt

**Ferramenta:** Claude

### Documentos fornecidos

| Arquivo | Tipo |
|---------|------|
| `entrega-final.md` | Documento de entrega do Exercício 1.1 — registro de etapas, scripts, outputs e análise crítica |
| `analise-de-inconsistencia-vs-FAQ.md` | Análise de inconsistências entre documentação formal e FAQ de atendimento |

### Conteúdo dos documentos

**`entrega-final.md`** documenta três etapas de análise:
- Etapa 1: Visão geral — mapa de temas cobertos e hipóteses de gaps (5 documentos resumidos enviados progressivamente)
- Etapa 2: Análise profunda — inconsistências entre PROC-042-v1 e PROC-042-v2
- Etapa 3: Cruzamento com prática informal — inconsistências entre documentação formal e FAQ de atendimento
- Mapa de riscos: 2 riscos de processo e 2 riscos de negócio
- Reflexão sobre Progressive Disclosure

**`analise-de-inconsistencia-vs-FAQ.md`** contém:
- Resumo executivo com 7 inconsistências identificadas
- Tabela de inconsistências (FAQ-INC-001 a FAQ-INC-007)
- Top 3 riscos prioritários
- 6 perguntas para validação com stakeholders

### Prompt enviado

```
Você é Product Specialist no projeto NovaTech e precisa desenhar a jornada do atendente
usando um assistente de IA com RAG, para apresentação interna e ao cliente.

Seu objetivo é gerar uma jornada textual clara, estruturada e orientada a operação real
do atendimento.

- Contexto do cenário: A NovaTech é uma empresa de logística com grande volume de
  documentação espalhada em SharePoint, wiki interna e planilhas. O time de atendimento
  hoje consulta em média 4 fontes por chamado e gasta cerca de 12 minutos buscando
  respostas. O objetivo do assistente é reduzir esse tempo para menos de 2 minutos,
  sempre respondendo com base documental e indicação de fonte.

- Dados do discovery: Os atendentes hoje abrem em média 4 fontes diferentes por chamado.
  As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%),
  política de devolução (20%) e outros (20%). Em 15% dos casos, o atendente não encontra
  resposta e escala para o supervisor.

- Considere estes riscos e restrições disponíveis no documento `entrega-final.md`
- Considere o documento `analise-de-inconsistencia-vs-FAQ.md` como insumo para o
  fluxo de fallback, feedback e guardrails

- Sua tarefa: Produza a jornada do atendente com os seguintes blocos obrigatórios:

    1. Visão geral da jornada: Explique em 5 a 8 linhas qual é o papel do assistente
       dentro do fluxo do atendente.

    2. Fluxo principal: Descreva passo a passo o caminho feliz, considerando:
        - atendente recebe dúvida do cliente
        - atendente consulta o assistente
        - assistente interpreta a pergunta
        - assistente recupera conteúdo relevante
        - assistente responde com fonte
        - atendente usa a resposta no atendimento
       Apresente em formato sequencial, com passos numerados.

    3. Fluxo de fallback: Descreva o que deve acontecer quando:
        - o assistente não encontra resposta confiável
        - há conflito entre documentos
        - a resposta encontrada é incompleta
        - o atendente discorda da resposta apresentada
        - o caso exige validação humana
       Mostre claramente para onde o caso deve ser encaminhado, por exemplo: supervisor,
       Compliance, Comercial ou Operações, conforme o tipo de dúvida.

    4. Fluxo de feedback: Descreva como o atendente informa que a resposta estava:
        - errada
        - desatualizada
        - incompleta
        - sem fonte suficiente
        - em conflito com a prática operacional
       Explique como esse feedback pode alimentar melhoria contínua da base de
       conhecimento e do assistente.

    5. Guardrails obrigatórios do assistente: Defina pelo menos 2 guardrails específicos
       ao contexto da NovaTech. Não quero guardrails genéricos. Eles devem ser aplicáveis
       ao domínio de logística, atendimento e RAG documental.

       Exemplos do tipo de especificidade esperada:
       - nunca inventar prazo de entrega sem base documental
       - nunca escolher silenciosamente entre versões contraditórias de procedimento
       - nunca usar FAQ informal como única fonte quando houver política formal conflitante

    Requisitos de qualidade:
    - Não invente capacidades técnicas não descritas.
    - Não use conhecimento externo.
    - Seja específico e operacional.
    - Priorize linguagem clara para negócio e delivery.
    - A jornada deve ser compreensível por pessoas não técnicas.
    - O conteúdo deve estar pronto para virar um diagrama visual depois.

    Formato obrigatório de saída: Entregue em Markdown, com esta estrutura:
    - Jornada do Atendente com Assistente de IA
        1. Visão geral da jornada
        2. Fluxo principal
        3. Fluxo de fallback
        4. Fluxo de feedback
        5. Guardrails do assistente
```

---

## Turno 2 — Saídas geradas

### Arquivos gerados

| Arquivo | Descrição |
|---------|-----------|
| `jornada-atendente-assistente-ia.md` | Documento Markdown com a jornada completa do atendente |

### Diagrama interativo gerado (inline no chat)

Widget HTML interativo renderizado diretamente na conversa, com quatro abas navegáveis:

| Aba | Conteúdo |
|-----|----------|
| Fluxo principal | 6 passos sequenciais clicáveis com detalhe operacional de cada etapa |
| Fallbacks | 5 situações de fallback (F1–F5) com gatilho e encaminhamento nomeado por área |
| Feedback | 5 categorias de avaliação + ciclo de melhoria contínua com prazos |
| Guardrails | 5 guardrails específicos ao domínio NovaTech, expansíveis com justificativa |

---

## Turno 3 — Entradas e prompt

### Prompt enviado

```
Extraia a conversa desse chat em um arquivo markdown, com as entradas, prompts e
documentos que forneci e como saída, nome de cada arquivo gerado
```

### Arquivo gerado

| Arquivo | Descrição |
|---------|-----------|
| `historico-chat-novatech.md` | Este arquivo — extração completa do histórico do chat |

---

## Resumo de arquivos gerados na conversa

| Arquivo | Turno | Tipo |
|---------|-------|------|
| `jornada-atendente-assistente-ia.md` | 2 | Documento Markdown — jornada do atendente com assistente de IA |
| `historico-chat-novatech.md` | 3 | Documento Markdown — extração deste histórico de chat |

# Registro da Conversa — Jornada do Atendente com Assistente de IA

> Documento de extração do chat: reúne as **entradas** (documentos e prompts fornecidos pelo usuário) e as **saídas** (arquivos gerados), na ordem em que ocorreram.

**Projeto:** Jornada do atendente · NovaTech — Intent + Discovery
**Data da sessão:** 02/06/2026

---

# Histórico do Chat — Tarefa 2 

**Ferramenta:** Claude Desing

## 1. Entradas fornecidas pelo usuário

### 1.1 Documento anexado (fonte de verdade — Tarefa 1)

- **Arquivo:** `uploads/jornada-atendente-assistente-ia.md`
- **Tipo:** Markdown (jornada textual completa)
- **Conteúdo (estrutura):**
  1. Visão geral da jornada (assistente como camada de consulta documental / RAG; meta de reduzir consulta de 12 min para < 2 min)
  2. **Fluxo principal** — Passos 1 a 6: dúvida do cliente → consulta ao assistente → interpretação → recuperação na base indexada (PROC-042-v2, SLA-2024, POL-001) → resposta com fonte e nível de confiança → uso no atendimento
  3. **Fluxo de fallback** — situações 3.1 a 3.5: sem resposta confiável, conflito entre documentos, resposta incompleta, atendente discorda, validação humana obrigatória; com encaminhamentos por tema (Supervisor, Operações, Comercial, Compliance, Gestão de Riscos — ramal 4500)
  4. **Fluxo de feedback** — categorias (errada, desatualizada, incompleta, fonte insuficiente, conflito com a prática), ciclo de tratamento (triagem → revisão humana → atualização documental → reindexação → fechamento) e periodicidade (24 h / semanal / mensal)
  5. **Guardrails do assistente** — 5 regras de domínio (não escolher versões silenciosamente, não usar FAQ informal como fonte única, não criar expectativa de exceção a política categórica, não responder SLA sem critérios/tier, não inferir prazo sem versão/vigência)

### 1.2 Prompt principal (Tarefa 2 — briefing do diagrama)

Texto colado pelo usuário definindo a tarefa de transformar a jornada textual em um **diagrama visual de fluxo** para apresentação ao time interno e ao cliente. Requisitos centrais:

- **Objetivo:** representar a operação do atendimento com assistente de IA, mostrando os 3 caminhos obrigatórios — **fluxo principal, fluxo de fallback e fluxo de feedback**.
- **Fonte de verdade:** usar exclusivamente a jornada textual da Tarefa 1; não inventar etapas, decisões, áreas ou capacidades técnicas.
- **O diagrama deve:** representar o processo de ponta a ponta (do recebimento da dúvida ao encerramento/escalonamento); destacar visualmente os 3 caminhos; mostrar os pontos de decisão; indicar quando o atendente responde e quando escala; deixar explícitos os destinos do fallback (supervisor, Compliance, Comercial, Operações); mostrar como o feedback retorna para melhoria da base e do assistente.
- **Requisitos de qualidade:** sem conhecimento externo; sem inventar capacidades; específico, operacional e claro; legível para público não técnico; coerência total com a Tarefa 1.
- **Estrutura esperada:** início do atendimento → consulta ao assistente → decisão sobre confiança e suficiência → caminho principal (resposta confiável) → fallback (conflito, baixa confiança, ausência de resposta ou validação humana) → feedback (resposta errada, desatualizada, incompleta ou sem fonte) → encerramento ou tratativa posterior.
- **Formato de saída:** (1) diagrama visual com os 3 caminhos diferenciados; (2) um título; (3) legenda curta explicando cada caminho.
- **Título sugerido:** *Jornada do Atendente com Assistente de IA — Fluxo Principal, Fallback e Feedback*.

### 1.3 Prompts de acompanhamento

| # | Pedido do usuário |
|---|-------------------|
| 1 | Criar o diagrama de fluxo a partir do briefing (Tarefa 2) |
| 2 | "Save this design as a PDF: Jornada do Atendente - Fluxo.html" (geração da versão para impressão / PDF) |
| 3 | "Extraia a conversa desse chat em um arquivo markdown, com as entradas, prompts e documentos que forneci e como saída, nome de cada arquivo gerado" |

> Observação: foi apresentado um formulário de perguntas de alinhamento (formato, orientação, estilo visual, nível de detalhe, destinos de escalonamento). As perguntas expiraram sem resposta e o trabalho seguiu com os padrões definidos pelo agente.

---

## 2. Saídas geradas (arquivos)

| Arquivo | Descrição | Finalidade |
|---------|-----------|------------|
| `Jornada do Atendente - Fluxo.html` | **Diagrama de fluxo interativo** com os 3 caminhos (principal, fallback, feedback), ponto de decisão, destinos de escalonamento explícitos e laço de retorno do feedback. Inclui legenda clicável que isola cada caminho. | Entregável principal — apresentação ao time e ao cliente |
| `Jornada do Atendente - Fluxo-print.html` | Cópia otimizada para impressão (A4 retrato, cores forçadas, escala ajustada à página, sem interatividade), com auto-abertura da caixa de impressão. | Geração do **PDF** via "Salvar como PDF" do navegador |
| `Registro da Conversa.md` | Este documento — extração das entradas, prompts e lista de arquivos gerados. | Registro / rastreabilidade da sessão |

### Detalhes do entregável principal

**Título do diagrama:** Jornada do Atendente com Assistente de IA — Fluxo Principal, Fallback e Feedback

**Legenda dos caminhos:**
- 🟢 **Principal** — resposta confiável: atendente usa a resposta e encerra com rastreabilidade (meta < 2 min).
- 🟠 **Fallback** — sem resposta confiável/rastreável: motivos (baixa confiança, conflito, incompleta, validação humana, discordância) → encaminhamento por tema (Supervisor, Operações, Comercial, Compliance, Gestão de Riscos) → caso registrado.
- 🔵 **Feedback** — melhoria contínua: avaliação → triagem → revisão humana → atualização documental → reindexação → fechamento do ciclo, com retorno à base e ao assistente (periodicidade 24 h / semanal / mensal).

---

*Extraído da sessão de chat do projeto "Jornada do atendente". Fonte de verdade do conteúdo: `uploads/jornada-atendente-assistente-ia.md` (Tarefa 1).*
