## Tarefa 1: Avaliação do Analista:

| # | Pergunta | Resposta | Classificação | Justificativa |
|---|----------|---------|--------------|-----------|
| 1 | "Qual o prazo de devolução para produtos standard?" | "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." | Correta | - |
| 2 | "Meu cliente é Silver. Qual o prazo de resolução?" | "O cliente Silver tem resolução em até 48h." | Parcialmente correta | Tempo de resolução para chamados gerais é de 48h e para incidentes críticos é de 4h |
| 3 | "Posso devolver carga perigosa classe 3?" | "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." | Parcialmente correta | De fato, cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão, esta resposta esta correnta. Entretanto, o cliente deve entrar em contato com o setor de Gestão de Riscos (ramal 4500) para tratamento individual e não escalar para o supervisor. |
| 4 | "Qual a política para carga danificada durante transporte?" | "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." | Incorreta | A única referência disponível é o FAQ informal (item 38), que descreve um processo diferente e não sustenta a resposta gerada. Por não ser um documento formal e revisado, não possul alta confiança |
| 5 | "Qual o SLA do cliente Enterprise?" | "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." | Correta | - |
| 6 | "Posso enviar carga perigosa com frete expresso?" | "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." | Parcialmente correta | Faltou avisar o cliente que na prática, demora uns 2 dias para conseguir a autorização, então o 'expresso' acaba não sendo tão expresso. Além da classificação estar incorreta, deveria ser "baixa" por se tratar de um documento informal |

---

## Tarefa 2: Avaliação do Claude: 

| # | Pergunta | Resposta | Classificação | Justificativa | Evidência na documentação |
|---|----------|---------|--------------|-----------|-----------|
| 1 | "Qual o prazo de devolução para produtos standard?" | "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." | Parcialmente correta | O prazo de 7 dias úteis e a abertura de chamado no portal estão corretos. No entanto, a resposta omite requisitos obrigatórios do chamado: além das fotos, é exigido o número do CT-e (Conhecimento de Transporte Eletrônico) e a especificação do motivo da devolução. A omissão desses itens pode levar o cliente a abrir um chamado incompleto, causando retrabalho | POL-001, seção 3.1 (prazo) e seção 3.3, itens 1 e 2 (procedimento completo: Portal, CT-e, mínimo 3 fotos com especificação das vistas, motivo) |
| 2 | "Meu cliente é Silver. Qual o prazo de resolução?" | "O cliente Silver tem resolução em até 48h." | Parcialmente correta | O prazo de 48h está correto, mas apenas para chamados gerais. A resposta não distingue o tipo de chamado: para incidentes críticos, o SLA de resolução do Silver é de até 8h — prazo significativamente diferente. A resposta sem essa distinção pode criar expectativa incorreta tanto para o atendente quanto para o cliente. | SLA-2024, seção 2, Tabela de SLAs: "Tempo de resolução (chamados gerais) — Silver: Até 48h úteis" e "Tempo de resolução (incidentes críticos) — Silver: Até 8h" |
| 3 | "Posso devolver carga perigosa classe 3?" | "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." | Parcialmente correta | A inelegibilidade para o processo padrão está correta. A restrição às classes 1 a 6 também está correta. Porém, o encaminhamento está impreciso: a documentação determina que o cliente deve contatar o setor de Gestão de Riscos pelo ramal 4500, não "escalar para o supervisor", que é um direcionamento interno e não o canal correto para o cliente. | POL-001, seção 3.2: "o cliente deve entrar em contato com o setor de Gestão de Riscos (ramal 4500) para tratamento individual" |
| 4 | "Qual a política para carga danificada durante transporte?" | "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." | Incorreta | Não existe documento formal (POL ou PROC) na base de conhecimento que trate de carga danificada em trânsito. A única referência disponível é o FAQ-Atendimento (item 38), que é um documento informal, não validado pelo Compliance ou Operações, e que descreve um processo diferente do apresentado (exige registro em 48h, encaminhamento para sinistros@novatech.com.br e envolvimento do Jurídico — não do atendimento normal). A resposta apresentou informação como "política" com confiança alta sem base em documento normativo. | FAQ-Atendimento, item 38 (único registro disponível, informal); Notas da documentação, "Gaps identificados", item 1: "não existe documento formal (POL ou PROC) sobre tratamento de carga danificada em trânsito" |
| 5 | "Qual o SLA do cliente Enterprise?" | "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." | Correta | A resposta identifica corretamente a ausência do tier Enterprise, lista os três tiers existentes com precisão e orienta adequadamente. Alinhada ao comportamento esperado de um assistente RAG bem calibrado: não inventa informação ausente e sinaliza incerteza com confiança baixa. | SLA-2024, seção 1: "A NovaTech classifica seus clientes em 3 (três) tiers" (Gold, Silver, Standard) e nota: "Não existem outros tiers além dos três listados acima." |
| 6 | "Posso enviar carga perigosa com frete expresso?" | "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." | Incorreta | Embora o FAQ-Atendimento (item 32) mencione essa prática, trata-se de um documento explicitamente classificado como informal, não validado pelo Compliance ou Operações. Não existe nenhum PROC ou POL formal que defina esse processo. A resposta foi apresentada com confiança alta e sem qualquer ressalva, o que é inadequado para uma informação sem respaldo normativo. O correto seria indicar que não há política formal documentada e recomendar confirmação com o Compliance antes de orientar o cliente. | FAQ-Atendimento, item 32 (única fonte, informal); Notas da documentação, "Contradições identificadas", item 4: "não existe documento formal (PROC ou POL) que defina esse processo. A informação pode ser prática informal não documentada." |

- **Padrão de risco identificado:** os erros mais graves (#4 e #6) têm origem na mesma raiz — o assistente tratou o FAQ-Atendimento como fonte normativa equivalente a POL/PROC/SLA, ignorando o aviso explícito de que o FAQ é informal e não validado. Isso indica necessidade de ajuste no pipeline de RAG, seja por metadado de confiabilidade por documento, seja por instrução explícita nos guardrails para tratar fontes informais com confiança degradada. O item #3 aponta falha de encaminhamento que pode comprometer a experiência do cliente diretamente.

### Comparacão: 

**Análise das convergências:**

Os itens 2, 3, 4 e 5 produziram a mesma classificação em ambas as avaliações, com justificativas alinhadas:

- Item 2: Ambos identificaram corretamente que a resposta omite o SLA de incidentes críticos. O analista detalhou o valor como 4h; a IA apontou 8h. Há uma pequena divergência factual interna aqui: o SLA-2024 registra 8h para resolução de incidentes críticos Silver — a IA está correta nesse detalhe específico.

- Item 3: Convergência total na lógica — restrição ao processo padrão correta, encaminhamento incorreto por substituir o ramal 4500 (Gestão de Riscos) por "escalar para o supervisor".

- Item 4: Convergência total. Ambos identificaram ausência de documento normativo e inadequação do FAQ informal como fonte para uma resposta afirmativa com confiança alta.

- Item 5: Convergência total. Resposta do assistente corretamente calibrada — recusa informação inexistente e orienta o atendente.

**Análise das divergências:**

- Item 1 — Analista: Correta | IA: Parcialmente correta
    
    O analista classificou a resposta como correta sem justificativa registrada. A IA identificou omissão de dois campos obrigatórios do chamado previstos na POL-001, seção 3.3, item 2: o número do CT-e e o motivo da devolução.

    Posição sustentada pela documentação: a IA está mais aderente à fonte. A POL-001 lista três requisitos explícitos para abertura do chamado; a resposta do assistente menciona apenas um (fotos). A omissão é verificável diretamente na seção 3.3.

    Hipótese sobre a divergência: o analista provavelmente avaliou a resposta sob uma lógica de suficiência prática — o prazo e o canal estão corretos, e a menção a fotos sinaliza que o cliente precisa documentar a solicitação. Sob essa ótica, a resposta "funciona" para o atendimento. A IA aplicou uma lógica de completude normativa: comparou a resposta item a item contra a lista documentada e sinalizou tudo que estava ausente. A diferença não é de erro de leitura, mas de critério de avaliação: suficiência operacional (analista) versus aderência total à fonte (IA). Para um sistema RAG cujo papel é instruir atendentes com precisão, o critério da IA é mais conservador e mais adequado ao risco.


- Item 6 — Analista: Parcialmente correta | IA: Incorreta

    O analista considerou a resposta parcialmente correta porque o conteúdo central (pode enviar com autorização do Compliance e documentação ANTT) está presente no FAQ informal, e a lacuna apontada foi a ausência do aviso prático sobre o prazo real de 2 dias para obter a autorização. A crítica ao nível de confiança ("deveria ser baixa") foi registrada como observação separada, não como fator de classificação.

    A IA classificou como incorreta com base em dois argumentos: (a) a fonte é informalmente classificada e não validada por nenhum documento normativo; (b) a informação pode representar prática informal não documentada, e apresentá-la com confiança alta é o cenário de maior risco operacional e regulatório da série — carga perigosa + processo sem respaldo formal.

    Posição sustentada pela documentação: ambas as leituras têm respaldo parcial. O FAQ item 32 existe e contém a informação. A nota de contradições da própria documentação registra explicitamente que "não existe documento formal (PROC ou POL) que defina esse processo". A divergência não é sobre o que a documentação diz — é sobre o peso que cada avaliador atribui à ausência de formalização.

    Hipótese sobre a divergência: o analista partiu do conteúdo disponível e avaliou o que faltou na resposta — a ressalva prática sobre o prazo real. A IA partiu da hierarquia das fontes e avaliou se a resposta deveria ter sido gerada naquele formato. São perguntas diferentes: "a resposta está incompleta?" (analista) versus "a resposta deveria existir com essa estrutura?" (IA). A IA aplicou um padrão mais restritivo porque seu prompt instrui a classificar como incorreta qualquer resposta sem evidência em documento normativo — e esse critério, quando aplicado a um FAQ explicitamente marcado como informal, produz classificação mais severa do que um analista que pondera o valor prático da informação disponível.

---

## Tarefa 4: Análise de erros e propostas de ajuste

### Item 1 — Prazo de devolução para produtos standard

**Classificação:** Parcialmente correta  
**Tipo de erro:** Informação incompleta

**Justificativa do erro:**

A resposta informa corretamente o prazo de 7 dias úteis e a necessidade de abertura de chamado no portal. No entanto, omite dois campos obrigatórios do chamado: o **número do CT-e** (Conhecimento de Transporte Eletrônico) e o **motivo da devolução**.

A POL-001, seção 3.3, item 2, lista três requisitos explícitos para abertura do chamado:

1. Número do CT-e
2. Fotos da mercadoria (mínimo 3, com especificação das vistas: embalagem externa, etiqueta de identificação e conteúdo)
3. Motivo da devolução

A resposta cobre apenas um deles (fotos), deixando o atendente com instrução incompleta que levará o cliente a abrir um chamado incompleto e sujeito a retrabalho na triagem.

**Evidência:** POL-001, seção 3.1 (prazo) e seção 3.3, itens 1 e 2 (procedimento completo).

**Ajuste proposto — Prompt:**

Incluir instrução explícita no system prompt determinando que, ao responder sobre procedimentos com etapas ou campos obrigatórios, o assistente deve reproduzir a lista completa conforme documentada, sem omitir itens.

**Exemplo de instrução:**

> *"Quando a resposta envolver um procedimento com múltiplos requisitos obrigatórios, liste todos os itens conforme a documentação. Não resuma ou omita campos."*

**Por que previne o erro:** Força o modelo a tratar listas de requisitos como atômicas — não como exemplos ilustrativos —, eliminando a tendência de sumarização que descarta campos considerados secundários pelo modelo mas obrigatórios pela documentação.

---

### Item 2 — SLA de resolução do cliente Silver

**Classificação:** Parcialmente correta  
**Tipo de erro:** Informação incompleta

**Justificativa do erro:**

A resposta apresenta o SLA de resolução de 48h, que está correto para **chamados gerais**. No entanto, omite que o mesmo cliente Silver possui SLA de resolução de **apenas 8h para incidentes críticos** — prazo seis vezes menor, com impacto contratual direto.

O SLA-2024, seção 2, distingue explicitamente as duas modalidades:

| Métrica | Silver |
|---------|--------|
| Tempo de resolução (chamados gerais) | Até 48h úteis |
| Tempo de resolução (incidentes críticos) | Até 8h |

A ausência dessa distinção na resposta pode gerar cumprimento incorreto de prazo e configurar violação de SLA.

**Evidência:** SLA-2024, seção 2 — Tabela de SLAs.

**Ajuste proposto — Pipeline:**

Implementar etapa de detecção de ambiguidade antes da geração da resposta. Quando a pergunta envolver um termo que possui múltiplas classificações na documentação (ex: "prazo de resolução" com variações por tipo de chamado), o pipeline deve recuperar e injetar no contexto todos os chunks relevantes para aquele termo antes de gerar a resposta.

**Conexão com o Cenário 2:** Este ajuste operacionaliza os guardrails G-04 e G-10 de `guardrails-assistente-novatech.md` e os outcomes OUT-06/OUT-08 de `requirements-v2.md`, reforçando no pipeline RAG a expansão de recuperação e o tratamento explícito de ambiguidade e baixa confiança.

Complementarmente, adicionar metadado de cobertura nos chunks do SLA-2024 para que a combinação `Silver + resolução` recupere obrigatoriamente tanto a linha de chamados gerais quanto a de incidentes críticos.

**Por que previne o erro:** Desacopla a responsabilidade de completude da resposta do comportamento de sumarização do modelo. O pipeline garante que o contexto injetado já contenha todas as variações relevantes, reduzindo a probabilidade de o modelo omitir uma delas durante a geração.

---

### Item 3 — Devolução de carga perigosa classe 3

**Classificação:** Parcialmente correta  
**Tipo de erro:** Informação incompleta

**Justificativa do erro:**

A restrição ao processo padrão está correta e bem fundamentada. O erro está no **encaminhamento**: a documentação define um canal específico voltado ao cliente — **Gestão de Riscos, ramal 4500**. A resposta orientou "escalar para o supervisor", que é um procedimento interno sem correspondência com o canal documentado.

Para o cliente, a diferença é concreta: um ramal de contato direto versus uma instrução opaca de escalação interna que não o direciona a lugar nenhum.

**Evidência:** POL-001, seção 3.2:

> *"o cliente deve entrar em contato com o setor de Gestão de Riscos (ramal 4500) para tratamento individual."*

**Ajuste proposto — Prompt:**

Adicionar instrução no system prompt para que, em situações de exceção ou inelegibilidade, o assistente sempre inclua o canal de tratamento alternativo conforme documentado — e não substitua por generalizações como "escalar" ou "falar com um especialista".

**Exemplo de instrução:**

> *"Ao informar que uma solicitação não é elegível pelo processo padrão, sempre informe o canal alternativo documentado (nome do setor, ramal ou e-mail), se disponível na fonte. Não substitua canais documentados por orientações genéricas de escalação."*

**Por que previne o erro:** Ancora o comportamento do modelo à informação concreta disponível na fonte, impedindo que ele substitua dados específicos (ramal, e-mail, setor) por formulações vagas que parecem corretas mas não orientam o cliente de forma acionável.

---

### Item 4 — Política para carga danificada durante transporte

**Classificação:** Incorreta  
**Tipo de erro:** Alucinação

**Justificativa do erro:**

A resposta apresentou como "política" uma informação que existe **apenas no FAQ-Atendimento, item 38** — documento classificado explicitamente na documentação como informal e não validado por Compliance ou Operações. 

Pelos critérios da skill de avaliação, este caso é melhor classificado como **alucinação** porque a fonte citada foi "Nenhuma". Isso significa que o modelo não apenas tratou uma fonte fraca como se fosse normativa; ele respondeu sem evidência recuperada no fluxo.

A distinção é importante: **fonte não confiável** se aplica quando há recuperação de um documento de baixa autoridade, como um FAQ informal, e o sistema falha em degradar a confiança. Já **alucinação** se aplica quando não há recuperação suportando a resposta e, ainda assim, o modelo produz conteúdo afirmativo.

Além disso, a resposta diverge até do próprio FAQ utilizado como fonte:

| Aspecto | Resposta do assistente | FAQ-Atendimento, item 38 |
|---------|------------------------|--------------------------|
| Prazo para registro | Não mencionado | Até 48h após o recebimento |
| Canal de encaminhamento | Não mencionado | sinistros@novatech.com.br |
| Área responsável | Implícito: atendimento normal | Jurídico — fora do atendimento normal |

A confiança "Alta" atribuída à resposta agrava o risco: sinaliza ao atendente que a informação é segura quando não há nenhum documento normativo que a sustente.

**Evidência:** FAQ-Atendimento, item 38 (única fonte disponível, informal); Notas da documentação — Gaps identificados, item 1:

> *"não existe documento formal (POL ou PROC) sobre tratamento de carga danificada em trânsito."*

**Ajuste proposto — Pipeline:**

Implementar classificação de confiabilidade por documento no momento da ingestão, atribuindo um nível como metadado de cada chunk:

**Conexão com o Cenário 2:** Este ajuste operacionaliza os guardrails G-01, G-08 e G-13 de `guardrails-assistente-novatech.md` e as constraints CN-01, CN-04 e CN-13 de `requirements-v2.md`, separando ausência de evidência de uso controlado de fonte informal.

| Nível | Exemplos |
|-------|----------|
| `normativo` | POL-001, PROC-042, PROC-042-v2 |
| `contratual` | SLA-2024 |
| `informal` | FAQ-Atendimento |

No momento da recuperação, quando a **única fonte disponível for de nível `informal`**, o pipeline deve:

1. Rebaixar automaticamente o score de confiança da resposta.
2. Bloquear a geração de resposta afirmativa sobre processos ou políticas.
3. Forçar a inclusão de aviso explícito ao atendente de que a informação não possui respaldo normativo.

**Por que previne o erro:** Desacopla a existência de informação no corpus da autoridade dessa informação. O pipeline passa a usar a qualidade da fonte — não apenas a relevância semântica — como critério de decisão sobre o que gerar e como apresentar.

---

### Item 6 — Envio de carga perigosa com frete expresso

**Classificação:** Incorreta  
**Tipo de erro:** Fonte não confiável

**Justificativa do erro:**

Mesmo padrão do Item 4: a resposta apoia-se exclusivamente no **FAQ-Atendimento, item 32**, ignorando que a própria documentação registra essa como uma contradição conhecida.

Não existe nenhum PROC ou POL que formalize o envio de carga perigosa com frete expresso. A resposta foi gerada com confiança alta e **sem nenhuma ressalva**, induzindo o atendente a orientar o cliente com aparente certeza sobre um processo que pode não existir formalmente.

Este é o cenário de maior risco operacional e regulatório: carga perigosa + processo não documentado + resposta afirmativa com alta confiança.

**Evidência:** FAQ-Atendimento, item 32 (única fonte, informal); Notas da documentação — Contradições identificadas, item 4:

> *"não existe documento formal (PROC ou POL) que defina esse processo. A informação pode ser prática informal não documentada."*

**Ajuste proposto — Interface:**

Exibir junto à resposta um **componente visual de alerta** sempre que a fonte recuperada for classificada como `informal` ou quando não houver nenhum documento normativo cobrindo o tema.

**Conexão com o Cenário 2:** Este ajuste operacionaliza a regra de `AGENTS.v2.md` que exige rotular conteúdo derivado apenas do FAQ como "informação não normativa, sujeita a confirmação" e concretiza os outcomes OUT-04/OUT-08 e as constraints CN-04/CN-14 de `requirements-v2.md` na camada de interface.

O alerta deve ser visível ao **atendente** (não ao cliente final) e conter:

- Classificação da fonte utilizada (ex: `⚠️ Fonte: FAQ informal — não validado por Compliance`)
- Aviso de ausência de respaldo normativo
- Instrução de confirmar com a área responsável antes de repassar ao cliente

**Por que previne o erro:** Preserva a utilidade da resposta — o atendente ainda vê a informação disponível — sem induzir falsa segurança. Transfere o julgamento final para o humano quando a base documental é insuficiente, sem bloquear o fluxo de atendimento.

---

### Padrão consolidado de erros

**Análise de raiz:**

- **Erros de informação incompleta (Itens 1, 2, 3)**  
Têm origem no comportamento de sumarização do modelo: ele tende a simplificar respostas corretas em vez de preservar listas e distinções completas da fonte. Os ajustes propostos via Prompt (itens 1 e 3) e Pipeline (item 2) atacam essa raiz de formas complementares — o Prompt instrui o modelo a não resumir requisitos obrigatórios; o Pipeline garante que o contexto injetado já contenha todas as variações relevantes do dado consultado.

- **Erro de alucinação (Item 4)**  
Tem origem na geração de conteúdo sem evidência recuperada no fluxo, apesar da resposta ter sido apresentada em formato afirmativo e com alta confiança. O ajuste proposto via Pipeline (item 4) atua nesse ponto ao exigir tratamento explícito para casos sem respaldo recuperado, evitando que o modelo transforme lacunas de recuperação em respostas categóricas.

- **Erro de fonte não confiável (Item 6)**  
Tem origem na ausência de diferenciação de autoridade das fontes no pipeline: o sistema trata FAQ informal e POL normativa como equivalentes no momento da recuperação e geração. Os ajustes propostos via Pipeline e Interface atuam de forma complementar — o Pipeline impede que fontes informais gerem respostas afirmativas sobre políticas; a Interface torna visível ao atendente quando a fonte utilizada não possui respaldo normativo.

> **Recomendação:** Os dois ajustes de Pipeline propostos (itens 2 e 4) podem ser implementados de forma integrada, pois compartilham a mesma infraestrutura de metadados. A implementação do metadado de confiabilidade por documento (item 4) habilita, sem custo adicional, a lógica de cobertura completa por tipo de chamado (item 2).
