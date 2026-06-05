# Análise de Inconsistências — Documentação Formal vs. FAQ de Atendimento

**Analista:** Product Specialist — Intent + Discovery  
**Data da análise:** 01/06/2026  
**Fontes utilizadas:**
- `mapa-de-temas-cobertos-e-hipoteses-de-gaps.md`
- `analise-de-inconsistencia.md`
- `FAQ-atendimento.md`

---

## Resumo Executivo

O cruzamento entre o FAQ de atendimento e a documentação formal (PROC-042-v1, PROC-042-v2, conforme mapeadas na análise de inconsistências e no mapa de temas) revelou **7 inconsistências**, entre contradições diretas, orientações sem respaldo normativo e regras formais aplicadas de forma divergente na prática. O risco mais crítico está na orientação do FAQ sobre descontos por volume (Item 45): o texto direciona o atendente à PROC-042 como se houvesse uma regra única e automática, ignorando que as duas versões vigentes têm critérios incompatíveis — o que pode gerar promessas comerciais inválidas ao cliente. O segundo risco relevante é a orientação sobre devolução de carga perigosa (Item 3): o FAQ instrui o atendente a não negar a possibilidade de devolução, sinalizando que exceções ocorrem, mas a POL-001 classifica cargas perigosas classes 1–6 como categoricamente inelegíveis para devolução padrão, sem menção a exceções autorizadas pelo ramal 4500. O terceiro risco é a ausência completa de documento normativo para seguro de carga (Item 22): o FAQ fornece percentuais concretos ao cliente (0,3% e 0,8%), mas o mapa de temas confirma que não há POL ou PROC correspondente no corpus — expondo a empresa a contestações sem base normativa. A combinação desses três pontos representa um vetor de risco direto para o relacionamento com clientes Gold e para a consistência financeira das operações. Nenhuma das inconsistências identificadas foi inventada: todas são rastreáveis a trechos literais dos documentos informados.

---

## Tabela de Inconsistências

| ID | Tema | Evidência — Documento Formal | Evidência — FAQ | Tipo de inconsistência |
|----|------|------------------------------|-----------------|------------------------|
| FAQ-INC-001 | Devolução de carga perigosa | POL-001 (via mapa de temas): *"Categorias inelegíveis para devolução padrão: cargas perigosas classes 1–6"*. Encaminhamento ao ramal 4500 previsto apenas para exceções de risco, não para autorização de devolução. | FAQ Item 3: *"não diga que é impossível — diga que precisa de tratamento especial"* e *"já tiveram casos em que o pessoal de Riscos autorizou exceção"* | Contradição direta: o FAQ orienta a deixar aberta a possibilidade de devolução; a POL-001 classifica cargas perigosas classes 1–6 como inelegíveis, sem prever exceções autorizadas pelo ramal 4500. |
| FAQ-INC-002 | Critério de uso da PROC-042 (v1 ou v2) | analise-de-inconsistencia.md (INC-006): *"Nenhum dos dois documentos declara formalmente qual versão está em vigor."* PROC-042-v2, Seção 5: *"Chamados novos a partir de 01/12/2023 devem usar os multiplicadores desta versão."* | FAQ Item 8: *"Na dúvida, use a mais recente (v2), mas se o cliente reclamar do valor, pode ser que o contrato dele ainda esteja na tabela antiga."* | Prática sem respaldo formal completo: o FAQ adota critério próprio (usar v2 por padrão) sem mencionar a regra de corte por data de chamado prevista na Seção 5 da v2, que é o único critério formal de transição disponível. |
| FAQ-INC-003 | Desconto por volume — limiar e mecanismo | PROC-042-v1, Seção 4: *"mais de 10 fretes especiais/mês [...] negociados pelo Comercial e registrados em aditivo contratual."* PROC-042-v2, Seção 4: *"a partir de 8 fretes especiais/mês [...] desconto de 5% [...] Acima de 15 fretes/mês, desconto de 10%."* | FAQ Item 45: *"Para clientes com mais de 10 fretes especiais por mês, existe desconto automático na tabela (veja PROC-042)."* | Contradição tripla: (a) o FAQ usa o limiar de 10 fretes/mês da v1, mas chama o desconto de "automático", característica da v2; (b) a v2 tem limiar de 8 fretes/mês, não 10; (c) nenhuma das duas versões é identificada como única fonte de verdade. O FAQ cria uma regra híbrida inexistente em qualquer dos documentos formais. |
| FAQ-INC-004 | Seguro de carga — percentuais e condições | Mapa de temas (G3): *"FAQ descreve percentuais de seguro (0,3% padrão; 0,8% perigosas) [...] mas não há POL ou PROC correspondente no corpus."* Nenhum documento normativo formal cobre este tema. | FAQ Item 22: *"O valor é 0,3% do valor declarado da mercadoria para cargas padrão e 0,8% para cargas perigosas. [...] Contratos mais antigos podem ter percentuais diferentes — confirme com o Comercial."* | Prática sem respaldo normativo: o FAQ fornece percentuais concretos ao cliente sem que exista qualquer POL ou PROC correspondente no corpus. Orientação operacional sem base formal documentada. |
| FAQ-INC-005 | SLA de resposta e resolução por tier | SLA-2024 (via mapa de temas): documento formal com *"SLAs de resposta e resolução por tier para chamados gerais e incidentes críticos"* e *"metodologia de medição (Azure DevOps, horário comercial 08h–18h)"*. Mapa de temas (sobreposições): *"FAQ descreve os SLAs de forma resumida, mas sem garantia de alinhamento com a versão formal."* | FAQ Item 41: *"O Gold tem 2h de resposta e 24h de resolução. Silver é 4h e 48h. Standard é 8h e 72h."* Sem menção à metodologia de medição, horário comercial ou distinção entre chamados gerais e incidentes críticos. | Regra formal aplicada de forma incompleta: o FAQ reproduz os valores de SLA sem os critérios de medição e sem distinção entre chamados gerais e incidentes críticos, criando risco de interpretação incorreta pelo atendente. |
| FAQ-INC-006 | Envio expresso de carga perigosa | Mapa de temas: *"PROC-043 está em processo de revisão pelo Compliance"* (via PROC-042-v2) e PROC-043 não está indexada no corpus. Nenhum documento formal disponível define o fluxo de autorização para frete expresso de carga perigosa. | FAQ Item 32: *"Sim, mas precisa de autorização do Compliance e a documentação ANTT tem que estar atualizada. Na prática, demora uns 2 dias para conseguir a autorização."* | Prática sem respaldo formal: o FAQ descreve um fluxo operacional (autorização pelo Compliance, prazo de 2 dias) para o qual não existe PROC vigente e acessível no corpus. A PROC-043, que seria a referência normativa, está em revisão e indisponível. |
| FAQ-INC-007 | Critério de priorização de chamado de rastreamento | SLA-2024 (via mapa de temas): classifica incidente crítico por *"4 critérios independentes"* formalmente definidos; metodologia de classificação não inclui valor de carga como critério mencionado no mapa. Nenhum documento formal indexado define R$ 50.000 como limiar de priorização. | FAQ Item 27: *"classifique como prioridade alta se for Gold ou se o valor da carga for acima de R$ 50.000."* | Prática sem respaldo formal: o FAQ usa um critério financeiro (R$ 50.000) para definir prioridade de chamado que não está presente em nenhum documento normativo disponível no corpus. |

---

## Top 3 Riscos Prioritários

### Risco 1 — FAQ-INC-003: Desconto por volume com regra híbrida inexistente
O FAQ combina o limiar da v1 (10 fretes/mês) com o mecanismo da v2 (desconto automático), criando uma regra que não existe em nenhum dos documentos formais. Um atendente que aplique essa orientação pode prometer ao cliente um desconto automático de percentual indefinido a partir de um limiar que já foi alterado pela versão mais recente. O impacto é financeiro e contratual: clientes com 8 ou 9 fretes/mês têm direito ao desconto pela v2, mas o FAQ os exclui; clientes com 10 fretes/mês podem receber a promessa de desconto automático sem que isso esteja contratualmente garantido se o contrato for baseado na v1.

### Risco 2 — FAQ-INC-001: Devolução de carga perigosa com expectativa indevida criada no cliente
O FAQ instrui o atendente a não fechar a possibilidade de devolução de carga perigosa, mencionando exceções ocorridas na prática. A POL-001, no entanto, classifica cargas perigosas classes 1–6 como inelegíveis para devolução padrão sem qualquer ressalva de exceção. Ao manter o cliente em expectativa de que uma autorização pelo ramal 4500 pode viabilizar a devolução, o atendente cria um compromisso informal não suportado pela política formal, expondo a empresa a contestações e à percepção de atendimento inconsistente.

### Risco 3 — FAQ-INC-004: Percentuais de seguro de carga sem base normativa
O FAQ fornece ao cliente percentuais concretos de seguro (0,3% e 0,8%) sem que exista qualquer documento normativo formal que os sustente no corpus analisado. Em caso de sinistro, contestação contratual ou auditoria, a empresa não dispõe de POL ou PROC que formalize esses valores. O próprio FAQ admite que "contratos mais antigos podem ter percentuais diferentes", reforçando a fragilidade da orientação. Este risco é agravado pela ausência de responsável formal pelo FAQ e pela ausência de validação por Compliance.

---

## Perguntas para Validação com Stakeholders

1. **Sobre FAQ-INC-001 (devolução de carga perigosa):** A POL-001 classifica cargas perigosas classes 1–6 como inelegíveis para devolução padrão. O ramal 4500 (Gestão de Riscos) tem autoridade formal para aprovar exceções a essa regra? Se sim, esse fluxo de exceção está documentado em alguma POL ou PROC vigente?

2. **Sobre FAQ-INC-003 e FAQ-INC-002 (descontos e versão vigente da PROC-042):** Os contratos de clientes ativos que operam acima de 8 fretes especiais/mês já foram atualizados para refletir a tabela de descontos da v2? Ou ainda há contratos vigentes baseados nos critérios da v1? Quem tem autoridade para formalizar a revogação da v1 e comunicar a mudança ao time de atendimento?

3. **Sobre FAQ-INC-004 (seguro de carga):** Existe uma política formal de seguro de carga (POL ou PROC) que não foi indexada no corpus analisado? Se não existe, os percentuais praticados (0,3% e 0,8%) têm respaldo em cláusula contratual padrão ou são aplicados informalmente pelo time?

4. **Sobre FAQ-INC-006 (frete expresso de carga perigosa):** Qual o status atual da revisão da PROC-043 pelo Compliance? Existe uma versão provisória em uso que o time de atendimento deveria estar consultando? O fluxo descrito no FAQ (autorização do Compliance em ~2 dias) está alinhado com a versão em revisão ou é prática informal?

5. **Sobre FAQ-INC-007 (critério de R$ 50.000 para priorização de rastreamento):** O limiar de R$ 50.000 para classificação de chamado de rastreamento como prioridade alta tem origem em algum documento formal não indexado, em política interna de operações ou é critério empírico adotado pelo time? Esse critério está alinhado com os 4 critérios de incidente crítico definidos na SLA-2024?

6. **Sobre FAQ-INC-005 (SLA resumido sem critérios de medição):** O FAQ reproduz os valores de SLA sem mencionar horário comercial (08h–18h) nem distinção entre chamados gerais e incidentes críticos. O time de atendimento tem acesso direto à SLA-2024 para consulta nos casos limítrofes, ou o FAQ é a única referência operacional utilizada na prática?

---

*Análise realizada exclusivamente com base nas três fontes informadas. Nenhum conhecimento externo foi utilizado. Todas as evidências são rastreáveis a trechos literais dos documentos.*
