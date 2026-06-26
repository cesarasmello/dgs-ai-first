# Histórico de Conversa — Projeto NovaTech (DDD + RAG)

**Exportado em:** 2025-06-09
**Participantes:** César Augusto (Product Specialist / DB1 Global Software) + Assistente Claude
**Projeto:** NovaTech — Assistente de IA para atendimento de logística
**Fase:** Estruturação de domínio (DDD) + Especificação do Query Endpoint
**Ferramenta usada:** Claude shat

---

## Índice de arquivos gerados

| # | Nome do arquivo | Descrição | Turno |
|---|---|---|---|
| 1 | `novatech-bounded-contexts.md` | Mapa de bounded contexts v1 (6 contextos) | Turno 1 |
| 2 | `novatech-glossario-linguagem-ubiqua.md` | Glossário de linguagem ubíqua v1 (31 termos) | Turno 2 |
| 3 | `requirements.md` | Especificação SDD do Query Endpoint v1 | Turno 3 |
| 4 | `revisao-critica-ex2.1-novatech.md` | Revisão crítica do Tech Lead (19 achados) | Turno 4 |
| 5 | `bounded-contexts-v2.md` | Mapa de bounded contexts v2 (corrigido) | Turno 5 |
| 6 | `glossario-linguagem-ubiqua-v2.md` | Glossário de linguagem ubíqua v2 (corrigido) | Turno 5 |
| 7 | `requirements-v2.md` | Especificação SDD do Query Endpoint v2 (corrigido) | Turno 5 |
| 8 | `historico-iteracao-v1-v2.md` | Tabela de correções v1→v2 + pendências remanescentes | Turno 5 |

---

## Turno 1 — Mapa de Bounded Contexts

### Entradas fornecidas

**Arquivo:** `anexo-a-documentacao-simulada-novatech.md`
Corpus de 5 documentos simulados do domínio NovaTech: FAQ de atendimento, política de devolução (POL-001), procedimento de frete especial (PROC-042 v1 e v2) e tabela de SLA (SLA-2024).

**Prompt:**

> Você é Product Specialist no projeto NovaTech, em fase de estruturação de domínio (DDD).
> Objetivo: Analisar o documento `anexo-a-documentacao-simulada-novatech.md` e propor um mapa de bounded contexts do assistente NovaTech, com fronteiras claras e justificadas por evidências textuais.
>
> Entrada obrigatória: `anexo-a-documentacao-simulada-novatech.md`
>
> Regras de decisão de fronteira:
> 1. Se um termo muda de significado conforme a área, separar contextos.
> 2. Se a regra possui dono distinto (ex.: Compliance, Operação, Financeiro), separar contextos.
> 3. Se misturar políticas aumenta risco de resposta incorreta, separar contextos.
> 4. Se dois grupos compartilham o mesmo vocabulário e as mesmas regras sem conflito, manter no mesmo contexto.
> 5. Em caso de dúvida, priorizar segurança de decisão e rastreabilidade da regra.
>
> Processo de execução:
> 1. Extrair termos de negócio recorrentes e seus significados.
> 2. Agrupar termos por regras, responsáveis e objetivos operacionais.
> 3. Propor bounded contexts candidatos.
> 4. Validar cada fronteira com os critérios de decisão.
> 5. Consolidar o mapa final sem sobreposição ambígua.
>
> Para cada bounded context, informar obrigatoriamente:
> - Nome do contexto
> - Missão do contexto (1 a 2 frases)
> - O que está dentro: decisões, regras, termos e eventos de negócio
> - O que está fora: decisões e regras que pertencem a outros contextos
> - Relações: Consome dados de quais contextos / Publica regras/eventos para quais contextos
> - Evidências no documento: citar trechos curtos que sustentam a definição
> - Riscos de fronteira: possíveis ambiguidades e como resolver
>
> Restrições de qualidade:
> - Não inventar regras não presentes no documento.
> - Se faltar informação, registrar como lacuna e indicar hipótese explícita.
> - Evitar contextos genéricos demais (ex.: "Operações" sem especialização).
> - Evitar duplicidade de responsabilidade entre contextos.
>
> Formato de saída obrigatório (Markdown):
> 1. Resumo executivo (máx. 8 linhas)
> 2. Tabela de bounded contexts com colunas: Contexto | Missão | Dentro | Fora | Consome de | Publica para | Evidências | Risco/Lacuna
> 3. Matriz de relações entre contextos (origem → destino, tipo de relação)
> 4. Lista final de lacunas e hipóteses
> 5. Checklist de validação: Sem sobreposição crítica / Sem regra órfã / Sem termo com significado conflitante dentro do mesmo contexto

### Saída gerada

**Arquivo:** `novatech-bounded-contexts.md`

Mapa com 6 bounded contexts identificados:
- BC1 — Devolução de Mercadorias
- BC2 — Precificação de Frete Especial
- BC3 — Gestão de SLA e Atendimento
- BC4 — Conformidade e Riscos de Carga
- BC5 — Sinistros e Carga Danificada
- BC6 — Seguro de Carga

6 lacunas documentadas (L1–L6), matriz de 9 relações entre contextos e checklist de validação completo.

---

## Turno 2 — Glossário de Linguagem Ubíqua

### Entradas fornecidas

**Arquivos:**
- `novatech-bounded-contexts.md` (gerado no Turno 1)
- `anexo-a-documentacao-simulada-novatech.md`

**Prompt:**

> Você é Product Specialist no projeto NovaTech e deve produzir um glossário de linguagem ubíqua com definições fechadas, consistentes e sem ambiguidade, priorizando termos críticos para respostas do assistente.
>
> Entradas obrigatórias:
> 1. Mapa de bounded contexts já definido no item 1, contendo para cada contexto: dentro, fora e relações.
> 2. Documento de referência do domínio NovaTech (Anexo A).
>
> Princípio de precedência:
> 1. O mapa de contexts define fronteiras semânticas e escopo de uso dos termos.
> 2. O documento de domínio fornece evidências textuais e definições operacionais.
> 3. Em conflito entre termos de contextos distintos, preservar a distinção por contexto em vez de unificar indevidamente.
>
> Regras de extração:
> 1. Extraia apenas termos com relevância operacional, decisória, contratual ou de política de negócio.
> 2. Não invente termos, regras ou definições ausentes dos insumos.
> 3. Se houver variação de nome para o mesmo conceito no mesmo contexto, escolha termo canônico e registre sinônimos.
> 4. Se um termo tiver múltiplos significados, separar por contexto e explicitar a diferença.
> 5. Se a definição for insuficiente, marcar como Lacuna e propor hipótese curta, claramente rotulada como Hipótese.
> 6. Todo termo deve ter Contexto Dono (owner) e, quando aplicável, Contextos Relacionados.
> 7. Sempre que houver risco de confusão para LLM, incluir Regra de Desambiguação explícita.
>
> Critérios de qualidade da definição:
> 1. Específica e testável no contexto de negócio.
> 2. Sem circularidade.
> 3. Sem linguagem vaga sem condição objetiva.
> 4. Diferenciação clara de conceitos próximos.
> 5. Coerência com limites dentro/fora do mapa de contexts.
>
> Formato de saída obrigatório (Markdown):
> - Seção 1: Resumo de cobertura por contexto — tabela com colunas: Contexto | Quantidade de termos | Termos críticos | Lacunas
> - Seção 2: Glossário — tabela por termo com 11 campos: Termo canônico, Definição oficial, Contexto Dono, Contextos Relacionados, Fora de escopo, Regra de desambiguação para LLM, Exemplo válido, Confusão comum a evitar, Sinônimos/variantes, Evidência textual, Status
> - Seção 3: Lacunas e hipóteses
> - Seção 4: Checklist de validação

### Saída gerada

**Arquivo:** `novatech-glossario-linguagem-ubiqua.md`

31 termos canônicos distribuídos pelos 6 bounded contexts. Cada entrada com os 11 campos obrigatórios. Decisões de fronteira semântica documentadas para "prazo" (3 instâncias distintas), "reembolso" (3 instâncias distintas) e "carga perigosa" (dono único em BC4 com delegação ativa para todos os outros contextos).

---

## Turno 3 — Especificação SDD do Query Endpoint (requirements.md v1)

### Entradas fornecidas

**Arquivos:**
- `glossario-linguagem-ubiqua-v1.md`
- `bounded-contexts-v1.md`

**ADRs referenciados (não fornecidos como arquivo, declarados no prompt):**
- ADR-0001: modelo LLM Azure OpenAI GPT-4o
- ADR-0002: estratégia de contexto e budget de tokens
- ADR-0003: tratamento de documentos contraditórios com metadado de vigência
- ADR-0004: pipeline de RAG e lições do protótipo

**Dados de discovery declarados no prompt:**
- 4 categorias principais de perguntas: prazos de entrega, regras de frete, política de devolução e SLAs
- 15% das perguntas cruzam duas categorias
- Atendente precisa da resposta em menos de 30 segundos

**Requisito legado declarado no prompt:**
- Assistente responde sobre SLA, frete e devoluções
- Deve citar fonte
- Não pode inventar informação
- Em contradição, expor versões e priorizar vigente
- Atualização documental em até 24h

**Prompt:**

> Você é Product Specialist no projeto NovaTech. Sua tarefa é gerar uma especificação de requisitos do query endpoint no formato SDD.
>
> Objetivo: Gerar um único documento `requirements.md`, orientado a resultado de negócio, com fronteiras de escopo derivadas do recorte de domínio e critérios verificáveis por QA.
>
> Instruções de elaboração:
> 1. Produza a spec do módulo query endpoint, não do sistema inteiro.
> 2. Outcomes devem ser orientados a resultado para atendente e cliente final, não a implementação técnica.
> 3. Scope boundaries devem ser explicitamente derivados dos bounded contexts de entrada.
> 4. Constraints devem cobrir negócio, compliance, tempo de resposta, fonte, vigência documental, idioma e não alucinação.
> 5. Prior decisions deve referenciar explicitamente ADR-0001, ADR-0002, ADR-0003 e ADR-0004, com impacto no módulo.
> 6. Verification criteria deve ser testável, objetivo e mensurável pelo QA, com cenários positivos e de exceção.
> 7. Incluir comportamento para perguntas cross-context e para ausência de evidência suficiente.
> 8. Não inventar regras fora das entradas; quando faltar dado, registrar em Open Questions.
> 9. Linguagem em português formal, clara e prescritiva.
>
> Estrutura obrigatória do documento:
> 1. Título / 2. Contexto do módulo / 3. Outcomes / 4. Scope boundaries / 5. Constraints / 6. Prior decisions / 7. Verification criteria / 8. Non-goals / 9. Open questions / 10. Assumptions / 11. Glossary dependencies
>
> Formato de saída: exclusivamente o conteúdo final de um arquivo `requirements.md` em Markdown, sem prefácio, notas externas ou placeholders genéricos sem justificativa.

### Saída gerada

**Arquivo:** `requirements.md` (v1)

Documento com 7 outcomes (OUT-01 a OUT-07), 6 scope boundaries (SB-01 a SB-06), 12 constraints (CN-01 a CN-12), 4 prior decisions (ADR-0001 a ADR-0004), 13 verification criteria (CV-01 a CV-13), 7 non-goals, 7 open questions, 7 assumptions e tabela de 31 glossary dependencies.

---

## Turno 4 — Revisão Crítica do Tech Lead

### Entradas fornecidas

**Arquivos:**
- `glossario-linguagem-ubiqua-v1.md`
- `requirements-v1.md`
- `mockup-formato-PDF-v1.pdf` — mockup da interface Teams com 4 estados anotados: resposta padrão (confiança alta), conflito de versões, confiança baixa com escalação e sem evidência suficiente
- `bounded-contexts-v1.md`

**Prompt:**

> Você é Tech Lead no projeto NovaTech e deve fazer uma revisão crítica dos artefatos do Exercício 2.1.
>
> Objetivo: Identificar ambiguidades, inconsistências e riscos de implementação/teste nos artefatos, sem reescrever o conteúdo nesta etapa.
>
> Entradas:
> 1. Mapa de bounded contexts + linguagem ubíqua.
> 2. requirements.md do query endpoint (formato SDD).
> 3. Mockup da interface no Teams.
>
> Escopo da revisão:
> 1. Coerência entre bounded contexts e scope boundaries.
> 2. Termos ambíguos ou mal definidos no glossário.
> 3. Outcomes orientados a resultado (não feature técnica).
> 4. Constraints completas (fonte, vigência, não alucinação, tempo de resposta, idioma).
> 5. Verification criteria testáveis e mensuráveis pelo QA.
> 6. Alinhamento entre requirements e mockup (sem conflito funcional).
> 7. Cobertura de cenários cross-context e baixa confiança.
> 8. Conformidade com ADR-0001, ADR-0002, ADR-0003 e ADR-0004.
>
> Regras:
> 1. Não alterar os artefatos.
> 2. Não inventar requisitos fora das entradas.
> 3. Classificar cada achado por severidade: Crítico, Alto, Médio, Baixo.
> 4. Para cada achado, informar evidência objetiva e impacto.
> 5. Sugerir correção objetiva e verificável.
>
> Formato de saída (Markdown):
> 1. Resumo executivo com quantidade de achados por severidade.
> 2. Tabela de achados com colunas: ID | Severidade | Artefato | Trecho/Referência | Problema identificado | Impacto | Correção recomendada | Critério de aceite da correção
> 3. Lista de ambiguidades prioritárias (top 5).
> 4. Lista de decisões pendentes para Product Specialist/QA.
> 5. Checklist final: pronto para ajuste? (Sim/Não + justificativa).

### Saída gerada

**Arquivo:** `revisao-critica-ex2.1-novatech.md`

19 achados classificados: 3 críticos, 7 altos, 6 médios, 3 baixos. 5 ambiguidades prioritárias (AMB-01 a AMB-05). 8 decisões pendentes (DP-01 a DP-08). Resultado do checklist: ⚠️ Não — com condições (3 bloqueadores identificados).

**Achados críticos:**
- A-01: CN-11/CV-13 — responsabilidade de atualização documental atribuída ao módulo errado
- A-02: AS-03 sem CV de exceção para ambiguidade de intenção entre BCs
- A-03: CN-03 restrito ao conflito PROC-042, sem cobertura de futuros conflitos

---

## Turno 5 — Ajuste dos Artefatos (v1 → v2)

### Entradas fornecidas

**Arquivos:**
- `analise-de-ambiguidade.md` (relatório de revisão do Tech Lead, gerado no Turno 4)
- `glossario-linguagem-ubiqua-v1.md`
- `requirements-v1.md`
- `bounded-contexts-v1.md`

**ADRs referenciados:** ADR-0001, ADR-0002, ADR-0003, ADR-0004 (mesmos do Turno 3)

**Prompt:**

> Você é Product Specialist no projeto NovaTech. Sua tarefa é ajustar os artefatos textuais com base na revisão do Tech Lead, sem alterar o mockup nesta etapa.
>
> Objetivo: Aplicar correções no mapa de bounded contexts, glossário de linguagem ubíqua e requirements.md, eliminando ambiguidades e garantindo testabilidade.
>
> Entradas obrigatórias:
> 1. Relatório de revisão do Tech Lead (analise-de-ambiguidade).
> 2. Mapa de bounded contexts atual (bounded-contexts-v1).
> 3. Glossário atual (glossario-linguagem-ubiqua-v1).
> 4. requirements.md atual (requirements-v1).
> 5. ADR-0001, ADR-0002, ADR-0003 e ADR-0004.
>
> Escopo permitido:
> 1. Refinar fronteiras dentro, fora e relações dos contexts.
> 2. Ajustar definições ambíguas no glossário e regras de desambiguação.
> 3. Corrigir outcomes, scope boundaries, constraints e verification criteria no requirements.md.
> 4. Garantir coerência com ADRs e com os dados de discovery.
>
> Escopo proibido:
> 1. Não alterar mockup.
> 2. Não inventar requisitos fora das entradas.
> 3. Não remover rastreabilidade entre achado e correção.
>
> Regras de qualidade obrigatórias:
> 1. Outcomes orientados a resultado de usuário.
> 2. Scope boundaries derivados dos bounded contexts.
> 3. Verification criteria objetivos, mensuráveis e testáveis por QA.
> 4. Termos críticos com definição operacional e sem ambiguidade.
> 5. Conformidade explícita com as ADRs.
>
> Formato de saída:
> 1. Versão ajustada completa de: Mapa de bounded contexts + Glossário + requirements.md
> 2. Histórico de iteração em tabela com colunas: ID do achado | Artefato impactado | Antes | Depois | Justificativa | Critério de aceite
> 3. Lista final de pendências remanescentes, se houver.

### Saídas geradas

**Arquivo 1:** `bounded-contexts-v2.md`

Correções aplicadas: A-07 (protocolo de nome coloquial adicionado ao BC4), A-14 (independência da triagem BC1 vs. relógio de SLA BC3 explicitada), A-18 (coluna "Publica para" renomeada para "Encaminha para" em BC6), nova lacuna L7 criada.

**Arquivo 2:** `glossario-linguagem-ubiqua-v2.md`

Termos atualizados: Termo 4 (Triagem — A-14), Termo 23 (Relógio de SLA — A-14), Termo 24 (Carga perigosa — A-07 com protocolo de Caso 1 e Caso 2), Termo 25 (Cadeia de frio — A-05 com distinção sensor informado/não informado), Termo 29 (Registro de ocorrência — DP-04 adicionada). Nova lacuna L7 documentada.

**Arquivo 3:** `requirements-v2.md`

Principais mudanças: 8 outcomes (OUT-08 novo), 12 constraints revistas + 2 novas (CN-13, CN-14), 16 verification criteria (CV-14, CV-15, CV-16 novos), 9 non-goals (NG-08, NG-09 novos), OQ-01 fechada + OQ-08 e OQ-09 novas, AS-03 e AS-04 revisados. Todos os 19 achados rastreados.

**Arquivo 4:** `historico-iteracao-v1-v2.md`

Tabela de 19 correções com colunas: ID do achado | Severidade | Artefato impactado | Antes (v1) | Depois (v2) | Justificativa | Critério de aceite. Lista de 6 pendências remanescentes (DP-02, DP-04, DP-05, DP-06, DP-07, DP-08).

---

## Turno 6 — Extração do histórico de conversa

### Entrada fornecida

Solicitação verbal:

> Extraia a conversa desse chat em um arquivo markdown, com as entrastadas, pronpts e documentos que forneci e como saída, nome de cada arquivo gerado

### Saída gerada

**Arquivo:** `historico-conversa-novatech.md` *(este arquivo)*

---

## Resumo geral do projeto nesta sessão

| Fase | Artefato | Versão | Status |
|---|---|---|---|
| DDD — Bounded Contexts | `novatech-bounded-contexts.md` | v1 | Substituído por v2 |
| DDD — Bounded Contexts | `bounded-contexts-v2.md` | v2 | Vigente |
| DDD — Glossário | `novatech-glossario-linguagem-ubiqua.md` | v1 | Substituído por v2 |
| DDD — Glossário | `glossario-linguagem-ubiqua-v2.md` | v2 | Vigente |
| SDD — Requirements | `requirements.md` | v1 | Substituído por v2 |
| SDD — Requirements | `requirements-v2.md` | v2 | Vigente |
| QA — Revisão crítica | `revisao-critica-ex2.1-novatech.md` | — | Referência permanente |
| Rastreabilidade | `historico-iteracao-v1-v2.md` | — | Referência permanente |
| Exportação | `historico-conversa-novatech.md` | — | Este documento |

**Artefatos vigentes para próxima iteração:** `bounded-contexts-v2.md`, `glossario-linguagem-ubiqua-v2.md`, `requirements-v2.md`

**Pendências bloqueantes antes da homologação:** DP-02 (critérios numéricos de confiança), DP-04 (prazo 48h Jurídico), DP-05 (streaming vs. bloqueado), DP-06 (botão Comparar versões), DP-07 (budget de tokens), DP-08 (pausa triagem BC1 fora horário comercial)
