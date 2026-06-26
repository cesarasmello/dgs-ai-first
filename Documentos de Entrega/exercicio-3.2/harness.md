# Harness de Produto — Assistente de IA NovaTech
**Documento:** Governança e Validação para Melhoria Contínua  
**Contexto:** Pós-go-live do assistente RAG NovaTech (staging → produção ativo)  
**Restrição obrigatória:** Guardrails G-01 a G-14 (`guardrails-assistente-novatech.md` v1.0) são imutáveis durante todo o ciclo de evolução.  
**Versão:** 1.0  
**Data:** 25/06/2026  

---

## 1. Objetivo do Harness

Este harness define o mecanismo contínuo de governança do assistente de IA da NovaTech após o go-live. Seu propósito não é ser um checklist de release pontual, mas estabelecer o sistema operacional pelo qual toda mudança — de qualquer origem e magnitude — é capturada, diagnosticada, testada, aprovada e monitorada sem degradar a qualidade das respostas nem violar os guardrails definidos no Cenário 2.2.

O harness opera sobre três eixos:

1. **Feedback → diagnóstico → melhoria**: transformar sinais operacionais em ações rastreáveis.
2. **Regression testing**: garantir que o que funcionava continue funcionando após qualquer mudança.
3. **Human-in-the-loop**: definir com precisão onde o julgamento humano é insubstituível e quem o exerce.

---

## 2. Princípios de Governança do Produto

**P-01 — Guardrails são imutáveis por padrão.**  
Nenhuma mudança de prompt, pipeline ou documentação pode relaxar ou remover um guardrail existente sem processo formal de revisão com aprovação de Compliance e Product Owner. Adicionar guardrails é permitido a qualquer momento.

**P-02 — Toda mudança é tratada como potencial causa de regressão.**  
Mudanças em sistemas de IA têm efeitos colaterais não lineares. Alterar um prompt pode melhorar respostas de devolução e degradar respostas de SLA. O harness pressupõe isso e exige evidência de não-regressão antes de qualquer promoção a produção.

**P-03 — Causa raiz antes de correção.**  
Nenhuma mudança é implementada sem diagnóstico documentado de causa raiz. Correções aplicadas sem diagnóstico são proibidas — criam dívida de governança e opacidade nos logs de auditoria.

**P-04 — Rastreabilidade completa.**  
Cada problema identificado gera um item rastreável (ticket) que percorre o fluxo do harness do início ao fim. O ciclo se fecha somente quando a melhoria está em produção monitorada e o ticket é encerrado com evidência.

**P-05 — Separação de responsabilidades por papel.**  
Produto define o que mudar. QA valida se a mudança não quebra nada. Tech Lead aprova a implementação técnica. Compliance aprova mudanças que tocam em guardrails normativos. Operações executa o deploy e monitora pós-produção.

---

## 3. Fluxo de Feedback Ponta a Ponta

### 3.1 Captura de Feedback

O feedback chega por três canais:

**Canal A — Feedback estruturado inline (atendente):**  
Ao final de cada resposta do assistente, o atendente pode acionar um botão de avaliação com opções: ✓ Correta | ✗ Incorreta | ⚠ Incompleta | ⚠ Tom inadequado. Se negativo, um campo obrigatório solicita: (a) o que estava errado, (b) qual seria a resposta esperada, e (c) o DOC-ID que suportaria a resposta correta (quando aplicável). Esse feedback é armazenado com log completo: ID da conversa, timestamp, pergunta do cliente, resposta gerada, chunks recuperados e metadados de versão documental.

**Canal B — Escalada de atendimento (operações):**  
Quando um atendente encaminha uma conversa a atendimento humano por insatisfação com o assistente, o sistema registra automaticamente o motivo da escalada e vincula ao log da conversa. Essa escalada é tratada como feedback implícito negativo de alta prioridade.

**Canal C — Auditoria periódica (produto/QA):**  
Semanalmente, Produto e QA amostram aleatoriamente 50 conversas dos últimos 7 dias para avaliação cega — sem ver o feedback do atendente previamente. Casos com discrepância entre avaliação humana e avaliação do atendente são sinalizados para calibração.

### 3.2 Categorização

Todo feedback negativo é categorizado em uma das seguintes classes antes de prosseguir para diagnóstico:

| Código | Categoria | Descrição |
|--------|-----------|-----------|
| F-01 | Erro normativo | Resposta com prazo, valor, multiplicador ou elegibilidade incorretos |
| F-02 | Violação de guardrail | Resposta que transgride G-01 a G-14 (detectada por humano ou por validador) |
| F-03 | Falso negativo de recuperação | Assistente declarou "não encontrei" quando a informação existia na base |
| F-04 | Versionamento incorreto | Resposta usou versão desatualizada de documento |
| F-05 | Registro inadequado | Resposta em tom coloquial, informal ou incompatível com o padrão institucional |
| F-06 | Encaminhamento omitido | Caso que deveria ter sido escalado ao ramal/canal competente não foi |
| F-07 | Alucinação factual | Número, tier ou condição sem respaldo em nenhum chunk recuperado |
| F-08 | Cobertura ausente | Tema não coberto pela base; comportamento do assistente foi inadequado ao declarar ausência |

A categorização é feita pelo analista de Operações que recebe o ticket. Em caso de dúvida entre F-01 e F-02, prevalece F-02 (mais restritivo).

### 3.3 Diagnóstico de Causa Raiz

A causa raiz é apurada seguindo a hierarquia abaixo, nessa ordem:

**Nível 1 — Falha de recuperação (pipeline):**  
Verificar nos logs se o chunk com a resposta correta foi recuperado na consulta. Se não foi, investigar: score de similaridade abaixo do limiar? Sinônimo ausente no glossário? Índice desatualizado? Metadado de versão incorreto?  
→ Ação: ajuste de retriever, expansão de sinônimos, reindexação ou correção de metadados.

**Nível 2 — Falha de seleção de versão:**  
Verificar se o filtro de versionamento (G-03) foi acionado corretamente. O chunk recuperado era da versão correta para o contexto do chamado?  
→ Ação: ajuste no filtro de versionamento do pipeline (Tech Lead).

**Nível 3 — Falha documental:**  
O chunk correto foi recuperado, mas o documento-fonte contém erro, está desatualizado ou tem ambiguidade não prevista?  
→ Ação: atualização do documento-fonte + reindexação. Requer aprovação do responsável pela documentação normativa (Compliance ou área de negócio).

**Nível 4 — Falha de prompt:**  
O chunk correto foi recuperado e está na versão certa, mas o modelo gerou resposta incorreta. A instrução de sistema é ambígua, insuficiente ou ausente para esse cenário?  
→ Ação: ajuste de prompt. Requer validação de QA e aprovação de Produto antes de staging.

**Nível 5 — Comportamento emergente do modelo:**  
O chunk foi recuperado, o prompt está correto, mas o modelo produziu resposta inesperada de forma não determinística. Nenhum dos níveis anteriores explica o erro.  
→ Ação: adição de guardrail determinístico (código) para cobrir o caso. Escalada a Tech Lead e Produto. Se o comportamento for recorrente, avaliar migração de versão de modelo.

### 3.4 Decisão de Ação por Causa Raiz

| Causa raiz identificada | Ação primária | Responsável |
|------------------------|---------------|-------------|
| Chunk não recuperado — limiar de score | Ajustar limiar ou estratégia de busca | Tech Lead |
| Sinônimo ausente no glossário | Adicionar sinônimo canônico ao glossário + reindexar | Produto + Tech Lead |
| Índice desatualizado | Reindexação do corpus | Tech Lead + Operações |
| Filtro de versionamento incorreto | Corrigir lógica do filtro no pipeline | Tech Lead |
| Documento-fonte desatualizado | Atualizar documento + reindexar | Compliance + área de negócio |
| Instrução de prompt insuficiente | Ajustar prompt de sistema | Produto (com revisão de QA) |
| Comportamento emergente do modelo | Adicionar guardrail determinístico | Tech Lead + Produto |
| Guardrail violado (qualquer causa) | Tratamento prioritário — ver Seção 5 | Produto + Compliance |

### 3.5 Validação Antes de Produção

Toda melhoria derivada de feedback percorre obrigatoriamente:

1. **Implementação em ambiente de desenvolvimento** (Tech Lead).
2. **Execução da suíte de regression testing** (QA) — ver Seção 4.
3. **Revisão de aprovação humana** quando aplicável — ver Seção 5.
4. **Deploy em staging** com execução dos casos de teste do feedback original para confirmar resolução.
5. **Soak period** de 48 horas em staging com monitoramento de métricas de qualidade.
6. **Aprovação formal de promoção** a produção (Produto + Tech Lead).
7. **Monitoramento pós-deploy** por 72 horas com alertas ativos.

---

## 4. Estratégia de Regression Testing de Produto

### 4.1 Estrutura da Suíte de Testes

A suíte é composta por quatro camadas, executadas em sequência a cada mudança:

**Camada 1 — Testes de guardrail (G-01 a G-14):**  
Para cada guardrail, existe ao menos um caso de teste positivo (o assistente deve respeitar) e um caso de teste adversarial (input projetado para induzir violação). Os guardrails determinísticos (G-01, G-03, G-04, G-06, G-07, G-09, G-10, G-12) têm verificação automatizada por validadores de pipeline. Os guardrails probabilísticos (G-02, G-05, G-08, G-11, G-13, G-14) têm avaliação humana por amostra na Camada 4.

**Camada 2 — Testes de regressão de domínio (golden set):**  
Um conjunto fixo de 80 casos de teste cobrindo os cenários mais frequentes e de maior risco: consultas de prazo de devolução (cargas comuns e perigosas), consultas de SLA por tier, consultas de multiplicador de frete por versão de PROC-042, e consultas fora da cobertura documental. Para cada caso, a resposta esperada está documentada com o DOC-ID e a seção que a suporta. Aprovação: 100% dos casos de Camada 1 e ≥ 95% dos casos de Camada 2.

**Camada 3 — Testes de não-regressão por categoria de mudança:**  
Dependendo do tipo de mudança (prompt, pipeline, reindexação, atualização documental), um subconjunto específico de casos é executado, conforme a matriz:

| Tipo de mudança | Camadas obrigatórias |
|-----------------|----------------------|
| Ajuste de prompt | 1, 2, 4 |
| Ajuste de pipeline (retriever, filtros) | 1, 2, 3 |
| Reindexação parcial | 1, 2 (casos do domínio reindexado) |
| Atualização documental | 1, 2 (casos do documento atualizado), 3 |
| Atualização de versão de modelo | 1, 2, 3, 4 (suíte completa) |

**Camada 4 — Avaliação humana por amostra:**  
Para mudanças de prompt ou de versão de modelo, QA avalia manualmente 30 respostas geradas em staging — 15 do golden set e 15 novos inputs — verificando especificamente os guardrails probabilísticos (tom, encaminhamentos, tratamento de ausência de informação, aplicação de disposições transitórias).

### 4.2 Casos de Teste Obrigatórios

Todo caso de teste deve ter os seguintes campos documentados: ID, categoria (guardrail | domínio | adversarial), input exato, DOC-ID esperado na citação, resposta esperada resumida, e critério de aprovação/reprovação.

Os tipos de caso obrigatórios são:

- **Casos de caminho feliz por domínio:** devolução de carga comum, SLA Gold/Silver/Standard, frete com PROC-042-v2, declaração de ausência para temas não cobertos.
- **Casos adversariais por guardrail:** input que tenta obter prazo de devolução para carga perigosa (G-02/G-07), input que menciona tier "Platinum" (G-09), consulta de SLA sem o termo "Gold" (G-04 — expansão de sinônimos), input que poderia gerar número sem base documental (G-06).
- **Casos de limite de versionamento:** chamado anterior a 01/12/2023 com PROC-042 v1, chamado posterior com v2, chamado em período transitório.
- **Casos de falha de recuperação controlada:** consulta com terminologia não canônica para SLA (G-04/G-10), consulta sobre tema coberto mas com formulação indireta.
- **Casos de encaminhamento:** carga perigosa sem categoria informada (G-14), tema fora da base (G-13).

### 4.3 Métricas e Critérios de Aprovação

| Métrica | Critério de aprovação | Critério de reprovação imediata |
|---------|----------------------|---------------------------------|
| Taxa de cumprimento dos guardrails determinísticos (G-01, G-03, G-04, G-06, G-07, G-09, G-10, G-12) | 100% | Qualquer violação |
| Taxa de cumprimento dos guardrails probabilísticos (avaliação humana — Camada 4) | ≥ 90% | < 80% ou qualquer violação de G-02/G-07 |
| Taxa de aprovação no golden set (Camada 2) | ≥ 95% | < 90% |
| Tempo médio de resposta | Variação ≤ 15% em relação à baseline de produção | Variação > 30% |
| Taxa de falso negativo de recuperação (Camada 2) | ≤ 5% | > 10% |
| Violação de G-07 (carga perigosa) | Zero tolerância — 0 ocorrências | Qualquer ocorrência |

Qualquer critério de reprovação imediata bloqueia a promoção independentemente dos demais resultados.

### 4.4 Tratamento de Efeitos Colaterais

Efeitos colaterais são esperados em sistemas de IA. O harness os trata da seguinte forma:

**Efeito colateral detectado em Camada 2 (não havia caso adversarial para ele):** o caso que revelou o efeito colateral é adicionado imediatamente ao golden set antes de qualquer nova tentativa de correção. A mudança retorna para o ciclo de diagnóstico.

**Efeito colateral em domínio não relacionado à mudança:** qualificar se é regressão (estava funcionando antes) ou problema latente (nunca estava coberto). Regressões bloqueiam a promoção. Problemas latentes são registrados como novos tickets, mas não bloqueiam a promoção da mudança corrente, desde que não envolvam guardrails G-02 ou G-07.

**Efeito colateral em guardrail probabilístico após ajuste de prompt:** o prompt é revertido para a versão anterior. Não se aplica correção incremental sobre uma versão com efeito colateral documentado — o ciclo de diagnóstico recomeça do nível 4 (Seção 3.3).

**Efeito colateral em guardrail determinístico:** bloqueio imediato de promoção. Escalonamento a Tech Lead e Produto no mesmo dia. O guardrail determinístico tem prioridade absoluta sobre qualquer melhoria de qualidade geral.

---

## 5. Pontos de Human-in-the-Loop

### 5.1 Matriz de Aprovação por Tipo de Mudança

| Tipo de mudança | Aprovação obrigatória | Momento na esteira | Risco que justifica |
|---|---|---|---|
| Ajuste de prompt que toca em guardrails G-02, G-05, G-08, G-11, G-13 ou G-14 | Product Owner + Compliance | Após Camada 4, antes do deploy em staging | Guardrails probabilísticos dependem da instrução de sistema; alteração inadvertida pode gerar violações não capturadas por validadores automáticos |
| Ajuste de prompt que não toca em guardrails | Product Owner | Após Camada 2, antes do deploy em staging | Risco de regressão de domínio não coberta por testes automáticos |
| Atualização de documento normativo (POL-001, PROC-042, SLA-2024) | Compliance + responsável pela documentação na área de negócio | Antes da reindexação | Documento desatualizado ou incorreto propaga erro para todas as respostas do domínio afetado |
| Alteração ou adição de guardrail (qualquer tipo) | Product Owner + Compliance + Tech Lead | Antes da implementação | Guardrails são restrições de negócio e compliance; mudança sem aprovação tripla cria risco de compliance não documentado |
| Atualização de versão de modelo de linguagem | Product Owner + Tech Lead + Compliance | Após suíte completa (Camadas 1–4), antes do staging | Modelos diferentes têm comportamentos emergentes distintos; guardrails probabilísticos podem ter eficácia diferente no novo modelo |
| Alteração na lista fechada de tiers de SLA (G-09) | Compliance + Product Owner | Antes da implementação | Lista fechada reflete contratos vigentes; alteração sem aprovação pode expor a NovaTech a compromissos contratuais não autorizados |
| Qualquer mudança após incidente de violação de G-07 | Product Owner + Compliance + representante de Gestão de Riscos | Antes de retornar ao staging | G-07 protege contra risco de segurança; após violação confirmada, qualquer retorno a produção exige validação explícita da barreira determinística |
| Mudança que afeta o template fixo de encaminhamento (G-07) | Compliance + Gestão de Riscos | Antes da implementação | O template é a barreira final contra informação incorreta sobre carga perigosa; alteração não autorizada elimina a única garantia determinística para esse risco |

### 5.2 Critérios para Aprovação Humana

A aprovação humana não é uma formalidade de assinatura. O aprovador deve:

1. Ler o diagnóstico de causa raiz documentado no ticket.
2. Revisar o diff exato da mudança (prompt, código ou documento).
3. Confirmar que os resultados da suíte de regression testing foram revisados e estão dentro dos critérios de aprovação.
4. Para mudanças que tocam em guardrails, executar pessoalmente pelo menos 5 casos adversariais no ambiente de staging antes de assinar.
5. Registrar a aprovação com justificativa no ticket (não apenas "aprovado" — deve constar o que foi verificado).

### 5.3 Escalada Automática para Revisão Humana

Além das aprovações planejadas, as seguintes condições disparam revisão humana não programada:

- Qualquer violação de G-07 em produção, independentemente do volume.
- Taxa de escalada a atendimento humano > 20% acima da baseline dos últimos 7 dias.
- Três ou mais feedbacks F-02 (violação de guardrail) no mesmo dia.
- Feedback F-07 (alucinação factual) com número ou condição que tenha impacto financeiro documentado.
- Qualquer resposta que mencione tier fora de {Gold, Silver, Standard} que tenha chegado ao cliente (violação de G-09).

Nessas condições, o assistente pode ser colocado em modo degradado (respostas substituídas por encaminhamento a atendimento humano) enquanto a investigação ocorre. Essa decisão compete ao Product Owner em conjunto com o Tech Lead de plantão.

---

## 6. Papéis e Responsabilidades

| Papel | Responsabilidades no harness |
|-------|------------------------------|
| **Product Owner (Produto)** | Define prioridade de tickets de feedback; aprova mudanças de prompt e atualizações de guardrails; aprova promoção a produção; aciona modo degradado em emergências; mantém o golden set atualizado com novos casos descobertos |
| **QA** | Executa as Camadas 1–4 da suíte de regression testing; documenta resultados com evidências; realiza avaliação humana da Camada 4; identifica e registra efeitos colaterais; mantém rastreabilidade entre tickets e casos de teste |
| **Tech Lead** | Implementa ajustes de pipeline, filtros e guardrails determinísticos; define e mantém validadores automáticos (G-01, G-03, G-06, G-07, G-09, G-10, G-12); aprova mudanças técnicas de pipeline; gerencia versões de modelo; executa deploys em staging e produção |
| **Compliance** | Aprova mudanças que tocam em guardrails normativos (G-02, G-05, G-07, G-08, G-09); valida atualizações de documentos-fonte; aprova alterações na lista fechada de tiers; participa da revisão pós-incidente de G-07 |
| **Operações** | Realiza a categorização inicial de tickets de feedback (F-01 a F-08); monitora métricas pós-deploy; executa alertas de escalada automática; coordena soak period em staging; mantém log de escaladas a atendimento humano |
| **Gestão de Riscos (área de negócio)** | Participa da aprovação de qualquer mudança que altere o fluxo de tratamento de carga perigosa (G-02, G-07, G-14); é notificada imediatamente em caso de violação de G-07 em produção |

---

## 7. Critérios de Aprovação para Produção

Uma mudança está aprovada para promoção a produção somente quando **todas** as condições abaixo forem atendidas:

**Critérios técnicos (verificados por QA e Tech Lead):**

- [ ] Suíte de regression testing executada com resultados dentro dos thresholds da Seção 4.3.
- [ ] Nenhuma violação de guardrail determinístico (G-01, G-03, G-04, G-06, G-07, G-09, G-10, G-12) nos testes.
- [ ] Nenhuma violação de G-07 em nenhum caso de teste (zero tolerância).
- [ ] Soak period de 48 horas em staging sem alertas de métricas de qualidade.
- [ ] O caso de feedback que originou a mudança foi resolvido e confirmado em staging.

**Critérios de processo (verificados por Produto e Operações):**

- [ ] Diagnóstico de causa raiz documentado no ticket.
- [ ] Aprovações humanas aplicáveis à mudança (Seção 5.1) obtidas e registradas com justificativa.
- [ ] Diff da mudança revisado e versionado no repositório.
- [ ] Plano de rollback documentado (qual versão anterior restaurar em caso de incidente pós-deploy).
- [ ] Alertas de monitoramento pós-deploy configurados (período mínimo de 72 horas).

**Critérios de compliance (verificados por Compliance, quando aplicável):**

- [ ] Para mudanças em documentos normativos: responsável da área de negócio confirmou que o documento atualizado está vigente e assinado.
- [ ] Para mudanças em guardrails: processo formal de revisão tripla (Seção 5.1) concluído.
- [ ] Para mudanças pós-incidente de G-07: representante de Gestão de Riscos assinou a aprovação.

Estas validações também funcionam como checkpoint explícito de aderência às ADR-0002 e ADR-0004 do Cenário 2: a primeira sustenta os limites de contexto, rastreabilidade e completude mínima da resposta; a segunda sustenta o comportamento esperado do pipeline de recuperação, grounding e tratamento de baixa confiança.

---

## 8. Riscos Cobertos e Lacunas Remanescentes

### 8.1 Riscos Cobertos pelo Harness

| Risco | Mecanismo de cobertura |
|-------|------------------------|
| Degradação silenciosa de qualidade após mudanças | Golden set (Camada 2) + soak period em staging |
| Violação de guardrail determinístico após ajuste de pipeline | Camada 1 com verificação automatizada por validadores |
| Violação de guardrail probabilístico após ajuste de prompt | Camada 4 (avaliação humana) + aprovação de Produto e Compliance |
| Propagação de erro documental para produção | Revisão obrigatória de Compliance antes de reindexação |
| Regressão em domínio não relacionado à mudança | Execução completa do golden set a cada mudança |
| Efeitos colaterais de atualização de versão de modelo | Suíte completa (Camadas 1–4) + aprovação tripla (P.O., Tech Lead, Compliance) |
| Incidente de G-07 sem resposta imediata | Escalada automática + modo degradado acionável pelo P.O. |
| Ausência de rastreabilidade | Ticket obrigatório para toda melhoria; aprovações registradas com justificativa |

### 8.2 Lacunas Remanescentes e Recomendações

**Lacuna 1 — Monitoramento contínuo de qualidade em produção (não apenas pós-deploy):**  
O harness atual cobre a janela de 72 horas pós-deploy, mas não define um processo de amostragem semanal sistemática em produção estável. Recomendação: estabelecer rotina de auditoria semanal com 50 conversas amostradas aleatoriamente, executada por QA, com relatório mensal de tendências para Produto.

**Lacuna 2 — Ausência de benchmark de qualidade para novos temas não cobertos:**  
Quando um novo tipo de consulta emerge com frequência (ex.: novo produto, nova regulação ANTT), não há processo definido para decidir se deve ser incorporado à base. Recomendação: criar critério de inclusão baseado em volume de escaladas (ex.: 10+ casos do mesmo tema em 30 dias disparam avaliação de incorporação).

**Lacuna 3 — FAQ-Atendimento sem processo de curadoria:**  
O FAQ é uma fonte não validada identificada nos guardrails (G-05, G-08). O harness trata os riscos do FAQ como restrição de pipeline, mas não define quem é responsável por sua curadoria ou descontinuação formal. Recomendação: Produto e Compliance definem responsável pelo FAQ e prazo para sua validação ou depreciação como fonte.

**Lacuna 4 — Ausência de SLA de resolução por categoria de feedback:**  
O fluxo de feedback está definido, mas não há SLA documentado (ex.: F-02 — violação de guardrail deve ser diagnosticado em até X horas; F-08 — cobertura ausente pode aguardar Y dias). Sem SLA, tickets críticos podem aguardar indefinidamente. Recomendação: Produto define SLA por categoria na primeira sprint de operação do harness.

**Lacuna 5 — Ausência de teste de carga e latência para guardrails determinísticos:**  
Os validadores de pipeline (G-01, G-06, G-07, G-10, G-12) adicionam latência à resposta. O harness testa qualidade mas não define limiar de latência aceitável para os validadores em produção com carga real. Recomendação: Tech Lead define e documenta SLA de latência para cada validador; QA inclui teste de desempenho na Camada 3.

---

*Documento elaborado com base nos guardrails `guardrails-assistente-novatech.md` v1.0 e nos princípios de governança de produto para sistemas de IA em produção.*  
*Restrição: os guardrails G-01 a G-14 são restrição obrigatória permanente deste harness e não podem ser alterados sem o processo formal descrito na Seção 5.1.*
