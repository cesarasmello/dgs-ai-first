# Tarefa 1 - Etapa 1 | Mapa de temas e hipóteses de gaps

---

## Seção 1: Mapa de temas cobertos

### FAQ-Atendimento (confiabilidade: Baixa)
- Tratamento operacional de cargas perigosas (devolução e envio expresso)
- Cálculo de frete especial (referência informal às duas versões da PROC-042)
- Tiers de clientes (confirmação de Gold, Silver, Standard; negação de Platinum)
- Seguro de carga (percentuais por tipo de carga e data de contrato)
- Rastreamento parado (orientação prática por rota)
- Sinistros por avaria (registro em 48h, encaminhamento ao Jurídico)
- SLA de resposta e resolução por tier (referência à SLA-2024)
- Política de descontos (autonomia restrita ao Comercial)

### POL-001 — Política de Devolução (confiabilidade: Alta)
- Prazo padrão de devolução (7 dias úteis pós-recebimento)
- Categorias inelegíveis para devolução padrão (cargas perigosas classes 1–6, cadeia de frio rompida, lacre violado)
- Procedimento de devolução passo a passo (Portal do Cliente, CT-e, fotos, triagem, coleta reversa, reembolso)
- Devoluções parciais por volume (reembolso proporcional ao CT-e)
- Custos de frete reverso (responsabilidade por tipo de ocorrência)
- Encaminhamento de exceções ao ramal 4500 (Gestão de Riscos)

### PROC-042-v1 — Frete Especial v1 (confiabilidade: Baixa-média)
- Fórmula de cálculo de frete especial (valor base × multiplicador regional × fator de peso)
- Multiplicadores regionais — versão 2023 (Sul 1,2 / Sudeste 1,0 / Centro-Oeste 1,3 / Nordeste 1,4 / Norte 1,6)
- Fatores de peso por faixa (1,0 / 1,2 / 1,5)
- Prazo adicional de manuseio (+2 dias úteis)
- Condições especiais (aprovação para cargas >5.000kg; encaminhamento à PROC-043 para perigosas)
- Desconto por volume (>10 fretes/mês, via Comercial e aditivo contratual)

### PROC-042-v2 — Frete Especial v2 (confiabilidade: Média)
- Fórmula de cálculo (mesma estrutura da v1, parâmetros revisados)
- Multiplicadores regionais — versão nov/2023 (Sul 1,3 / Sudeste 1,1 / Centro-Oeste 1,4 / Nordeste 1,5 / Norte 1,8)
- Fatores de peso revisados (1,0 / 1,15 / 1,4)
- Prazo adicional de manuseio revisado (+3 dias úteis)
- Desconto por volume estruturado em faixas automáticas (5% >8 fretes/mês; 10% >15 fretes/mês)
- Disposições transitórias (corte em 01/12/2023 para aplicação de versão)

### SLA-2024 — Tabela de SLA (confiabilidade: Alta)
- Critérios de classificação por tier (Gold, Silver, Standard)
- SLAs de resposta e resolução por tier para chamados gerais e incidentes críticos
- Definição formal de incidente crítico (4 critérios independentes)
- Regime de penalidades por descumprimento (3 níveis mensais)
- Metodologia de medição (Azure DevOps, horário comercial 08h–18h)
- Gerente de conta dedicado e relatórios de performance por tier

---

## Seção 2: Sobreposições e potenciais conflitos

### 2.1 Sobreposições temáticas (temas cobertos por mais de um documento)

| Tema | Documentos | Tipo |
|------|-----------|------|
| Tiers de clientes (Gold, Silver, Standard) | SLA-2024 + FAQ | Complementar — FAQ confirma informalmente o que SLA-2024 formaliza |
| Cargas perigosas | POL-001 + PROC-042-v1 + PROC-042-v2 + FAQ | Complementar com risco de conflito — cada documento trata um aspecto diferente (devolução, frete, atendimento), mas sem referências cruzadas explícitas entre si |
| Frete especial (cálculo) | PROC-042-v1 + PROC-042-v2 + FAQ | **Conflito direto** — parâmetros divergentes entre v1 e v2; FAQ referencia ambas sem critério de escolha |
| Ramal 4500 / Gestão de Riscos | POL-001 + FAQ | Complementar — POL-001 formaliza o encaminhamento; FAQ orienta operacionalmente |
| SLA de resposta e resolução | SLA-2024 + FAQ | Complementar com risco de desatualização — FAQ descreve os SLAs de forma resumida, mas sem garantia de alinhamento com a versão formal |
| Desconto por volume | PROC-042-v1 + PROC-042-v2 | **Conflito direto** — v1 exige negociação via Comercial para >10 fretes/mês; v2 automatiza descontos a partir de 8 fretes/mês |
| Avaria/sinistro em carga | POL-001 (prazo 48h + encaminhamento ao Jurídico via e-mail) + FAQ (mesmo prazo, mesmo encaminhamento) | Complementar — consistência entre os dois, mas POL-001 usa o e-mail sinistros@novatech.com.br enquanto FAQ não cita o e-mail |

### 2.2 Temas exclusivos (cobertos por apenas um documento)

| Tema | Documento exclusivo |
|------|-------------------|
| Devoluções parciais por volume | POL-001 |
| Custos de frete reverso por tipo de ocorrência | POL-001 |
| Cadeia de frio rompida como critério de inelegibilidade | POL-001 |
| Disposições transitórias entre versões da PROC-042 | PROC-042-v2 |
| Penalidades contratuais por descumprimento de SLA | SLA-2024 |
| Definição formal de incidente crítico | SLA-2024 |
| Gerente de conta dedicado para Gold | SLA-2024 |
| Seguro de carga (percentuais e condições) | FAQ (sem documento normativo correspondente identificado) |
| Interceptação de carga em trânsito | POL-001 (remete à PROC-088, não indexada) |

---

## Seção 3: Hipóteses de gaps

| Gap | Evidência | Impacto no negócio | Confiança | Pergunta para discovery |
|-----|-----------|--------------------|-----------|------------------------|
| **G1 — Coexistência sem hierarquia entre PROC-042-v1 e v2** | Ambos os documentos declaram explicitamente que não há indicação formal de vigência ou obsolescência; FAQ referencia as duas versões sem critério de escolha | Cotações inconsistentes para clientes na mesma rota/peso; risco de subdeclaração ou sobrecobran ça de frete | Alto | Existe alguma comunicação interna ou e-mail que formalize a data de vigência da v2 e a revogação da v1? Quem tem autoridade para formalizar essa decisão? |
| **G2 — Ausência de SLA diferenciado por tier no processo de devolução** | POL-001 define prazos operacionais únicos (triagem em 4h, coleta em 2 dias úteis, reembolso em 5 dias úteis) sem distinção por tier; SLA-2024 não menciona devolução | Clientes Gold podem receber o mesmo tratamento de clientes Standard em devoluções, contrariando o valor percebido do tier e possivelmente os contratos | Alto | O contrato Gold prevê algum SLA diferenciado para devoluções? Existe pressão de clientes Gold sobre esse ponto? |
| **G3 — Seguro de carga sem documento normativo formal** | FAQ descreve percentuais de seguro (0,3% padrão; 0,8% perigosas) com ressalva de que contratos anteriores a 2023 podem ter percentuais diferentes, mas não há POL ou PROC correspondente no corpus | Risco de orientação incorreta ao cliente; ausência de base normativa para contestações ou sinistros envolvendo seguro | Alto | Existe uma política formal de seguro de carga? Está versionada? Onde está armazenada? |
| **G4 — PROC-088 (Interceptação de Carga) ausente do corpus** | POL-001 remete à PROC-088 para cargas ainda em trânsito, mas o documento não está indexado | Atendentes sem acesso ao procedimento correto para interceptação; risco de orientação incorreta em situações de urgência | Alto | A PROC-088 existe e está vigente? Está disponível no SharePoint ou apenas em sistema legado? |
| **G5 — PROC-043 (Frete de Cargas Perigosas) em revisão pelo Compliance** | PROC-042-v2 informa que a PROC-043 está em processo de revisão; nenhuma versão da PROC-043 está no corpus | Orientações para cotação e atendimento de cargas perigosas acima de 500kg podem estar desatualizadas ou indisponíveis | Alto | Qual o status atual da revisão da PROC-043? Há uma versão provisória em uso? Existe previsão de conclusão? |
| **G6 — Política de desconto por volume inconsistente entre v1 e v2 da PROC-042** | v1: desconto negociado pelo Comercial para >10 fretes/mês; v2: desconto automático de 5% para >8 fretes/mês e 10% para >15 fretes/mês | Clientes com contratos baseados na v1 podem reclamar de desconto não aplicado ou aplicado incorretamente; risco de perda de receita se v2 for aplicada retroativamente | Médio | Os contratos vigentes já incorporam a nova tabela de descontos da v2? Existe aditivo contratual assinado ou a mudança é unilateral? |
| **G7 — Tabela mensal de fretes (valor base) sem localização referenciada** | Ambas as versões da PROC-042 mencionam "tarifa publicada na tabela mensal de fretes" sem indicar onde está armazenada ou como acessá-la | Impossibilidade de cálculo autônomo de frete especial sem consulta humana adicional; risco de inconsistência se a tabela for atualizada sem comunicação ao time | Médio | Onde está publicada a tabela mensal de fretes? Quem a atualiza e com qual frequência? Existe notificação automática para o time ao atualizá-la? |
| **G8 — Ausência de SLA para aprovação de cargas acima de 5.000kg** | PROC-042-v1 e v2 exigem aprovação prévia do gerente de operações regional para cargas acima de 5.000kg, sem prazo definido para essa aprovação | Cliente pode ficar sem previsão de cotação; risco de perda de negócio ou de SLA de atendimento sendo comprometido por gargalo interno | Médio | Existe um SLA interno para aprovação do gerente regional? Como o time de atendimento deve comunicar o cliente enquanto aguarda a aprovação? |
| **G9 — Horário comercial não definido para incidentes críticos de Silver e Standard** | SLA-2024 especifica que o relógio de SLA não pausa para incidentes críticos de clientes Gold, mas é omisso sobre Silver e Standard | Ambiguidade operacional: o atendimento fora do horário comercial para incidentes críticos de Silver/Standard pode variar por interpretação individual | Médio | O relógio de SLA para incidentes críticos de Silver e Standard pausa fora do horário comercial? Há escala de plantão para esses tiers? |
| **G10 — 38 dos 47 itens do FAQ não foram indexados** | FAQ-atendimento cobre apenas 9 dos 47 itens originais; os demais não estão disponíveis no corpus | Lacuna de cobertura desconhecida: podem existir orientações operacionais críticas, gambicarras consolidadas ou conflitos adicionais com documentos normativos nos itens não indexados | Baixo | É possível obter o FAQ completo com todos os 47 itens? Existe revisão periódica do FAQ por Compliance ou Operações? |

---

## Seção 4: Resumo executivo

O corpus analisado cobre as principais áreas operacionais do atendimento NovaTech — devolução, frete especial, SLA e orientações práticas de atendimento —, porém com qualidade normativa desigual: dois documentos são formalmente validados (POL-001 e SLA-2024), dois estão em situação de vigência indefinida (PROC-042-v1 e v2) e um é informal sem validação de Compliance (FAQ). O risco mais crítico identificado é a coexistência não resolvida entre as duas versões da PROC-042, que gera parâmetros de frete diretamente conflitantes e pode resultar em cobranças inconsistentes para clientes na mesma rota. Um segundo risco de alto impacto é a ausência de documento normativo formal para seguro de carga, tema que o FAQ aborda com ressalvas mas que não possui POL ou PROC correspondente no corpus. A ausência de SLA diferenciado por tier no processo de devolução representa uma inconsistência entre o que o contrato Gold promete em atendimento e o que a política de devolução entrega na prática. Três documentos referenciados como dependências críticas estão ausentes ou indisponíveis (PROC-088, PROC-043 em revisão, tabela mensal de fretes), o que limita a autonomia operacional do time de atendimento. A prioridade imediata para o discovery humano deve ser a formalização da hierarquia entre as versões da PROC-042 e o levantamento da política de seguro de carga, antes de qualquer indexação desse corpus em ambiente de produção.
