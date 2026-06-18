# Requirements — Query Endpoint do Assistente NovaTech

**Documento:** requirements.md
**Versão:** 2.0
**Data de emissão:** 2025-06-09
**Responsável:** Product Specialist — NovaTech
**Status:** Em revisão
**Origem da versão anterior:** requirements-v1.md
**Revisão aplicada:** analise-de-ambiguidade.md (Tech Lead)
**Achados aplicados:** A-01, A-02, A-03, A-04, A-05, A-06, A-07, A-08, A-09, A-10, A-11, A-12, A-13, A-14, A-15, A-16, A-17, A-18 (BC), A-19

---

## 1. Contexto do módulo

O Query Endpoint é o único ponto de entrada do assistente NovaTech para perguntas formuladas por atendentes e clientes finais sobre o domínio de operações da empresa. O módulo recebe uma pergunta em linguagem natural, executa recuperação de evidências no corpus documental via pipeline de RAG (ADR-0004), gera uma resposta fundamentada em linguagem natural por meio do modelo GPT-4o (ADR-0001) e a retorna com citação de fonte, identificação de vigência documental e, quando aplicável, sinalização de conflito, lacuna ou estado de confiança reduzido.

O módulo opera sobre um domínio estruturado em seis bounded contexts — BC1 a BC6 — cujas fronteiras semânticas, termos canônicos e regras de desambiguação estão formalizados no Glossário de Linguagem Ubíqua v2. O comportamento do endpoint deve ser inteiramente derivável das regras de negócio documentadas nesses insumos; nenhuma regra pode ser inferida ou inventada pelo modelo fora das fontes ingeridas.

Este documento especifica exclusivamente o módulo Query Endpoint. Outros módulos do sistema NovaTech — pipeline de ingestão documental, autenticação, administração de corpus e interfaces de front-end — estão fora do escopo desta especificação.

---

## 2. Outcomes

Os outcomes a seguir descrevem resultados observáveis para o atendente e para o cliente final, não decisões de implementação.

### OUT-01 — Resposta objetiva dentro do prazo de atendimento

O atendente recebe uma resposta fundamentada e completa para a pergunta formulada dentro do limite de 30 segundos a partir do envio, permitindo que o atendimento ao cliente prossiga sem quebra de fluxo. **"Resposta completa"** é definida como o conjunto: (a) corpo informativo, (b) citação de fonte, (c) indicador de estado de confiança e (d) ressalva de lacuna, quando aplicável. Nenhum desses elementos pode ser omitido por razão de desempenho.

**Critérios de verificação associados:** CV-01, CV-02.

### OUT-02 — Rastreabilidade total da resposta

Todo atendente e cliente final consegue identificar, em cada resposta, qual documento normativo ou fonte documental embasou cada afirmação, possibilitando auditoria e contestação fundamentada.

**Critérios de verificação associados:** CV-03, CV-04.

### OUT-03 — Respostas seguras em cenários de conflito documental

Quando dois documentos do corpus apresentam regras divergentes para o mesmo tema, o atendente recebe ambas as versões explicitadas, com indicação da versão a adotar como padrão (aquela com metadado de vigência mais recente, conforme ADR-0003), sem que o endpoint colapse os valores ou silencie o conflito. O caso atualmente conhecido é o conflito entre PROC-042 v1 e PROC-042 v2; o comportamento se aplica a qualquer futuro conflito documentado no corpus.

**Critérios de verificação associados:** CV-05, CV-06.

### OUT-04 — Sinalização explícita de lacunas e ausência de alucinação

Quando a pergunta incide sobre tema sem documento normativo formal — especialmente BC5 (sinistros e carga danificada) e BC6 (seguro de carga) — o atendente recebe resposta com estado de confiança identificado como "baixa confiança" ou "sem evidência suficiente" (conforme o caso), ressalva explícita sobre a origem informal da informação e orientação de confirmação com o setor competente.

**Critérios de verificação associados:** CV-07, CV-08.

### OUT-05 — Roteamento correto de cargas especiais

Quando a pergunta envolve carga perigosa com classe ANTT identificada, carga refrigerada com suspeita de ruptura de cadeia de frio ou carga com lacre violado, o atendente recebe orientação imediata de encaminhamento ao contexto BC4 (Gestão de Riscos, ramal 4500), sem que o endpoint processe a solicitação como devolução ou frete padrão. Quando a carga é descrita por nome coloquial ambíguo, o endpoint solicita confirmação da classificação ANTT antes de decidir o roteamento.

**Critérios de verificação associados:** CV-09, CV-10.

### OUT-06 — Respostas coerentes para perguntas cross-context

Para os 15% das perguntas que cruzam dois bounded contexts — por exemplo, devolução com carga acima de 500 kg (BC1 + BC2), ou SLA em chamado de devolução (BC1 + BC3) — o atendente recebe uma resposta que integra as regras dos dois contextos sem contradição interna e sem supressão de uma das regras. Quando a intenção da pergunta for ambígua entre dois contextos, o endpoint sinaliza a ambiguidade e solicita esclarecimento ao atendente.

**Critérios de verificação associados:** CV-11, CV-12, CV-14.

### OUT-07 — Endpoint sempre opera sobre o corpus mais atualizado disponível

O endpoint responde com base na versão mais recente dos documentos disponíveis no corpus no momento da consulta. A responsabilidade pelo prazo de ingestão e atualização do corpus pertence ao pipeline de ingestão documental (especificado separadamente). O endpoint não é responsável por falhas de ingestão, mas deve sempre consultar o corpus disponível na chamada, sem cache de versão anterior.

**Critérios de verificação associados:** CV-13.
*(Alterado de v1 — origem: A-01, A-08)*

### OUT-08 — Comportamento definido em degradação e baixa confiança

Quando o endpoint não puder fornecer resposta de alta confiança — por ausência de evidência no corpus, por evidência insuficiente ou por timeout do pipeline de RAG — o atendente recebe resposta com estado de confiança sinalizado, orientação de encaminhamento ao supervisor e, quando aplicável, a referência parcial disponível. O endpoint não retorna silêncio nem mensagem de erro técnico sem orientação ao atendente.

**Critérios de verificação associados:** CV-15.
*(Novo — origem: A-06)*

---

## 3. Scope boundaries

As fronteiras abaixo derivam diretamente dos seis bounded contexts definidos no Mapa de Bounded Contexts v2 do assistente NovaTech.

### SB-01 — BC1: Devolução de Mercadorias

**Coberto:** Regras de elegibilidade para devolução padrão, prazo de devolução (7 dias úteis), procedimento de triagem (4 horas úteis — métrica interna independente do relógio de SLA de BC3), coleta reversa (até 2 dias úteis após aprovação), custo de devolução por responsabilidade, reembolso de devolução (até 5 dias úteis após recebimento no CD), devoluções parciais por CT-e e prazo expirado com roteamento ao Comercial. Quando a data de recebimento não for informada na pergunta, o endpoint deve solicitá-la condicionalmente antes de confirmar elegibilidade.

**Excluído:** Cálculo do valor numérico do frete reverso (pertence a BC2); decisão de elegibilidade de cargas perigosas, refrigeradas ou lacradas (pertence a BC4); apuração de responsabilidade por dano ocorrido em trânsito (pertence a BC5); mercadorias ainda em trânsito (PROC-088, fora do corpus disponível).

*(Atualizado v2: adicionada regra de data de recebimento ausente — origem: A-09)*

### SB-02 — BC2: Precificação de Frete Especial

**Coberto:** Fórmula de cálculo de frete especial (`valor_base × multiplicador_regional × fator_de_peso`), multiplicadores regionais e fatores de peso conforme PROC-042-v2, prazo adicional de entrega para frete especial (+3 dias úteis, v2), desconto por volume (5% a partir de 8 fretes/mês; 10% acima de 15 fretes/mês, v2), requisito de aprovação gerencial para cargas acima de 5.000 kg. Para a categoria "prazos de entrega", o endpoint informa o prazo adicional (+3 dias úteis) e explicita que o prazo total depende do prazo padrão da rota, que não está disponível no corpus.

**Excluído:** Valor base da tarifa mensal (variável externa — NG-04); cálculo de frete para cargas abaixo de 500 kg (lacuna L5); cálculo de frete para cargas perigosas acima de 500 kg (PROC-043, em revisão pelo Compliance); definição de SLA de atendimento (pertence a BC3).

*(Atualizado v2: adicionada distinção prazo adicional vs. prazo total — origem: A-19)*

### SB-03 — BC3: Gestão de SLA e Atendimento

**Coberto:** Classificação de clientes em tiers (Gold, Silver, Standard); SLA de primeira resposta e de resolução por tier para chamados gerais e incidentes críticos; quatro critérios objetivos de classificação de incidente crítico; penalidades por violação de SLA (1ª, 2ª e 3ª+ violações no mês); comportamento do relógio de SLA para chamados gerais e incidentes críticos Gold.

**Excluído:** Tiers não documentados (Platinum ou similares, descontinuados em 2022); alteração contratual de tier (competência do Comercial); regras de cálculo de frete (BC2); processo de triagem de devolução (BC1 — a triagem de 4h é regra interna independente do relógio de SLA de BC3); decisão sobre conformidade de cargas perigosas (BC4).

*(Atualizado v2: explicitada independência entre triagem BC1 e relógio de SLA BC3 — origem: A-14)*

### SB-04 — BC4: Conformidade e Riscos de Carga

**Coberto:** Identificação de cargas perigosas conforme classes 1 a 6 da ANTT (Resolução nº 5.947/2021); critério de ruptura de cadeia de frio (temperatura fora da faixa da nota fiscal por mais de 30 minutos contínuos, conforme sensor IoT, quando o tempo for informado pelo atendente); condição de exceção para lacre violado com documentação de entrega; encaminhamento obrigatório ao ramal 4500 (Gestão de Riscos) para cargas que não se enquadram no processo padrão. **Quando a carga for descrita por nome coloquial que não permite classificação ANTT inequívoca, o endpoint solicita confirmação da classificação antes de decidir o roteamento.**

**Excluído:** Processo interno da Gestão de Riscos após o ramal 4500 (lacuna L1); processo formal de autorização de frete expresso para carga perigosa (lacuna L6); tabela de frete PROC-043 (em revisão pelo Compliance).

*(Atualizado v2: adicionada regra de nome coloquial ambíguo — origem: A-07)*

### SB-05 — BC5: Sinistros e Carga Danificada

**Coberto:** Distinção entre carga danificada em trânsito e devolução padrão; prazo de registro de ocorrência (até 48 horas após recebimento, interpretação atual: horas corridas — pendente de validação pelo Jurídico, DP-04); requisitos documentais do registro (fotos e laudo); encaminhamento ao e-mail sinistros@novatech.com.br; natureza do ressarcimento (integral, condicionado à comprovação de responsabilidade pelo Jurídico).

**Excluído:** Processo de investigação do Jurídico; prazo de conclusão da investigação (sem documento normativo — lacuna L2); garantia de ressarcimento; devoluções por desistência do cliente (BC1).

**Restrição obrigatória:** Toda resposta neste boundary deve identificar explicitamente que a fonte é o FAQ-38 (documento informal, não validado por Compliance) e recomendar confirmação com o setor competente.

### SB-06 — BC6: Seguro de Carga

**Coberto:** Percentuais de seguro de carga como adicional contratual (0,3% para cargas padrão; 0,8% para cargas perigosas), com escopo restrito a contratos firmados a partir de 2023.

**Excluído:** Confirmação de percentual para contratos pré-2023 (competência do Comercial); contratação de seguro; processo de acionamento do seguro após sinistro.

**Restrição obrigatória:** Toda resposta neste boundary deve identificar explicitamente que a fonte é o FAQ-22 (documento informal, não validado por Compliance), que os percentuais citados são válidos apenas para contratos a partir de 2023, e que contratos anteriores requerem confirmação com o Comercial.

---

## 4. Constraints

### CN-01 — Não alucinação

O endpoint não pode afirmar, inferir ou calcular qualquer valor, prazo, regra ou procedimento que não seja diretamente recuperável do corpus documental ingerido. Informações ausentes do corpus devem ser identificadas como fora do escopo disponível, com orientação de encaminhamento ao setor competente.

### CN-02 — Citação obrigatória de fonte

Toda afirmação factual contida na resposta deve ser acompanhada da identificação da fonte documental de origem (código do documento, seção e, quando disponível, parágrafo). O formato de citação segue o padrão definido em conjunto com a equipe de front-end (Pendência DP-03-formato, vinculada a OQ-03). Respostas sem citação de fonte para afirmações factuais são inválidas.

### CN-03 — Identificação de vigência documental e exposição de conflitos *(generalizado — A-03)*

Toda resposta que citar documento com versão concorrente no corpus deve: (a) identificar explicitamente a versão do documento de origem de cada dado citado, (b) expor o conflito ao atendente em vez de silenciá-lo, e (c) indicar como versão padrão aquela com metadado de vigência mais recente, conforme ADR-0003. Quando o metadado de vigência estiver ausente, tratar como lacuna e sinalizar na resposta. O conflito atualmente conhecido e obrigatório de cobrir é PROC-042 v1 vs. PROC-042-v2 (padrão para chamados a partir de 01/12/2023). Futuros conflitos documentados no corpus são automaticamente cobertos por esta constraint.

### CN-04 — Rotulação de fontes informais

Respostas baseadas em FAQ-38 (BC5) ou FAQ-22 (BC6) devem ser rotuladas como derivadas de documento informal não validado por Compliance. A resposta deve incluir indicação explícita de confirmação com o setor responsável (Jurídico para BC5; Comercial para BC6).

### CN-05 — Idioma

Todas as respostas devem ser redigidas em português brasileiro formal. Termos técnicos e denominações de documentos (CT-e, PROC-042, SLA, POL-001) devem ser mantidos no formato canônico definido no Glossário de Linguagem Ubíqua v2.

### CN-06 — Tempo de resposta

O endpoint deve retornar a resposta completa (conforme definição em OUT-01) dentro do limite de 30 segundos a partir do recebimento da pergunta, em condição de carga normal de operação. A definição precisa de "carga normal" (percentil de carga e volume de requisições simultâneas) está pendente de definição com base em dados do protótipo (OQ-02). O modo de medição do tempo (streaming vs. resposta bloqueada) é definido em OQ-08 *(ver DP-05)*.

*(Atualizado v2: referência à OQ-08 adicionada — origem: A-04)*

### CN-07 — Budget de tokens e completude da resposta

A resposta deve ser completa dentro do budget de tokens definido em ADR-0002. Em nenhuma hipótese a resposta pode ser truncada de forma a omitir citação de fonte, identificação de versão documental ou ressalva de lacuna. Se a resposta necessitar de compressão, comprimir o corpo informativo antes de suprimir metadados de rastreabilidade. O valor numérico do budget de tokens está registrado em OQ-09 como pendência de ADR-0002 *(ver DP-07)*.

*(Atualizado v2: OQ-09 adicionada para rastrear budget numérico — origem: A-13)*

### CN-08 — Sem invenção de tiers

O endpoint não pode confirmar, reconhecer ou operar sobre tiers de cliente não documentados no SLA-2024 (Gold, Silver, Standard). Solicitações que referenciem tier Platinum ou qualquer denominação não reconhecida devem ser respondidas com informação dos tiers válidos e orientação de verificação do contrato.

### CN-09 — Roteamento diferenciado para cargas especiais *(revisado — A-07)*

A presença de carga especial na pergunta ativa o seguinte protocolo, que não pode ser suprimido:

- **Classificação ANTT explícita (classe 1–6):** roteamento imediato para BC4 (ramal 4500), independentemente do contexto principal da pergunta.
- **Descrição por nome coloquial potencialmente perigoso** (ex.: "produto químico", "bateria de lítio", "cloro", "tinta industrial"): o endpoint pergunta ao atendente se a carga possui classificação ANTT antes de decidir o roteamento. Em caso de dúvida persistente do atendente, aplicar o princípio de segurança e encaminhar BC4.
- **Carga refrigerada:** aplicar regra de Termo 25 do glossário (sensor informado vs. não informado).
- **Lacre violado:** verificar existência de documentação antes de encaminhar BC4 (conforme Termo 26 do glossário).

### CN-10 — Restrição de escopo para frete abaixo de 500 kg

O endpoint não pode aplicar a fórmula de frete especial a cargas abaixo de 500 kg. Perguntas sobre frete nessa faixa de peso devem ser respondidas com a informação de que o escopo disponível cobre apenas frete especial (acima de 500 kg) e com orientação de contato com o Comercial.

### CN-11 — Consistência de corpus na consulta *(revisado — A-01, A-08)*

O endpoint deve sempre consultar o corpus disponível no momento da chamada, sem reutilizar resultado de consulta anterior (sem cache de corpus). A responsabilidade pelo prazo de atualização do corpus (até 24 horas após publicação de documento) pertence ao pipeline de ingestão documental, especificado separadamente. Falhas de atualização do corpus são imputadas ao pipeline, não ao endpoint.

### CN-12 — Ausência de promessas sobre processos não documentados

O endpoint não pode prometer resultado, prazo ou aprovação para processos sem PROC formal: processo da Gestão de Riscos após o ramal 4500 (lacuna L1) e autorização de frete expresso para carga perigosa pelo Compliance (lacuna L6).

### CN-13 — Estados de confiança com critério objetivo de atribuição *(novo — A-15)*

O endpoint atribui um dos três estados de confiança a cada resposta, com base nos seguintes critérios objetivos:

- **Confiança alta:** evidência direta e completa no corpus, sem conflito de versões, sem lacuna documental para o tema consultado.
- **Confiança baixa:** evidência presente no corpus mas proveniente de fonte informal (FAQ), ou evidência que referencia documento fora do corpus (ex.: PROC-088), ou pergunta cujo tema tem lacuna formal identificada (L1–L7).
- **Sem evidência suficiente:** tema fora do escopo do corpus disponível (ex.: frete abaixo de 500 kg), ou ausência total de resultado relevante na recuperação RAG.

O estado de confiança é parte obrigatória da resposta (incluído na definição de "resposta completa" de OUT-01).

### CN-14 — Comportamento em degradação e timeout *(novo — A-06)*

Quando o pipeline de RAG não retornar resultado relevante dentro do tempo compatível com o limite de 30 segundos (CN-06), o endpoint deve: (a) retornar resposta com estado "sem evidência suficiente", (b) informar ao atendente que a consulta não pôde ser processada dentro do prazo e (c) exibir a ação de escalação ao supervisor. O endpoint não retorna silêncio nem erro técnico não interpretado. A ação de escalação ao supervisor é uma orientação ao atendente (texto + botão na interface, conforme mockup p.3); o endpoint não executa nenhuma ação técnica de roteamento de chamada.

---

## 5. Prior decisions

### ADR-0001 — Modelo LLM: Azure OpenAI GPT-4o

**Impacto no módulo:** O endpoint utiliza o GPT-4o como modelo de geração de resposta. As instruções de sistema (system prompt) devem incluir as regras de desambiguação por bounded context do Glossário v2, os guardrails de não alucinação, as restrições de citação de fonte, o protocolo de nome coloquial de carga perigosa (CN-09) e os critérios de atribuição de estado de confiança (CN-13). A escolha do modelo implica capacidade de seguir instruções complexas e de operar sobre contexto de recuperação extenso, viabilizando o comportamento cross-context (OUT-06) e a sinalização de ambiguidade de intenção (CV-14).

### ADR-0002 — Estratégia de contexto e budget de tokens

**Impacto no módulo:** O budget de tokens condiciona diretamente o constraint CN-07. O valor numérico do budget está pendente de definição (OQ-09 / DP-07). A estratégia de contexto deve priorizar, na montagem do prompt: (a) trechos recuperados mais relevantes pelo pipeline de RAG, (b) metadados de vigência dos documentos e (c) regras de desambiguação dos bounded contexts envolvidos. Quando o budget impuser compressão, os metadados de rastreabilidade (fonte, versão, estado de confiança, ressalvas de lacuna) têm prioridade sobre o corpo informativo.

### ADR-0003 — Tratamento de documentos contraditórios com metadado de vigência

**Impacto no módulo:** O endpoint consome o metadado de vigência produzido pelo pipeline de ingestão para determinar qual versão apresentar como padrão em conflitos. A regra é geral (CN-03): qualquer conflito documentado no corpus é tratado pelo mesmo protocolo. O caso atual conhecido é PROC-042 v1 vs. v2. A ausência de metadado de vigência para um documento é tratada como lacuna e sinalizada na resposta com estado de confiança baixo.

### ADR-0004 — Pipeline de RAG e lições do protótipo

**Impacto no módulo:** O endpoint é dependente do pipeline de RAG para recuperar os trechos documentais que embasam a resposta. As lições do protótipo estabelecem que: (a) a recuperação deve ser guiada pelo bounded context identificado na pergunta; (b) perguntas cross-context devem disparar recuperação em múltiplos índices; (c) a ausência de resultado relevante na recuperação é o sinal primário para o estado "sem evidência suficiente" (CN-13), não a incerteza do modelo gerador; (d) quando o roteamento de intenção falhar ou retornar contexto ambíguo, o endpoint deve sinalizar a ambiguidade ao atendente (CV-14, CN-09).

---

## 6. Verification criteria

Os critérios abaixo são verificáveis por teste funcional e por teste de regressão. Cada critério identifica o outcome ou constraint associado, o cenário de teste e a condição de aprovação mensurável.

### CV-01 — Tempo de resposta em carga normal *(revisado — A-04)*

**Associado a:** OUT-01, CN-06
**Cenário positivo:** Pergunta de contexto único submetida ao endpoint em ambiente de produção com carga normal (percentil e volume definidos em OQ-02).
**Condição de aprovação — resposta bloqueada (padrão até definição de OQ-08):** O endpoint retorna resposta completa em tempo menor que 30 segundos, medido do timestamp de recebimento da requisição ao timestamp de entrega do último byte da resposta ao cliente.
**Nota de pendência:** Se a interface adotar streaming (OQ-08 / DP-05), a condição de aprovação será reformulada para: time-to-first-token ≤ X segundos E tempo de resposta completa ≤ 30 segundos. Os valores de X serão definidos em OQ-08.
**Frequência:** Executado em suite de smoke test a cada deploy e monitorado continuamente com alertas para p95 > 25 segundos.

### CV-02 — Completude da resposta para as quatro categorias primárias *(revisado — A-19)*

**Associado a:** OUT-01
**Cenário positivo:** Uma pergunta representativa de cada uma das quatro categorias do discovery (prazos de entrega, regras de frete, política de devolução, SLAs) é submetida ao endpoint.
**Condição de aprovação:** Cada resposta contém: (a) regra ou valor solicitado, (b) citação de fonte, (c) identificação de vigência quando aplicável, (d) estado de confiança. Nenhum dos quatro elementos pode estar ausente.
**Condição específica para "prazos de entrega de frete especial":** A resposta informa o prazo adicional (+3 dias úteis, v2) e explicita que o prazo total depende do prazo padrão da rota, que não está disponível no corpus. Resposta que apresentar prazo total calculado reprova o teste.

### CV-03 — Citação de fonte em toda afirmação factual

**Associado a:** OUT-02, CN-02
**Cenário positivo:** Pergunta sobre prazo de reembolso de devolução.
**Condição de aprovação:** A resposta cita explicitamente "POL-001 §3.3" (ou equivalente) como fonte. Resposta sem citação de fonte é reprovada automaticamente.
**Cenário de exceção:** Pergunta sobre tema sem documento normativo (ex.: seguro de carga).
**Condição de aprovação para exceção:** A resposta cita "FAQ-22" e inclui ressalva de que o documento é informal e não validado por Compliance.

### CV-04 — Ausência de afirmação sem fonte rastreável *(revisado — A-12)*

**Associado a:** OUT-02, CN-01
**Cenário de exceção:** Pergunta sobre frete padrão (abaixo de 500 kg).
**Condição de aprovação:** O endpoint não apresenta valor ou fórmula de cálculo. A resposta informa que o escopo disponível cobre apenas frete especial (acima de 500 kg), atribui estado "sem evidência suficiente" e orienta contato com o Comercial.
**Suite de regressão automatizada:** As seguintes perguntas de lacuna compõem a suite de regressão com condição de aprovação objetiva por pergunta: (1) "Qual o frete para 300 kg?" → sem valor/fórmula + estado sem evidência; (2) "Como funciona o seguro para contratos de 2021?" → percentual com ressalva FAQ-22 + orientação Comercial; (3) "O que acontece após ligar para o ramal 4500?" → sem processo inventado + estado confiança baixa; (4) "Como faço frete expresso para carga perigosa?" → sem prazo/confirmação inventada + estado confiança baixa. Revisão humana complementar de 20 perguntas por sprint mantida como processo de qualidade, não como critério primário.

*(Atualizado v2: suite de regressão automatizada adicionada — origem: A-12)*

### CV-05 — Exposição do conflito PROC-042 v1 vs. v2

**Associado a:** OUT-03, CN-03
**Cenário positivo:** Pergunta "Qual o fator de peso para carga de 2.000 kg?"
**Condição de aprovação:** A resposta (a) indica os dois valores (v1: 1.2; v2: 1.15), (b) identifica as versões de origem de cada valor, (c) informa que PROC-042-v2 é o padrão para chamados novos e (d) recomenda confirmação com o Comercial para contratos anteriores a 01/12/2023.
**Reprovação automática:** Resposta que apresenta apenas um valor sem mencionar o conflito.

### CV-06 — Adoção da versão correta por padrão

**Associado a:** OUT-03, CN-03
**Cenário positivo:** Pergunta "Qual o multiplicador regional para entrega no Norte?"
**Condição de aprovação:** A resposta apresenta o valor da v2 (1.8) como padrão e menciona o valor da v1 (1.6) com a devida identificação de versão.
**Reprovação automática:** Resposta que apresenta apenas o valor da v1 sem ressalva.

### CV-07 — Rotulação de fonte informal em BC5

**Associado a:** OUT-04, CN-04
**Cenário positivo:** Pergunta "Como funciona o processo de carga danificada?"
**Condição de aprovação:** A resposta (a) descreve o processo conforme FAQ-38, (b) identifica explicitamente que a fonte é o FAQ-38, documento informal não validado por Compliance, (c) informa que o processo passa pelo Jurídico, (d) fornece o e-mail sinistros@novatech.com.br e (e) atribui estado "confiança baixa". Ausência de qualquer um dos cinco elementos reprova o teste.

*(Atualizado v2: elemento (e) — estado de confiança — adicionado — origem: A-15)*

### CV-08 — Rotulação de fonte informal em BC6

**Associado a:** OUT-04, CN-04
**Cenário positivo:** Pergunta "Qual o percentual de seguro de carga?"
**Condição de aprovação:** A resposta (a) cita os percentuais de FAQ-22 (0,3% e 0,8%), (b) identifica que a fonte é informal, (c) restringe a validade a contratos a partir de 2023, (d) orienta confirmação com o Comercial para contratos anteriores e (e) atribui estado "confiança baixa". Ausência de qualquer um dos cinco elementos reprova o teste.

*(Atualizado v2: elemento (e) adicionado — origem: A-15)*

### CV-09 — Roteamento obrigatório de carga perigosa com classe ANTT explícita *(revisado — A-07)*

**Associado a:** OUT-05, CN-09
**Cenário positivo — classificação ANTT explícita:** Pergunta "Posso devolver uma carga de gás comprimido (classe 2 ANTT)?"
**Condição de aprovação:** A resposta informa que carga de gás (classe 2 ANTT) não é elegível para devolução pelo processo padrão e orienta contato com a Gestão de Riscos pelo ramal 4500. Não promete resultado do processo de BC4.
**Cenário de exceção — nome coloquial ambíguo:** Pergunta "Posso devolver uma carga de produto de limpeza industrial?"
**Condição de aprovação para exceção:** A resposta pergunta ao atendente se a carga possui classificação ANTT antes de decidir o roteamento. Não assume que é perigosa nem que não é.
**Cenário de exceção — frete de carga perigosa:** Pergunta sobre frete de carga com classe ANTT.
**Condição de aprovação:** A resposta informa que cargas perigosas seguem PROC-043 (não PROC-042) e encaminha para BC4, sem aplicar a fórmula de frete especial.

### CV-10 — Roteamento de carga refrigerada com cadeia rompida *(revisado — A-05)*

**Associado a:** OUT-05, CN-09
**Cenário positivo — sensor informado:** Pergunta "O sensor registrou temperatura fora da faixa por 45 minutos. Posso solicitar devolução?"
**Condição de aprovação:** A resposta identifica que 45 minutos superam o critério de 30 minutos contínuos, informa que a carga não é elegível para devolução padrão e encaminha ao ramal 4500.
**Cenário de exceção — sensor não informado:** Pergunta "A temperatura da carga ficou fora do ideal. Posso devolver?"
**Condição de aprovação para exceção:** A resposta orienta o atendente a verificar o registro do sensor IoT (tempo de temperatura fora da faixa em minutos contínuos) antes de decidir a elegibilidade. Não afirma elegibilidade nem inelegibilidade sem o dado do sensor.

*(Atualizado v2 — origem: A-05)*

### CV-11 — Coerência em pergunta cross-context BC1 + BC2 *(revisado — A-09)*

**Associado a:** OUT-06
**Cenário positivo — com data de recebimento:** Pergunta "Quanto custa a devolução por desistência de uma carga de 1.500 kg com destino ao Nordeste? A carga foi recebida há 3 dias úteis."
**Condição de aprovação:** A resposta (a) confirma elegibilidade (dentro do prazo de 7 dias úteis), (b) informa que o custo usa os multiplicadores do frete original (POL-001 §3.5), (c) apresenta multiplicador Nordeste (1.5, v2) e fator de peso para 1.500 kg (1.15, v2), (d) cita ambas as fontes e (e) menciona o conflito v1/v2.
**Cenário de exceção — sem data de recebimento:** Pergunta "Quanto custa a devolução por desistência de uma carga de 1.500 kg com destino ao Nordeste?" (sem data).
**Condição de aprovação para exceção:** A resposta solicita a data de recebimento antes de confirmar elegibilidade, ou responde condicionalmente ("se dentro do prazo de 7 dias úteis, a devolução é elegível e o custo é calculado da seguinte forma..."). Não confirma elegibilidade sem a data.

*(Atualizado v2: cenário de exceção sem data adicionado — origem: A-09)*

### CV-12 — Coerência em pergunta cross-context BC1 + BC3 *(revisado — A-14)*

**Associado a:** OUT-06
**Cenário positivo — horário comercial:** Pergunta de cliente Gold: "Abri um chamado de devolução agora (10h, segunda-feira). Quando recebo retorno?"
**Condição de aprovação:** A resposta informa (a) que o SLA de primeira resposta para Gold é de até 2 horas úteis (SLA-2024 §2) — compromisso de BC3, e (b) que a triagem do chamado de devolução ocorre em até 4 horas úteis (POL-001 §3.3) — prazo interno de BC1, distinguindo claramente os dois como métricas independentes. A resposta não pode colapsar os dois prazos em um único número.
**Cenário de exceção — fora do horário comercial:** Pergunta de cliente Gold: "Abri um chamado de devolução às 17h30 de uma sexta-feira. Quando recebo retorno?"
**Condição de aprovação para exceção:** A resposta informa (a) que o relógio de SLA de BC3 pausa às 18h para chamados gerais Gold e retoma às 8h na segunda-feira, (b) que o prazo de triagem de BC1 é de 4 horas úteis e que sua regra de pausa fora do horário comercial não está documentada no corpus — o assistente não pode afirmar pausa nem corrida, e indica que para esclarecimento o atendente deve consultar as operações internas. A resposta não pode afirmar que a triagem pausa ou não pausa sem base documental.

*(Atualizado v2: cenário de exceção fora do horário comercial adicionado — origem: A-14)*

### CV-13 — Consistência de corpus na consulta *(revisado — A-01)*

**Associado a:** OUT-07, CN-11
**Responsabilidade:** Este critério é de integração (endpoint + pipeline de ingestão). Falha neste CV é imputada ao pipeline, não ao endpoint, salvo quando demonstrado que o endpoint utilizou resultado de chamada anterior (cache de corpus).
**Cenário de teste:** O pipeline de ingestão publica uma versão atualizada de um documento. Após confirmação de ingestão pelo pipeline, uma pergunta que só pode ser respondida com base no documento atualizado é submetida ao endpoint.
**Condição de aprovação:** O endpoint retorna resposta baseada no documento atualizado, citando-o corretamente. Resposta baseada na versão anterior do documento, após confirmação de ingestão, indica reutilização de cache de corpus pelo endpoint — falha do endpoint.
**Frequência:** Verificado após cada atualização documental relevante.

*(Atualizado v2: responsabilidade de falha explicitada — origem: A-01)*

### CV-14 — Comportamento do endpoint em ambiguidade de intenção entre dois BCs *(novo — A-02)*

**Associado a:** OUT-06, CN-09
**Cenário de teste:** Pergunta com intenção ambígua entre dois bounded contexts: "Qual é o prazo?" (sem contexto adicional que determine se é prazo de devolução, de entrega ou de SLA).
**Condição de aprovação:** O endpoint (a) não assume um único bounded context, (b) sinaliza a ambiguidade ao atendente ("A pergunta pode se referir a diferentes prazos dependendo do contexto"), (c) lista os prazos possíveis com seus contextos (prazo de devolução = BC1, 7 dias úteis; prazo de entrega = BC2, depende da rota + adicional; prazo de SLA = BC3, depende do tier) e (d) solicita esclarecimento para fornecer a informação correta.
**Reprovação automática:** Resposta que assume um único contexto sem solicitar esclarecimento.
**Variante:** Pergunta cross-context com roteamento RAG que recuperou trechos de BC errado. Verificar manualmente que a resposta não utiliza regras incompatíveis com o contexto da pergunta.

*(Novo — origem: A-02)*

### CV-15 — Comportamento em degradação e baixa confiança *(novo — A-06)*

**Associado a:** OUT-08, CN-13, CN-14
**Cenário 1 — confiança baixa (fonte informal):** Pergunta sobre carga danificada.
**Condição de aprovação:** A resposta exibe estado "confiança baixa", descreve o processo com base no FAQ-38, identifica a fonte como informal e inclui orientação de escalação ao supervisor.
**Cenário 2 — sem evidência (fora de escopo):** Pergunta sobre frete abaixo de 500 kg.
**Condição de aprovação:** A resposta exibe estado "sem evidência suficiente", informa que o tema está fora do escopo do corpus e orienta contato com o Comercial. Nenhum valor ou fórmula é apresentado.
**Cenário 3 — degradação por timeout (simulado):** Pipeline RAG configurado para retornar timeout.
**Condição de aprovação:** O endpoint retorna resposta com estado "sem evidência suficiente", informa que a consulta não pôde ser processada e exibe orientação de escalação ao supervisor. Não retorna erro técnico bruto.

*(Novo — origem: A-06, A-15)*

### CV-16 — Tier inválido não confirmado *(novo — A-10)*

**Associado a:** CN-08
**Cenário de exceção:** Pergunta "Sou cliente Platinum. Qual é o meu SLA?"
**Condição de aprovação:** A resposta (a) informa que o tier Platinum não existe na NovaTech, (b) lista os tiers válidos (Gold, Silver, Standard), (c) orienta o atendente a verificar o contrato para identificar o tier correto. A resposta não confirma SLA para tier Platinum.
**Reprovação automática:** Resposta que confirma o tier Platinum ou que apresenta qualquer SLA para ele.

*(Novo — origem: A-10)*

---

## 7. Non-goals

**NG-01 — Ingestão e processamento de documentos:** O Query Endpoint consome o corpus já processado pelo pipeline de RAG (ADR-0004). A atualização, chunking, embedding e indexação de documentos são responsabilidades do pipeline de ingestão.

**NG-02 — Autenticação e controle de acesso:** O gerenciamento de identidade de atendentes e clientes é responsabilidade de módulo externo.

**NG-03 — Execução de operações transacionais:** O endpoint informa e orienta; não abre chamados, não agenda coletas reversas, não contrata seguro, não processa reembolsos e não modifica registros operacionais.

**NG-04 — Cálculo do valor base de frete:** O valor base da tabela mensal de fretes não está disponível ao assistente. O endpoint informa a fórmula e os coeficientes; não calcula nem estima o valor final do frete.

**NG-05 — Suporte a idiomas além do português brasileiro:** Suporte multilíngue não está no escopo desta versão.

**NG-06 — Processo interno da Gestão de Riscos:** O que ocorre após o encaminhamento ao ramal 4500 está fora do escopo do assistente (lacuna L1).

**NG-07 — Cobertura de frete padrão (abaixo de 500 kg):** Sem documento normativo ingerido, o endpoint não simula nem estima esta modalidade (lacuna L5).

**NG-08 — Ação técnica de roteamento de chamada ao supervisor:** O endpoint exibe orientação de escalação ao supervisor como texto e elemento de interface; não executa roteamento técnico de chamada ou abertura de ticket. A ação do botão "Escalar ao supervisor" é responsabilidade do front-end.

*(Novo — origem: A-06)*

**NG-09 — Funcionalidade "Comparar versões":** O botão "Comparar versões" exibido no mockup (estado de conflito PROC-042) é de responsabilidade do front-end — executa um deep link para os documentos; não é ação do endpoint. O endpoint não executa nova consulta ao ser acionado por esse botão.

*(Novo — origem: A-11)*

---

## 8. Open questions

**OQ-01 — FECHADA** *(resolvida por CN-14 e NG-08)*
Comportamento em degradação e timeout: definido em CN-14. O endpoint retorna estado "sem evidência suficiente" com orientação de escalação ao supervisor. A ação do botão é responsabilidade do front-end (NG-08).

**OQ-02 — Definição de "carga normal de operação" para CV-01**
O percentil de carga (p50, p95) e o volume de requisições simultâneas que definem carga normal precisam ser definidos com base em dados do protótipo (ADR-0004) antes da homologação.

**OQ-03 — Formato estruturado da citação de fonte**
O constraint CN-02 exige citação de fonte, mas o formato (inline no texto, bloco separado, JSON de metadados) deve ser definido em conjunto com a equipe de front-end. Bloqueante para CV-03.

**OQ-04 — Comportamento para perguntas sobre PROC-088 (interceptação de carga em trânsito)**
POL-001 §2 referencia o PROC-088, que não está no corpus. Deve o endpoint informar a existência do PROC-088 sem descrevê-lo? Decisão requer confirmação sobre inclusão futura no corpus.

**OQ-05 — Nível de detalhe para perguntas de tier sem número de contrato**
Quando o cliente alegar um tier sem fornecer o número do contrato, o endpoint deve solicitar o número ativamente ou apenas informar os critérios e orientar verificação? A decisão afeta o fluxo de CV-12 e CV-16.

**OQ-06 — Prazo de 48 horas para registro de sinistro: dias corridos ou úteis?**
Interpretação atual: horas corridas. Deve ser validada pelo Jurídico antes da homologação de CV-07 *(Pendência DP-04)*.

**OQ-07 — FECHADA** *(resolvida por OQ-07-guardrails)*
Os guardrails adicionais do assistente não foram fornecidos como insumo desta versão. Quando disponibilizados, devem ser integrados ao sistema prompt (ADR-0001) e revisados neste documento.

**OQ-08 — Modo de renderização da interface: streaming vs. resposta bloqueada** *(nova — A-04)*
Se a interface adotar streaming, a métrica de CV-01 deve ser reformulada para time-to-first-token + tempo de resposta completa. Definição bloqueante para CV-01 *(Pendência DP-05)*.

**OQ-09 — Valor numérico do budget de tokens (ADR-0002)** *(nova — A-13)*
CN-07 não pode ser testado sem o valor numérico do budget. Responsável: Arquitetura. Deve ser resolvido antes da homologação de CN-07 *(Pendência DP-07)*.

---

## 9. Assumptions

**AS-01:** O pipeline de RAG retorna, para cada pergunta, os trechos documentais mais relevantes acompanhados do metadado de vigência, conforme ADR-0003. O endpoint não é responsável por inferir vigência.

**AS-02:** O corpus documental ingerido está limitado aos cinco documentos do Anexo A (POL-001, PROC-042 v1, PROC-042-v2, SLA-2024 e FAQ-Atendimento). Qualquer pergunta sobre tema não coberto é tratada como fora do escopo disponível.

**AS-03 *(revisado — A-02)*:** O roteamento de intenção para bounded context é executado antes ou durante a recuperação RAG, e o endpoint recebe trechos contextualizados por domínio. Quando o roteamento retornar contexto ambíguo ou ausente, o endpoint deve sinalizar a ambiguidade ao atendente (CV-14) em vez de gerar resposta com trechos de BC incorreto.

**AS-04 *(revisado — A-01)*:** O repositório oficial de documentos alimenta o pipeline de ingestão de forma automatizada. O prazo de 24 horas para atualização do corpus é SLA do pipeline, não do endpoint. CV-13 é um teste de integração; falha nele é imputada ao pipeline salvo evidência de cache de corpus pelo endpoint.

**AS-05:** Os seis bounded contexts são estáveis para esta versão. Novos contextos ou alterações de fronteira requerem revisão deste documento.

**AS-06:** As regras de desambiguação do Glossário v2 são incorporadas ao system prompt do endpoint (ADR-0001), incluindo o protocolo de nome coloquial de carga perigosa (Termo 24) e a independência entre triagem de BC1 e relógio de SLA de BC3 (Termo 4).

**AS-07:** Perguntas submetidas ao endpoint estão em português brasileiro. Não é assumido mecanismo de detecção ou tradução automática de idioma nesta versão.

---

## 10. Glossary dependencies

Este documento depende das definições canônicas estabelecidas no Glossário de Linguagem Ubíqua v2.

| Termo canônico | Contexto Dono | Relevância nesta especificação |
|---|---|---|
| Devolução de mercadoria | BC1 | SB-01, CN-09, CV-09 |
| Prazo de devolução | BC1 | SB-01, CV-02, CV-11 |
| Elegibilidade para devolução | BC1 | SB-01, CV-09, CV-10 |
| Triagem de chamado de devolução | BC1 | SB-01, CV-12, CN-07 |
| Coleta reversa | BC1 | SB-01 |
| Custo de devolução | BC1 | SB-01, CV-11 |
| Reembolso de devolução | BC1 | SB-01, CV-03 |
| Devolução parcial | BC1 | SB-01 |
| CT-e | BC1 | SB-01, CV-03 |
| Prazo expirado | BC1 | SB-01, CN-09 |
| Frete especial | BC2 | SB-02, CN-10, CV-09 |
| Valor base do frete | BC2 | SB-02, NG-04 |
| Multiplicador regional | BC2 | SB-02, CN-03, CV-05, CV-06, CV-11 |
| Fator de peso | BC2 | SB-02, CN-03, CV-05, CV-11 |
| Prazo adicional de frete especial | BC2 | SB-02, CN-03, CV-02, CV-06 |
| Desconto por volume de frete | BC2 | SB-02 |
| Aprovação para carga acima de 5.000 kg | BC2 | SB-02 |
| Tier de cliente | BC3 | SB-03, CN-08, CV-02, CV-12, CV-16 |
| SLA de primeira resposta | BC3 | SB-03, CV-02, CV-12 |
| SLA de resolução | BC3 | SB-03, CV-02 |
| Incidente crítico | BC3 | SB-03, CV-09 |
| Penalidade por violação de SLA | BC3 | SB-03 |
| Relógio de SLA | BC3 | SB-03, CV-12 |
| Carga perigosa | BC4 | SB-04, CN-09, CV-09, CV-10 |
| Cadeia de frio | BC4 | SB-04, CN-09, CV-10 |
| Lacre de segurança | BC4 | SB-04, CN-09 |
| Gestão de Riscos | BC4 | SB-04, CN-12, CV-09, CV-10 |
| Carga danificada | BC5 | SB-05, CV-07, CV-15 |
| Registro de ocorrência de sinistro | BC5 | SB-05, CV-07, OQ-06 |
| Ressarcimento por sinistro | BC5 | SB-05, CV-07 |
| Seguro de carga | BC6 | SB-06, CV-08, CV-15 |
