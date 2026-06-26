# Jornada do Atendente com Assistente de IA

---

## 1. Visão geral da jornada

O assistente de IA atua como uma camada de consulta documental posicionada entre o atendente e a base de conhecimento da NovaTech. Quando o atendente recebe uma dúvida do cliente, ele formula a pergunta no assistente em linguagem natural, sem precisar saber em qual documento a resposta está. O assistente interpreta a intenção da pergunta, recupera os trechos mais relevantes da base documental indexada e responde indicando a fonte. O atendente usa essa resposta para orientar o cliente de forma rápida e rastreável. Nenhuma resposta é gerada sem base documental: se o assistente não encontrar evidência suficiente, ele sinaliza o limite e aciona o fluxo de fallback. O objetivo é reduzir o tempo de consulta de 12 minutos para menos de 2 minutos, eliminando a necessidade de abrir múltiplas fontes manualmente.

---

## 2. Fluxo principal

**Passo 1 — Dúvida do cliente**
O cliente entra em contato com uma dúvida operacional. Exemplos frequentes: prazo de entrega de carga especial, regra de frete por volume, política de devolução, SLA de atendimento.

**Passo 2 — Atendente consulta o assistente**
O atendente digita a pergunta no assistente em linguagem natural. Exemplo: *"Qual o prazo de entrega para carga perigosa classe 3 enviada para a região Sul?"*

**Passo 3 — Assistente interpreta a pergunta**
O assistente identifica as entidades relevantes (tipo de carga, região, prazo) e formula internamente as queries de recuperação para a base documental.

**Passo 4 — Assistente recupera conteúdo relevante**
O assistente busca os trechos mais aderentes nos documentos indexados (ex.: PROC-042-v2, SLA-2024, POL-001) e ranqueia as evidências por relevância e versão vigente.

**Passo 5 — Assistente responde com fonte**
A resposta é apresentada ao atendente com: (a) a orientação em linguagem clara, (b) o trecho do documento que fundamenta a resposta, (c) o nome e versão do documento fonte, (d) nível de confiança da resposta (alto / médio / baixo).

**Passo 6 — Atendente usa a resposta no atendimento**
O atendente repassa a informação ao cliente. Se necessário, o atendente pode exibir o trecho fonte ao cliente como respaldo. O atendimento é concluído com rastreabilidade.

---

## 3. Fluxo de fallback

O fluxo de fallback é acionado sempre que o assistente não consegue fornecer uma resposta confiável e rastreável. Cada situação tem encaminhamento específico.

### 3.1 — Assistente não encontra resposta confiável
**Gatilho:** Nenhum documento indexado cobre o tema da pergunta ou a confiança da recuperação é baixa.
**Ação:** O assistente exibe a mensagem: *"Não encontrei evidência documental suficiente para responder a esta pergunta. Recomendo escalonamento."*
**Encaminhamento:** Supervisor de atendimento → registra o caso como gap de cobertura documental.

### 3.2 — Conflito entre documentos
**Gatilho:** O assistente identifica trechos contraditórios em dois ou mais documentos sobre o mesmo tema (ex.: PROC-042-v1 vs. PROC-042-v2 com limiares de desconto diferentes).
**Ação:** O assistente exibe os dois trechos conflitantes com identificação de versão e alerta: *"Há conflito entre documentos. Não é possível escolher automaticamente a versão correta."*
**Encaminhamento:** Supervisor → Operações (conflitos de procedimento) ou Comercial (conflitos de regra comercial). O assistente nunca escolhe silenciosamente a versão.

### 3.3 — Resposta incompleta
**Gatilho:** O assistente encontra parte da informação, mas a resposta não cobre todos os elementos da dúvida (ex.: encontra o prazo, mas não a condição de exceção).
**Ação:** O assistente entrega a parte encontrada, sinalizando: *"Resposta parcial — os seguintes aspectos da pergunta não foram cobertos pelos documentos disponíveis: [lista]."*
**Encaminhamento:** Supervisor → identifica se o gap é de indexação ou de ausência documental.

### 3.4 — Atendente discorda da resposta
**Gatilho:** O atendente reconhece que a resposta do assistente contraria a prática operacional que ele aplica no dia a dia.
**Ação:** O atendente aciona o botão "Discordo desta resposta" e registra o motivo em campo livre.
**Encaminhamento:** O caso entra na fila de revisão de conteúdo. Nenhuma resposta é alterada automaticamente com base no feedback individual — a revisão passa por validação humana.

### 3.5 — Caso exige validação humana obrigatória
**Gatilho:** Temas sensíveis identificados como de alto risco na análise de inconsistências: carga perigosa, exceções a políticas formais, descontos fora da tabela, autorizações de Compliance.
**Encaminhamento por tipo de dúvida:**
- Devolução de carga perigosa → **Gestão de Riscos** (ramal 4500, somente para triagem; a autorização formal deve ser documentada em POL vigente)
- Desconto por volume acima da tabela → **Comercial**
- Autorização de frete expresso de carga perigosa → **Compliance** (aguardar PROC-043 vigente)
- SLA em casos limítrofes → **Supervisor**, com consulta direta à SLA-2024
- Seguro de carga com percentual divergente → **Comercial** (percentuais do FAQ não têm respaldo normativo formal)

---

## 4. Fluxo de feedback

O fluxo de feedback permite que os atendentes contribuam ativamente para a melhoria contínua da base de conhecimento e do comportamento do assistente.

### 4.1 — Como o atendente aciona o feedback
Ao final de cada consulta, o atendente pode avaliar a resposta por meio de uma das categorias:

| Categoria | Exemplo de uso |
|-----------|----------------|
| **Resposta errada** | O assistente indicou prazo incorreto ou regra revogada |
| **Resposta desatualizada** | O documento fonte existe, mas foi substituído por versão mais recente |
| **Resposta incompleta** | Faltou condição de exceção ou critério relevante |
| **Fonte insuficiente** | A resposta veio de FAQ informal sem respaldo em POL ou PROC |
| **Conflito com a prática** | A regra documentada contradiz o que o time opera hoje |

Cada feedback inclui: categoria, descrição livre do atendente, ID do chamado e documento fonte indicado pelo assistente.

### 4.2 — O que acontece com o feedback
1. **Triagem automática:** feedbacks das categorias "errado" e "conflito com a prática" são priorizados automaticamente para revisão.
2. **Revisão humana:** equipe responsável pela base de conhecimento analisa o feedback, identifica se é gap de indexação, versão desatualizada ou ausência de documento formal.
3. **Atualização documental:** se o problema for na fonte (documento desatualizado ou ausente), o caso é encaminhado para a área responsável pelo documento (Operações, Compliance ou Comercial) para atualização oficial.
4. **Atualização do índice:** após validação, o documento atualizado é reindexado e o comportamento do assistente é corrigido na próxima janela de atualização.
5. **Fechamento do ciclo:** o atendente que abriu o feedback recebe confirmação de que o problema foi tratado ou descartado com justificativa.

### 4.3 — Periodicidade
- Feedbacks críticos (resposta errada em temas de alto risco): revisão em até **24 horas**.
- Feedbacks de conteúdo desatualizado: revisão no ciclo **semanal**.
- Gaps documentais (ausência de POL ou PROC): encaminhados para discovery no ciclo **mensal**.

---

## 5. Guardrails do assistente

Os guardrails abaixo são específicos ao domínio de logística, atendimento e RAG documental da NovaTech. Não são regras genéricas — são restrições derivadas das inconsistências identificadas na análise documental.

### Guardrail 1 — Nunca escolher silenciosamente entre versões contraditórias de procedimento
Se dois documentos indexados contiverem regras incompatíveis sobre o mesmo tema (exemplo: PROC-042-v1 com limiar de 10 fretes/mês e PROC-042-v2 com limiar de 8 fretes/mês), o assistente não deve selecionar automaticamente uma das versões sem alertar o atendente. A resposta deve expor o conflito, apresentar os dois trechos com identificação de versão e acionar o fluxo de fallback 3.2. A escolha da versão vigente é responsabilidade humana — não do assistente.

### Guardrail 2 — Nunca usar FAQ informal como única fonte quando houver política formal conflitante
O FAQ de atendimento pode conter orientações práticas não respaldadas por POL ou PROC vigentes (exemplo: percentuais de seguro de carga, critério de R$ 50.000 para priorização, fluxo de autorização de frete expresso). O assistente deve sempre cruzar a resposta do FAQ com a documentação formal disponível. Se houver divergência, a resposta deve indicar a fonte formal como referência prioritária e sinalizar que a orientação do FAQ diverge. Se não existir documento formal sobre o tema, o assistente deve declarar explicitamente a ausência de base normativa — nunca validar o FAQ como autoridade isolada.

### Guardrail 3 — Nunca criar expectativa de exceção a uma política categórica sem respaldo documental
Para temas classificados como inelegíveis por política formal sem ressalva de exceção (exemplo: devolução de cargas perigosas classes 1–6 conforme POL-001), o assistente não deve formular a resposta de forma aberta ou condicional (como "pode haver exceções dependendo da autorização"). Se a política é categórica, a resposta deve refletir isso. Qualquer exceção que o atendente queira explorar deve seguir o fluxo de fallback 3.5, com encaminhamento para a área responsável — nunca ser inferida pelo assistente a partir de práticas informais relatadas no FAQ.

### Guardrail 4 — Nunca responder sobre SLA sem incluir os critérios de medição e o tier do cliente
Respostas sobre prazo de resposta ou resolução de chamados devem sempre incluir: (a) o tier do cliente (Gold, Silver, Standard), (b) os valores correspondentes da SLA-2024, e (c) os critérios de medição aplicáveis (horário comercial, distinção entre chamado geral e incidente crítico). Respostas que apresentem apenas os valores numéricos de SLA sem esses critérios são consideradas incompletas pelo assistente, que deve complementar a resposta ou sinalizar a incompletude antes de entregá-la ao atendente.

### Guardrail 5 — Nunca inferir prazo de entrega sem identificar versão de documento e data de vigência
Prazos de entrega dependem de versões específicas de procedimentos que podem ter sido revisados. O assistente deve sempre indicar: (a) de qual documento e versão o prazo foi extraído, (b) se há outra versão do mesmo documento indexada na base, (c) a data de vigência declarada no documento, quando disponível. Se a versão vigente não puder ser determinada com certeza, o assistente deve sinalizar a ambiguidade antes de fornecer o prazo — nunca entregar um valor de prazo sem rastreabilidade de fonte e versão.

---

*Documento gerado com base em: `entrega-final.md` e `analise-de-inconsistencia-vs-FAQ.md`*
*Data: 01/06/2026 | Projeto: NovaTech — Intent + Discovery*
