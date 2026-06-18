# Mapa de Bounded Contexts — Assistente NovaTech

**Versão:** 2.0
**Data:** 2025-06-09
**Origem da versão anterior:** bounded-contexts-v1.md
**Revisão aplicada:** analise-de-ambiguidade.md (Tech Lead)
**Achados aplicados:** A-07 (parcial — BC4), A-18

---

## 1. Resumo executivo

A análise do corpus de 5 documentos identificou **6 bounded contexts** com responsáveis, vocabulários e regras claramente distintos. Os contextos de maior criticidade são **Devolução de Mercadorias** (regra normativa obrigatória, POL-001) e **Precificação de Frete Especial** (conflito ativo entre PROC-042 v1 e v2). A maior **lacuna estrutural** está em **Sinistros e Carga Danificada** e **Seguro de Carga**, que existem apenas como conhecimento informal no FAQ, sem nenhum documento normativo de suporte. O contexto de **Conformidade e Riscos** age como guardião transversal: toda carga perigosa deve ser roteada para ele antes de qualquer decisão de devolução, frete ou seguro. O contexto de **Gestão de SLA** é o único com responsabilidade dual documentada (Diretoria Comercial + Diretoria de Operações) e governa os prazos de triagem do contexto de Devolução. Nenhum documento cobre frete padrão (abaixo de 500 kg), o que constitui uma lacuna de domínio fora do escopo do assistente atual.

---

## 2. Tabela de Bounded Contexts

| # | Contexto | Missão | Dentro | Fora | Consome de | Encaminha para / Publica para | Evidências (trecho) | Risco / Lacuna |
|---|---|---|---|---|---|---|---|---|
| **BC1** | **Devolução de Mercadorias** | Decidir elegibilidade, prazo, custo e procedimento de toda devolução pós-entrega. | Prazo 7 dias úteis; triagem 4h úteis (regra interna, independente do relógio de SLA de BC3); coleta reversa; reembolso em 5d; custo por responsabilidade; devoluções parciais por CT-e. | Cálculo de frete reverso (BC2); elegibilidade de carga perigosa, refrigerada e lacrada (BC4); apuração de dano em trânsito (BC5). | BC2 (valor frete reverso), BC3 (SLA de triagem por tier), BC4 (decisão sobre cargas especiais) | BC5 (sinaliza carga danificada), BC4 (encaminha perigosas/refrigeradas/lacradas) | POL-001 §3.1: "7 dias úteis após recebimento confirmado no tracking"; §3.3: "4 horas úteis para triagem"; §3.5: "defeito da NovaTech → devolução sem custo". | (1) Prazo "7 dias" conta a partir do tracking; se tracking falhar, data-base é responsabilidade de BC3 (rastrear incidente). (2) Regra de pausa do prazo de triagem fora do horário comercial não está documentada no corpus — registrada como Pendência DP-08. |
| **BC2** | **Precificação de Frete Especial** | Calcular o valor e prazo de fretes acima de 500 kg usando tabela vigente de multiplicadores e fatores de peso. | Fórmula `valor_base × regional × peso`; multiplicadores por região; fator de peso; +3 dias úteis (v2); desconto por volume; aprovação gerencial acima de 5.000 kg. | Decisão de elegibilidade de devolução (BC1); conformidade regulatória de cargas perigosas (BC4); definição de SLA (BC3); frete para cargas abaixo de 500 kg (lacuna L5). | BC4 (verificar se carga é perigosa → tabela PROC-043) | BC1 (custo do frete reverso), BC4 (confirmação de peso e tipo) | PROC-042 v1 §2: "Fator de peso = 1.0 / 1.2 / 1.5"; PROC-042-v2 §2: "1.0 / 1.15 / 1.4"; v2 §3: "+3 dias úteis (antes eram +2)". | **Conflito crítico**: dois documentos vigentes com multiplicadores, fatores de peso e prazos diferentes. Regra de transição (v2 §5) já expirou (dez/2023) mas PROC-042 v1 não foi arquivado. Hipótese: usar v2 como vigente; registrar dependência de confirmação pelo Comercial para contratos antigos (FAQ-8). |
| **BC3** | **Gestão de SLA e Atendimento** | Definir os compromissos contratuais de tempo de resposta e resolução por tier de cliente, e classificar incidentes críticos. | Tiers Gold/Silver/Standard; SLAs de primeira resposta e resolução; SLA de tracking (99,5/99/98%); incidentes críticos (4 critérios); penalidades por violação; medição via Azure DevOps. Relógio de SLA pausa fora do horário comercial (08h–18h, dias úteis) para chamados gerais; não pausa para incidentes críticos Gold. | Regras de cálculo de frete (BC2); processo de triagem de devolução (BC1 — a triagem de 4h é regra interna de BC1, não de BC3); conformidade ANTT (BC4). | BC1 (informa SLA de primeira resposta por tier, que pode ser anterior às 4h de triagem interna), BC5 (carga danificada acima de R$100k → incidente crítico) | BC1 (SLA de triagem por tier), BC2 (implícito: urgência de cálculo para Gold), BC4 (carga perigosa com irregularidade → crítico) | SLA-2024 §2: "Gold: 2h resposta / 24h resolução"; §3: "valor declarado >R$100k desconhecido >6h = crítico"; §3: "carga perigosa com qualquer irregularidade = crítico". | Responsabilidade dual (Comercial + Operações) pode gerar conflito em revisões de SLA. Hipótese: Comercial decide tier; Operações decide classificação de incidente. |
| **BC4** | **Conformidade e Riscos de Carga** | Custodiar as regras de elegibilidade e tratamento especial de cargas reguladas (perigosas, refrigeradas, lacradas), acionando Gestão de Riscos para decisões individuais. | Classes ANTT 1–6 (definição técnica obrigatória); regra de cadeia de frio (sensor IoT, >30 min); lacre de segurança violado com e sem documentação; encaminhamento ao ramal 4500; referência à PROC-043 (em revisão). **Regra de identificação por nome coloquial:** quando a carga for descrita por nome que não permite classificação ANTT inequívoca (ex.: "produto químico", "bateria", "tinta"), o assistente deve solicitar ao atendente a classificação ANTT antes de decidir o roteamento, evitando falso positivo (roteamento indevido) e falso negativo (omissão de risco de segurança). | Cálculo de valor de frete (BC2); processo de devolução padrão (BC1); sinistros (BC5). | BC1 (recebe solicitação de devolução de perigosa), BC2 (recebe carga perigosa >500kg), BC3 (incidente crítico de carga perigosa) | BC1, BC2, BC3 (publica decisão de elegibilidade e rotas de escalação) | POL-001 §3.2: "cargas perigosas classes 1–6 ANTT NÃO elegíveis para processo padrão"; "cadeia de frio rompida >30 min contínuos conforme sensor IoT"; FAQ-32: "expresso com perigosa requer autorização do Compliance". | (1) Processo de escalação para ramal 4500 não está documentado formalmente (lacuna L1). (2) Identificação por nome coloquial introduz ambiguidade operacional — regra de solicitação de classificação ANTT adicionada nesta versão (A-07). |
| **BC5** | **Sinistros e Carga Danificada** | Gerir o processo de apuração e reembolso de mercadorias danificadas em trânsito, com encaminhamento ao Jurídico. | Registro de ocorrência em até 48h após recebimento (interpretação atual: horas corridas — Pendência DP-04 para validação pelo Jurídico); exigência de fotos e laudo; investigação de responsabilidade da NovaTech; reembolso integral se comprovado; encaminhamento a sinistros@novatech.com.br. | Devoluções por desistência do cliente (BC1); cálculo de frete (BC2). | BC3 (valor >R$100k → incidente crítico), BC1 (distinguir devolução de sinistro) | BC3 (publica incidente crítico por carga de alto valor), Jurídico (externo ao assistente) | FAQ-38: "carga danificada em trânsito tem processo diferente de devolução"; "registrar em até 48h com fotos e laudo"; "reembolso integral se comprovada responsabilidade"; "encaminhar para sinistros@novatech.com.br — passa pelo Jurídico". | **Lacuna crítica**: toda a regra deste contexto existe apenas no FAQ informal, sem POL ou PROC formal. Hipótese operacional: aplicar FAQ-38 até publicação de normativo; sinalizar ao usuário que o processo "passa pelo Jurídico" e não pelo atendimento normal. Prazo de 48h: horas corridas por hipótese — validação Jurídico pendente (DP-04). |
| **BC6** | **Seguro de Carga** | Informar as condições e percentuais de seguro de carga oferecidos pela NovaTech como adicional contratual. | Percentual 0,3% (padrão) e 0,8% (perigosas); escopo: contratos a partir de 2023; orientação para consultar Comercial em contratos antigos. | Cálculo de frete (BC2); elegibilidade de devolução (BC1); regras de conformidade (BC4). | BC4 (distinção entre carga padrão e perigosa para alíquota), BC2 (contratos antigos com frete diferenciado) | **Encaminha para** (orientação ao usuário, sem integração técnica): Comercial (externo ao assistente). ⚠️ "Encaminha para" substituiu "Publica para" neste contexto: o assistente orienta o usuário a contatar o Comercial; não existe publicação técnica de dados para esse setor (A-18). | FAQ-22: "NovaTech oferece seguro como adicional — 0,3% do valor declarado para padrão e 0,8% para perigosas; contratos a partir de 2023". | **Lacuna crítica**: única fonte é FAQ-22 (documento informal, não validado por Compliance). Sem POL ou PROC formal, este contexto não deve ser acionado com confiança plena. Hipótese: responder com os dados do FAQ, mas sempre indicar confirmação pelo Comercial. |

---

## 3. Matriz de Relações entre Contextos

| Origem | Destino | Tipo de relação | Gatilho |
|--------|---------|-----------------|---------|
| BC1 — Devolução | BC4 — Conformidade | **Delegação obrigatória** | Carga perigosa com classe ANTT conhecida, refrigerada ou lacrada → BC1 não decide sozinho. Carga descrita por nome coloquial ambíguo → BC4 solicita classificação antes de decidir. |
| BC1 — Devolução | BC5 — Sinistros | **Roteamento** | Carga danificada em trânsito não é devolução padrão → redirecionamento |
| BC2 — Precificação | BC1 — Devolução | **Dados (pull)** | BC1 consome custo do frete reverso calculado por BC2 |
| BC3 — SLA | BC1 — Devolução | **Restrição contratual** | SLA de primeira resposta por tier pode ser anterior às 4h de triagem interna de BC1. Os dois prazos são métricas independentes e não devem ser colapsados. |
| BC3 — SLA | BC4 — Conformidade | **Escalação automática** | Qualquer irregularidade com carga perigosa → incidente crítico imediato |
| BC4 — Conformidade | BC2 — Precificação | **Referência normativa** | Carga perigosa >500 kg → tabela PROC-043, não PROC-042 |
| BC4 — Conformidade | BC3 — SLA | **Evento** | Carga perigosa com irregularidade publica incidente crítico para BC3 |
| BC5 — Sinistros | BC3 — SLA | **Evento** | Carga >R$100k com status desconhecido >6h publica incidente crítico |
| BC6 — Seguro | BC4 — Conformidade | **Dados (pull)** | BC6 consome classificação de carga (padrão vs. perigosa) para determinar alíquota |

---

## 4. Lista de Lacunas e Hipóteses

**L1 — Processo formal de Gestão de Riscos (BC4)**
Lacuna: POL-001 §3.2 menciona o ramal 4500, mas não existe PROC documentando o que acontece depois. Hipótese: orientar o cliente a ligar para o ramal 4500 e registrar que o tratamento é individual, sem prometer resultado.

**L2 — Política formal de Carga Danificada (BC5)**
Lacuna: Toda a regra existe apenas no FAQ-38 (não validado por Compliance). Hipótese operacional: seguir FAQ-38, explicitar que o processo passa pelo Jurídico (não pelo atendimento padrão), e indicar sinistros@novatech.com.br.

**L3 — Documento normativo de Seguro de Carga (BC6)**
Lacuna: Único dado disponível está em FAQ-22. Hipótese: fornecer os percentuais com ressalva explícita de confirmação pelo Comercial, especialmente para contratos pré-2023.

**L4 — Versão vigente do PROC-042 (BC2)**
Lacuna: Dois documentos coexistem sem hierarquia formal. A regra de transição do §5 da v2 já expirou. Hipótese: adotar v2 como padrão para chamados novos; alertar que contratos antigos podem referenciar v1.

**L5 — Frete padrão abaixo de 500 kg**
Lacuna: Nenhum documento cobre essa faixa. Hipótese: fora do escopo do assistente até inclusão de documento normativo específico.

**L6 — Frete expresso para carga perigosa (FAQ-32)**
Lacuna: FAQ-32 descreve um processo informal sem PROC. A nota da PROC-043 indica que ela está "em revisão pelo Compliance". Hipótese: sinalizar que é possível com autorização do Compliance, sem detalhar prazos que podem mudar.

**L7 — Pausa do prazo de triagem (BC1) fora do horário comercial (nova — A-14 / DP-08)**
Lacuna: POL-001 §3.3 define "4 horas úteis" para triagem, mas não especifica se esse prazo segue a mesma regra de pausa do relógio de SLA de BC3 (pausa entre 18h e 8h para chamados gerais). Hipótese conservadora: a triagem de 4h de BC1 é regra interna independente do relógio de BC3; até validação operacional com a equipe de Operações (DP-08), o assistente deve informar os dois prazos como métricas distintas sem afirmar pausa.

---

## 5. Checklist de Validação

**Sem sobreposição crítica**
- Devolução (BC1) e Sinistros (BC5) tratam de eventos distintos: BC1 cobre mercadoria após entrega bem-sucedida; BC5 cobre dano ocorrido durante o transporte. Separação sustentada por FAQ-38. ✅
- Conformidade (BC4) e Precificação (BC2) não se sobrepõem: BC4 decide *se* a carga pode circular; BC2 decide *quanto* custa. Carga perigosa >500kg é tratada por PROC-043, fora do escopo de BC2. ✅
- SLA (BC3) governa *tempo de atendimento*; a triagem de 4h de BC1 é regra interna de processo — as duas métricas são explicitamente independentes nesta versão. ✅

**Sem regra órfã**
- Regra de desconto por volume (PROC-042 §4 / v2 §4): alocada em BC2. ✅
- Penalidades por violação de SLA (SLA-2024 §4): alocadas em BC3. ✅
- Regra de crédito (5% / 10%) para violação de SLA: pertence a BC3, referenciada como evento para BC1. ✅
- Regra de lacre violado com documentação (POL-001 §3.2): alocada em BC4. ✅
- Seguro de carga (FAQ-22): isolado em BC6. ✅
- Regra de identificação de carga perigosa por nome coloquial: alocada em BC4 (nova nesta versão). ✅

**Sem termo com significado conflitante dentro do mesmo contexto**
- "Prazo": em BC1 = dias para solicitar devolução; em BC2 = dias de entrega; em BC3 = tempo de atendimento. ✅
- "Carga perigosa": único dono semântico é BC4 (definição ANTT). BC1 e BC2 delegam para BC4. ✅
- "Reembolso": em BC1 = crédito de devolução; em BC5 = ressarcimento por sinistro. ✅
- "Encaminha para" vs. "Publica para": distinção explicitada em BC6 para eliminar ambiguidade sobre integração técnica (A-18). ✅
