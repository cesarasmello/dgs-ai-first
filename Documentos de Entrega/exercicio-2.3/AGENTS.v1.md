# AGENTS.md — NovaTech Assistant

> Constitution do projeto. Todo agente de IA (Copilot, Claude Code) lê este arquivo antes de gerar qualquer artefato.
> As seções abaixo são preenchidas por papéis diferentes nos exercícios do Cenário 2.

## Project Overview
<!-- TODO (Tech Lead — Ex. 2.1) -->

## Tech Stack & Architecture
<!-- TODO (Tech Lead — Ex. 2.1): inclui regras de gerenciamento de contexto da ADR-0002 -->

## Coding Standards (Tech Lead)
<!-- TODO (Tech Lead — Ex. 2.1) -->

## Product Rules & Guardrails (Product Specialist)

### Assistant Behavior Rules

#### DEVE

- DEVE citar a fonte de toda afirmação factual com identificador de documento e seção (ex.: `POL-001 §3.1`, `SLA-2024 §2`, `PROC-042-v2 §2.1`).
- DEVE incluir o campo `source_document` em todo JSON de resposta, identificando documento, seção e versão do chunk utilizado. Resposta sem `source_document` é inválida.
- DEVE responder em português brasileiro, com tom formal e objetivo, sem gírias.
- DEVE priorizar o PROC-042-v2 para todo chamado novo (multiplicadores regionais, fator de peso, prazo adicional de +3 dias úteis e limiares de desconto 8/15 fretes-mês).
- DEVE aplicar o PROC-042 v1 exclusivamente a chamados abertos antes de 01/12/2023 que ainda estejam em processamento (PROC-042-v2 §5).
- DEVE, ao detectar versões conflitantes de um mesmo documento, usar a versão mais recente e sinalizar o conflito na resposta.
- DEVE bloquear o fluxo padrão de devolução para cargas perigosas (classes ANTT 1–6), cargas refrigeradas com cadeia de frio rompida e cargas com lacre violado sem documentação, e encaminhar à Gestão de Riscos (ramal 4500) — POL-001 §3.2.
- DEVE, quando a carga for descrita por nome coloquial potencialmente perigoso sem classe ANTT informada, perguntar ao atendente se a carga possui classificação ANTT 1–6 antes de decidir o roteamento (Glossário, Termo 24).
- DEVE classificar o chamado como incidente crítico quando **qualquer um** dos 4 critérios do SLA-2024 §3 for atendido, ativando os SLAs reduzidos correspondentes.
- DEVE validar todo tier de cliente contra o conjunto fechado {Gold, Silver, Standard} — SLA-2024 §1.
- DEVE rotular como "informação não normativa, sujeita a confirmação" todo conteúdo cuja única fonte seja o FAQ-Atendimento (ex.: seguro de carga 0,3%/0,8%, carga danificada, frete expresso para carga perigosa).
- DEVE distinguir explicitamente SLA de primeira resposta (BC3), SLA de resolução (BC3) e prazo de triagem de devolução de 4h úteis (BC1) ao informar prazos — nunca colapsá-los em um único número.
- DEVE sinalizar explicitamente respostas de baixa confiança (`low_confidence: true`) e escalar ao supervisor humano quando o confidence score ficar abaixo do limiar definido na spec do query endpoint.

#### NÃO DEVE

- NÃO DEVE inventar, estimar ou extrapolar números, SLAs, multiplicadores, fatores de peso, percentuais, prazos ou valores ausentes dos chunks recuperados.
- NÃO DEVE informar ou estimar o valor base do frete — variável externa consultada apenas no sistema interno (PROC-042-v2 §2).
- NÃO DEVE confirmar tiers inexistentes (ex.: "Platinum") — deve informar os três tiers válidos e solicitar o número do contrato (SLA-2024 §1; FAQ-15).
- NÃO DEVE aplicar a fórmula de frete especial a cargas com peso igual ou inferior a 500 kg — não há documento normativo para frete padrão; encaminhar ao Comercial.
- NÃO DEVE misturar parâmetros do PROC-042 v1 e do PROC-042-v2 no mesmo cálculo.
- NÃO DEVE processar devolução de carga perigosa pelo processo padrão, nem afirmar que a devolução é "impossível" — o tratamento é individual via Gestão de Riscos.
- NÃO DEVE prometer resultado, prazo ou aprovação da Gestão de Riscos, do Compliance ou do Jurídico — processos internos não documentados (Lacunas L1 e L2).
- NÃO DEVE calcular o prazo total de entrega de frete especial — pode informar apenas o adicional (+3 dias úteis, v2), pois o prazo padrão da rota não está no corpus.
- NÃO DEVE afirmar que o prazo de triagem de BC1 pausa fora do horário comercial — regra não documentada (Pendência DP-08).
- NÃO DEVE responder com base em conhecimento externo ao corpus indexado da NovaTech.

#### QUANDO EM DÚVIDA

- QUANDO o confidence score estiver abaixo do limiar: DEVE responder com a ressalva de baixa confiança e DEVE escalar ao supervisor humano, nunca apresentar a resposta como definitiva.
- QUANDO houver indício não confirmado de carga perigosa: DEVE adotar o princípio de segurança e encaminhar à Gestão de Riscos (ramal 4500).
- QUANDO houver conflito documental sem regra de transição aplicável: DEVE priorizar a versão mais recente, sinalizar o conflito e orientar confirmação com o Comercial para contratos anteriores.
- QUANDO a informação não existir no corpus (frete < 500 kg, processo pós-ramal 4500, prazo de ressarcimento de sinistro): DEVE declarar a indisponibilidade e encaminhar ao setor responsável, sem preencher a lacuna com hipóteses.

### Ubiquitous Language Glossary

| Termo | Definição Operacional | Ambiguidade/Risco | Fonte |
|---|---|---|---|
| cliente Gold | Tier de contrato anual > R$ 500.000 OU > 200 operações/mês. SLA: resposta 2h úteis / resolução 24h úteis; incidentes críticos 30 min / 4h, com relógio de SLA **sem pausa**. Gerente de conta dedicado. | Cliente pode alegar tier indevido; relógio sem pausa vale apenas para críticos Gold. | SLA-2024 §1, §2, §5 |
| cliente Silver | Tier de contrato anual entre R$ 100.000 e R$ 500.000 OU 50–200 operações/mês. SLA: resposta 4h úteis / resolução 48h úteis; críticos 1h / 8h. | Não possui gerente de conta dedicado; não confundir SLAs com os de Gold. | SLA-2024 §1, §2 |
| cliente Standard | Tier residual (todos os demais clientes). SLA: resposta 8h úteis / resolução 72h úteis; críticos 2h / 24h. Revisão anual. | "Standard" não significa "sem SLA" — os prazos são contratuais. | SLA-2024 §1, §2 |
| carga perigosa | Mercadoria nas classes ANTT 1–6 (Res. ANTT nº 5.947/2021). Não elegível para devolução padrão nem para frete via PROC-042 (segue PROC-043). Qualquer irregularidade = incidente crítico. | Nome coloquial ("cloro", "bateria de lítio") não confirma nem descarta a classificação — perguntar a classe ANTT antes de rotear. | POL-001 §3.2; Glossário v2, Termo 24 |
| devolução padrão | Solicitação via Portal do Cliente em até 7 dias úteis após recebimento confirmado, com CT-e + mínimo 3 fotos + motivo. Triagem em 4h úteis; coleta reversa em 2 dias úteis; reembolso em 5 dias úteis após recebimento no CD. | Não cobre carga danificada em trânsito (sinistro, BC5) nem categorias de exceção de BC4. Dias úteis, não corridos. | POL-001 §3.1, §3.3; Glossário v2, Termos 1–2 |
| SLA de resolução | Tempo máximo contratual para solução efetiva do chamado: Gold 24h / Silver 48h / Standard 72h úteis; críticos 4h / 8h / 24h. | Não confundir com SLA de primeira resposta nem com prazo de reembolso de devolução (5 dias úteis, BC1). | SLA-2024 §2; Glossário v2, Termo 20 |
| SLA de primeira resposta | Tempo máximo contratual para o primeiro retorno ao cliente (inclusive "estamos verificando"): Gold 2h / Silver 4h / Standard 8h úteis; críticos 30 min / 1h / 2h. | Primeira resposta ≠ resolução ≠ conclusão da triagem de devolução (4h úteis, métrica independente de BC1). | SLA-2024 §2; FAQ-41; Glossário v2, Termo 19 |
| multiplicador regional | Fator da fórmula de frete especial, por região de destino. Valores vigentes (v2): Sul 1.3, Sudeste 1.1, Centro-Oeste 1.4, Nordeste 1.5, Norte 1.8. | Conflito ativo v1/v2 — usar v1 (ex.: Norte 1.6) gera subcobrança. Nunca misturar versões. | PROC-042-v2 §2.1; Glossário v2, Termo 13 |
| fator de peso | Coeficiente por faixa de peso (v2): 1.0 (500–1.000 kg), 1.15 (1.001–3.000 kg), 1.4 (> 3.000 kg). | A v1 usa 1.2/1.5 nas faixas superiores; apenas a faixa 500–1.000 kg coincide entre versões. | PROC-042-v2 §2; Glossário v2, Termo 14 |
| frete especial | Modalidade exclusiva para cargas > 500 kg: `Valor base × Multiplicador regional × Fator de peso`, com prazo adicional de +3 dias úteis (v2). Cargas perigosas > 500 kg seguem PROC-043. | Não é frete expresso (conceito não normatizado, FAQ-32). Não aplicar a cargas ≤ 500 kg (Lacuna L5). | PROC-042-v2 §1–§3; Glossário v2, Termo 11 |
| incidente crítico | Chamado que atende a **pelo menos um** dos critérios: (1) carga > R$ 100.000 com status desconhecido > 6h; (2) carga perigosa com qualquer irregularidade; (3) > 5 chamados do mesmo cliente em 24h sobre o mesmo problema; (4) risco à segurança de pessoas. | "Alta prioridade" alegada pelo cliente não classifica o chamado — os critérios são objetivos e basta um. | SLA-2024 §3; Glossário v2, Termo 21 |
| source_document | Campo obrigatório do JSON de resposta do query endpoint, identificando o documento-fonte (ID, seção e versão) de cada afirmação gerada a partir dos chunks recuperados. | Omissão ou atribuição incorreta quebra a rastreabilidade — resposta sem `source_document` deve ser bloqueada pelo validador. | `specs/query-endpoint/requirements.md`; Anexo C (`response-builder.ts`) |

### Code Generation Constraints

- Toda resposta estruturada do query endpoint DEVE incluir `source_document` (documento, seção, versão). Implementar como campo obrigatório (não opcional) no schema Zod de `src/functions/query/validator.ts` e na montagem em `src/functions/query/response-builder.ts`.
- Validadores de tier DEVEM usar enum fechado — `z.enum(['Gold', 'Silver', 'Standard'])` em `src/shared/types.ts` e `src/functions/query/validator.ts` — e bloquear qualquer valor fora do conjunto, retornando erro de domínio definido em `src/shared/errors.ts`.
- `src/services/response-validator.ts` DEVE rejeitar respostas que contenham valores numéricos (multiplicadores, fatores, percentuais, prazos, SLAs) ausentes dos chunks recuperados — verificação determinística de números da resposta contra o conteúdo dos chunks.
- `src/services/response-validator.ts` DEVE validar multiplicadores regionais e fatores de peso contra os conjuntos canônicos da v2 (`{1.3, 1.1, 1.4, 1.5, 1.8}` e `{1.0, 1.15, 1.4}`), rejeitando valores da v1 em chamados novos.
- A lógica de versionamento de documentos DEVE priorizar PROC-042-v2 como padrão e tratar a exceção transitória explicitamente: chamados abertos antes de 01/12/2023 ainda em processamento usam a v1 (PROC-042-v2 §5). Implementar a regra em `src/services/prompt-builder.ts` e indexar metadados `document_id`, `section` e `version` em `src/pipeline/indexer.ts`.
- Fluxos de devolução de carga perigosa NÃO PODEM seguir o processo padrão: o roteamento DEVE direcionar para o fluxo de escalonamento à Gestão de Riscos (ramal 4500), nunca para o fluxo de triagem padrão de BC1. Codificar a regra de classificação ANTT 1–6 como verificação prévia no `prompt-builder` e como gate determinístico no `response-validator`.
- Respostas com confidence score abaixo do limiar definido em `specs/query-endpoint/requirements.md` DEVEM retornar `low_confidence: true` no payload e acionar o caminho de escalonamento ao supervisor em `response-builder.ts` — nunca suprimir a flag.
- O system prompt em `prompts/system-prompt.md` DEVE incorporar as regras DEVE/NÃO DEVE desta seção; toda alteração DEVE ser registrada em `prompts/prompt-changelog.md`.
- `tests/fixtures/` DEVE conter casos cobrindo no mínimo: tier inválido ("Platinum"), conflito v1/v2 de multiplicadores, carga perigosa por classe ANTT e por nome coloquial, resposta com número inexistente nos chunks, e resposta de baixa confiança.
- Respostas geradas DEVEM estar em português brasileiro formal; strings de UI e mensagens de erro voltadas ao atendente seguem o mesmo padrão.

### Repository Spec References

- [`specs/query-endpoint/requirements.md`](specs/query-endpoint/requirements.md) — Documento de Spec (requirements v2.0): define `source_document` obrigatório, limiar de confidence score, detecção de conflito documental e comportamento multi-domínio; é a fonte primária dos guardrails do endpoint de consulta.
- [`specs/pipeline-ingestao/requirements.md`](specs/pipeline-ingestao/requirements.md) — Define a ingestão dos 5 documentos-fonte com metadados de documento, seção e versão, pré-requisito para citação de fonte e priorização v1/v2.
- [`prompts/system-prompt.md`](prompts/system-prompt.md) — System prompt versionado onde as regras comportamentais (DEVE/NÃO DEVE/QUANDO EM DÚVIDA) devem ser refletidas.
- [`prompts/prompt-changelog.md`](prompts/prompt-changelog.md) — Registro obrigatório de toda mudança de prompt (data, autor, motivo, resultado esperado), garantindo auditabilidade dos guardrails.
- [`prompts/eval/golden-queries.json`](prompts/eval/golden-queries.json) — Perguntas de referência que DEVEM incluir casos de guardrail (tier inexistente, carga perigosa, conflito v1/v2, baixa confiança) com respostas esperadas.
- [`src/functions/query/validator.ts`](src/functions/query/validator.ts) — Validação de input com enum fechado de tiers e schemas Zod alinhados a esta seção.
- [`src/functions/query/response-builder.ts`](src/functions/query/response-builder.ts) — Montagem da resposta com `source_document` e flag `low_confidence` obrigatórios.
- [`src/services/prompt-builder.ts`](src/services/prompt-builder.ts) — Injeção dos chunks com regra de prioridade PROC-042-v2 e tratamento da exceção transitória.
- [`src/services/response-validator.ts`](src/services/response-validator.ts) — Harness determinístico que bloqueia números fora dos chunks, parâmetros da v1 e tiers inválidos.
- [`src/pipeline/indexer.ts`](src/pipeline/indexer.ts) — Indexação no Azure AI Search com metadados de versão e seção que viabilizam o `source_document`.
- [`src/shared/types.ts`](src/shared/types.ts) — Tipos de domínio (Tier, SourceDocument, classificação ANTT) compartilhados entre validadores e serviços.
- [`tests/fixtures/`](tests/fixtures/) — Chunks, queries e respostas esperadas que materializam os casos de guardrail em testes automatizados.
- [`docs/adr/`](docs/adr/) — Registrar como ADR a decisão de adotar PROC-042-v2 como versão padrão (formato `NNNN-titulo-da-decisao.md`).
- [`skills/artifact/create-rag-endpoint.md`](skills/artifact/create-rag-endpoint.md) — Skill de geração de endpoints RAG que DEVE incorporar as restrições de `source_document`, confiança e validação determinística.


## Testing Standards (QA)
<!-- TODO (QA — Ex. 2.1) -->

## Project Management Rules (Delivery Manager)
<!-- TODO (Delivery Manager — Ex. 2.3) -->

## Build & Deploy
<!-- TODO (Tech Lead — Ex. 2.1) -->
