# Glossário de Linguagem Ubíqua — Assistente NovaTech

**Versão:** 2.0
**Data:** 2025-06-09
**Origem da versão anterior:** glossario-linguagem-ubiqua-v1.md
**Revisão aplicada:** analise-de-ambiguidade.md (Tech Lead)
**Achados aplicados:** A-07 (Termo 24 — Carga perigosa), A-14 (Termo 4 — Triagem)

> **Princípio de uso:** Este glossário define o vocabulário canônico do domínio NovaTech para uso pelo assistente de IA. As definições têm precedência sobre interpretações coloquiais. Em caso de conflito entre contextos, a separação semântica por Contexto Dono deve ser preservada. Termos marcados como **Lacuna** não devem ser respondidos com confiança plena sem ressalva explícita.

---

## Seção 1: Resumo de cobertura por contexto

| Contexto | Quantidade de termos | Termos críticos | Lacunas |
|---|---|---|---|
| BC1 — Devolução de Mercadorias | 10 | Prazo de devolução, Elegibilidade, Coleta reversa, Triagem, Reembolso, Frete reverso, CT-e, Devolução parcial | 1 (data-base quando tracking falha) + 1 nova (pausa da triagem fora do horário comercial — L7) |
| BC2 — Precificação de Frete Especial | 7 | Frete especial, Valor base, Multiplicador regional, Fator de peso, Desconto por volume, Prazo adicional | 2 (versão vigente PROC-042, frete padrão) |
| BC3 — Gestão de SLA e Atendimento | 6 | Tier de cliente, SLA de resposta, SLA de resolução, Incidente crítico, Penalidade por violação, Relógio de SLA | 0 |
| BC4 — Conformidade e Riscos de Carga | 4 | Carga perigosa *(atualizado v2)*, Cadeia de frio, Lacre de segurança, Gestão de Riscos | 1 (processo pós-ramal 4500) |
| BC5 — Sinistros e Carga Danificada | 3 | Carga danificada, Registro de ocorrência, Sinistro, Responsabilidade da NovaTech | 1 (ausência de POL/PROC formal) + 1 (prazo 48h corridas vs. úteis — DP-04) |
| BC6 — Seguro de Carga | 1 | Seguro de carga, Percentual de seguro | 1 (ausência de documento normativo) |
| **Total** | **31** | **—** | **7 (6 originais + 1 nova L7)** |

---

## Seção 2: Glossário de linguagem ubíqua

---

### BC1 — Devolução de Mercadorias

---

#### Termo 1: Devolução de mercadoria

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Devolução de mercadoria |
| **Definição oficial** | Solicitação formal feita pelo cliente para retornar ao centro de distribuição da NovaTech uma mercadoria já entregue, iniciada exclusivamente pelo Portal do Cliente, dentro do prazo de 7 dias úteis a partir da data de recebimento confirmada no sistema de tracking. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC2 (custo do frete reverso), BC4 (elegibilidade de cargas especiais), BC5 (distinção com sinistro) |
| **Fora de escopo** | Não se aplica a mercadorias ainda em trânsito (ver PROC-088); não se aplica a carga danificada em trânsito (BC5); não se aplica a cargas perigosas, refrigeradas com cadeia rompida ou lacradas sem documentação (BC4). |
| **Regra de desambiguação para LLM** | Se o cliente mencionar carga danificada *durante o transporte*, não tratar como devolução — redirecionar para BC5 (Sinistros). Se mencionar carga perigosa ou descrever produto potencialmente perigoso por nome coloquial, não processar como devolução padrão — verificar classificação ANTT e redirecionar para BC4. |
| **Exemplo válido** | Cliente recebeu produto errado em 10/06/2025 e abre chamado em 15/06/2025 (3 dias úteis depois): devolução elegível, sem custo ao cliente. |
| **Confusão comum a evitar** | Não confundir com "sinistro" (carga danificada em trânsito): devolução ocorre após entrega bem-sucedida; sinistro é apurado pelo Jurídico. |
| **Sinônimos/variantes** | Solicitação de devolução, retorno de mercadoria, coleta reversa (refere-se à etapa logística, não ao ato da solicitação) |
| **Evidência textual** | POL-001 §1: "define as regras e procedimentos para devolução de mercadorias transportadas pela NovaTech, aplicável a todos os tipos de cliente e categorias de carga, salvo exceções." |
| **Status** | Definido |

---

#### Termo 2: Prazo de devolução

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Prazo de devolução |
| **Definição oficial** | Janela de 7 (sete) dias úteis, contada a partir da data de recebimento confirmada no sistema de tracking, durante a qual o cliente pode solicitar devolução pelo processo padrão. Sábados, domingos e feriados nacionais não são contados. Após esse prazo, a solicitação não é elegível para devolução padrão e deve ser encaminhada ao Comercial. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC3 (tracking é responsabilidade de SLA de disponibilidade do portal) |
| **Fora de escopo** | Não se confunde com prazo de entrega (BC2) nem com prazo de SLA de atendimento (BC3). |
| **Regra de desambiguação para LLM** | Quando o cliente perguntar "qual é o prazo?", identificar o contexto: prazo de *solicitar* devolução = 7 dias úteis (BC1); prazo de *entrega* do frete = prazo padrão da rota + adicional (BC2); prazo de *atendimento* do chamado = SLA por tier (BC3). |
| **Exemplo válido** | Recebimento confirmado na segunda-feira (dia 1). Prazo expira na quarta-feira da semana seguinte (dia 7 útil). |
| **Confusão comum a evitar** | "7 dias" não são dias corridos — são dias úteis. Não confundir com prazo de triagem (4 horas úteis) nem com prazo de coleta reversa (2 dias úteis). |
| **Sinônimos/variantes** | Janela de devolução, período de devolução |
| **Evidência textual** | POL-001 §3.1: "O cliente pode solicitar a devolução de mercadorias em até 7 (sete) dias úteis após a data de recebimento confirmada no sistema de tracking." |
| **Status** | Definido |

---

#### Termo 3: Elegibilidade para devolução

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Elegibilidade para devolução |
| **Definição oficial** | Condição que uma solicitação de devolução deve satisfazer para ser processada pelo fluxo padrão de BC1. Uma devolução é elegível quando: (a) é solicitada dentro do prazo de 7 dias úteis, (b) a carga não pertence às categorias de exceção de BC4 (perigosas, refrigeradas com cadeia rompida, lacre violado sem documentação), e (c) a documentação obrigatória é fornecida (CT-e, mínimo 3 fotos, motivo). |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC4 (categorias não elegíveis pelo processo padrão) |
| **Fora de escopo** | Não determina o *custo* da devolução — isso é regra separada (ver Custo de devolução). |
| **Regra de desambiguação para LLM** | Elegibilidade e custo são verificações independentes: uma devolução pode ser elegível (dentro do prazo, documentação correta) e ainda ter custo para o cliente (se for desistência). Não confundir "inelegível" com "sem custo". |
| **Exemplo válido** | Carga padrão, prazo dentro de 7 dias úteis, CT-e e fotos anexados → elegível. Carga perigosa no mesmo prazo → não elegível pelo processo padrão → encaminhar BC4. |
| **Confusão comum a evitar** | Inelegibilidade não significa que a devolução é impossível — significa que requer tratamento fora do processo padrão (Gestão de Riscos ou Comercial). |
| **Sinônimos/variantes** | Apto para devolução, devolução válida |
| **Evidência textual** | POL-001 §3.3: "O time de atendimento tem 4 horas úteis para triagem do chamado (verificar elegibilidade, documentação e prazo)." |
| **Status** | Definido |

---

#### Termo 4: Triagem de chamado de devolução *(atualizado v2 — A-14)*

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Triagem de chamado de devolução |
| **Definição oficial** | Verificação realizada pelo time de atendimento em até 4 horas úteis após a abertura do chamado, que avalia simultaneamente: (a) se a carga é elegível para devolução padrão, (b) se a documentação está completa (CT-e, 3 fotos, motivo), e (c) se o prazo de 7 dias úteis ainda vigora. Resulta em aprovação ou recusa do chamado. **O prazo de triagem de 4 horas úteis é regra interna de BC1, independente do relógio de SLA de BC3.** A regra de pausa fora do horário comercial (08h–18h, dias úteis) aplica-se ao relógio de SLA de BC3, mas não está documentada para a triagem de BC1 — hipótese conservadora: tratar como métricas independentes até validação operacional (Pendência DP-08). |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC3 (o SLA de primeira resposta de BC3 pode ser anterior às 4h de triagem de BC1 para clientes Gold — as duas métricas são independentes e não devem ser colapsadas) |
| **Fora de escopo** | Não inclui a decisão sobre custo da devolução — essa é tomada após aprovação. |
| **Regra de desambiguação para LLM** | Distinguir obrigatoriamente os dois prazos ao responder: (1) SLA de primeira resposta de BC3 (Gold: 2h; Silver: 4h; Standard: 8h) — compromisso contratual de atendimento; (2) triagem de devolução de BC1 (4h úteis) — verificação interna de elegibilidade. Resposta ao cliente ≠ conclusão de triagem. Para chamados fora do horário comercial: informar que a triagem de BC1 segue prazo interno e orientar sobre o relógio de SLA de BC3 conforme o tier, sem afirmar pausa do prazo de triagem. |
| **Exemplo válido** | Chamado Gold aberto às 9h (segunda). SLA de primeira resposta de BC3 deve ser atendido até 11h. Triagem de BC1 concluída até 13h (independente). |
| **Confusão comum a evitar** | "Triagem aprovada" não significa que a coleta foi agendada. Não colapsar SLA de primeira resposta (BC3) e triagem (BC1) em um único prazo ao informar o cliente. |
| **Sinônimos/variantes** | Verificação de chamado, análise de elegibilidade |
| **Evidência textual** | POL-001 §3.3: "O time de atendimento tem 4 horas úteis para triagem do chamado (verificar elegibilidade, documentação e prazo)." |
| **Status** | Definido ⚠️ (pausa do prazo de triagem fora do horário comercial não documentada no corpus — Lacuna L7, Pendência DP-08) |

---

#### Termo 5: Coleta reversa

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Coleta reversa |
| **Definição oficial** | Operação logística agendada pela NovaTech para retirar a mercadoria aprovada para devolução no endereço do cliente, realizada em até 2 dias úteis após a aprovação da triagem. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC2 (o custo do frete reverso usa os mesmos multiplicadores do frete original, quando o custo é do cliente) |
| **Fora de escopo** | Não é sinônimo de "devolução de mercadoria" — é apenas a etapa logística, não o processo completo. |
| **Regra de desambiguação para LLM** | Coleta reversa refere-se exclusivamente ao transporte físico de retorno. O reembolso é processado *após* o recebimento no centro de distribuição, não no momento da coleta. |
| **Exemplo válido** | Triagem aprovada na terça-feira → coleta agendada para quinta-feira (2 dias úteis). |
| **Confusão comum a evitar** | A coleta ser agendada não equivale ao reembolso ser liberado. Reembolso ocorre em até 5 dias úteis após recebimento no CD. |
| **Sinônimos/variantes** | Frete reverso (quando referido ao custo), retirada reversa |
| **Evidência textual** | POL-001 §3.3: "a coleta reversa é agendada em até 2 dias úteis após aprovação." |
| **Status** | Definido |

---

#### Termo 6: Custo de devolução

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Custo de devolução |
| **Definição oficial** | Responsabilidade financeira pelo frete reverso, determinada pela origem da devolução: (a) se o motivo for defeito ou erro da NovaTech → sem custo para o cliente; (b) se for desistência do cliente (carga correta, sem defeito) → custo do frete reverso é do cliente, calculado com os mesmos multiplicadores do frete original (BC2); (c) se o prazo estiver expirado → não elegível para devolução padrão. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC2 (fornece os multiplicadores para cálculo do frete reverso quando o custo é do cliente) |
| **Fora de escopo** | Não se aplica a sinistros (BC5), onde o ressarcimento é integral se comprovada responsabilidade da NovaTech. |
| **Regra de desambiguação para LLM** | Antes de informar custo de devolução, verificar o motivo: devolução por erro da NovaTech = gratuita; devolução por desistência = cobrada com multiplicadores de BC2. Não inverter. |
| **Exemplo válido** | Cliente recebeu produto correto e quer devolver por desistência → paga o frete reverso calculado pela tabela de multiplicadores regionais (BC2). |
| **Confusão comum a evitar** | "Devolução gratuita" só se aplica quando o erro foi da NovaTech. Não generalizar para todos os casos. |
| **Sinônimos/variantes** | Custo do frete reverso, responsabilidade pelo frete de retorno |
| **Evidência textual** | POL-001 §3.5: "Defeito ou erro da NovaTech: devolução sem custo. Desistência do cliente: custo do frete reverso é do cliente, calculado com os mesmos multiplicadores do frete original." |
| **Status** | Definido |

---

#### Termo 7: Reembolso de devolução

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Reembolso de devolução |
| **Definição oficial** | Crédito ou devolução financeira processado pela NovaTech em até 5 dias úteis após o recebimento da mercadoria devolvida no centro de distribuição. Em devoluções parciais, o valor é proporcional ao peso/valor do volume devolvido, conforme o CT-e. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC5 (reembolso integral por sinistro é conceito distinto, gerido pelo Jurídico) |
| **Fora de escopo** | Não confundir com crédito por violação de SLA (BC3), que é percentual sobre o valor do frete do chamado. |
| **Regra de desambiguação para LLM** | Existem três tipos de crédito no domínio NovaTech: (1) reembolso de devolução (BC1, até 5d úteis após recebimento no CD); (2) ressarcimento por sinistro (BC5, após investigação pelo Jurídico); (3) crédito por violação de SLA (BC3, 5% ou 10% sobre o frete). Nunca misturar os três. |
| **Exemplo válido** | Mercadoria chega no CD na segunda. Reembolso processado até segunda da semana seguinte (5 dias úteis). |
| **Confusão comum a evitar** | O prazo de 5 dias úteis começa no recebimento *no CD*, não na data da coleta reversa. |
| **Sinônimos/variantes** | Crédito de devolução, ressarcimento de devolução |
| **Evidência textual** | POL-001 §3.3: "O reembolso ou crédito é processado em até 5 dias úteis após o recebimento da mercadoria devolvida no centro de distribuição." |
| **Status** | Definido |

---

#### Termo 8: Devolução parcial

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Devolução parcial |
| **Definição oficial** | Modalidade de devolução em que o cliente retorna um ou mais volumes individuais de uma entrega com múltiplos volumes, sem necessidade de devolver a totalidade da carga. Cada volume segue o mesmo procedimento de BC1, e o reembolso é proporcional ao peso/valor do volume devolvido, conforme o CT-e. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC2 (cálculo de frete reverso proporcional ao volume) |
| **Fora de escopo** | Não se aplica quando a entrega foi de volume único. |
| **Regra de desambiguação para LLM** | A base de cálculo do reembolso parcial é o CT-e: usar peso/valor *do volume específico*, não o valor total da nota. |
| **Exemplo válido** | Entrega com 5 volumes; cliente devolve 2 → reembolso calculado proporcionalmente ao peso/valor dos 2 volumes no CT-e. |
| **Confusão comum a evitar** | Devolução parcial não tem prazo diferenciado — o prazo de 7 dias úteis se aplica igualmente. |
| **Sinônimos/variantes** | Devolução de volume, retorno parcial |
| **Evidência textual** | POL-001 §3.4: "o cliente pode devolver volumes individuais. O cálculo de reembolso é proporcional ao peso/valor do volume devolvido, conforme o CT-e." |
| **Status** | Definido |

---

#### Termo 9: CT-e (Conhecimento de Transporte Eletrônico)

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | CT-e |
| **Definição oficial** | Documento fiscal eletrônico que comprova o transporte de mercadoria e contém os dados da carga (peso, valor declarado, remetente, destinatário). No contexto de devolução, é o identificador obrigatório do chamado e a base de cálculo para reembolsos proporcionais em devoluções parciais. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | BC2 (o CT-e referencia o frete original para cálculo do frete reverso) |
| **Fora de escopo** | Não é produzido ou calculado pelo assistente — é fornecido pelo cliente. |
| **Regra de desambiguação para LLM** | Se o cliente não fornecer o CT-e, o chamado não pode ser triado. Orientar o cliente a localizar o número antes de prosseguir. |
| **Exemplo válido** | Número CT-e: 35240812345678000123570010000012341234567890. |
| **Confusão comum a evitar** | CT-e não é nota fiscal (NF-e) — são documentos distintos. O assistente não deve aceitar NF-e como substituto do CT-e no processo de devolução. |
| **Sinônimos/variantes** | Conhecimento de transporte, número do CT-e |
| **Evidência textual** | POL-001 §3.3: "O chamado deve incluir: número do CT-e (Conhecimento de Transporte Eletrônico), fotos da mercadoria no estado atual [...] e motivo da devolução." |
| **Status** | Definido |

---

#### Termo 10: Prazo expirado (devolução)

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Prazo expirado |
| **Definição oficial** | Condição em que a solicitação de devolução é recebida após os 7 dias úteis do recebimento confirmado no tracking. Nessa condição, a devolução não é elegível pelo processo padrão de BC1 e deve ser encaminhada ao Comercial para negociação caso a caso. |
| **Contexto Dono** | BC1 — Devolução de Mercadorias |
| **Contextos Relacionados** | — |
| **Fora de escopo** | Prazo expirado em BC1 não implica que BC3 violou SLA — são métricas independentes. |
| **Regra de desambiguação para LLM** | Prazo expirado não significa "devolução impossível" — significa que o cliente precisa ser encaminhado ao Comercial. O assistente não deve encerrar a conversa, mas orientar o roteamento correto. |
| **Exemplo válido** | Recebimento confirmado em 01/06. Solicitação em 12/06 (8 dias úteis depois) → prazo expirado → encaminhar ao Comercial. |
| **Confusão comum a evitar** | Não confundir prazo expirado com inelegibilidade por tipo de carga (BC4) — as causas e os encaminhamentos são diferentes. |
| **Sinônimos/variantes** | Fora do prazo, prazo vencido |
| **Evidência textual** | POL-001 §3.5: "Prazo expirado (solicitação após 7 dias úteis): não elegível para devolução padrão. Encaminhar ao Comercial para negociação caso a caso." |
| **Status** | Definido |

---

### BC2 — Precificação de Frete Especial

*(Termos 11 a 17 — sem alterações em relação à v1)*

---

#### Termo 11: Frete especial

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Frete especial |
| **Definição oficial** | Modalidade de frete aplicável exclusivamente a cargas com peso acima de 500 kg, calculado pela fórmula: `Valor do frete = Valor base × Multiplicador regional × Fator de peso`. Cargas perigosas acima de 500 kg são excluídas deste cálculo e seguem a PROC-043. |
| **Contexto Dono** | BC2 — Precificação de Frete Especial |
| **Contextos Relacionados** | BC1 (frete reverso de desistência usa os mesmos multiplicadores), BC4 (cargas perigosas acima de 500 kg são tratadas pela PROC-043, não por esta fórmula) |
| **Fora de escopo** | Não se aplica a cargas abaixo de 500 kg (lacuna L5 — sem documento normativo para frete padrão). |
| **Regra de desambiguação para LLM** | Se o cliente perguntar sobre frete de carga abaixo de 500 kg, informar que não há documento normativo disponível no escopo do assistente e orientar contato com o Comercial. Não aplicar a fórmula de frete especial para cargas abaixo de 500 kg. |
| **Exemplo válido** | Carga de 800 kg para o Nordeste: `Valor base × 1.5 (multiplicador Nordeste) × 1.0 (fator de peso 500–1.000 kg)`. |
| **Confusão comum a evitar** | Frete especial não é frete expresso. Frete expresso é conceito não formalizado em documento normativo (FAQ-32, L6). |
| **Sinônimos/variantes** | Frete de carga pesada, frete acima de 500 kg |
| **Evidência textual** | PROC-042 §1: "fórmula e os parâmetros para cálculo de frete especial aplicável a cargas com peso acima de 500kg." |
| **Status** | Definido ⚠️ (conflito ativo entre v1 e v2 nos valores dos parâmetros — ver L4) |

---

#### Termo 12: Valor base do frete

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Valor base do frete |
| **Definição oficial** | Tarifa publicada mensalmente na tabela de fretes da NovaTech, disponível no sistema interno (`\\novatech-fs\comercial\tabelas\frete-base-AAAAMM.xlsx`). É o primeiro fator da fórmula de frete especial e não está disponível diretamente ao assistente — deve ser consultado no sistema interno. |
| **Contexto Dono** | BC2 — Precificação de Frete Especial |
| **Contextos Relacionados** | — |
| **Fora de escopo** | O assistente não armazena nem calcula o valor base — é uma variável externa mensal. |
| **Regra de desambiguação para LLM** | Nunca inventar ou estimar o valor base. Se necessário para cálculo, informar ao cliente que o valor base é consultado no sistema interno da NovaTech e orientar o atendente a acessá-lo. |
| **Exemplo válido** | Tabela de junho/2025: `frete-base-202506.xlsx`. |
| **Confusão comum a evitar** | Valor base ≠ valor final do frete. É apenas o primeiro multiplicando da fórmula. |
| **Sinônimos/variantes** | Tarifa base, tarifa publicada |
| **Evidência textual** | PROC-042 §2: "Valor base = tarifa publicada na tabela mensal de fretes." |
| **Status** | Definido |

---

#### Termo 13: Multiplicador regional

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Multiplicador regional |
| **Definição oficial** | Fator numérico aplicado ao valor base do frete conforme a região de destino da carga, utilizado no cálculo de frete especial. Os valores variam conforme a versão do PROC-042 em uso. Versão v2 (adotada como padrão para chamados a partir de 01/12/2023): Sul = 1.3, Sudeste = 1.1, Centro-Oeste = 1.4, Nordeste = 1.5, Norte = 1.8. |
| **Contexto Dono** | BC2 — Precificação de Frete Especial |
| **Contextos Relacionados** | BC1 (frete reverso por desistência usa os mesmos multiplicadores do frete original) |
| **Fora de escopo** | Não se aplica a cargas perigosas acima de 500 kg (PROC-043). Não se aplica a fretes abaixo de 500 kg. |
| **Regra de desambiguação para LLM** | Usar sempre os multiplicadores da v2 para chamados novos. Se o cliente contestar com valores da v1, informar que a v2 é o padrão atual e que contratos anteriores podem ter condições específicas a confirmar com o Comercial. Nunca misturar multiplicadores das duas versões no mesmo cálculo. |
| **Exemplo válido** | Destino: Norte → multiplicador = 1.8 (v2). |
| **Confusão comum a evitar** | Os multiplicadores da v1 e v2 são diferentes para todas as regiões. Usar v1 (Norte = 1.6) quando deveria ser v2 (Norte = 1.8) resulta em cálculo incorreto. |
| **Sinônimos/variantes** | Fator regional, coeficiente regional |
| **Evidência textual** | PROC-042-v2 §2.1: "Multiplicadores regionais atualizados em novembro/2023: Sul 1.3, Sudeste 1.1, Centro-Oeste 1.4, Nordeste 1.5, Norte 1.8." |
| **Status** | Definido ⚠️ (conflito v1/v2 — usar v2 como padrão) |

---

#### Termo 14: Fator de peso

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Fator de peso |
| **Definição oficial** | Coeficiente numérico aplicado à fórmula de frete especial com base na faixa de peso da carga. Versão v2 (padrão para chamados novos): 1.0 para 500–1.000 kg; 1.15 para 1.001–3.000 kg; 1.40 para acima de 3.000 kg. |
| **Contexto Dono** | BC2 — Precificação de Frete Especial |
| **Contextos Relacionados** | — |
| **Fora de escopo** | Não se aplica a cargas abaixo de 500 kg. |
| **Regra de desambiguação para LLM** | Usar fator de peso da v2. Não usar v1 (1.0 / 1.2 / 1.5) para chamados novos. |
| **Exemplo válido** | Carga de 2.500 kg → fator de peso = 1.15 (v2). |
| **Confusão comum a evitar** | O fator de peso 1.0 para a faixa de 500–1.000 kg é idêntico em v1 e v2, mas as faixas acima divergem. |
| **Sinônimos/variantes** | Coeficiente de peso, multiplicador de peso |
| **Evidência textual** | PROC-042-v2 §2: "Fator de peso = 1.0 para cargas de 500kg a 1.000kg; 1.15 para cargas de 1.001kg a 3.000kg; 1.4 para cargas acima de 3.000kg." |
| **Status** | Definido ⚠️ (conflito v1/v2 — usar v2 como padrão) |

---

#### Termo 15: Prazo adicional de frete especial

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Prazo adicional de frete especial |
| **Definição oficial** | Acréscimo de dias úteis ao prazo padrão da rota, aplicado a fretes especiais (acima de 500 kg) para compensar o manuseio e roteirização de carga pesada. Versão v2 (padrão): +3 dias úteis. |
| **Contexto Dono** | BC2 — Precificação de Frete Especial |
| **Contextos Relacionados** | BC3 (prazo de entrega é referência para contagem de SLA de tracking) |
| **Fora de escopo** | Não confundir com prazo de triagem de devolução (4h úteis, BC1) nem com prazo de SLA de atendimento (BC3). |
| **Regra de desambiguação para LLM** | Usar +3 dias úteis (v2) para chamados novos. Se cliente citar +2 dias, informar que a versão vigente é v2. O prazo adicional é somado ao prazo padrão da rota, que não está disponível no corpus; informar o adicional (+3d) mas não calcular o prazo total. |
| **Exemplo válido** | Rota com prazo padrão de 5 dias úteis + frete especial = 8 dias úteis de prazo total (v2). |
| **Confusão comum a evitar** | O assistente pode informar o adicional (+3d) mas não pode calcular o prazo total sem o prazo base da rota. |
| **Sinônimos/variantes** | Dias adicionais para carga pesada, acréscimo de prazo |
| **Evidência textual** | PROC-042-v2 §3: "+3 dias úteis adicionais para manuseio e roteirização de carga pesada (anteriormente era +2 dias na versão anterior)." |
| **Status** | Definido ⚠️ (conflito v1/v2 — usar v2 como padrão) |

---

#### Termo 16: Desconto por volume de frete

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Desconto por volume de frete |
| **Definição oficial** | Redução percentual sobre o multiplicador regional, aplicada automaticamente a clientes que atingem o limiar de fretes especiais por mês. Conforme v2 (padrão): 5% de desconto a partir de 8 fretes especiais/mês; 10% a partir de 15 fretes/mês. Descontos adicionais requerem aprovação da Diretoria Comercial. |
| **Contexto Dono** | BC2 — Precificação de Frete Especial |
| **Contextos Relacionados** | — |
| **Fora de escopo** | O atendente não tem autonomia para conceder descontos fora dos limiares definidos (FAQ-45). |
| **Regra de desambiguação para LLM** | Se cliente solicitar desconto: verificar se atingiu os limiares de v2 (8 ou 15 fretes/mês). Se sim, aplicar o percentual. Se não, informar que descontos adicionais dependem de aprovação do Comercial. Não usar os limiares da v1 (>10 fretes/mês). |
| **Exemplo válido** | Cliente com 10 fretes especiais no mês → desconto de 5% sobre o multiplicador regional (v2). |
| **Confusão comum a evitar** | O desconto incide sobre o *multiplicador regional*, não sobre o valor total do frete. |
| **Sinônimos/variantes** | Desconto de fidelidade de frete, desconto automático |
| **Evidência textual** | PROC-042-v2 §4: "a partir de 8 fretes especiais/mês para o mesmo cliente, aplicar desconto de 5% sobre o multiplicador regional. Acima de 15 fretes/mês, desconto de 10%." |
| **Status** | Definido ⚠️ (limiares conflitam com v1 — usar v2) |

---

#### Termo 17: Aprovação para carga acima de 5.000 kg

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Aprovação para carga acima de 5.000 kg |
| **Definição oficial** | Requisito obrigatório de autorização prévia do gerente de operações regional antes do processamento de qualquer frete especial para cargas acima de 5.000 kg. Presente em ambas as versões do PROC-042. |
| **Contexto Dono** | BC2 — Precificação de Frete Especial |
| **Contextos Relacionados** | — |
| **Fora de escopo** | Não é responsabilidade do assistente ou do atendente — é aprovação gerencial. |
| **Regra de desambiguação para LLM** | Se a carga superar 5.000 kg, informar ao cliente que o processamento requer aprovação do gerente regional antes de prosseguir. Não calcular nem confirmar frete sem mencionar essa condição. |
| **Exemplo válido** | Carga de 6.000 kg → antes de calcular frete, verificar aprovação do gerente regional. |
| **Confusão comum a evitar** | A regra de aprovação se aplica independentemente da região ou tipo de carga (exceto perigosas, que seguem PROC-043). |
| **Sinônimos/variantes** | Autorização gerencial para carga pesada |
| **Evidência textual** | PROC-042 §4 e PROC-042-v2 §4: "Cargas acima de 5.000kg requerem aprovação prévia do gerente de operações regional." |
| **Status** | Definido |

---

### BC3 — Gestão de SLA e Atendimento

*(Termos 18 a 23 — sem alterações em relação à v1)*

---

#### Termo 18: Tier de cliente

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Tier de cliente |
| **Definição oficial** | Classificação contratual do cliente da NovaTech em uma de três categorias — Gold, Silver ou Standard — com base no volume mensal de operações ou no valor do contrato anual. Determina os SLAs aplicáveis. Não existem outros tiers. |
| **Contexto Dono** | BC3 — Gestão de SLA e Atendimento |
| **Contextos Relacionados** | BC1 (tier Gold pode demandar triagem dentro do SLA de resposta de 2h, anterior às 4h de triagem interna) |
| **Fora de escopo** | O assistente não classifica ou altera tiers — eles são definidos contratualmente. Se o cliente alegar tier não reconhecido, verificar o contrato. |
| **Regra de desambiguação para LLM** | Se o cliente mencionar "Platinum" ou qualquer outro tier não listado: informar que os tiers válidos são Gold, Silver e Standard, e solicitar o número do contrato para verificação. Nunca confirmar tier não reconhecido. |
| **Exemplo válido** | Contrato anual de R$ 600.000 → Gold. Contrato de R$ 80.000 → Standard. |
| **Confusão comum a evitar** | Tier Platinum não existe na NovaTech (FAQ-15). Era parte de programa descontinuado em 2022. |
| **Sinônimos/variantes** | Nível de cliente, categoria de cliente, plano de serviço |
| **Evidência textual** | SLA-2024 §1: "A NovaTech classifica seus clientes em 3 (três) tiers [...] Não existem outros tiers além dos três listados acima." |
| **Status** | Definido |

---

#### Termo 19: SLA de primeira resposta

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | SLA de primeira resposta |
| **Definição oficial** | Compromisso contratual de tempo máximo para que o time de atendimento dê o primeiro retorno ao cliente após a abertura do chamado, mesmo que seja apenas confirmação de recebimento. Varia por tier: Gold = 2h úteis; Silver = 4h úteis; Standard = 8h úteis (chamados gerais). Para incidentes críticos: Gold = 30 min; Silver = 1h; Standard = 2h. |
| **Contexto Dono** | BC3 — Gestão de SLA e Atendimento |
| **Contextos Relacionados** | BC1 (o SLA de primeira resposta de BC3 é independente da triagem de BC1; os dois prazos não devem ser colapsados ao informar o cliente) |
| **Fora de escopo** | Não é o mesmo que conclusão da triagem de devolução (BC1). |
| **Regra de desambiguação para LLM** | "Primeira resposta" = qualquer retorno ao cliente, inclusive "estamos analisando". Não confundir com resolução do problema. Para Gold em incidente crítico: 30 minutos sem pausa por horário comercial. |
| **Exemplo válido** | Chamado Gold aberto às 10h → primeira resposta deve ocorrer até 12h (chamado geral) ou até 10h30 (incidente crítico). |
| **Confusão comum a evitar** | SLA de resposta não é prazo de resolução. Um cliente Gold pode receber a primeira resposta em 2h e ter o problema resolvido em até 24h. |
| **Sinônimos/variantes** | Prazo de resposta, tempo de resposta, SLA de atendimento inicial |
| **Evidência textual** | SLA-2024 §2: "Gold: Até 2h úteis (resposta) / Até 24h úteis (resolução). FAQ-41: 'Resposta é quando a gente dá o primeiro retorno ao cliente (mesmo que seja estamos verificando).'" |
| **Status** | Definido |

---

#### Termo 20: SLA de resolução

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | SLA de resolução |
| **Definição oficial** | Compromisso contratual de tempo máximo para solução efetiva do chamado. Varia por tier: Gold = 24h úteis; Silver = 48h úteis; Standard = 72h úteis (chamados gerais). Para incidentes críticos: Gold = 4h; Silver = 8h; Standard = 24h. |
| **Contexto Dono** | BC3 — Gestão de SLA e Atendimento |
| **Contextos Relacionados** | — |
| **Fora de escopo** | Não se confunde com prazo de reembolso de devolução (BC1) nem com prazo de entrega (BC2). |
| **Regra de desambiguação para LLM** | Ao informar prazos ao cliente, distinguir sempre: (a) quando ele receberá *resposta* e (b) quando o problema será *resolvido*. Não colapsá-los num único número. |
| **Exemplo válido** | Cliente Silver com incidente crítico → resposta em até 1h; resolução em até 8h. |
| **Confusão comum a evitar** | Para incidentes críticos de Gold, o relógio de SLA *não para* fora do horário comercial. Para chamados gerais, pausa entre 18h e 8h. |
| **Sinônimos/variantes** | Prazo de resolução, tempo de fechamento do chamado |
| **Evidência textual** | SLA-2024 §2 e §5: "O relógio de SLA pausa fora do horário comercial (08h–18h, dias úteis) para chamados gerais, mas não pausa para incidentes críticos de clientes Gold." |
| **Status** | Definido |

---

#### Termo 21: Incidente crítico

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Incidente crítico |
| **Definição oficial** | Chamado classificado como crítico quando atende a pelo menos um dos quatro critérios: (1) carga com valor declarado acima de R$ 100.000 com status desconhecido há mais de 6 horas; (2) carga perigosa com qualquer irregularidade de documentação ou rastreamento; (3) mais de 5 chamados do mesmo cliente nas últimas 24h sobre o mesmo problema; (4) qualquer situação com risco à segurança de pessoas. Ativa SLAs reduzidos e, para Gold, relógio sem pausa. |
| **Contexto Dono** | BC3 — Gestão de SLA e Atendimento |
| **Contextos Relacionados** | BC4 (critério 2 envolve carga perigosa), BC5 (critério 1 pode envolver carga com alto valor danificada) |
| **Fora de escopo** | A classificação como incidente crítico não substitui o processo de BC4 para cargas perigosas nem o de BC5 para sinistros — aciona SLAs diferenciados *em paralelo*. |
| **Regra de desambiguação para LLM** | Verificar os 4 critérios ao receber qualquer chamado. Presença de *qualquer um* deles classifica o chamado como crítico. Não exigir múltiplos critérios simultaneamente. |
| **Exemplo válido** | Carga de R$ 150.000 sem atualização de status por 7h → incidente crítico (critério 1). |
| **Confusão comum a evitar** | "Alta prioridade" mencionada pelo cliente não equivale a incidente crítico — a classificação é objetiva, baseada nos 4 critérios acima. |
| **Sinônimos/variantes** | Chamado crítico, ocorrência crítica |
| **Evidência textual** | SLA-2024 §3: "Um incidente é classificado como crítico quando atende a pelo menos um dos seguintes critérios." |
| **Status** | Definido |

---

#### Termo 22: Penalidade por violação de SLA

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Penalidade por violação de SLA |
| **Definição oficial** | Consequência contratual aplicada quando um SLA é descumprido no mês. Escala por frequência: 1ª violação = registro interno, sem impacto contratual; 2ª violação = crédito de 5% sobre o valor do frete do chamado afetado; 3ª violação ou mais = crédito de 10% + reunião obrigatória com gerente de conta (Gold) ou gerente de operações (Silver/Standard). |
| **Contexto Dono** | BC3 — Gestão de SLA e Atendimento |
| **Contextos Relacionados** | BC1 (o crédito de 5%/10% é sobre o frete do chamado, não sobre o reembolso de devolução) |
| **Fora de escopo** | Não é o mesmo que reembolso de devolução (BC1) nem ressarcimento por sinistro (BC5). |
| **Regra de desambiguação para LLM** | Ao informar créditos ao cliente: identificar se é crédito por violação de SLA (BC3) ou reembolso por devolução (BC1). Nunca somar ou confundir os dois. |
| **Exemplo válido** | 3ª violação de SLA em março para cliente Silver → crédito de 10% + reunião com gerente de operações. |
| **Confusão comum a evitar** | A 1ª violação não gera crédito — apenas registro interno. Não informar crédito automático para toda e qualquer violação. |
| **Sinônimos/variantes** | Crédito por violação de SLA, compensação por SLA |
| **Evidência textual** | SLA-2024 §4: "Segunda violação no mesmo mês: crédito de 5% sobre o valor do frete do chamado afetado." |
| **Status** | Definido |

---

#### Termo 23: Relógio de SLA

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Relógio de SLA |
| **Definição oficial** | Contador de tempo que mede o cumprimento do SLA a partir do timestamp de abertura do chamado no Azure DevOps. Para chamados gerais: pausa fora do horário comercial (18h às 8h, dias úteis, exceto feriados). Para incidentes críticos de clientes Gold: não pausa em nenhuma hipótese. Aplica-se exclusivamente a BC3 — não regula o prazo de triagem de BC1. |
| **Contexto Dono** | BC3 — Gestão de SLA e Atendimento |
| **Contextos Relacionados** | BC1 (o relógio de SLA de BC3 e o prazo de triagem de BC1 são métricas independentes — ver Lacuna L7) |
| **Fora de escopo** | Não se aplica ao prazo de devolução (BC1) nem ao prazo de entrega (BC2). |
| **Regra de desambiguação para LLM** | Para incidentes críticos Gold, calcular o tempo sempre em horas corridas. Para chamados gerais, descontar o período fora do horário comercial. Não aplicar a lógica de pausa do relógio de SLA ao prazo de triagem de BC1 sem validação operacional (DP-08). |
| **Exemplo válido** | Chamado geral Gold aberto às 17h30 → SLA pausa às 18h, retoma às 8h do dia seguinte. |
| **Confusão comum a evitar** | A regra de pausa do relógio de SLA (BC3) não foi confirmada como aplicável ao prazo de triagem de BC1. Não inferir equivalência sem confirmação operacional. |
| **Sinônimos/variantes** | Contador de SLA, timer de SLA, medição de SLA |
| **Evidência textual** | SLA-2024 §5: "O relógio de SLA pausa fora do horário comercial (08h-18h, dias úteis) para chamados gerais, mas não pausa para incidentes críticos de clientes Gold." |
| **Status** | Definido |

---

### BC4 — Conformidade e Riscos de Carga

---

#### Termo 24: Carga perigosa *(atualizado v2 — A-07)*

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Carga perigosa |
| **Definição oficial** | Mercadoria classificada nas classes 1 a 6 da ANTT, conforme Resolução ANTT nº 5.947/2021: classe 1 = explosivos; classe 2 = gases; classe 3 = líquidos inflamáveis; classe 4 = sólidos inflamáveis; classe 5 = oxidantes e peróxidos; classe 6 = substâncias tóxicas e infectantes. Qualquer irregularidade com carga perigosa é automaticamente um incidente crítico (BC3). Não é elegível para devolução padrão (BC1) nem para frete especial padrão (BC2 — segue PROC-043). |
| **Contexto Dono** | BC4 — Conformidade e Riscos de Carga |
| **Contextos Relacionados** | BC1 (não elegível para devolução padrão), BC2 (não calculada pela PROC-042 — segue PROC-043), BC3 (qualquer irregularidade = incidente crítico), BC6 (seguro de carga perigosa = 0,8%) |
| **Fora de escopo** | O assistente não decide sobre tratamento individual de carga perigosa — encaminha à Gestão de Riscos (ramal 4500). |
| **Regra de desambiguação para LLM** | **Caso 1 — Classificação ANTT explícita na pergunta:** Ao identificar carga com classe ANTT 1–6 (ex.: "gás comprimido — classe 2", "substância inflamável — classe 3"), acionar imediatamente BC4 e encaminhar ao ramal 4500. Não processar como carga padrão. **Caso 2 — Descrição por nome coloquial sem classe ANTT:** Quando a carga for descrita apenas por nome coloquial potencialmente perigoso (ex.: "cloro", "tinta industrial", "bateria de lítio", "produto químico de limpeza"), o assistente NÃO deve assumir que é perigosa nem que não é. Deve perguntar ao atendente: "Essa carga possui classificação ANTT de carga perigosa (classes 1 a 6)?" Somente após a resposta do atendente, tomar a decisão de roteamento. Esse protocolo evita falso positivo (roteamento de carga não perigosa para BC4, bloqueando atendimento padrão) e falso negativo (omissão de risco de segurança para carga perigosa). |
| **Exemplo válido — Caso 1** | Atendente informa: "carga de gás comprimido (classe 2 ANTT)" → encaminhar imediatamente ao ramal 4500. |
| **Exemplo válido — Caso 2** | Atendente informa: "carga de produto de limpeza industrial" → assistente pergunta se a carga tem classificação ANTT. Se atendente confirmar classe ANTT → encaminhar BC4. Se negar → processar como carga padrão. Se incerto → encaminhar BC4 por precaução (princípio de segurança). |
| **Confusão comum a evitar** | Não classificar como perigosa apenas pela descrição coloquial (falso positivo). Não ignorar nome coloquial potencialmente perigoso sem verificar (falso negativo). Em caso de dúvida, o princípio de segurança orienta encaminhar BC4. |
| **Sinônimos/variantes** | Carga regulada, carga ANTT, produto perigoso |
| **Evidência textual** | POL-001 §3.2: "Cargas perigosas classificadas nas classes 1 a 6 da ANTT [...] NÃO são elegíveis para devolução pelo processo padrão." |
| **Status** | Definido *(atualizado v2 — regra de desambiguação por nome coloquial adicionada, origem: A-07)* |

---

#### Termo 25: Cadeia de frio

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Cadeia de frio |
| **Definição oficial** | Condição de conservação térmica contínua de carga refrigerada, definida pela faixa de temperatura especificada na nota fiscal. A cadeia de frio é considerada *rompida* quando a temperatura fica fora dessa faixa por mais de 30 minutos contínuos, conforme registro do sensor IoT. Carga refrigerada com cadeia rompida não é elegível para devolução padrão. |
| **Contexto Dono** | BC4 — Conformidade e Riscos de Carga |
| **Contextos Relacionados** | BC1 (cadeia rompida = não elegível para devolução padrão) |
| **Fora de escopo** | A definição de "faixa de temperatura correta" é extraída da nota fiscal do produto, não de tabela genérica da NovaTech. |
| **Regra de desambiguação para LLM** | O critério de ruptura é objetivo: temperatura fora da faixa *por mais de 30 minutos contínuos* conforme sensor IoT. **Se o atendente informar o tempo registrado pelo sensor:** aplicar o critério diretamente (>30 min = rompida; ≤30 min = não rompida pelo critério documental). **Se o atendente não informar o tempo do sensor:** orientar a verificar o registro do sensor IoT antes de decidir elegibilidade; não presumir ruptura nem integridade. |
| **Exemplo válido** | Carne bovina com faixa de -18°C a -15°C. Sensor registra -10°C por 45 min contínuos → cadeia rompida → encaminhar ramal 4500. |
| **Confusão comum a evitar** | "Carga refrigerada" por si só não impede devolução — somente se a cadeia de frio estiver *rompida* conforme registro do sensor. |
| **Sinônimos/variantes** | Controle de temperatura, cold chain |
| **Evidência textual** | POL-001 §3.2: "Cargas refrigeradas que tenham rompido a cadeia de frio (temperatura fora da faixa especificada na nota fiscal por mais de 30 minutos contínuos, conforme registro do sensor IoT)." |
| **Status** | Definido *(Regra de desambiguação atualizada v2 para distinguir sensor informado vs. não informado — origem: A-05)* |

---

#### Termo 26: Lacre de segurança

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Lacre de segurança |
| **Definição oficial** | Dispositivo de inviolabilidade aplicado à carga para garantir que o conteúdo não foi acessado durante o transporte. Carga com lacre violado não é elegível para devolução padrão, *exceto* quando a violação foi documentada no ato de entrega com assinatura do motorista e do recebedor. |
| **Contexto Dono** | BC4 — Conformidade e Riscos de Carga |
| **Contextos Relacionados** | BC1 (exceção documental permite elegibilidade para devolução) |
| **Fora de escopo** | O assistente não verifica fisicamente o lacre — baseia-se na documentação fornecida. |
| **Regra de desambiguação para LLM** | Se o cliente informar lacre violado: perguntar se há documentação do ato de entrega com assinatura do motorista e recebedor. Se sim → pode prosseguir para triagem padrão. Se não → encaminhar à Gestão de Riscos. |
| **Exemplo válido** | Lacre violado com termo assinado pelo motorista e recebedor na entrega → elegível para devolução padrão. |
| **Confusão comum a evitar** | Lacre violado não é bloqueio absoluto — a exceção documental existe e deve ser verificada antes de recusar a devolução. |
| **Sinônimos/variantes** | Lacre inviolável, lacre quebrado |
| **Evidência textual** | POL-001 §3.2: "Cargas com lacre de segurança violado, salvo quando a violação for documentada no ato de entrega com assinatura do motorista e do recebedor." |
| **Status** | Definido |

---

#### Termo 27: Gestão de Riscos

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Gestão de Riscos |
| **Definição oficial** | Setor interno da NovaTech responsável pelo tratamento individual de casos que não são elegíveis para os processos padrão de devolução, especialmente cargas perigosas, refrigeradas com cadeia rompida e lacres violados sem documentação. Contato: ramal 4500. |
| **Contexto Dono** | BC4 — Conformidade e Riscos de Carga |
| **Contextos Relacionados** | BC1 (recebe encaminhamento de devoluções não elegíveis), BC2 (recebe cargas perigosas para frete) |
| **Fora de escopo** | O processo interno da Gestão de Riscos após receber o chamado não está documentado no escopo disponível (Lacuna L1). |
| **Regra de desambiguação para LLM** | Gestão de Riscos ≠ Jurídico (BC5) ≠ Compliance. São setores diferentes: Gestão de Riscos atua em BC4; Jurídico atua em BC5; Compliance atua na autorização de cargas perigosas para frete expresso (não formalizado). Ao encaminhar ao ramal 4500, não prometer resultado, prazo ou aprovação. |
| **Exemplo válido** | Cliente tenta devolver carga de gás (classe 2 ANTT) → assistente encaminha para ramal 4500. |
| **Confusão comum a evitar** | Encaminhar à Gestão de Riscos não é o mesmo que negar a devolução — é redirecionar para tratamento especializado. |
| **Sinônimos/variantes** | Setor de Riscos, ramal 4500 |
| **Evidência textual** | POL-001 §3.2: "o cliente deve entrar em contato com o setor de Gestão de Riscos (ramal 4500) para tratamento individual." |
| **Status** | Definido ⚠️ (processo interno após ramal 4500 não documentado — Lacuna L1) |

---

### BC5 — Sinistros e Carga Danificada

*(Termos 28 a 30 — sem alterações em relação à v1, exceto nota de pendência DP-04 no Termo 29)*

---

#### Termo 28: Carga danificada

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Carga danificada |
| **Definição oficial** | Mercadoria que apresenta dano físico ocorrido durante o transporte pela NovaTech, identificado pelo cliente após o recebimento. Segue processo distinto da devolução padrão: registro de ocorrência em até 48 horas (horas corridas — ver Pendência DP-04), com fotos e laudo, encaminhado a sinistros@novatech.com.br para apuração pelo Jurídico. |
| **Contexto Dono** | BC5 — Sinistros e Carga Danificada |
| **Contextos Relacionados** | BC1 (não é devolução padrão), BC3 (carga >R$100k com status desconhecido >6h = incidente crítico) |
| **Fora de escopo** | Não segue o fluxo de devolução de BC1. Não é processado pelo atendimento normal. |
| **Regra de desambiguação para LLM** | Distinguir se o cliente quer devolver carga *correta e íntegra* (BC1) ou reportar carga *danificada durante o transporte* (BC5). Perguntas-chave: "a mercadoria chegou com dano físico?" ou "você quer devolver por outro motivo?". |
| **Exemplo válido** | Televisor entregue com tela quebrada após transporte → carga danificada → BC5, sinistros@novatech.com.br. |
| **Confusão comum a evitar** | Carga danificada em trânsito ≠ devolução por desistência. O processo, o prazo e o responsável são completamente diferentes. |
| **Sinônimos/variantes** | Avaria de carga, mercadoria avariada, carga com dano |
| **Evidência textual** | FAQ-38: "Carga danificada em trânsito tem processo diferente de devolução." |
| **Status** | Definido ⚠️ (fonte exclusivamente FAQ informal — Lacuna L2) |

---

#### Termo 29: Registro de ocorrência de sinistro

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Registro de ocorrência de sinistro |
| **Definição oficial** | Notificação formal do cliente sobre dano à carga durante o transporte, que deve ser feita em até 48 horas após o recebimento (interpretação atual: horas corridas — Pendência DP-04 para validação pelo Jurídico), acompanhada de fotos e laudo (quando disponível), enviada ao e-mail sinistros@novatech.com.br. |
| **Contexto Dono** | BC5 — Sinistros e Carga Danificada |
| **Contextos Relacionados** | — |
| **Fora de escopo** | O prazo de 48h de BC5 é distinto do prazo de 7 dias úteis de BC1. Não confundir os dois. |
| **Regra de desambiguação para LLM** | Para sinistros, o prazo é de 48 horas (interpretação atual: corridas, não úteis — sujeita a validação pelo Jurídico, DP-04). Para devolução, o prazo é de 7 dias úteis. Ao informar o prazo de 48h, incluir a ressalva de que a natureza do prazo (corridas vs. úteis) está pendente de confirmação normativa. |
| **Exemplo válido** | Entrega recebida às 14h de segunda → registro de sinistro deve ser feito até às 14h de quarta (hipótese: horas corridas). |
| **Confusão comum a evitar** | O prazo de 48h para sinistro não é o mesmo que o prazo de triagem de 4h úteis de BC1. |
| **Sinônimos/variantes** | Abertura de sinistro, boletim de ocorrência de dano |
| **Evidência textual** | FAQ-38: "O cliente precisa registrar a ocorrência em até 48h após o recebimento, com fotos e laudo se possível." |
| **Status** | Definido ⚠️ (fonte exclusivamente FAQ informal — Lacuna L2; prazo corridas vs. úteis pendente — DP-04) |

---

#### Termo 30: Ressarcimento por sinistro

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Ressarcimento por sinistro |
| **Definição oficial** | Reembolso integral da carga ao cliente, concedido após investigação pelo Jurídico da NovaTech e comprovação de responsabilidade da NovaTech pelo dano. Não possui prazo definido em documento normativo. |
| **Contexto Dono** | BC5 — Sinistros e Carga Danificada |
| **Contextos Relacionados** | BC1 (distinto do reembolso de devolução, que tem prazo de 5 dias úteis e não exige investigação jurídica) |
| **Fora de escopo** | O processo de investigação do Jurídico não está detalhado no escopo disponível. |
| **Regra de desambiguação para LLM** | Ressarcimento por sinistro = investigação + comprovação + reembolso integral (BC5). Reembolso de devolução = crédito proporcional em até 5 dias úteis após recebimento no CD (BC1). Nunca informar prazo de 5 dias para sinistro. |
| **Exemplo válido** | Carga avariada investigada pelo Jurídico, NovaTech comprovada responsável → reembolso integral do valor declarado no CT-e. |
| **Confusão comum a evitar** | "Reembolso integral" de sinistro não é garantido automaticamente — depende de investigação que comprove responsabilidade da NovaTech. |
| **Sinônimos/variantes** | Indenização por dano, reembolso de sinistro |
| **Evidência textual** | FAQ-38: "a NovaTech investiga e, se comprovada responsabilidade nossa, reembolsa integralmente." |
| **Status** | Definido ⚠️ (fonte exclusivamente FAQ informal — Lacuna L2) |

---

### BC6 — Seguro de Carga

---

#### Termo 31: Seguro de carga

| Campo | Conteúdo |
|---|---|
| **Termo canônico** | Seguro de carga |
| **Definição oficial** | Serviço adicional oferecido pela NovaTech para cobertura de sinistros, contratado separadamente. O percentual é calculado sobre o valor declarado da mercadoria: 0,3% para cargas padrão e 0,8% para cargas perigosas, válido para contratos firmados a partir de 2023. Contratos anteriores podem ter percentuais diferentes — confirmação obrigatória com o Comercial. |
| **Contexto Dono** | BC6 — Seguro de Carga |
| **Contextos Relacionados** | BC4 (a alíquota depende da classificação da carga como perigosa ou padrão) |
| **Fora de escopo** | Não é parte do frete — é contratação adicional. O assistente não contrata seguro, apenas informa condições. |
| **Regra de desambiguação para LLM** | Sempre informar que os percentuais valem para contratos a partir de 2023 e que contratos mais antigos devem ser confirmados com o Comercial. Nunca afirmar que o seguro está incluído no frete. |
| **Exemplo válido** | Carga padrão com valor declarado de R$ 50.000 → seguro = R$ 150,00 (0,3%). |
| **Confusão comum a evitar** | Seguro de carga não é o mesmo que ressarcimento por sinistro. O seguro é preventivo e contratado antes do transporte; o ressarcimento (BC5) é reparatório, após ocorrência. |
| **Sinônimos/variantes** | Seguro de transporte, cobertura de carga |
| **Evidência textual** | FAQ-22: "A NovaTech oferece seguro de carga como adicional. O valor é 0,3% do valor declarado [...] para cargas padrão e 0,8% para cargas perigosas. [...] isso vale para contratos a partir de 2023." |
| **Status** | Definido ⚠️ (fonte exclusivamente FAQ informal — Lacuna L3) |

---

## Seção 3: Lacunas e hipóteses

| Termo | Contexto | Lacuna | Hipótese | Risco de erro para LLM |
|---|---|---|---|---|
| Processo pós-ramal 4500 | BC4 — Conformidade | Não há PROC documentando o que a Gestão de Riscos faz após receber o chamado de carga perigosa. | Tratamento é individual e não padronizado; o assistente deve encaminhar ao ramal 4500 sem prometer resultado ou prazo. | **Alto**: LLM pode inventar um fluxo inexistente ou prometer aprovação que não é garantida. |
| Carga danificada (processo formal) | BC5 — Sinistros | Não existe POL ou PROC formal sobre tratamento de carga danificada. Toda a regra vem do FAQ-38, não validado por Compliance. | Aplicar FAQ-38 como guia operacional provisório; sempre indicar que o processo "passa pelo Jurídico" e encaminhar sinistros@novatech.com.br. | **Alto**: LLM pode apresentar regras do FAQ como normativas, induzindo cliente a expectativas incorretas sobre prazos ou certeza de reembolso. |
| Seguro de carga (documento normativo) | BC6 — Seguro | Não existe POL ou PROC formal sobre seguro de carga. Única fonte é FAQ-22, não validada por Compliance. | Fornecer percentuais (0,3% / 0,8%) com ressalva explícita de que são informações não normativas e que confirmação com o Comercial é obrigatória, especialmente para contratos pré-2023. | **Médio**: LLM pode confirmar valores como definitivos, gerando conflito contratual se o cliente tiver percentual diferente. |
| Versão vigente do PROC-042 | BC2 — Precificação | Dois documentos coexistem sem hierarquia formal. Regra de transição já expirou. | Adotar PROC-042-v2 como padrão para chamados novos; para contratos anteriores a dez/2023, alertar sobre possível divergência e orientar confirmação com o Comercial. | **Alto**: LLM pode usar multiplicadores da v1 (mais baixos) em vez da v2 (mais altos), gerando subcobrança ou contestação de valores. |
| Frete padrão (abaixo de 500 kg) | BC2 — Precificação | Não há documento normativo cobrindo frete para cargas abaixo de 500 kg. | Informar que o escopo disponível cobre apenas frete especial (>500 kg); encaminhar ao Comercial para cotação de frete padrão. | **Médio**: LLM pode aplicar a fórmula de frete especial a cargas abaixo de 500 kg, gerando cálculo inválido. |
| Frete expresso para carga perigosa | BC4 — Conformidade | FAQ-32 descreve o processo informalmente, mas não existe PROC formal. PROC-043 está em revisão pelo Compliance. | Informar ao cliente que frete expresso para carga perigosa pode ser possível com autorização do Compliance; sem garantia de prazo. Nunca confirmar como processo padrão. | **Alto**: LLM pode confirmar possibilidade ou prazo de autorização com falsa precisão. |
| Pausa do prazo de triagem (BC1) fora do horário comercial | BC1 — Devolução | POL-001§3.3 define "4 horas úteis" para triagem, mas não especifica se segue a regra de pausa do relógio de SLA de BC3. | Hipótese conservadora: tratar como métricas independentes; informar os dois prazos sem afirmar pausa da triagem. Validação pendente com Operações (DP-08). | **Médio**: LLM pode afirmar que o prazo de triagem pausa fora do horário comercial (por analogia com BC3) sem base documental, gerando orientação incorreta ao atendente. |

---

## Seção 4: Checklist de validação

**Cobertura completa dos contextos**
- BC1: 10 termos. Triagem (Termo 4) atualizado com separação explícita de BC3 (A-14). ✅
- BC2: 7 termos, sem alteração. ✅
- BC3: 6 termos. Relógio de SLA (Termo 23) atualizado com nota de independência em relação à triagem de BC1. ✅
- BC4: 4 termos. Carga perigosa (Termo 24) atualizado com protocolo de nome coloquial (A-07). Cadeia de frio (Termo 25) atualizado com distinção sensor informado/não informado (A-05). ✅
- BC5: 3 termos. Registro de ocorrência (Termo 29) atualizado com nota de pendência DP-04. ✅
- BC6: 1 termo, sem alteração. ✅

**Sem definição circular** ✅
**Sem linguagem vaga sem condição objetiva** ✅
**Sem duplicidade semântica entre contextos** ✅
**Regras de desambiguação para LLM presentes em todos os termos críticos** ✅
**Lacunas identificadas e rotuladas** ✅ (7 lacunas — 6 originais + L7 nova)
