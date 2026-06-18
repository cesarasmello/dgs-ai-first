# Requirements — Query Endpoint do Assistente NovaTech

**Documento:** requirements.md
**Versão:** 1.0
**Data de emissão:** 2025-06-09
**Responsável:** Product Specialist — NovaTech
**Status:** Em revisão

---

## 1. Contexto do módulo

O Query Endpoint é o único ponto de entrada do assistente NovaTech para perguntas formuladas por atendentes e clientes finais sobre o domínio de operações da empresa. O módulo recebe uma pergunta em linguagem natural, executa recuperação de evidências no corpus documental via pipeline de RAG (ADR-0004), gera uma resposta fundamentada em linguagem natural por meio do modelo GPT-4o (ADR-0001) e a retorna com citação de fonte, identificação de vigência documental e, quando aplicável, sinalização de conflito ou lacuna.

O módulo opera sobre um domínio estruturado em seis bounded contexts — BC1 a BC6 — cujas fronteiras semânticas, termos canônicos e regras de desambiguação estão formalizados no Glossário de Linguagem Ubíqua consolidado. O comportamento do endpoint deve ser inteiramente derivável das regras de negócio documentadas nesses insumos; nenhuma regra pode ser inferida ou inventada pelo modelo fora das fontes ingeridas.

Este documento especifica exclusivamente o módulo Query Endpoint. Outros módulos do sistema NovaTech — pipeline de ingestão documental, autenticação, administração de corpus e interfaces de front-end — estão fora do escopo desta especificação.

---

## 2. Outcomes

Os outcomes a seguir descrevem resultados observáveis para o atendente e para o cliente final, não decisões de implementação.

### OUT-01 — Resposta objetiva dentro do prazo de atendimento

O atendente recebe uma resposta fundamentada e completa para a pergunta formulada em menos de 30 segundos a partir do envio, permitindo que o atendimento ao cliente prossiga sem quebra de fluxo.

**Critérios de verificação associados:** CV-01, CV-02.

### OUT-02 — Rastreabilidade total da resposta

Todo atendente e cliente final consegue identificar, em cada resposta, qual documento normativo ou fonte documental embasou cada afirmação, possibilitando auditoria e contestação fundamentada.

**Critérios de verificação associados:** CV-03, CV-04.

### OUT-03 — Respostas seguras em cenários de conflito documental

Quando dois documentos vigentes apresentam regras divergentes para o mesmo tema — especificamente o conflito entre PROC-042 v1 e PROC-042 v2 — o atendente recebe ambas as versões explicitadas, com indicação da versão a adotar como padrão para chamados novos, sem que o endpoint colapso os valores ou silencie o conflito.

**Critérios de verificação associados:** CV-05, CV-06.

### OUT-04 — Sinalização explícita de lacunas e ausência de alucinação

Quando a pergunta incide sobre tema sem documento normativo formal — BC5 (sinistros e carga danificada) e BC6 (seguro de carga) — o atendente recebe resposta com ressalva explícita sobre a origem informal da informação e orientação de confirmação com o setor competente, sem que o assistente afirme como normativo algo que não o é.

**Critérios de verificação associados:** CV-07, CV-08.

### OUT-05 — Roteamento correto de cargas especiais

Quando a pergunta envolve carga perigosa, carga refrigerada com suspeita de ruptura de cadeia de frio ou carga com lacre violado, o atendente recebe orientação imediata de encaminhamento ao contexto BC4 (Gestão de Riscos, ramal 4500), sem que o endpoint processe a solicitação como devolução ou frete padrão.

**Critérios de verificação associados:** CV-09, CV-10.

### OUT-06 — Respostas coerentes para perguntas cross-context

Para os 15% das perguntas que cruzam dois bounded contexts — por exemplo, devolução com carga acima de 500 kg (BC1 + BC2), ou SLA em chamado de devolução por desistência (BC1 + BC3) — o atendente recebe uma resposta que integra as regras dos dois contextos sem contradição interna e sem supressão de uma das regras.

**Critérios de verificação associados:** CV-11, CV-12.

### OUT-07 — Corpus documental sempre atualizado dentro do prazo contratual

Toda alteração documental no corpus de ingestão fica disponível para recuperação pelo endpoint em até 24 horas após a publicação do documento atualizado, garantindo que o atendente nunca opere com versão desatualizada por período superior ao contratual.

**Critérios de verificação associados:** CV-13.

---

## 3. Scope boundaries

As fronteiras abaixo derivam diretamente dos seis bounded contexts definidos no Mapa de Bounded Contexts do assistente NovaTech. Para cada fronteira, está explicitado o que o endpoint cobre e o que está excluído.

### SB-01 — BC1: Devolução de Mercadorias

**Coberto:** Regras de elegibilidade para devolução padrão, prazo de devolução (7 dias úteis), procedimento de triagem (4 horas úteis), coleta reversa (até 2 dias úteis após aprovação), custo de devolução por responsabilidade, reembolso de devolução (até 5 dias úteis após recebimento no CD), devoluções parciais por CT-e e prazo expirado com roteamento ao Comercial.

**Excluído:** Cálculo do valor numérico do frete reverso (pertence a BC2); decisão de elegibilidade de cargas perigosas, refrigeradas ou lacradas (pertence a BC4); apuração de responsabilidade por dano ocorrido em trânsito (pertence a BC5); mercadorias ainda em trânsito (PROC-088, fora do corpus disponível).

### SB-02 — BC2: Precificação de Frete Especial

**Coberto:** Fórmula de cálculo de frete especial (`valor_base × multiplicador_regional × fator_de_peso`), multiplicadores regionais e fatores de peso conforme PROC-042-v2, prazo adicional de entrega para frete especial (+3 dias úteis, v2), desconto por volume (5% a partir de 8 fretes/mês; 10% acima de 15 fretes/mês, v2), requisito de aprovação gerencial para cargas acima de 5.000 kg.

**Excluído:** Valor base da tarifa mensal (variável externa consultada no sistema interno, não disponível ao assistente); cálculo de frete para cargas abaixo de 500 kg (lacuna L5 — sem documento normativo); cálculo de frete para cargas perigosas acima de 500 kg (PROC-043, em revisão pelo Compliance); definição de SLA de atendimento do chamado de frete (pertence a BC3).

### SB-03 — BC3: Gestão de SLA e Atendimento

**Coberto:** Classificação de clientes em tiers (Gold, Silver, Standard) e critérios de elegibilidade por tier; SLA de primeira resposta e de resolução por tier para chamados gerais e incidentes críticos; quatro critérios objetivos de classificação de incidente crítico; penalidades por violação de SLA (1ª, 2ª e 3ª+ violações no mês); comportamento do relógio de SLA para chamados gerais e incidentes críticos Gold.

**Excluído:** Tiers não documentados (Platinum ou similares, descontinuados em 2022); alteração contratual de tier (competência do Comercial); regras de cálculo de frete (BC2); processo de triagem de devolução (BC1); decisão sobre conformidade de cargas perigosas (BC4).

### SB-04 — BC4: Conformidade e Riscos de Carga

**Coberto:** Identificação de cargas perigosas conforme classes 1 a 6 da ANTT (Resolução nº 5.947/2021); critério de ruptura de cadeia de frio (temperatura fora da faixa da nota fiscal por mais de 30 minutos contínuos, conforme sensor IoT); condição de exceção para lacre violado com documentação de entrega; encaminhamento obrigatório ao ramal 4500 (Gestão de Riscos) para cargas que não se enquadram no processo padrão.

**Excluído:** Processo interno da Gestão de Riscos após o ramal 4500 (lacuna L1 — sem PROC documentado); processo formal de autorização de frete expresso para carga perigosa (lacuna L6 — apenas FAQ-32 informal); tabela de frete PROC-043 para cargas perigosas (em revisão pelo Compliance).

### SB-05 — BC5: Sinistros e Carga Danificada

**Coberto:** Distinção entre carga danificada em trânsito e devolução padrão; prazo de registro de ocorrência (até 48 horas após recebimento); requisitos documentais do registro (fotos e laudo); encaminhamento ao e-mail sinistros@novatech.com.br; natureza do ressarcimento (integral, condicionado à comprovação de responsabilidade da NovaTech pelo Jurídico).

**Excluído:** Processo de investigação do Jurídico (não documentado no corpus disponível); prazo de conclusão da investigação (sem documento normativo — lacuna L2); garantia de ressarcimento (condicionada a investigação); devoluções por desistência do cliente (BC1).

**Restrição obrigatória:** Toda resposta neste boundary deve identificar explicitamente que a fonte é o FAQ-38 (documento informal, não validado por Compliance) e recomendar confirmação com o setor competente.

### SB-06 — BC6: Seguro de Carga

**Coberto:** Percentuais de seguro de carga como adicional contratual (0,3% para cargas padrão; 0,8% para cargas perigosas), com escopo restrito a contratos firmados a partir de 2023.

**Excluído:** Confirmação de percentual para contratos pré-2023 (competência do Comercial); contratação de seguro (não é função do assistente); processo de acionamento do seguro após sinistro (não documentado no corpus).

**Restrição obrigatória:** Toda resposta neste boundary deve identificar explicitamente que a fonte é o FAQ-22 (documento informal, não validado por Compliance), que os percentuais citados são válidos apenas para contratos a partir de 2023, e que contratos anteriores requerem confirmação com o Comercial.

---

## 4. Constraints

### CN-01 — Não alucinação

O endpoint não pode afirmar, inferir ou calcular qualquer valor, prazo, regra ou procedimento que não seja diretamente recuperável do corpus documental ingerido. Informações ausentes do corpus devem ser identificadas como fora do escopo disponível, com orientação de encaminhamento ao setor competente.

### CN-02 — Citação obrigatória de fonte

Toda afirmação factual contida na resposta deve ser acompanhada da identificação da fonte documental de origem (código do documento, seção e, quando disponível, parágrafo). Respostas sem citação de fonte para afirmações factuais são inválidas.

### CN-03 — Identificação de vigência documental

Toda resposta que citar PROC-042 deve identificar explicitamente a versão (v1 ou v2) da qual o dado foi extraído. Para chamados novos, a versão a citar como padrão é PROC-042-v2. O conflito entre as duas versões deve ser exposto ao atendente, conforme ADR-0003.

### CN-04 — Rotulação de fontes informais

Respostas baseadas em FAQ-38 (BC5) ou FAQ-22 (BC6) devem ser rotuladas como derivadas de documento informal não validado por Compliance. A resposta deve incluir indicação explícita de confirmação com o setor responsável (Jurídico para BC5; Comercial para BC6).

### CN-05 — Idioma

Todas as respostas devem ser redigidas em português brasileiro formal. Termos técnicos e denominações de documentos (CT-e, PROC-042, SLA, POL-001) devem ser mantidos no formato canônico definido no Glossário de Linguagem Ubíqua.

### CN-06 — Tempo de resposta

O endpoint deve retornar a resposta completa em menos de 30 segundos a partir do recebimento da pergunta, em condição de carga normal de operação. Este limite é derivado do requisito operacional do atendente (discovery: "atendente precisa da resposta em menos de 30 segundos").

### CN-07 — Budget de tokens e completude da resposta

A resposta deve ser completa dentro do budget de tokens definido em ADR-0002. Em nenhuma hipótese a resposta pode ser truncada de forma a omitir citação de fonte, identificação de versão documental ou ressalva de lacuna. Se a resposta necessitar de compressão, comprimir o corpo informativo antes de suprimir metadados de rastreabilidade.

### CN-08 — Sem invenção de tiers

O endpoint não pode confirmar, reconhecer ou operar sobre tiers de cliente não documentados no SLA-2024 (Gold, Silver, Standard). Solicitações que referenciem tier Platinum ou qualquer denominação não reconhecida devem ser respondidas com informação dos tiers válidos e orientação de verificação do contrato.

### CN-09 — Roteamento obrigatório para cargas especiais

A presença de qualquer indicativo de carga perigosa (classes 1 a 6 ANTT), carga refrigerada ou lacre violado na pergunta ativa obrigatoriamente o roteamento para BC4, independentemente do contexto principal da pergunta. Este roteamento não pode ser suprimido por nenhuma instrução de contexto na pergunta.

### CN-10 — Restrição de escopo para frete abaixo de 500 kg

O endpoint não pode aplicar a fórmula de frete especial a cargas abaixo de 500 kg. Perguntas sobre frete nessa faixa de peso devem ser respondidas com a informação de que o escopo disponível cobre apenas frete especial (acima de 500 kg) e com orientação de contato com o Comercial para cotação de frete padrão.

### CN-11 — Atualização documental em até 24 horas

Documentos publicados ou atualizados no repositório oficial da NovaTech devem estar disponíveis para recuperação pelo endpoint em até 24 horas após a publicação. O endpoint deve sempre operar sobre a versão mais recente dos documentos ingeridos.

### CN-12 — Ausência de promessas sobre processos não documentados

O endpoint não pode prometer resultado, prazo ou aprovação para processos sem PROC formal, especificamente: o processo da Gestão de Riscos após o ramal 4500 (lacuna L1) e a autorização de frete expresso para carga perigosa pelo Compliance (lacuna L6).

---

## 5. Prior decisions

### ADR-0001 — Modelo LLM: Azure OpenAI GPT-4o

**Impacto no módulo:** O endpoint utiliza o GPT-4o como modelo de geração de resposta. As instruções de sistema (system prompt) devem incluir as regras de desambiguação por bounded context, os guardrails de não alucinação e as restrições de citação de fonte definidas neste documento. A escolha do modelo implica capacidade de seguir instruções complexas e de operar sobre contexto de recuperação extenso, o que viabiliza o comportamento cross-context (OUT-06) sem degradação da precisão.

### ADR-0002 — Estratégia de contexto e budget de tokens

**Impacto no módulo:** O budget de tokens condiciona diretamente o constraint CN-07. A estratégia de contexto deve priorizar, na montagem do prompt, os trechos recuperados mais relevantes pelo pipeline de RAG, os metadados de vigência dos documentos e as regras de desambiguação dos bounded contexts envolvidos na pergunta. Quando o budget impuser compressão, os metadados de rastreabilidade (fonte, versão, ressalvas de lacuna) têm prioridade sobre o corpo informativo da resposta.

### ADR-0003 — Tratamento de documentos contraditórios com metadado de vigência

**Impacto no módulo:** O endpoint deve consumir o metadado de vigência atribuído a cada documento no pipeline de ingestão para determinar qual versão apresentar como padrão em situações de conflito. No caso específico do conflito PROC-042 v1 vs. PROC-042-v2, o metadado de vigência deve indicar PROC-042-v2 como documento padrão para chamados novos (a partir de 01/12/2023), e o endpoint deve expor o conflito ao atendente em vez de silenciá-lo (constraint CN-03, OUT-03). A ausência de metadado de vigência para um documento é tratada como lacuna e sinalizada na resposta.

### ADR-0004 — Pipeline de RAG e lições do protótipo

**Impacto no módulo:** O endpoint é dependente do pipeline de RAG para recuperar os trechos documentais que embasam a resposta. As lições do protótipo estabelecem que: (a) a recuperação deve ser guiada pelo bounded context identificado na pergunta, de forma a reduzir recuperação de trechos de contextos não relevantes; (b) perguntas cross-context devem disparar recuperação em múltiplos índices, um por bounded context envolvido; (c) a ausência de resultado relevante na recuperação é o sinal primário para acionar o comportamento de lacuna (CN-01, OUT-04), não a incerteza do modelo gerador.

---

## 6. Verification criteria

Os critérios abaixo são verificáveis por teste funcional e por teste de regressão. Cada critério identifica o outcome ou constraint associado, o cenário de teste (positivo ou de exceção) e a condição de aprovação mensurável.

### CV-01 — Tempo de resposta em carga normal

**Associado a:** OUT-01, CN-06
**Cenário positivo:** Pergunta de contexto único (ex.: "Qual o prazo para solicitar devolução?") submetida ao endpoint em ambiente de produção com carga normal.
**Condição de aprovação:** O endpoint retorna resposta completa em tempo menor que 30 segundos, medido do timestamp de recebimento da requisição ao timestamp de última palavra da resposta.
**Frequência:** Executado em suite de smoke test a cada deploy e monitorado continuamente em produção com alertas para p95 > 25 segundos.

### CV-02 — Completude da resposta para as quatro categorias primárias

**Associado a:** OUT-01
**Cenário positivo:** Uma pergunta representativa de cada uma das quatro categorias identificadas no discovery (prazos de entrega, regras de frete, política de devolução, SLAs) é submetida ao endpoint.
**Condição de aprovação:** Cada resposta contém: (a) a regra ou valor solicitado, (b) a citação de fonte, (c) a identificação de vigência documental quando aplicável. Nenhuma das quatro respostas pode estar truncada ou omitir qualquer um dos três elementos.

### CV-03 — Citação de fonte em toda afirmação factual

**Associado a:** OUT-02, CN-02
**Cenário positivo:** Pergunta sobre prazo de reembolso de devolução.
**Condição de aprovação:** A resposta cita explicitamente "POL-001 §3.3" (ou equivalente) como fonte. Resposta sem citação de fonte é reprovada automaticamente.
**Cenário de exceção:** Pergunta sobre tema sem documento normativo (ex.: seguro de carga).
**Condição de aprovação para exceção:** A resposta cita "FAQ-22" e inclui ressalva de que o documento é informal e não validado por Compliance.

### CV-04 — Ausência de afirmação sem fonte rastreável

**Associado a:** OUT-02, CN-01
**Cenário de exceção:** Pergunta sobre frete padrão (abaixo de 500 kg).
**Condição de aprovação:** O endpoint não apresenta valor ou fórmula de cálculo. A resposta informa que o escopo disponível cobre apenas frete especial (acima de 500 kg) e orienta contato com o Comercial. A ausência de dado inventado é verificada por revisão humana de uma amostra de 20 perguntas por sprint sobre temas de lacuna.

### CV-05 — Exposição do conflito PROC-042 v1 vs. v2

**Associado a:** OUT-03, CN-03
**Cenário positivo:** Pergunta "Qual o fator de peso para carga de 2.000 kg?"
**Condição de aprovação:** A resposta indica os dois valores (v1: 1.2; v2: 1.15), identifica as versões de origem de cada valor, informa que PROC-042-v2 é o padrão para chamados novos e recomenda confirmação com o Comercial para contratos anteriores a 01/12/2023.
**Reprovação automática:** Resposta que apresenta apenas um valor sem mencionar o conflito.

### CV-06 — Adoção da versão correta por padrão

**Associado a:** OUT-03, CN-03
**Cenário positivo:** Pergunta "Qual o multiplicador regional para entrega no Norte?"
**Condição de aprovação:** A resposta apresenta o valor da v2 (1.8) como padrão e menciona o valor da v1 (1.6) com a devida identificação de versão.
**Reprovação automática:** Resposta que apresenta apenas o valor da v1 sem ressalva.

### CV-07 — Rotulação de fonte informal em BC5

**Associado a:** OUT-04, CN-04
**Cenário positivo:** Pergunta "Como funciona o processo de carga danificada?"
**Condição de aprovação:** A resposta (a) descreve o processo conforme FAQ-38, (b) identifica explicitamente que a fonte é o FAQ-38, documento informal não validado por Compliance, (c) informa que o processo passa pelo Jurídico e (d) fornece o e-mail sinistros@novatech.com.br. Ausência de qualquer um dos quatro elementos reprova o teste.

### CV-08 — Rotulação de fonte informal em BC6

**Associado a:** OUT-04, CN-04
**Cenário positivo:** Pergunta "Qual o percentual de seguro de carga?"
**Condição de aprovação:** A resposta (a) cita os percentuais de FAQ-22 (0,3% e 0,8%), (b) identifica que a fonte é informal, (c) restringe a validade a contratos a partir de 2023 e (d) orienta confirmação com o Comercial para contratos anteriores. Ausência de qualquer um dos quatro elementos reprova o teste.

### CV-09 — Roteamento obrigatório de carga perigosa

**Associado a:** OUT-05, CN-09
**Cenário positivo:** Pergunta "Posso devolver uma carga de gás comprimido?"
**Condição de aprovação:** A resposta informa que carga de gás (classe 2 ANTT) não é elegível para devolução pelo processo padrão e orienta contato com a Gestão de Riscos pelo ramal 4500. A resposta não pode processar a solicitação como devolução padrão nem prometer resultado do processo de BC4.
**Cenário de exceção:** Pergunta sobre frete de carga perigosa (BC2 + BC4).
**Condição de aprovação para exceção:** A resposta informa que cargas perigosas acima de 500 kg seguem a PROC-043 (não a PROC-042) e encaminha para BC4, sem aplicar a fórmula de frete especial.

### CV-10 — Roteamento de carga refrigerada com cadeia rompida

**Associado a:** OUT-05, CN-09
**Cenário positivo:** Pergunta "O sensor registrou temperatura fora da faixa por 45 minutos. Posso solicitar devolução?"
**Condição de aprovação:** A resposta identifica o critério de ruptura de cadeia de frio (>30 minutos contínuos, sensor IoT), informa que a carga não é elegível para devolução padrão e encaminha ao ramal 4500 (Gestão de Riscos). Não pode confirmar ou negar elegibilidade sem verificar o registro do sensor IoT.

### CV-11 — Coerência em pergunta cross-context BC1 + BC2

**Associado a:** OUT-06
**Cenário positivo:** Pergunta "Quanto custa a devolução por desistência de uma carga de 1.500 kg com destino ao Nordeste?"
**Condição de aprovação:** A resposta (a) confirma que a devolução por desistência é elegível pelo processo padrão de BC1 (se dentro do prazo), (b) informa que o custo do frete reverso usa os multiplicadores do frete original (POL-001 §3.5), (c) apresenta o multiplicador regional do Nordeste (1.5, PROC-042-v2) e o fator de peso para 1.500 kg (1.15, PROC-042-v2), (d) cita ambas as fontes e (e) menciona o conflito v1/v2. A omissão de qualquer uma das regras de um dos dois contextos reprova o teste.

### CV-12 — Coerência em pergunta cross-context BC1 + BC3

**Associado a:** OUT-06
**Cenário positivo:** Pergunta de cliente Gold: "Abri um chamado de devolução agora. Quando recebo retorno?"
**Condição de aprovação:** A resposta informa (a) que o SLA de primeira resposta para Gold é de até 2 horas úteis (SLA-2024 §2) e (b) que a triagem do chamado de devolução ocorre em até 4 horas úteis (POL-001 §3.3), distinguindo claramente os dois prazos como métricas independentes. A resposta não pode colapsar os dois prazos em um único número.

### CV-13 — Atualização documental em até 24 horas

**Associado a:** OUT-07, CN-11
**Cenário de teste:** Um documento é publicado no repositório oficial em tempo T. Uma pergunta que só pode ser respondida com base nesse documento é submetida ao endpoint em T + 25 horas.
**Condição de aprovação:** O endpoint retorna resposta baseada no documento recém-publicado, citando-o corretamente. Resposta baseada na versão anterior do documento reprova o teste.
**Frequência:** Verificado após cada atualização documental relevante e como parte do processo de publicação de novos POLs e PROCs.

---

## 7. Non-goals

Os itens abaixo estão explicitamente fora do escopo deste módulo e não devem ser implementados nem assumidos como comportamento esperado.

**NG-01 — Ingestão e processamento de documentos:** O Query Endpoint consome o corpus já processado pelo pipeline de RAG (ADR-0004). A atualização, chunking, embedding e indexação de documentos são responsabilidades do pipeline de ingestão, não deste módulo.

**NG-02 — Autenticação e controle de acesso:** O gerenciamento de identidade de atendentes e clientes, incluindo validação de tier de cliente por autenticação, é responsabilidade de módulo externo ao endpoint.

**NG-03 — Execução de operações transacionais:** O endpoint informa e orienta; não abre chamados, não agenda coletas reversas, não contrata seguro, não processa reembolsos e não modifica registros em sistemas operacionais da NovaTech.

**NG-04 — Cálculo do valor base de frete:** O valor base da tabela mensal de fretes é uma variável externa armazenada em sistema interno (`\\novatech-fs\comercial\tabelas\frete-base-AAAAMM.xlsx`) não disponível ao assistente. O endpoint não calcula nem estima o valor final do frete especial — informa a fórmula e os coeficientes aplicáveis.

**NG-05 — Suporte a idiomas além do português brasileiro:** A especificação cobre exclusivamente respostas em português brasileiro formal. Suporte multilíngue não está no escopo desta versão.

**NG-06 — Processo interno da Gestão de Riscos:** O que ocorre após o encaminhamento ao ramal 4500 está fora do escopo do assistente e não deve ser descrito, inferido ou prometido (lacuna L1).

**NG-07 — Cobertura de frete padrão (abaixo de 500 kg):** Não há documento normativo ingerido que cubra frete para cargas abaixo de 500 kg. O endpoint não simula nem estima esta modalidade (lacuna L5).

---

## 8. Open questions

**OQ-01 — Timeout e comportamento de degradação**
Se o pipeline de RAG não retornar resultado dentro do budget de tempo compatível com o limite de 30 segundos (CN-06), qual deve ser o comportamento do endpoint? Deve retornar resposta parcial com aviso, retornar erro estruturado ou redirecionar para atendente humano? Decisão requer alinhamento com Arquitetura e Operações.

**OQ-02 — Definição de "carga normal de operação" para CV-01**
O critério CV-01 condiciona o SLA de 30 segundos à "carga normal de operação". O percentil de carga que define essa condição (p50, p95) e o volume de requisições simultâneas precisam ser definidos com base em dados de uso do protótipo (ADR-0004) antes da homologação.

**OQ-03 — Formato estruturado da citação de fonte**
O constraint CN-02 exige citação de fonte em toda afirmação factual, mas não prescreve o formato da citação (inline no texto, bloco separado, JSON de metadados). O formato deve ser definido em conjunto com a equipe de front-end para garantir renderização adequada na interface do atendente.

**OQ-04 — Comportamento para perguntas sobre PROC-088 (interceptação de carga em trânsito)**
POL-001 §2 referencia o PROC-088 para mercadorias em trânsito, mas este documento não está no corpus disponível. Deve o endpoint informar a existência do PROC-088 sem descrevê-lo, ou deve tratar perguntas sobre carga em trânsito como fora do escopo? A decisão requer confirmação sobre se PROC-088 será incluído em futura iteração do corpus.

**OQ-05 — Nível de detalhe para perguntas de tier sem número de contrato**
Quando o cliente alegar um tier sem fornecer o número do contrato, o endpoint deve solicitar o número ativamente ou apenas informar os critérios de elegibilidade e orientar verificação? O comportamento precisa ser alinhado com o time de atendimento para evitar atrito desnecessário no fluxo.

**OQ-06 — Prazo de 48 horas para registro de sinistro: dias corridos ou úteis?**
O FAQ-38 define o prazo de registro de ocorrência de sinistro como "até 48h após o recebimento", mas não especifica se são horas corridas ou úteis. Como não há documento normativo formal para BC5 (lacuna L2), a interpretação atual é de horas corridas. Esta interpretação deve ser validada pelo Jurídico antes da homologação.

**OQ-07 — Guardrails adicionais do assistente**
A entrada de discovery referencia a existência de guardrails já definidos para o assistente, mas estes não foram fornecidos como insumo desta especificação. Caso existam guardrails que restrinjam comportamentos adicionais além dos aqui definidos (por exemplo, restrições de tom, filtros de conteúdo ou limites de assunto), devem ser integrados a este documento na próxima iteração.

---

## 9. Assumptions

**AS-01:** O pipeline de RAG (ADR-0004) retorna, para cada pergunta, os trechos documentais mais relevantes acompanhados do metadado de vigência do documento de origem, conforme especificado em ADR-0003. O Query Endpoint não é responsável por inferir vigência; consome o metadado produzido pelo pipeline.

**AS-02:** O corpus documental ingerido está limitado aos cinco documentos identificados no Anexo A (POL-001, PROC-042 v1, PROC-042-v2, SLA-2024 e FAQ-Atendimento). Qualquer pergunta sobre tema não coberto por esses documentos é tratada como fora do escopo disponível, conforme CN-01.

**AS-03:** A identificação do bounded context relevante para cada pergunta é executada antes ou durante a etapa de recuperação do pipeline de RAG, de forma que o endpoint receba trechos contextualizados por domínio. O mapeamento de intenção para bounded context não é responsabilidade do módulo de geração do endpoint.

**AS-04:** O repositório oficial de documentos da NovaTech alimenta o pipeline de ingestão de forma automatizada ou semiautomatizada, de modo que o prazo de 24 horas para atualização do corpus (CN-11) é tecnicamente realizável sem intervenção manual por pergunta.

**AS-05:** Os seis bounded contexts definidos no Mapa de Bounded Contexts são estáveis para o escopo desta versão do assistente. Adição de novos contextos ou alteração das fronteiras existentes requer revisão desta especificação.

**AS-06:** As regras de desambiguação por termo canônico definidas no Glossário de Linguagem Ubíqua são incorporadas ao system prompt do endpoint como restrições de comportamento, de forma que o modelo GPT-4o (ADR-0001) opere dentro dos limites semânticos de cada bounded context.

**AS-07:** Perguntas submetidas ao endpoint estão em português brasileiro. Não é assumido mecanismo de detecção ou tradução automática de idioma nesta versão.

---

## 10. Glossary dependencies

Este documento depende das definições canônicas estabelecidas no Glossário de Linguagem Ubíqua do assistente NovaTech. Os termos abaixo são utilizados nesta especificação com o significado preciso definido no glossário; qualquer alteração no glossário que modifique a definição oficial de um desses termos requer revisão deste documento.

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
| Multiplicador regional | BC2 | SB-02, CN-03, CV-05, CV-06, CV-11 |
| Fator de peso | BC2 | SB-02, CN-03, CV-05, CV-11 |
| Prazo adicional de frete especial | BC2 | SB-02, CN-03, CV-06 |
| Desconto por volume de frete | BC2 | SB-02 |
| Aprovação para carga acima de 5.000 kg | BC2 | SB-02 |
| Tier de cliente | BC3 | SB-03, CN-08, CV-02, CV-12 |
| SLA de primeira resposta | BC3 | SB-03, CV-02, CV-12 |
| SLA de resolução | BC3 | SB-03, CV-02 |
| Incidente crítico | BC3 | SB-03, CV-09 |
| Penalidade por violação de SLA | BC3 | SB-03 |
| Relógio de SLA | BC3 | SB-03, CV-12 |
| Carga perigosa | BC4 | SB-04, CN-09, CV-09, CV-10 |
| Cadeia de frio | BC4 | SB-04, CN-09, CV-10 |
| Lacre de segurança | BC4 | SB-04, CN-09 |
| Gestão de Riscos | BC4 | SB-04, CN-12, CV-09, CV-10 |
| Carga danificada | BC5 | SB-05, CV-07 |
| Registro de ocorrência de sinistro | BC5 | SB-05, CV-07, OQ-06 |
| Ressarcimento por sinistro | BC5 | SB-05, CV-07 |
| Seguro de carga | BC6 | SB-06, CV-08 |
