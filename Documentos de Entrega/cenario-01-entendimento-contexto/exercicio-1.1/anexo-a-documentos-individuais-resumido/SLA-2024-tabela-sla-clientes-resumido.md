# SLA-2024 — Tabela de SLA por Tipo de Cliente — Resumo Curado

## Metadados

- **tipo_documento:** Documento contratual — compromissos formais de nível de serviço com clientes
- **versao:** 2024.1
- **data_referencia:** 02/01/2024
- **responsavel:** Diretoria Comercial e Diretoria de Operações (responsabilidade conjunta)
- **status_normativo:** Validado — documento contratual com classificação formal e responsáveis definidos
- **publico_alvo:** Time de atendimento ao cliente, gerentes de conta e operações; referência contratual para clientes
- **tema_principal:** Definição de tiers de clientes, SLAs por tier, critérios de incidente crítico, penalidades por descumprimento e metodologia de medição
- **escopo_resumo:** Documento integral — cobre critérios de classificação, tabela completa de SLAs, definição de incidente crítico, regime de penalidades e regras de medição
- **dependencias_referencias:** Sistema de chamados Azure DevOps (medição de timestamps de SLA)
- **confiabilidade:** Alta — documento contratual versionado, com dois responsáveis formais, data definida e classificação explícita de uso obrigatório

## Resumo

O documento define três tiers de clientes (Gold, Silver e Standard) com critérios objetivos de elegibilidade por valor de contrato anual ou volume de operações mensais, e estabelece SLAs diferenciados por tier para tempo de primeira resposta e resolução em chamados gerais (2h/24h, 4h/48h e 8h/72h, respectivamente) e incidentes críticos (30min/4h, 1h/8h e 2h/24h). Incidentes críticos são definidos por quatro critérios independentes, incluindo carga de alto valor com status desconhecido, carga perigosa com irregularidade e risco à segurança de pessoas. O regime de penalidades por descumprimento escala em três níveis mensais: registro interno, crédito de 5% e crédito de 10% com reunião obrigatória. A medição é feita pelo Azure DevOps com pausa do relógio fora do horário comercial para chamados gerais, exceto para incidentes críticos de clientes Gold. Hipóteses de gap: ausência de SLA específico para o processo de devolução (coberto na POL-001 sem referência cruzada ao SLA por tier); sem definição de horário comercial para clientes Silver e Standard em incidentes críticos (a pausa é mencionada apenas para Gold); ausência de critério de rebaixamento ou upgrade de tier durante o ano contratual; sem cobertura de SLA para canais alternativos ao sistema de chamados (telefone, e-mail); e ausência de referência ao processo de contestação de penalidades pelo cliente.
