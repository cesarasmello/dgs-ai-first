# Guardrails do Assistente de IA da NovaTech

**Exercício:** 2.2 — Formalização de Guardrails
**Projeto:** Assistente de Atendimento NovaTech (pipeline RAG)
**Fontes de verdade:** POL-001 v3.1, PROC-042 v1.0, PROC-042-v2 v2.0, SLA-2024 v2024.1, FAQ-Atendimento (informal), guardrails informais nº 1–4 e incidentes simulados nº 1–3
**Versão:** 1.0
**Data:** 10/06/2026

---

## 1. Convenções deste documento

- **Enforcement via prompt (probabilístico):** regra expressa nas instruções de sistema do assistente. Depende da aderência do modelo; não há garantia formal de cumprimento em 100% das respostas.
- **Enforcement via código (determinístico):** regra implementada como lógica do pipeline (filtros de recuperação, validadores pós-geração, roteadores, listas fechadas). O cumprimento independe do comportamento do modelo.
- **Referência aos incidentes simulados:**
  - **INC-1:** o assistente informou prazo de devolução de 7 dias para carga perigosa, quando cargas perigosas (classes 1–6 ANTT) não são elegíveis à devolução padrão (POL-001, seção 3.2).
  - **INC-2:** o assistente citou PROC-042, seção 2, mas utilizou os multiplicadores da versão 1 (desatualizada) em vez da v2 vigente.
  - **INC-3:** o assistente declarou não haver informação sobre SLA Gold, embora o documento SLA-2024 contivesse a resposta.

---

## 2. Tabela de guardrails

| Categoria | Guardrail | Tipo de enforcement | Justificativa | Incidente prevenido |
|---|---|---|---|---|
| DEVE | **G-01.** Toda afirmação normativa (prazo, valor, multiplicador, elegibilidade, SLA) deve conter citação estruturada no formato `[DOC-ID vX.Y, seção N]`, correspondente a um chunk efetivamente recuperado na consulta corrente. | Código (determinístico) | Um validador pós-geração pode verificar mecanicamente se a citação está presente e se o DOC-ID/versão citados constam nos metadados dos chunks recuperados. | INC-2, INC-3 |
| DEVE | **G-02.** Antes de informar qualquer prazo de devolução, o assistente deve verificar a categoria da carga contra as exceções da POL-001, seção 3.2 (cargas perigosas classes 1–6 ANTT, refrigeradas com quebra de cadeia de frio, lacre violado); para essas categorias, a resposta deve direcionar à Gestão de Riscos (ramal 4500), nunca ao prazo de 7 dias úteis. | Prompt (probabilístico) | A identificação da categoria da carga exige interpretação semântica da pergunta do cliente, que não é verificável por regra fixa antes da geração. | INC-1 |
| DEVE | **G-03.** Para chamados abertos a partir de 01/12/2023, o pipeline deve recuperar exclusivamente a PROC-042-v2; a PROC-042 v1 só pode ser servida quando o chamado for anterior a 01/12/2023 e ainda estiver em processamento (disposições transitórias, PROC-042-v2, seção 5). | Código (determinístico) | A seleção de versão é uma regra binária baseada em metadado de data, implementável como filtro de versionamento no retriever, sem depender do julgamento do modelo. | INC-2 |
| DEVE | **G-04.** Consultas sobre SLA devem ser roteadas para o índice do SLA-2024 com expansão de sinônimos canônicos do glossário (ex.: "Gold", "tier Gold", "cliente Gold", "SLA Gold"), garantindo a recuperação da tabela da seção 2 por tier e métrica. | Código (determinístico) | A expansão de consulta e o roteamento por intenção de SLA são etapas do pipeline executadas antes da geração, com comportamento reprodutível. | INC-3 |
| DEVE | **G-05.** As respostas devem ser redigidas em português formal, em tom institucional, sem reproduzir o registro coloquial do FAQ-Atendimento (ex.: "na prática, a gente orienta…"). | Prompt (probabilístico) | Registro linguístico é um atributo estilístico da geração, controlável apenas por instrução, não por validação binária. | INC-1 (a reprodução do tom informal do FAQ Item 3 favorece respostas que relativizam a inelegibilidade de cargas perigosas) |
| NÃO DEVE | **G-06.** O assistente não deve emitir nenhum número (prazo, multiplicador, fator de peso, percentual, valor em R$) que não esteja literalmente presente nos chunks recuperados na consulta corrente. | Código (determinístico) | Um verificador de aterramento numérico pode extrair os números da resposta e confrontá-los, por comparação exata, com os números dos chunks, bloqueando a resposta em caso de divergência. | INC-1, INC-2 |
| NÃO DEVE | **G-07.** O assistente não deve responder sobre devolução de carga perigosa: detectada a coocorrência de intenção "devolução" com termos de carga perigosa (classes 1–6, ANTT, inflamável, explosivo, tóxico etc.), o pipeline deve substituir a geração livre por template fixo de encaminhamento à Gestão de Riscos (ramal 4500). | Código (determinístico) | Por se tratar de risco de segurança e compliance, a combinação intenção + categoria deve acionar um desvio de fluxo obrigatório, e o template fixo elimina a variabilidade da geração. | INC-1 |
| NÃO DEVE | **G-08.** O assistente não deve utilizar o FAQ-Atendimento como fonte para afirmações normativas (prazos, valores, elegibilidade, SLA); o FAQ só pode ser citado como orientação operacional complementar, com a ressalva explícita de que não é documento validado. | Prompt (probabilístico) | Distinguir se uma pergunta exige resposta normativa ou apenas orientação prática requer julgamento semântico do modelo sobre a intenção do cliente. | INC-1 (o FAQ Item 3 sugere exceções não previstas na POL-001) |
| NÃO DEVE | **G-09.** O assistente não deve reconhecer ou mencionar tiers de SLA fora da lista fechada {Gold, Silver, Standard}; qualquer ocorrência de tier inexistente (ex.: "Platinum") na resposta deve ser bloqueada e substituída pela orientação do SLA-2024, seção 1. | Código (determinístico) | A validação contra uma lista fechada de valores permitidos é uma checagem exata, executável sobre o texto gerado sem ambiguidade. | INC-3 (erros de domínio sobre tiers comprometem a confiabilidade das respostas de SLA) |
| QUANDO EM DÚVIDA | **G-10.** Se a primeira recuperação não retornar chunks relevantes para uma consulta de SLA, frete ou devolução, o pipeline deve reexecutar a busca com reformulação (sinônimos do glossário, decomposição da pergunta) antes de permitir qualquer declaração de "informação não encontrada". | Código (determinístico) | O laço de nova tentativa com reformulação é uma política de pipeline com gatilho objetivo (score de recuperação abaixo do limiar), não uma decisão do modelo. | INC-3 |
| QUANDO EM DÚVIDA | **G-11.** Quando chunks de versões distintas do mesmo procedimento (PROC-042 v1 e v2) chegarem ao contexto, o assistente deve aplicar as disposições transitórias (v2, seção 5), responder com base na versão aplicável ao caso e declarar explicitamente qual versão utilizou e por quê. | Prompt (probabilístico) | A aplicação da regra transitória exige interpretar a situação do chamado descrita pelo cliente, o que ocorre dentro da geração e não pode ser totalmente resolvido por filtro prévio. | INC-2 |
| QUANDO EM DÚVIDA | **G-12.** Se valores homólogos divergirem entre chunks recuperados (ex.: multiplicador da região Sul = 1.2 na v1 e 1.3 na v2), o pipeline deve sinalizar conflito documental; nesse caso, o assistente responde apenas com a versão vigente ou, na impossibilidade de determinação, encaminha a atendimento humano. | Código (determinístico) | A comparação de valores extraídos de campos homólogos (mesma região, mesma faixa de peso) entre documentos é uma checagem exata sobre dados estruturados. | INC-2 |
| QUANDO EM DÚVIDA | **G-13.** Quando o tema não estiver coberto pela documentação normativa (ex.: frete padrão abaixo de 500kg, seguro de carga, carga danificada em trânsito), o assistente deve declarar explicitamente que a informação não consta da base oficial e encaminhar ao canal competente (Comercial; sinistros@novatech.com.br; ramal 4500), sem extrapolar a partir do FAQ. | Prompt (probabilístico) | Reconhecer a ausência de cobertura e escolher o canal de encaminhamento adequado exige raciocínio sobre o conteúdo recuperado, não sendo redutível a uma regra fixa. | INC-3 (a declaração de ausência passa a ser um ato deliberado e auditável, e não um falso negativo de recuperação); INC-1 |
| QUANDO EM DÚVIDA | **G-14.** Se a pergunta sobre devolução não informar a categoria da carga (classe ANTT, refrigerada, lacre), o assistente deve solicitar essa informação antes de citar qualquer prazo ou elegibilidade. | Prompt (probabilístico) | Identificar que falta um dado essencial e formular a pergunta de esclarecimento é um comportamento conversacional, controlável apenas por instrução. | INC-1 |

---

## 3. Síntese dos principais riscos mitigados

1. **Risco de segurança e compliance (INC-1):** informar prazo de devolução para cargas perigosas viola a POL-001, seção 3.2, e pode induzir o cliente a manusear e transportar carga das classes 1–6 ANTT fora do fluxo da Gestão de Riscos. Mitigado por G-02, G-05, G-06, G-07, G-08 e G-14, com o desvio determinístico de fluxo (G-07) como barreira final.
2. **Risco financeiro e contratual por versionamento documental (INC-2):** a coexistência da PROC-042 v1 e v2 sem hierarquia formal no repositório permite cotações com multiplicadores, fatores de peso e prazos defasados, gerando cobrança incorreta e passivo comercial. Mitigado por G-01, G-03, G-06, G-11 e G-12.
3. **Risco de falso negativo de recuperação (INC-3):** declarar "não encontrei" quando a resposta existe (tabela SLA-2024) degrada a confiança no assistente e pode levar o cliente Gold a não exigir compromissos contratuais a que tem direito. Mitigado por G-04, G-10 e G-13, que separam a ausência real de informação da falha de recuperação.
4. **Risco de contaminação por fonte não validada:** o FAQ-Atendimento, informal e sem responsável, contém orientações que relativizam regras normativas e dados sem respaldo documental (seguro de carga, frete expresso para carga perigosa). Mitigado por G-05, G-08 e G-13.
5. **Risco de alucinação factual de domínio:** invenção de números, tiers ou condições inexistentes (ex.: tier "Platinum"). Mitigado por G-06 e G-09, ambos verificáveis de forma exata sobre o texto gerado.

---

## 4. Guardrails de implementação obrigatória em código

Os guardrails abaixo não podem depender exclusivamente de instrução de prompt, pois protegem contra os riscos de maior severidade e admitem verificação exata:

1. **G-07 — Bloqueio de geração livre para devolução de carga perigosa**, com template fixo de encaminhamento ao ramal 4500 (risco de segurança; tolerância zero a falha probabilística).
2. **G-06 — Verificador de aterramento numérico**, que rejeita respostas com números ausentes dos chunks recuperados.
3. **G-03 — Filtro de versionamento no retriever**, servindo a PROC-042-v2 como padrão e a v1 apenas para chamados transitórios anteriores a 01/12/2023.
4. **G-12 — Detector de conflito entre valores homólogos** de versões distintas, com escalonamento a atendimento humano quando a versão vigente não puder ser determinada.
5. **G-01 — Validador de citação estruturada**, exigindo correspondência entre as citações da resposta e os metadados dos chunks recuperados.
6. **G-10 — Política de nova recuperação com reformulação** antes de qualquer declaração de "informação não encontrada".
7. **G-09 — Validação por lista fechada de tiers de SLA** {Gold, Silver, Standard}.

---

*Documento elaborado exclusivamente a partir do Anexo A — Documentação Simulada da NovaTech, dos guardrails informais nº 1–4 e dos incidentes simulados nº 1–3, sem injeção de conhecimento externo.*
