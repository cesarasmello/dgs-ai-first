# Evidência de Uso de IA — Avaliação do Assistente NovaTech

**Projeto:** NovaTech — Assistente de Atendimento (RAG)  
**Atividade:** Avaliação de qualidade de respostas em staging + diagnóstico de erros e ajustes de produto  
**Ferramenta:** Claude (Anthropic) — interface claude.ai  
**Papel:** Product Specialist  
**Data da sessão:** 2025-01  

---

## Sobre este documento

Este documento registra a sessão completa de uso da IA como evidência de aplicação da ferramenta no contexto do trabalho de produto. Estão reproduzidos os prompts enviados, os documentos utilizados como insumo e as saídas geradas pelo modelo em cada etapa da conversa.

---

## Documentos utilizados como insumo

### Documento 1 — Fonte de verdade

**Arquivo:** `anexo-a-documentacao-simulada-novatech.md`  
**Conteúdo:** Documentação oficial simulada da NovaTech, composta por 5 subdocumentos:

| ID | Nome | Tipo |
|----|------|------|
| POL-001 | Política de Devolução de Mercadorias (v3.1) | Normativo |
| PROC-042 | Procedimento de Cálculo de Frete Especial (v1.0) | Normativo |
| PROC-042-v2 | Procedimento de Cálculo de Frete Especial Revisado (v2.0) | Normativo |
| SLA-2024 | Tabela de SLA por Tipo de Cliente (v2024.1) | Contratual |
| FAQ-Atendimento | Perguntas Frequentes do Time de Suporte | Informal (não validado) |

### Documento 2 — Objeto de avaliação

**Arquivo:** `Perguntas-e-resposatas.md`  
**Conteúdo:** 6 pares pergunta/resposta gerados pelo assistente em ambiente de staging, com indicação de fonte citada e nível de confiança autodeclarado.

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|----------|--------------|-----------|
| 1 | "Qual o prazo de devolução para produtos standard?" | "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." | POL-001, seção 3.2 | Alta |
| 2 | "Meu cliente é Silver. Qual o prazo de resolução?" | "O cliente Silver tem resolução em até 48h." | SLA-2024 | Alta |
| 3 | "Posso devolver carga perigosa classe 3?" | "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." | POL-001, seção 3.2 | Alta |
| 4 | "Qual a política para carga danificada durante transporte?" | "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." | Nenhuma | Alta |
| 5 | "Qual o SLA do cliente Enterprise?" | "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." | — | Baixa |
| 6 | "Posso enviar carga perigosa com frete expresso?" | "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." | FAQ-Atendimento, item 32 | Alta |

---

## Interação 1 — Avaliação de qualidade dos pares pergunta/resposta

### Prompt enviado

> Haja como Product Specialist.
> Sua tarefa é avaliar cada par de pergunta e resposta do documento de perguntas e respostas usando a documentação simulada como única fonte de verdade.
>
> Objetivo:
> Classificar cada resposta como:
> - Correta
> - Parcialmente correta
> - Incorreta
> e justificar
>
> Instruções:
> 1. Leia todas as perguntas e respostas do documento de perguntas e respostas.
> 2. Compare cada resposta com a documentação anexada: anexo-a-documentacao-simulada-novatech.
> 3. Para cada item, determine se a resposta está totalmente aderente, parcialmente aderente ou incorreta.
> 4. Justifique a classificação com base no conteúdo da documentação.
> 5. Quando possível, cite o trecho, seção, tópico ou evidência que sustenta sua análise.
> 6. Se a resposta trouxer informação não encontrada na documentação, classifique como incorreta por falta de evidência.
> 7. Se a resposta estiver em parte correta, mas incompleta, ambígua ou com detalhe divergente, classifique como parcialmente correta.
> 8. Não invente informação. Se a documentação não permitir confirmar algo, deixe isso explícito.
>
> Formato de saída:
> Apresente a análise em tabela com as colunas:
> - Nº
> - Pergunta
> - Resposta avaliada
> - Classificação
> - Justificativa
> - Evidência na documentação

**Arquivos anexados ao prompt:**
- `anexo-a-documentacao-simulada-novatech.md`
- `Perguntas-e-resposatas.md`

---

### Saída gerada pelo modelo

| Nº | Pergunta | Resposta avaliada | Classificação | Justificativa | Evidência na documentação |
|----|----------|-------------------|---------------|---------------|--------------------------|
| 1 | "Qual o prazo de devolução para produtos standard?" | "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." | **Parcialmente correta** | O prazo de 7 dias úteis e a abertura de chamado no portal estão corretos. No entanto, a resposta omite requisitos obrigatórios do chamado: além das fotos, é exigido o número do CT-e e a especificação do motivo da devolução. A omissão desses itens pode levar o cliente a abrir um chamado incompleto, causando retrabalho. | POL-001, seção 3.1 (prazo) e seção 3.3, itens 1 e 2 (procedimento completo: Portal, CT-e, mínimo 3 fotos com especificação das vistas, motivo) |
| 2 | "Meu cliente é Silver. Qual o prazo de resolução?" | "O cliente Silver tem resolução em até 48h." | **Parcialmente correta** | O prazo de 48h está correto, mas apenas para chamados gerais. A resposta não distingue o tipo de chamado: para incidentes críticos, o SLA de resolução do Silver é de até 8h — prazo significativamente diferente. A resposta sem essa distinção pode criar expectativa incorreta tanto para o atendente quanto para o cliente. | SLA-2024, seção 2, Tabela de SLAs: "Tempo de resolução (chamados gerais) — Silver: Até 48h úteis" e "Tempo de resolução (incidentes críticos) — Silver: Até 8h" |
| 3 | "Posso devolver carga perigosa classe 3?" | "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." | **Parcialmente correta** | A inelegibilidade para o processo padrão está correta. A restrição às classes 1 a 6 também está correta. Porém, o encaminhamento está impreciso: a documentação determina que o cliente deve contatar o setor de Gestão de Riscos pelo ramal 4500, não "escalar para o supervisor", que é um direcionamento interno e não o canal correto para o cliente. | POL-001, seção 3.2: "o cliente deve entrar em contato com o setor de Gestão de Riscos (ramal 4500) para tratamento individual" |
| 4 | "Qual a política para carga danificada durante transporte?" | "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." | **Incorreta** | Não existe documento formal (POL ou PROC) na base de conhecimento que trate de carga danificada em trânsito. A única referência disponível é o FAQ-Atendimento (item 38), que é um documento informal, não validado pelo Compliance ou Operações, e que descreve um processo diferente do apresentado (exige registro em 48h, encaminhamento para sinistros@novatech.com.br e envolvimento do Jurídico). A resposta apresentou informação como "política" com confiança alta sem base em documento normativo. | FAQ-Atendimento, item 38 (único registro disponível, informal); Notas da documentação, "Gaps identificados", item 1 |
| 5 | "Qual o SLA do cliente Enterprise?" | "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." | **Correta** | A resposta identifica corretamente a ausência do tier Enterprise, lista os três tiers existentes com precisão e orienta adequadamente. Alinhada ao comportamento esperado de um assistente RAG bem calibrado: não inventa informação ausente e sinaliza incerteza com confiança baixa. | SLA-2024, seção 1: "A NovaTech classifica seus clientes em 3 (três) tiers" (Gold, Silver, Standard) e nota: "Não existem outros tiers além dos três listados acima." |
| 6 | "Posso enviar carga perigosa com frete expresso?" | "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." | **Incorreta** | Embora o FAQ-Atendimento (item 32) mencione essa prática, trata-se de um documento explicitamente classificado como informal, não validado pelo Compliance ou Operações. Não existe nenhum PROC ou POL formal que defina esse processo. A resposta foi apresentada com confiança alta e sem qualquer ressalva, o que é inadequado para uma informação sem respaldo normativo. | FAQ-Atendimento, item 32 (única fonte, informal); Notas da documentação, "Contradições identificadas", item 4 |

**Resumo executivo gerado pelo modelo:**

| Classificação | Qtd | Itens |
|---|---|---|
| Correta | 1 | #5 |
| Parcialmente correta | 3 | #1, #2, #3 |
| Incorreta | 2 | #4, #6 |

> Padrão de risco identificado: os erros mais graves (#4 e #6) têm origem na mesma raiz — o assistente tratou o FAQ-Atendimento como fonte normativa equivalente a POL/PROC/SLA, ignorando o aviso explícito de que o FAQ é informal e não validado. Isso indica necessidade de ajuste no pipeline de RAG, seja por metadado de confiabilidade por documento, seja por instrução explícita nos guardrails para tratar fontes informais com confiança degradada. O item #3 aponta falha de encaminhamento que pode comprometer a experiência do cliente diretamente.

---

## Interação 2 — Análise de erros e ajustes de produto

### Prompt enviado

> Para cada resposta classificada como "parcialmente correta" ou "incorreta":
> 1. Classifique o tipo principal de erro em uma destas categorias:
> - Alucinação: a resposta inventa informação não sustentada pela documentação.
> - Fonte não confiável: a resposta se apoia em fonte inadequada, informal, desatualizada ou menos confiável do que a fonte esperada.
> - Informação incompleta: a resposta traz apenas parte da informação correta, omite condição importante ou deixa margem para interpretação errada.
> 2. Justifique a classificação do erro com base na documentação fornecida.
> 3. Proponha um ajuste de produto para reduzir a recorrência desse erro, escolhendo uma das frentes abaixo:
> - Prompt: mudança nas instruções dadas ao modelo.
> - Interface: mudança na forma como a resposta, fonte, alerta ou confirmação é apresentada ao usuário.
> - Pipeline: mudança no fluxo de recuperação, validação, filtragem, ranking, ou bloqueio da resposta.
> 4. Explique objetivamente por que o ajuste proposto ajuda a prevenir esse tipo de erro.

---

### Saída gerada pelo modelo

#### Item 1 — Parcialmente correta | Informação incompleta | Ajuste: Prompt

A resposta informa corretamente o prazo e o canal de abertura, mas omite dois campos obrigatórios do chamado: o número do CT-e e o motivo da devolução. A POL-001, seção 3.3, item 2, lista três requisitos explícitos. A resposta cobre apenas um deles (fotos).

**Ajuste:** Incluir instrução no system prompt determinando que, ao responder sobre procedimentos com múltiplos requisitos obrigatórios, o assistente deve reproduzir a lista completa conforme documentada, sem omitir itens.

> *"Quando a resposta envolver um procedimento com múltiplos requisitos obrigatórios, liste todos os itens conforme a documentação. Não resuma ou omita campos."*

---

#### Item 2 — Parcialmente correta | Informação incompleta | Ajuste: Pipeline

A resposta apresenta o SLA de 48h correto para chamados gerais, mas omite que incidentes críticos têm resolução de 8h para o mesmo tier — seis vezes menor, com impacto contratual direto.

**Ajuste:** Implementar etapa de detecção de ambiguidade no pipeline: quando a pergunta envolver termo com múltiplas classificações na documentação, recuperar e injetar todos os chunks relevantes antes da geração. Adicionar metadado de cobertura nos chunks do SLA-2024 para que `Silver + resolução` recupere obrigatoriamente as duas linhas da tabela.

---

#### Item 3 — Parcialmente correta | Informação incompleta | Ajuste: Prompt

A restrição está correta, mas o encaminhamento indica "escalar para o supervisor" em vez do canal documentado: Gestão de Riscos, ramal 4500. Para o cliente, a diferença é concreta.

**Ajuste:** Instrução no system prompt para que, em situações de inelegibilidade, o assistente sempre inclua o canal alternativo documentado (nome do setor, ramal ou e-mail), sem substituir por generalizações de escalação.

> *"Ao informar que uma solicitação não é elegível pelo processo padrão, sempre informe o canal alternativo documentado (nome do setor, ramal ou e-mail), se disponível na fonte."*

---

#### Item 4 — Incorreta | Fonte não confiável | Ajuste: Pipeline

A resposta apresentou como "política" uma informação sustentada apenas pelo FAQ informal (item 38), que não foi validado por Compliance ou Operações. A própria resposta diverge do FAQ utilizado como fonte: omitiu o prazo de 48h para registro, o e-mail sinistros@novatech.com.br e o envolvimento do Jurídico.

**Ajuste:** Classificar documentos por nível de confiabilidade na ingestão (`normativo`, `contratual`, `informal`). Quando a única fonte for `informal`, o pipeline deve: (a) rebaixar o score de confiança; (b) bloquear geração de resposta afirmativa sobre políticas ou processos; (c) forçar inclusão de aviso ao atendente.

---

#### Item 6 — Incorreta | Fonte não confiável | Ajuste: Interface

Mesmo padrão do item 4: resposta afirmativa com confiança alta baseada exclusivamente no FAQ informal, sem nenhum documento normativo sustentando o processo. Cenário de maior risco: carga perigosa + processo não documentado + confiança alta.

**Ajuste:** Exibir componente visual de alerta ao atendente (não ao cliente) sempre que a fonte for `informal` ou inexistente no corpus normativo, contendo: classificação da fonte, aviso de ausência de respaldo normativo e instrução de confirmar com a área responsável antes de repassar ao cliente.

---

**Consolidado gerado pelo modelo:**

| Item | Tipo de erro | Frente de ajuste |
|---|---|---|
| 1 | Informação incompleta | Prompt |
| 2 | Informação incompleta | Pipeline |
| 3 | Informação incompleta | Prompt |
| 4 | Fonte não confiável | Pipeline |
| 6 | Fonte não confiável | Interface |

> Os dois ajustes de Pipeline (itens 2 e 4) são complementares e implementáveis de forma integrada: a infraestrutura de metadados de confiabilidade por documento habilita, sem custo adicional, a lógica de cobertura completa por tipo de chamado.

---

## Interação 3 — Geração do documento markdown

### Prompt enviado

> aplique a saída em um documento markdown

---

### Saída gerada pelo modelo

Arquivo gerado: `analise-erros-ajustes-produto-novatech.md`

Estrutura do documento produzido:

```
# Análise de Erros e Ajustes de Produto — Assistente NovaTech
  - Cabeçalho com metadados (projeto, escopo, fonte de verdade, data)
  - Sumário executivo (tabela consolidada)
  - Seção por item (1, 2, 3, 4, 6):
    - Classificação e tipo de erro
    - Justificativa do erro com evidência citada
    - Ajuste proposto (frente + instrução de exemplo)
    - Explicação do mecanismo de prevenção
  - Padrão consolidado de erros (distribuição + análise de raiz)
  - Recomendação de implementação integrada dos ajustes de pipeline
```

---

## Arquivos gerados nesta sessão

| Arquivo | Tipo | Descrição |
|---------|------|-----------|
| `analise-erros-ajustes-produto-novatech.md` | Markdown | Análise completa de erros e ajustes de produto |
| `evidencia-uso-ia-novatech.md` | Markdown | Este documento — registro da sessão como evidência de uso da IA |

---

## Síntese do uso da IA nesta sessão

| Aspecto | Detalhe |
|---------|---------|
| **Papel assumido pelo modelo** | Product Specialist — avaliação de qualidade de assistente RAG |
| **Documentos fornecidos como contexto** | 2 arquivos (fonte de verdade + objeto de avaliação) |
| **Interações realizadas** | 3 prompts encadeados |
| **Outputs produzidos** | Tabela de avaliação com 6 itens classificados + análise de erro para 5 itens + documento markdown estruturado |
| **Técnicas de prompt aplicadas** | Persona (role assignment), instrução por etapas numeradas, definição de formato de saída, restrição de fonte (única fonte de verdade), encadeamento de contexto entre interações |
| **Valor entregue** | Diagnóstico rastreável de qualidade do assistente em staging, com classificação de erros, evidências documentais e propostas de ajuste por frente (Prompt / Pipeline / Interface) prontas para backlog de produto |
