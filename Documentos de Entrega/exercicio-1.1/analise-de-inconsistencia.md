# Análise de Inconsistências — PROC-042 vs PROC-042-v2

**Analista:** Product Specialist — Intent + Discovery  
**Data da análise:** 01/06/2026  
**Documentos analisados:**
- `PROC-042-frete-especial-v1.md` (Versão 1.0 — emitida em 03/03/2023)
- `PROC-042-v2-frete-especial-revisado.md` (Versão 2.0 — emitida em 10/11/2023)

---

## Resumo Executivo

A análise comparativa entre o PROC-042 v1 e o PROC-042 v2 identificou **6 inconsistências**, distribuídas entre regras conflitantes com valores numéricos divergentes, fluxos operacionais alterados sem revogação formal da versão anterior, condições de exceção presentes apenas em um dos documentos e ausência de hierarquia documental clara.

Os dois documentos coexistem no repositório sem que o v2 declare formalmente a revogação do v1, o que cria risco operacional direto: equipes distintas podem estar aplicando fórmulas, prazos e regras de desconto incompatíveis para o mesmo tipo de carga.

Os pontos de maior risco são:

1. **Fator de peso divergente** — os multiplicadores aplicados à faixa de 1.001kg–3.000kg e acima de 3.000kg diferem entre os dois documentos, impactando diretamente o valor cobrado ao cliente.
2. **Multiplicadores regionais divergentes** — todas as cinco regiões têm valores diferentes entre v1 e v2, sem critério de aplicação claro para chamados em andamento.
3. **Prazo de entrega divergente** — v1 prevê +2 dias úteis; v2 prevê +3 dias úteis. Compromissos firmados com clientes podem estar baseados em versões diferentes.
4. **Regra de desconto por volume incompatível** — v1 não define percentual de desconto; v2 define regras objetivas (5% a partir de 8 fretes/mês; 10% acima de 15/mês), criando disparidade na negociação comercial.
5. **Nota sobre revisão do PROC-043** — presente apenas no v2, ausente no v1, podendo levar operadores a aplicar a referência sem ciência da instabilidade.
6. **Ausência de declaração formal de substituição** — nenhum dos dois documentos indica de forma explícita qual versão está vigente, criando risco sistêmico de uso simultâneo.

**Recomendação geral:** A Diretoria Comercial deve emitir formalmente a obsolescência do PROC-042 v1, consolidar as regras no v2 e garantir comunicação às áreas operacionais sobre os critérios de transição já descritos na Seção 5 do v2.

---

## Matriz de Inconsistências

| ID | Tipo | Trecho no PROC-042-v2 | Trecho no PROC-042-v1 | Descrição da inconsistência | Recomendação | Responsável sugerido |
|----|------|-----------------------|-----------------------|-----------------------------|--------------|----------------------|
| INC-001 | Regra conflitante — valor numérico | Seção 2: *"Fator de peso = 1.0 para cargas de 500kg a 1.000kg; **1.15** para cargas de 1.001kg a 3.000kg; **1.4** para cargas acima de 3.000kg."* | Seção 2: *"Fator de peso = 1.0 para cargas de 500kg a 1.000kg; **1.2** para cargas de 1.001kg a 3.000kg; **1.5** para cargas acima de 3.000kg."* | Os fatores de peso para as faixas de 1.001kg–3.000kg e acima de 3.000kg diferem entre as versões (v1: 1.2 e 1.5 / v2: 1.15 e 1.4). Aplicações simultâneas das duas versões produzem valores de frete distintos para a mesma carga. | Confirmar qual tabela de fatores está vigente, deprecar formalmente a v1 e comunicar a mudança às equipes de cotação e faturamento. | Diretoria Comercial |
| INC-002 | Regra duplicada com valores diferentes | Seção 2.1: *"Sul: 1.3 / Sudeste: 1.1 / Centro-Oeste: 1.4 / Nordeste: 1.5 / Norte: 1.8"* (atualizados em novembro/2023) | Seção 2.1: *"Sul: 1.2 / Sudeste: 1.0 / Centro-Oeste: 1.3 / Nordeste: 1.4 / Norte: 1.6"* | Todos os cinco multiplicadores regionais são divergentes entre v1 e v2. A v2 apresenta valores superiores em todas as regiões. A coexistência dos documentos permite cotações inconsistentes para o mesmo destino. | Publicar oficialmente os multiplicadores da v2 como únicos válidos a partir de 01/12/2023 (conforme Seção 5 do v2) e arquivar a tabela da v1. | Diretoria Comercial / Gerência de Operações |
| INC-003 | Fluxo operacional divergente | Seção 3: *"prazo padrão da rota + **3 dias úteis** adicionais para manuseio e roteirização de carga pesada (anteriormente era + 2 dias na versão anterior)"* | Seção 3: *"prazo padrão da rota + **2 dias úteis** adicionais para manuseio de carga pesada"* | O prazo adicional para frete especial é de 2 dias úteis no v1 e de 3 dias úteis no v2. O próprio v2 reconhece a alteração, mas sem que o v1 tenha sido formalmente revogado, SLAs comunicados a clientes podem estar baseados em versões diferentes. | Garantir que todos os contratos e comunicações com clientes reflitam o prazo de +3 dias úteis da v2. Revogar formalmente o prazo de +2 dias do v1. | Diretoria Comercial / Atendimento ao Cliente |
| INC-004 | Regra conflitante — condição de desconto | Seção 4: *"a partir de 8 fretes especiais/mês para o mesmo cliente, aplicar desconto de 5% sobre o multiplicador regional. Acima de 15 fretes/mês, desconto de 10%. Descontos maiores requerem aprovação da Diretoria Comercial."* | Seção 4: *"Descontos de volume (mais de 10 fretes especiais/mês para o mesmo cliente) devem ser negociados pelo Comercial e registrados em aditivo contratual."* | As regras de desconto por volume são incompatíveis: v1 exige negociação e aditivo contratual acima de 10 fretes/mês, sem percentual definido; v2 define percentuais automáticos (5% a partir de 8 fretes/mês; 10% acima de 15/mês) com aprovação da Diretoria apenas para descontos maiores. O limiar de ativação (8 vs. 10 fretes/mês) e o mecanismo de concessão são diferentes. | Definir a regra única de desconto a ser aplicada, comunicar ao time Comercial e eliminar a ambiguidade contratual gerada pela coexistência das duas versões. | Diretoria Comercial |
| INC-005 | Condição de exceção presente em um documento e ausente no outro | Seção 4: *"Nota: a PROC-043 está em processo de revisão pelo Compliance e pode sofrer alterações."* | Seção 4: *"Cargas perigosas com peso acima de 500kg seguem tabela específica (PROC-043: Frete de Cargas Perigosas)."* (sem nenhuma nota sobre revisão) | O v2 alerta que a PROC-043 está sob revisão pelo Compliance, informação ausente no v1. Operadores que consultem apenas o v1 aplicarão a referência sem ciência da instabilidade da norma subsidiária, podendo incorrer em não-conformidade. | Incluir comunicado formal sobre o status da PROC-043 em todos os documentos que a referenciam, ou emitir circular temporária até a conclusão da revisão. | Compliance / Diretoria Comercial |
| INC-006 | Ausência de hierarquia documental — risco sistêmico | Seção Status: *"Este documento não possui indicação formal de que substitui o PROC-042 v1. Ambos coexistem no SharePoint sem hierarquia clara."* | Seção Status: *"Este documento não possui indicação formal de vigência ou obsolescência no sistema da NovaTech. Coexiste com a versão PROC-042-v2."* | Nenhum dos dois documentos declara formalmente qual versão está em vigor. A ausência de marcação de obsolescência no v1 e de declaração de substituição no v2 cria risco sistêmico: qualquer colaborador que acesse o repositório pode utilizar a versão incorreta sem saber. A Seção 5 do v2 define critérios de transição por data de abertura de chamado, mas essa seção não é referenciada nem conhecida por quem consulta apenas o v1. | Implementar controle de versão formal no sistema de gestão documental: marcar o v1 como "Obsoleto" e o v2 como "Vigente". Adicionar cabeçalho de substituição no v2 e link de redirecionamento no v1. | Gestão Documental / Diretoria Comercial |

---

*Análise realizada exclusivamente com base no conteúdo dos documentos PROC-042 v1 e PROC-042 v2. Nenhum conhecimento externo foi utilizado.*
