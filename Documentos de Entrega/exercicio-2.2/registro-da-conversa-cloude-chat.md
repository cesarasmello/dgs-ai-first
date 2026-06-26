# Exportação da Conversa — Exercício 2.2: Formalização de Guardrails (NovaTech)

**Finalidade:** registro auditável da conversa, contendo as entradas (prompts e documentos fornecidos pelo participante) e, como saída, o nome de cada arquivo gerado em cada turno.
**Data da conversa:** 10/06/2026
**Participante:** César Augusto (Product Specialist — DGS)
**Ferramenta usada:** Claude shat

---

## Sumário de artefatos gerados

| # | Turno | Arquivo gerado | Conteúdo |
|---|---|---|---|
| 1 | Turno 1 | `guardrails-assistente-novatech.md` | Documento de guardrails (G-01 a G-14) organizado em DEVE / NÃO DEVE / QUANDO EM DÚVIDA, com tipo de enforcement, justificativa e incidente prevenido |
| 2 | Turno 2 | `analise-enforcement-guardrails-novatech.md` | Análise de enforcement por guardrail (prompt vs código vs híbrido), com risco se ficar só no prompt e observação de implementação |
| 3 | Turno 3 | `matriz-guardrail-incidente-novatech.md` | Matriz de rastreabilidade guardrail → incidente, com prevenção primária/secundária e camadas de defesa por incidente |
| 4 | Turno 4 | `exportacao-conversa-exercicio-2-2-novatech.md` | Este documento (exportação auditável da conversa) |

---

## Turno 1

### Entrada — Documento fornecido

- **`anexo-a-documentacao-simulada-novatech.md`** — Anexo A com os 5 documentos simulados da NovaTech: POL-001 (Política de Devolução, v3.1), PROC-042 (Frete Especial, v1.0), PROC-042-v2 (Frete Especial Revisado, v2.0), SLA-2024 (Tabela de SLA por Tipo de Cliente, v2024.1) e FAQ-Atendimento (informal, não controlado), além das notas de meta-informação (contradições e gaps identificados).

### Entrada — Prompt do participante

> Você está atuando como Product Specialist/Analista de IA em um projeto corporativo da NovaTech.
>
> **Contexto:** Estamos no exercício 2.2 da fase de estruturação do trabalho. O objetivo é formalizar guardrails do assistente de IA da NovaTech como um artefato consumível por humanos e agentes.
>
> **Fonte de verdade:** a documentação da NovaTech; os guardrails informais já definidos (1. Sempre citar fonte. 2. Nunca inventar prazos ou valores. 3. Quando não encontrar resposta, dizer explicitamente. 4. Responder em português formal.); e os incidentes simulados:
> 1. O assistente respondeu que o prazo de devolução para carga perigosa é 7 dias, quando na verdade cargas perigosas NÃO podem ser devolvidas.
> 2. O assistente citou PROC-042, seção 2, mas usou multiplicadores da versão 1 desatualizada em vez da v2 vigente.
> 3. O assistente disse que não encontrou informação sobre SLA Gold, embora o documento SLA-2024 contivesse a resposta.
>
> **Tarefa:** Elabore um documento de guardrails do assistente organizado em DEVE / NÃO DEVE / QUANDO EM DÚVIDA. Para cada guardrail: escreva a regra de forma prescritiva e específica ao domínio da NovaTech; classifique como enforcement via prompt (probabilístico) ou enforcement via código (determinístico); justifique a classificação em 1 frase; relacione o guardrail com pelo menos um dos incidentes simulados que ele ajuda a prevenir.
>
> **Regras de elaboração:** não escrever guardrails genéricos demais; considerar o domínio de logística, SLA, frete, devolução e versionamento documental; diferenciar claramente prompt vs código; português formal; evitar ambiguidades.
>
> **Formato de saída:** tabela com as colunas Categoria, Guardrail, Tipo de enforcement, Justificativa, Incidente prevenido. Depois da tabela: (1) síntese dos principais riscos mitigados; (2) lista curta dos guardrails que obrigatoriamente deveriam ser implementados em código.
>
> **Critérios de qualidade:** guardrails específicos ao domínio da NovaTech; classificação prompt vs código tecnicamente correta; cada guardrail conectado a um risco concreto; texto utilizável diretamente como entregável do exercício.

### Saída — Arquivo gerado

- **`guardrails-assistente-novatech.md`** — 14 guardrails (5 DEVE, 4 NÃO DEVE, 5 QUANDO EM DÚVIDA), tabela com classificação de enforcement e incidente prevenido, síntese de 5 riscos mitigados e lista de 7 guardrails de implementação obrigatória em código (G-07, G-06, G-03, G-12, G-01, G-10, G-09).

---
