# Exportação de Conversa — Seção "Product Rules & Guardrails" do AGENTS.md (NovaTech Assistant)

**Data da conversa:** 11/06/2026
**Participante:** César Augusto (Product Specialist — projeto NovaTech Assistant)
**Contexto:** Cenário 2 — fase de estruturação do repositório `db1/novatech-assistant`. Escrita da seção "Product Rules & Guardrails" do AGENTS.md, de responsabilidade do Product Specialist (Ex. 2.3).
**Ferramenta usada:** Claude shat

---

## 1. Documentos fornecidos pelo participante (uploads)

| # | Arquivo | Descrição | Uso na conversa |
|---|---|---|---|
| 1 | `AGENTS.md` | Template do AGENTS.md do projeto, com seções vazias (TODO) atribuídas por papel. Define o título exato e a posição da seção "Product Rules & Guardrails (Product Specialist)". | Referência estrutural para o título e formato da seção. |
| 2 | `anexo-a-documentacao-simulada-novatech.md` | Documentação simulada da NovaTech (fonte de verdade): POL-001 v3.1 (Política de Devolução), PROC-042 v1 e PROC-042-v2 (Frete Especial), SLA-2024 (Tabela de SLA por tier), FAQ-Atendimento (informal), além das contradições e gaps mapeados. | Fonte primária das regras de comportamento, valores canônicos (multiplicadores, SLAs, prazos) e regras de priorização v1/v2. |
| 3 | `anexo-c-estrutura-repositorio.md` | Estrutura do repositório `db1/novatech-assistant`: árvore de diretórios, convenções de specs/skills/prompts/ADRs/testes, estado atual e exemplo de configuração MCP. | Fonte das referências de repositório (specs, validators, response-builder, prompt-builder, fixtures etc.). |
| 4 | `glossario-linguagem-ubiqua-v2.md` | Glossário de linguagem ubíqua v2 com 31 termos canônicos organizados em 6 bounded contexts (BC1–BC6), incluindo regras de desambiguação para LLM, lacunas (L1–L7) e pendências (DP-04, DP-08). | Fonte do "Ubiquitous Language Glossary" e das regras de desambiguação incorporadas aos guardrails. |

> **Nota de auditoria:** O documento `guardrails-assistente-novatech.md`, citado como fonte 1 no prompt, **não foi enviado** entre os uploads. As regras de guardrail foram derivadas dos requisitos explicitados no próprio prompt e da documentação oficial dos Anexos A e C e do glossário v2. Pendência registrada: reconciliar a seção quando o documento de guardrails for fornecido (ex.: limiar numérico do confidence score, formato exato do JSON).

---

## 2. Turno 1 — Prompt do participante (verbatim)

> Você está atuando como Product Specialist no projeto NovaTech Assistant.
>
> Sua tarefa é escrever APENAS a seção "Product Rules & Guardrails" do AGENTS.md do projeto, pronta para colar no repositório.
>
> Contexto e fontes de verdade:
> 1. Guardrails (guardrails-assistente-novatech.md)
> 2. Anexo A — documentação simulada da NovaTech (anexo-a-documentacao-simulada-novatech.md)
> 3. Anexo C — estrutura do repositório NovaTech Assistant (anexo-c-estrutura-repositorio)
> 4. Template atual do AGENTS.md
> 5. glossario-linguagem-ubiqua-v2.md
>
> Objetivo:
> Produzir uma seção de AGENTS.md que seja:
> - machine-readable
> - prescritiva
> - útil para agentes de IA como Copilot e Claude Code
> - aderente ao domínio NovaTech
> - concreta o suficiente para influenciar geração de código e respostas do assistente
>
> Requisitos obrigatórios:
> 1. A saída deve conter a seção com o título exato:
> ## Product Rules & Guardrails (Product Specialist)
>
> 2. A seção deve estar organizada exatamente nestes blocos, nesta ordem:
> ### Assistant Behavior Rules
> ### Ubiquitous Language Glossary
> ### Code Generation Constraints
> ### Repository Spec References
>
> 3. Em "Assistant Behavior Rules":
> - Organize as regras em 3 subseções:
> #### DEVE
> #### NÃO DEVE
> #### QUANDO EM DÚVIDA
> - Escreva regras curtas, operacionais e prescritivas.
> - Use verbos normativos, como "DEVE", "NÃO DEVE", "DEVE priorizar", "DEVE bloquear", "DEVE escalar".
> - Não escreva texto narrativo longo.
> - As regras devem derivar dos guardrails do documento guardrails-assistente-novatech.md e da documentação oficial da NovaTech.
> - Inclua regras sobre:
>   - citação de fonte com identificador de documento e seção
>   - obrigatoriedade do campo source_document no JSON
>   - idioma e tom formal em português
>   - proibição de inventar números, SLAs, multiplicadores ou prazos
>   - tratamento de carga perigosa
>   - priorização da versão mais recente de documentos quando houver versões conflitantes
>   - comportamento de baixa confiança e escalonamento ao supervisor
>
> 4. Em "Ubiquitous Language Glossary":
> - Monte uma tabela markdown com as colunas:
> | Termo | Definição Operacional | Ambiguidade/Risco | Fonte |
> - Inclua termos que um LLM pode confundir sem contexto.
> - Utilize o documento glossario-linguagem-ubiqua-v2.md para criação do glossário
> - Inclua pelo menos estes termos:
>   - cliente Gold
>   - cliente Silver
>   - cliente Standard
>   - carga perigosa
>   - devolução padrão
>   - SLA de resolução
>   - SLA de primeira resposta
>   - multiplicador regional
>   - fator de peso
>   - frete especial
>   - incidente crítico
>   - source_document
> - As definições devem ser curtas e operacionais, não genéricas.
> - A coluna "Fonte" deve citar o documento e a seção correspondente.
>
> 5. Em "Code Generation Constraints":
> - Liste restrições concretas que devem impactar o código gerado no projeto.
> - Foque em instruções que um agente de código realmente consiga seguir.
> - Inclua obrigatoriamente regras como:
>   - toda resposta estruturada deve incluir source_document
>   - validadores devem bloquear tiers fora de Gold, Silver, Standard
>   - respostas não podem emitir números ausentes dos chunks recuperados
>   - lógica de versionamento deve priorizar PROC-042-v2 e tratar exceção transitória explicitamente
>   - fluxos de devolução de carga perigosa não podem seguir processo padrão
>   - respostas com baixa confiança devem sinalizar isso explicitamente
> - Sempre que possível, associe a restrição a pontos prováveis da arquitetura do repositório, como prompt builder, response validator, response builder, validators e specs.
>
> 6. Em "Repository Spec References":
> - Liste arquivos e diretórios relevantes do repositório que um agente deve consultar ou atualizar para cumprir essas regras.
> - Use caminhos de repositório em markdown.
> - Priorize referências como:
>   Documento de Spec (requirements-v2.md)
> - Para cada referência, explique em 1 linha por que ela existe e como se relaciona com os guardrails.
>
> 7. Regras de qualidade da saída:
> - Saída em Markdown puro.
> - Não incluir introdução, conclusão, explicação do raciocínio ou observações fora da seção.
> - Não mencionar que você "não tem acesso" ao repositório.
> - Não inventar documentos além dos fornecidos.
> - Se houver conflito documental, explicite a prioridade adotada dentro das regras.
> - Preserve linguagem formal e objetiva.
> - Favoreça listas e tabelas em vez de parágrafos longos.
>
> 8. Critérios de aceitação:
> - A seção final deve parecer pronta para commit.
> - Um agente de IA deve conseguir seguir as instruções sem interpretação excessiva.
> - O conteúdo deve refletir o domínio NovaTech e não um template genérico.
>
> Antes de responder, faça uma checagem silenciosa:
> - a saída está prescritiva?
> - há glossário útil?
> - há restrições concretas para geração de código?
> - há referências de repositório coerentes com o Anexo C?
> - a seção está pronta para ser colada no AGENTS.md?
> Responda somente com a seção final em Markdown.

---

## 3. Turno 1 — Processamento e saída do assistente

**Etapas executadas (auditoria de fontes consultadas):**

1. Listagem dos arquivos enviados (`/mnt/user-data/uploads/`) — constatada a ausência do `guardrails-assistente-novatech.md`.
2. Leitura integral do `anexo-a-documentacao-simulada-novatech.md` (incluindo as seções do PROC-042-v2 com multiplicadores atualizados: Sul 1.3, Sudeste 1.1, Centro-Oeste 1.4, Nordeste 1.5, Norte 1.8; fator de peso 1.0/1.15/1.4; prazo +3 dias úteis).
3. Leitura do `glossario-linguagem-ubiqua-v2.md` (estrutura completa dos 31 termos, BC1–BC6, lacunas L1–L7, pendências DP-04 e DP-08).
4. Uso do `AGENTS.md` (template) e do `anexo-c-estrutura-repositorio.md` (já presentes no contexto) para o título exato da seção e os caminhos de repositório.
5. Geração da seção com os 4 blocos na ordem exigida, derivação estrita das fontes, e prioridade documental explicitada (normativo mais recente > normativo anterior > FAQ informal com ressalva).

**Arquivo gerado:**

| Arquivo | Conteúdo |
|---|---|
| `product-rules-guardrails-agents-md.md` | Seção completa "## Product Rules & Guardrails (Product Specialist)" com: Assistant Behavior Rules (13 regras DEVE, 10 regras NÃO DEVE, 4 regras QUANDO EM DÚVIDA), Ubiquitous Language Glossary (tabela com os 12 termos obrigatórios, definições operacionais, riscos e fontes citadas por documento e seção), Code Generation Constraints (10 restrições associadas a `validator.ts`, `response-builder.ts`, `response-validator.ts`, `prompt-builder.ts`, `indexer.ts`, `types.ts`, fixtures e prompts versionados) e Repository Spec References (14 referências com justificativa de 1 linha cada). |

**Observação registrada na resposta:** transparência sobre a ausência do `guardrails-assistente-novatech.md` e sugestão de reconciliação posterior da seção quando o documento for enviado.

---

## 4. Turno 2 — Prompt do participante (verbatim)

> Extraia a conversa desse chat em um arquivo markdown, com as entrastadas, pronpts e documentos que forneci e como saída, nome de cada arquivo gerado

---

## 5. Turno 2 — Saída do assistente

**Arquivo gerado:**

| Arquivo | Conteúdo |
|---|---|
| `exportacao-conversa-product-rules-guardrails.md` | Este documento: exportação auditável da conversa com documentos fornecidos, prompts verbatim, etapas de processamento, saídas e nomes dos arquivos gerados. |

---

## 6. Resumo dos arquivos gerados na conversa

| # | Arquivo | Turno | Finalidade |
|---|---|---|---|
| 1 | `product-rules-guardrails-agents-md.md` | 1 | Seção "Product Rules & Guardrails (Product Specialist)" pronta para colar no `AGENTS.md` do repositório `db1/novatech-assistant`. |
| 2 | `exportacao-conversa-product-rules-guardrails.md` | 2 | Exportação auditável desta conversa. |

---

## 7. Pendências registradas

1. **Documento de guardrails ausente:** enviar `guardrails-assistente-novatech.md` para reconciliação da seção (possíveis ajustes: limiar numérico do confidence score, formato exato do JSON de resposta, regras adicionais não cobertas pelos Anexos).
2. **Commit:** colar a seção gerada no `AGENTS.md` sob o título "## Product Rules & Guardrails (Product Specialist)", substituindo o TODO correspondente (Ex. 2.3).
