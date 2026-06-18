# Entrega Final - Exercicio 1.1

### Organização dos arquivos

**Estratégia:** Antes de iniciar as atividades, analisei os arquivos fornecidos para compreender o conteúdo e organizar a estrutura de trabalho (prompts, outputs e evidências). Essa preparação facilitou a consulta dos materiais e o registro dos resultados durante cada etapa.

**Ferramentas Utilizadas:**
- GitHub Copilot para leitura, organização dos arquivos Markdown e integração com GitHub.
- Claude Chat para análise e execução das etapas.

---

### Etapa 1 - Visão geral

**Decisão de contexto (por que nessa ordem):**
1. Primeiro, enviei ao Claude apenas títulos, metadados e resumos dos 5 documentos.
2. Em seguida, pedi um mapa de temas cobertos e hipóteses de gaps.

Essa ordem foi escolhida para reduzir o volume inicial de contexto e priorizar entendimento macro antes da leitura profunda. Com isso, a IA conseguiu identificar padrões e possíveis lacunas sem se perder em detalhes. Se os 5 documentos completos fossem enviados de uma vez em um único prompt, a tendência seria de perda de foco, nem todos os documentos seriam analisados igualmente, maior chance de resposta superficial e menor consistência na identificação de conflitos.

**Script:**

- Documento de entrada:
	| Nome do arquivo | Tipo |
	|-----------------|------|
	| `FAQ-atendimento-resumido.md` | Resumo curado — Turno 1 |
	| `POL-001-politica-devolucao-resumido.md` | Resumo curado — Turno 2 |
	| `PROC-042-frete-especial-v1-resumido.md` | Resumo curado — Turno 3 |
	| `PROC-042-v2-frete-especial-revisado1-resumido.md` | Resumo curado — Turno 4 |
	| `SLA-2024-tabela-sla-clientes-resumido.md` | Resumo curado — Turno 5 |

- Prompt:
	> Você é Product Specialist no projeto NovaTech e está conduzindo a fase de Intent + Discovery.
	>
	> **Objetivo:** Gerar um mapa de temas cobertos e hipóteses de gaps usando os 5 documentos.
	>
	> **Instruções de execução:**
	> 1. Liste os temas principais cobertos por cada documento.
	> 2. Agrupe temas repetidos e temas exclusivos.
	> 3. Identifique possíveis conflitos ou zonas de sobreposição entre documentos.
	> 4. Levante hipóteses de gaps de informação que podem impactar atendimento, SLA, frete e devolução.
	> 5. Para cada hipótese de gap, indique: impacto no negócio, evidência, nível de confiança (alto/médio/baixo) e pergunta para discovery humano.
	> 6. Não inventar fatos além do que estiver nas entradas.
	>
	> **Formato de saída:** Seção 1 (Mapa de temas) / Seção 2 (Sobreposições e conflitos) / Seção 3 (Hipóteses de gaps em tabela) / Seção 4 (Resumo executivo 5–8 linhas).
	>
	> **Destino:** `entrega-final.md` com título "Tarefa 1 - Etapa 1 | Mapa de temas e hipóteses de gaps".
	
- Output: Arquivo `entrega-final.md` - Mapa completo de temas cobertos, sobreposições, 10 hipóteses de gaps com evidências e perguntas para discovery, e resumo executivo para negócio. 

**Outputs obtido:**
- Output/anexo-a-documentos-individuais-resumido/FAQ-atendimento-resumido.md
- Output/anexo-a-documentos-individuais-resumido/POL-001-politica-devolucao-resumido.md
- Output/anexo-a-documentos-individuais-resumido/PROC-042-frete-especial-v1-resumido.md
- Output/anexo-a-documentos-individuais-resumido/PROC-042-v2-frete-especial-revisado1-resumido.md
- Output/anexo-a-documentos-individuais-resumido/SLA-2024-tabela-sla-clientes-resumido.md
- Output/mapa-de-temas-cobertos-e-hipoteses-de-gaps.md

**Análise crítica da qualidade:**
- A resposta teve boa cobertura temática e ajudou a priorizar os próximos passos.
- Como esperado para uma visão geral, faltaram detalhes de conflito entre regras específicas.
- A qualidade foi adequada para triagem e definição da etapa 2.

---

### Etapa 2 - Análise profunda

**Decisão de contexto (por que nessa ordem):**
1. Com base no mapa da etapa 1, selecionei os dois documentos PROC-042 para aprofundamento: PROC-042-frete-especial-v1 e PROC-042-v2-frete-especial-revisado.
2. Solicitei análise de inconsistências e impactos operacionais.

Essa ordem foi escolhida para concentrar a janela de contexto nos documentos com maior potencial de risco e conflito, evitando dispersão com arquivos menos relevantes naquele momento.

**Script:**

- Documentos de entrada: `PROC-042-frete-especial-v1.md` e `PROC-042-v2-frete-especial-revisado.md`

- Prompt:
	> Você é Product Specialist em Intent + Discovery. Analise exclusivamente os documentos:
	> * PROC-042-frete-especial-v1
	> * PROC-042-v2-frete-especial-revisado
	>
	> **Objetivo:** Identificar inconsistências entre os dois documentos.
	>
	> Considere inconsistência quando houver:
	> * Regra conflitante
	> * Regra duplicada com valores diferentes
	> * Fluxo operacional divergente
	> * Condição de exceção presente em um documento e ausente no outro
	> * Termos iguais com significados diferentes
	>
	> **Regras:**
	> * Não use conhecimento externo.
	> * Não invente informações.
	> * Sempre cite evidência textual dos dois documentos para cada inconsistência.
	> * Se não houver inconsistências, declare explicitamente "Nenhuma inconsistência identificada".
	>
	> **Formato de saída:** Gerar conteúdo em Markdown para arquivo chamado `analise-de-inconsistencia.md` com seções:
	> * Resumo executivo
	> * Matriz de inconsistências
	>
	> Na seção "Matriz de inconsistências", use tabela com colunas:
	> ID | Tipo | Trecho no PROC-042-v2 | Trecho na PROC-042-v1 | Descrição da inconsistência | Recomendação | Responsável sugerido

- Output: Arquivo `analise-de-inconsistencia.md`

**Output obtido:**
- Output/analise-de-inconsistencia.md

**Análise crítica da qualidade:**
- Houve ganho de profundidade em relação à etapa 1, com apontamentos mais concretos de inconsistência.
- A análise ficou mais acionável para discovery por estar focada em regras críticas de negócio.
- Em comparação com a etapa 1, a precisão aumentou, mas ainda sem o contraste com prática real do atendimento.

---

### Etapa 3 - Cruzamento com prática informal

**Decisão de contexto (por que nessa ordem):**
1. Forneci os outputs das etapas 1 e 2 para preservar o raciocínio já refinado.
2. Incluí o FAQ-Atendimento completo para cruzar inconsistências formais com práticas informais.

Essa ordem foi escolhida para validar se o que está documentado formalmente é compatível com o que o time de atendimento realmente aplica no dia a dia.

**Script:**

- Documentos de entrada:  `mapa-de-temas-cobertos-e-hipoteses-de-gaps.md`, `analise-de-inconsistencia.md` e `FAQ-atendimento.md`

- Prompt:
	> Você é Product Specialist focado em Intent + Discovery.
	>
	> **Objetivo:** Cruzar inconsistências identificadas na documentação informada.
	>
	> **Fontes permitidas (não usar conhecimento externo):**
	> 1. mapa-de-temas-cobertos-e-hipoteses-de-gaps
	> 2. analise-de-inconsistencia
	> 3. FAQ-atendimento
	>
	> **Regras de análise:**
	> 1. Considere "inconsistência" como:
	>    * contradição entre documentos
	>    * prática do FAQ sem respaldo formal
	>    * regra formal que o FAQ orienta de forma divergente
	>
	> 2. Para cada inconsistência, traga evidências com trechos literais curtos de ambos os lados (formal vs FAQ).
	>
	> **Formato obrigatório de saída:**
	> * Resumo executivo (5 a 8 linhas)
	> * Tabela de inconsistências com colunas: ID | Tema | Evidência documento formal | Evidência FAQ
	> * Top 3 riscos prioritários
	> * Perguntas para validação com stakeholders (mínimo 5)
	>
	> 3. **Entrega:** Gere o conteúdo final em Markdown pronto para arquivo com o nome: `analise-de-inconsistencia-vs-FAQ.md`
	>
	> 4. **Critérios de qualidade:**
	>    * Não inventar informações
	>    * Não usar fontes fora das três informadas
	>    * Linguagem objetiva, sem jargão excessivo
	>    * Tudo deve ser rastreável a evidências dos documentos

Output: Arquivo `analise-de-inconsistencia-vs-FAQ.md`

**Output obtido:**
- Output/analise-de-inconsistencia-vs-FAQ.md

**Análise crítica da qualidade:**
- Esta foi a etapa com melhor qualidade para tomada de decisão, pois conectou regra formal e operação real.
- A análise ficou mais completa e útil para discovery, destacando divergências com impacto prático.
- Em comparação com as etapas anteriores, houve maior clareza sobre risco operacional e necessidade de governança documental.

---

### Mapa de riscos e tratamento no discovery

1. **Risco no Processo:**
	- **Risco 1:** Atualizações mensais feitas por 3 áreas (Operações, Compliance e Comercial) sem processo unificado de revisão.
		**Como levar para o discovery:** mapear o fluxo atual de publicação, definir dono por documento e instituir um rito único de validação antes da indexação no assistente.

	- **Risco 2:** Contradição entre versões de documentos (exemplo: procedimentos com mesma numeração e regras diferentes).
		**Como levar para o discovery:** criar regra de versionamento oficial, com status de vigência e bloqueio de conteúdo obsoleto no pipeline de conhecimento.
		
	- **Risco 3:** Dependência de conhecimento tácito da equipe ("perguntar para quem sabe") para resolver conflitos.
		**Como levar para o discovery:** registrar decisões recorrentes em base oficial, criar trilha de auditoria de respostas e estabelecer canal formal de feedback do atendimento.

2. **Risco no Negócio:**
	- **Risco 1 — FAQ-INC-003: Desconto por volume com regra híbrida inexistente**
		O FAQ combina o limiar da v1 (10 fretes/mês) com o mecanismo da v2 (desconto automático), criando uma regra que não existe em nenhum dos documentos formais. Um atendente que aplique essa orientação pode prometer ao cliente um desconto automático de percentual indefinido a partir de um limiar que já foi alterado pela versão mais recente. O impacto é financeiro e contratual: clientes com 8 ou 9 fretes/mês têm direito ao desconto pela v2, mas o FAQ os exclui; clientes com 10 fretes/mês podem receber a promessa de desconto automático sem que isso esteja contratualmente garantido se o contrato for baseado na v1.

	- **Risco 2 — FAQ-INC-001: Devolução de carga perigosa com expectativa indevida criada no cliente**
		O FAQ instrui o atendente a não fechar a possibilidade de devolução de carga perigosa, mencionando exceções ocorridas na prática. A POL-001, no entanto, classifica cargas perigosas classes 1–6 como inelegíveis para devolução padrão sem qualquer ressalva de exceção. Ao manter o cliente em expectativa de que uma autorização pelo ramal 4500 pode viabilizar a devolução, o atendente cria um compromisso informal não suportado pela política formal, expondo a empresa a contestações e à percepção de atendimento inconsistente.
---

### Reflexão sobre Progressive Disclosure

Se os cinco documentos completos fossem enviados de uma só vez no primeiro prompt, a tendência seria uma perda de foco, maior probabilidade de respostas superficiais e menor consistência na identificação de conflitos. Nesse cenário, aumentaria a chance de o modelo interpretar os documentos PROC-042-v1 e PROC-042-v2 como complementares, e não como conflitantes, além de a FAQ provavelmente concentrar mais atenção por ser o documento mais extenso. Com a abordagem progressiva, o contexto foi distribuído em etapas: primeiro, uma visão macro; depois, o aprofundamento direcionado; e, por fim, a validação com a prática informal.

Na prática, essa estratégia melhorou a qualidade da análise em três dimensões:
- Priorização: foco no que era mais crítico em cada momento.
- Precisão: redução de ruído e melhor identificação de inconsistências relevantes.
- Ação: outputs mais úteis para orientar discovery e definição de próximos passos.

---