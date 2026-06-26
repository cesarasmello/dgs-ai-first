# Registro de Conversa — Projeto NovaTech: Curadoria de Conhecimento para RAG

---

## Visão geral

Este arquivo registra todas as interações da sessão de trabalho do projeto NovaTech, detalhando os documentos fornecidos como entrada, os prompts utilizados e os arquivos gerados em cada etapa.

---

## Turno 1 — Resumo do FAQ-Atendimento

### Documento de entrada
| Campo | Valor |
|-------|-------|
| Nome | `FAQ-atendimento.md` |
| Tipo | FAQ interno informal |
| Conteúdo | 9 itens selecionados de 47 perguntas frequentes do time de suporte |

### Prompt
> Você é um Product Specialist com foco em curadoria de conhecimento para RAG e mapeamento de cobertura temática.
> Tarefa: A partir do documento-fonte, gerar um novo arquivo Markdown de resumo.
>
> **Entradas:**
> - Documento-fonte: `FAQ-atendimento.md`
> - Nome do arquivo de saída: `FAQ-atendimento-resumido.md`
>
> **Instruções obrigatórias:**
> 1. Preserve apenas três seções no arquivo final: Título, Metadados e Resumo.
> 2. Não inclua introdução, conclusão, exemplos extras ou comentários.
> 3. Em Metadados, use exatamente os campos: `tipo_documento`, `versao`, `data_referencia`, `responsavel`, `status_normativo`, `publico_alvo`, `tema_principal`, `escopo_resumo`, `dependencias_referencias`, `confiabilidade`. Se não houver informação, preencher como "Não informado".
> 4. O Resumo deve: ter entre 6 e 10 linhas; destacar temas cobertos; apontar possíveis lacunas para análise posterior (hipóteses de gap); manter linguagem objetiva em pt-BR.
> 5. Saída estritamente em Markdown válido.
> 6. Não inventar dados não presentes; quando necessário, usar "Não informado".
>
> **Formato de saída:** Título em H1 / Seção "Metadados" em H2 com lista chave: valor / Seção "Resumo" em H2 com parágrafo corrido.
>
> **Critério de aceitação:** Se houver qualquer seção além de Título, Metadados e Resumo, a saída está incorreta.

### Arquivo gerado
| Nome do arquivo | Descrição |
|-----------------|-----------|
| `FAQ-atendimento-resumido.md` | Resumo curado do FAQ com metadados estruturados e hipóteses de gap. Confiabilidade marcada como **Baixa**. |

---

## Turno 2 — Resumo da POL-001

### Documento de entrada
| Campo | Valor |
|-------|-------|
| Nome | `POL-001-politica-devolucao.md` |
| Tipo | Política normativa oficial |
| Conteúdo | Regras e procedimentos para devolução de mercadorias (versão 3.1, jan/2024) |

### Prompt
> Você é um Product Specialist com foco em curadoria de conhecimento para RAG e mapeamento de cobertura temática.
> Tarefa: A partir do documento-fonte, gerar um novo arquivo Markdown de resumo.
>
> **Entradas:**
> - Documento-fonte: `POL-001-politica-devolucao.md`
> - Nome do arquivo de saída: `POL-001-politica-devolucao-resumido.md`
>
> *(mesmas instruções obrigatórias do Turno 1)*

### Arquivo gerado
| Nome do arquivo | Descrição |
|-----------------|-----------|
| `POL-001-politica-devolucao-resumido.md` | Resumo curado da política de devolução com metadados estruturados. Confiabilidade marcada como **Alta**. |

---

## Turno 3 — Resumo da PROC-042-v1

### Documento de entrada
| Campo | Valor |
|-------|-------|
| Nome | `PROC-042-frete-especial-v1.md` |
| Tipo | Procedimento operacional |
| Conteúdo | Fórmula e parâmetros para cálculo de frete especial (versão 1.0, mar/2023) |

### Prompt
> Você é um Product Specialist com foco em curadoria de conhecimento para RAG e mapeamento de cobertura temática.
> Tarefa: A partir do documento-fonte, gerar um novo arquivo Markdown de resumo.
>
> **Entradas:**
> - Documento-fonte: `PROC-042-frete-especial-v1.md`
> - Nome do arquivo de saída: `PROC-042-frete-especial-v1-resumido.md`
>
> *(mesmas instruções obrigatórias do Turno 1)*

### Arquivo gerado
| Nome do arquivo | Descrição |
|-----------------|-----------|
| `PROC-042-frete-especial-v1-resumido.md` | Resumo curado da PROC-042 v1 com metadados estruturados. Confiabilidade marcada como **Baixa-média** por coexistência com v2 sem hierarquia formal. |

---

## Turno 4 — Resumo da PROC-042-v2

### Documento de entrada
| Campo | Valor |
|-------|-------|
| Nome | `PROC-042-v2-frete-especial-revisado.md` |
| Tipo | Procedimento operacional (revisão) |
| Conteúdo | Parâmetros atualizados de cálculo de frete especial (versão 2.0, nov/2023) |

### Prompt
> Você é um Product Specialist com foco em curadoria de conhecimento para RAG e mapeamento de cobertura temática.
> Tarefa: A partir do documento-fonte, gerar um novo arquivo Markdown de resumo.
>
> **Entradas:**
> - Documento-fonte: `PROC-042-v2-frete-especial-revisado.md`
> - Nome do arquivo de saída: `PROC-042-v2-frete-especial-revisado1-resumido.md`
>
> *(mesmas instruções obrigatórias do Turno 1)*

### Arquivo gerado
| Nome do arquivo | Descrição |
|-----------------|-----------|
| `PROC-042-v2-frete-especial-revisado1-resumido.md` | Resumo curado da PROC-042 v2 com metadados estruturados. Confiabilidade marcada como **Média** por ausência de revogação formal da v1 e dependência da PROC-043 em revisão. |

---

## Turno 5 — Resumo da SLA-2024

### Documento de entrada
| Campo | Valor |
|-------|-------|
| Nome | `SLA-2024-tabela-sla-clientes.md` |
| Tipo | Documento contratual |
| Conteúdo | Tabela de SLAs por tier de cliente com penalidades e metodologia de medição (versão 2024.1, jan/2024) |

### Prompt
> Você é um Product Specialist com foco em curadoria de conhecimento para RAG e mapeamento de cobertura temática.
> Tarefa: A partir do documento-fonte, gerar um novo arquivo Markdown de resumo.
>
> **Entradas:**
> - Documento-fonte: `SLA-2024-tabela-sla-clientes.md`
> - Nome do arquivo de saída: `SLA-2024-tabela-sla-clientes-resumido.md`
>
> *(mesmas instruções obrigatórias do Turno 1)*

### Arquivo gerado
| Nome do arquivo | Descrição |
|-----------------|-----------|
| `SLA-2024-tabela-sla-clientes-resumido.md` | Resumo curado da tabela de SLA com metadados estruturados. Confiabilidade marcada como **Alta** — documento contratual com dois responsáveis formais. |

---

## Turno 6 — Mapa de temas e hipóteses de gaps (corpus completo)

### Documentos de entrada
| Nome do arquivo | Tipo |
|-----------------|------|
| `FAQ-atendimento-resumido.md` | Resumo curado — Turno 1 |
| `POL-001-politica-devolucao-resumido.md` | Resumo curado — Turno 2 |
| `PROC-042-frete-especial-v1-resumido.md` | Resumo curado — Turno 3 |
| `PROC-042-v2-frete-especial-revisado1-resumido.md` | Resumo curado — Turno 4 |
| `SLA-2024-tabela-sla-clientes-resumido.md` | Resumo curado — Turno 5 |

### Prompt
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

### Arquivo gerado
| Nome do arquivo | Descrição |
|-----------------|-----------|
| `entrega-final.md` | Mapa completo de temas cobertos, sobreposições, 10 hipóteses de gaps com evidências e perguntas para discovery, e resumo executivo para negócio. |

---

## Turno 7 — Análise de inconsistências PROC-042-v1 × PROC-042-v2

### Documentos fornecidos
- `PROC-042-frete-especial-v1.md`
- `PROC-042-v2-frete-especial-revisado.md`

### Prompt do usuário

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
> ID | Tipo | Trecho no PROC-042-v2 | Trecho na POL-001 | Descrição da inconsistência | Recomendação | Responsável sugerido

### Arquivo gerado

**`analise-de-inconsistencia.md`**

Conteúdo produzido:
- Resumo executivo com 6 inconsistências identificadas
- Matriz de inconsistências com 6 entradas

| ID | Tipo | Descrição resumida |
|----|------|--------------------|
| INC-001 | Regra conflitante — valor numérico | Fatores de peso divergentes para faixas acima de 1.000kg (v1: 1.2/1.5 vs v2: 1.15/1.4) |
| INC-002 | Regra duplicada com valores diferentes | Todos os 5 multiplicadores regionais divergem entre v1 e v2 |
| INC-003 | Fluxo operacional divergente | Prazo adicional de manuseio: +2 dias úteis (v1) vs +3 dias úteis (v2) |
| INC-004 | Regra conflitante — condição de desconto | Limiares e mecanismos de desconto por volume incompatíveis entre as versões |
| INC-005 | Condição de exceção ausente em um documento | Alerta de revisão da PROC-043 presente apenas no v2 |
| INC-006 | Ausência de hierarquia documental | Nenhum dos documentos declara formalmente qual versão está vigente |

---

## Turno 8 — Análise de inconsistências documentação formal × FAQ

### Documentos fornecidos
- `mapa-de-temas-cobertos-e-hipoteses-de-gaps.md`
- `analise-de-inconsistencia.md` *(gerado no Turno 1)*
- `FAQ-atendimento.md`

### Prompt do usuário

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

### Arquivo gerado

**`analise-de-inconsistencia-vs-FAQ.md`**

Conteúdo produzido:
- Resumo executivo (7 linhas)
- Tabela de inconsistências com 7 entradas
- Top 3 riscos prioritários detalhados
- 6 perguntas para validação com stakeholders

| ID | Tema | Tipo |
|----|------|------|
| FAQ-INC-001 | Devolução de carga perigosa | Contradição direta com POL-001 |
| FAQ-INC-002 | Critério de uso da PROC-042 v1 vs v2 | Prática sem respaldo formal completo |
| FAQ-INC-003 | Desconto por volume — limiar e mecanismo | Regra híbrida inexistente em qualquer documento |
| FAQ-INC-004 | Seguro de carga — percentuais | Prática sem documento normativo de suporte |
| FAQ-INC-005 | SLA por tier | Regra formal reproduzida de forma incompleta |
| FAQ-INC-006 | Frete expresso de carga perigosa | Fluxo operacional sem PROC vigente disponível |
| FAQ-INC-007 | Critério de R$ 50.000 para rastreamento | Critério empírico sem base normativa identificada |

