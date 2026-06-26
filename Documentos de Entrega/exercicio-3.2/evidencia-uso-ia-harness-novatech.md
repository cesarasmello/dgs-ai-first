# Evidência de Uso de IA — Harness de Produto NovaTech

**Sessão:** Cenário de Governança e Validação — Assistente de IA NovaTech  
**Data:** 25/06/2026  
**Modelo utilizado:** Claude Sonnet 4.6 (claude.ai)  
**Papel assumido pelo modelo:** Product Specialist sênior responsável pelo harness de produto pré-go-live  

---

## 1. Insumo: Documento Anexado

### Arquivo: `guardrails-assistente-novatech.md`

**Tipo:** Documento de produto — formalização de guardrails  
**Versão:** 1.0  
**Data do documento:** 10/06/2026  
**Contexto:** Exercício 2.2 da trilha AI First Certification Track — NovaTech  

**Conteúdo do documento (reprodução estrutural):**

O documento define 14 guardrails para o assistente de IA NovaTech (pipeline RAG), organizados nas categorias DEVE, NÃO DEVE e QUANDO EM DÚVIDA, com as seguintes seções:

- **Seção 1 — Convenções:** define os dois tipos de enforcement (prompt/probabilístico e código/determinístico) e descreve três incidentes simulados que motivaram os guardrails:
  - INC-1: prazo de devolução informado incorretamente para carga perigosa
  - INC-2: multiplicadores de versão desatualizada da PROC-042 utilizados
  - INC-3: declaração de "não encontrei informação" sobre SLA Gold, quando a informação existia

- **Seção 2 — Tabela de guardrails (G-01 a G-14):** 14 guardrails com categoria, tipo de enforcement, justificativa e incidente prevenido:

| ID | Categoria | Enforcement | Resumo |
|----|-----------|-------------|--------|
| G-01 | DEVE | Código | Toda afirmação normativa deve conter citação estruturada `[DOC-ID vX.Y, seção N]` |
| G-02 | DEVE | Prompt | Verificar categoria da carga antes de informar prazo de devolução |
| G-03 | DEVE | Código | Filtro de versionamento: PROC-042-v2 para chamados a partir de 01/12/2023 |
| G-04 | DEVE | Código | Consultas de SLA roteadas para índice SLA-2024 com expansão de sinônimos |
| G-05 | DEVE | Prompt | Respostas em português formal; sem registro coloquial do FAQ |
| G-06 | NÃO DEVE | Código | Nenhum número sem respaldo literal nos chunks recuperados |
| G-07 | NÃO DEVE | Código | Devolução de carga perigosa: substituir geração livre por template fixo (ramal 4500) |
| G-08 | NÃO DEVE | Prompt | FAQ não pode ser fonte de afirmações normativas |
| G-09 | NÃO DEVE | Código | Tiers de SLA limitados à lista fechada {Gold, Silver, Standard} |
| G-10 | QUANDO EM DÚVIDA | Código | Reexecutar busca com reformulação antes de declarar "informação não encontrada" |
| G-11 | QUANDO EM DÚVIDA | Prompt | Aplicar disposições transitórias quando versões distintas chegarem ao contexto |
| G-12 | QUANDO EM DÚVIDA | Código | Detectar conflito entre valores homólogos de versões distintas |
| G-13 | QUANDO EM DÚVIDA | Prompt | Declarar ausência e encaminhar ao canal competente quando tema não coberto |
| G-14 | QUANDO EM DÚVIDA | Prompt | Solicitar categoria da carga antes de citar prazo, se não informada |

- **Seção 3 — Síntese dos riscos mitigados:** 5 riscos (segurança/compliance, financeiro/contratual, falso negativo de recuperação, contaminação por fonte não validada, alucinação factual).

- **Seção 4 — Guardrails de implementação obrigatória em código:** lista priorizada de 7 guardrails determinísticos, em ordem de criticidade.

---

## 2. Prompt Utilizado

### Prompt integral enviado ao modelo:

---

> **Haja como Product Specialist sênior responsável por definir o harness de produto de um assistente de IA antes do go-live.**
>
> **Contexto:**  
> Estou trabalhando no cenário de governança e validação da NovaTech. O assistente já possui pipeline de RAG funcional, endpoints implementados, bot em staging e guardrails de produto definidos no cenário anterior no formato DEVE / NÃO DEVE / QUANDO EM DÚVIDA. O objetivo agora é garantir evolução contínua do assistente sem degradação de qualidade, sem violação dos guardrails e com mecanismos claros de validação humana.
>
> **Sua tarefa:**  
> Projetar um harness de produto para melhoria contínua do assistente.
>
> O harness deve cobrir obrigatoriamente estes 3 pontos:
>
> **1. Processo de feedback**  
> Explique como o feedback do atendente deve ser tratado de ponta a ponta.  
> Inclua:  
> - como o feedback é capturado  
> - como é categorizado  
> - como se identifica a causa raiz do problema  
> - quando o caso exige ajuste de prompt, correção documental, reindexação, ajuste de pipeline ou abertura para time humano  
> - como a melhoria é validada antes de produção  
>
> **2. Regression testing de produto**  
> Explique como validar mudanças no assistente antes de publicação em produção.  
> Inclua:  
> - como testar se respostas existentes não pioraram  
> - como verificar que os guardrails continuam sendo respeitados  
> - quais tipos de casos de teste devem existir  
> - quais métricas ou critérios definem aprovação ou reprovação  
> - como tratar efeitos colaterais comuns de mudanças em IA  
>
> **3. Human-in-the-loop**  
> Defina quais mudanças ou situações exigem aprovação humana antes de ir para produção.  
> Inclua:  
> - quais tipos de mudança precisam de revisão humana  
> - quem deve aprovar cada tipo de mudança  
> - em que momento da esteira essa aprovação acontece  
> - quais riscos justificam esse ponto de controle  
>
> **Regras obrigatórias:**  
> - Considere que mudanças em sistemas de IA podem gerar regressões inesperadas.  
> - Preserve os guardrails do documento guardrails-assistente-novatech como restrição obrigatória durante toda a evolução do assistente.  
> - Não responda de forma genérica.  
> - Proponha um processo operacional, claro e aplicável.  
> - Sempre que possível, diferencie o que é responsabilidade de Produto, QA, Tech Lead, Operações e Compliance.  
> - Trate o harness como um mecanismo contínuo de governança, não apenas como checklist de release.  
>
> **Formato de saída:**  
> Documento markdown e estruture a resposta com estes blocos:  
> 1. Objetivo do harness  
> 2. Princípios de governança do produto  
> 3. Fluxo de feedback ponta a ponta  
> 4. Estratégia de regression testing de produto  
> 5. Pontos de human-in-the-loop  
> 6. Papéis e responsabilidades  
> 7. Critérios de aprovação para produção  
> 8. Riscos cobertos e lacunas remanescentes

---

**Técnicas de prompt aplicadas:**

| Técnica | Aplicação no prompt |
|---------|---------------------|
| Role assignment (persona) | "Haja como Product Specialist sênior responsável por definir o harness de produto" |
| Contextualização de estado | Descrição do estágio atual do projeto (pipeline RAG funcional, bot em staging, guardrails definidos) |
| Decomposição por subtarefas | 3 pontos obrigatórios numerados, cada um com sub-itens explícitos |
| Restrição de fonte | "Preserve os guardrails do documento guardrails-assistente-novatech como restrição obrigatória" |
| Instrução negativa | "Não responda de forma genérica" |
| Diferenciação de papéis | "Diferencie o que é responsabilidade de Produto, QA, Tech Lead, Operações e Compliance" |
| Enquadramento de propósito | "Trate o harness como mecanismo contínuo de governança, não apenas como checklist de release" |
| Especificação de formato | Estrutura de 8 blocos nomeados com títulos definidos |

---

## 3. Saída Gerada pelo Modelo

O modelo produziu o documento `harness.md`, com as seguintes seções e decisões estruturais:

### Estrutura do documento gerado

**Seção 1 — Objetivo do Harness**  
Define o harness como mecanismo operacional contínuo sobre três eixos: feedback → diagnóstico → melhoria; regression testing; e human-in-the-loop.

**Seção 2 — Princípios de Governança (P-01 a P-05)**  
Cinco princípios operacionais:
- P-01: Guardrails G-01 a G-14 são imutáveis por padrão
- P-02: Toda mudança é tratada como potencial causa de regressão
- P-03: Causa raiz obrigatória antes de qualquer correção
- P-04: Rastreabilidade completa via ticket do início ao fim
- P-05: Separação de responsabilidades por papel

**Seção 3 — Fluxo de Feedback Ponta a Ponta**  
- 3 canais de captura: feedback inline (atendente), escalada de atendimento (operações), auditoria periódica (produto/QA)
- 7 categorias de feedback (F-01 a F-07): erro normativo, violação de guardrail, falso negativo de recuperação, versionamento incorreto, registro inadequado, encaminhamento omitido, alucinação factual, cobertura ausente
- Hierarquia de diagnóstico em 5 níveis: falha de recuperação → falha de versionamento → falha documental → falha de prompt → comportamento emergente do modelo
- Tabela de ação por causa raiz com responsável por linha
- Processo de validação em 7 etapas antes de produção

**Seção 4 — Estratégia de Regression Testing**  
- Suíte em 4 camadas: guardrails determinísticos (automatizados), golden set de domínio (80 casos), testes específicos por tipo de mudança, avaliação humana de guardrails probabilísticos
- Matriz de camadas obrigatórias por tipo de mudança
- Tipos de caso obrigatórios: caminho feliz, adversariais por guardrail, limite de versionamento, falha de recuperação controlada, encaminhamento
- Tabela de métricas com critérios de aprovação e reprovação imediata (G-07 com zero tolerância explícita)
- Tratamento de 4 tipos de efeito colateral

**Seção 5 — Pontos de Human-in-the-Loop**  
- Matriz de aprovação: 8 tipos de mudança × {aprovador, momento na esteira, risco justificador}
- Critérios de qualidade para aprovação humana (não apenas assinatura — inclui execução de casos adversariais em staging)
- 5 condições de escalada automática não programada (incluindo violação de G-07 com zero tolerância e modo degradado acionável pelo P.O.)

**Seção 6 — Papéis e Responsabilidades**  
Tabela com 6 papéis (Product Owner, QA, Tech Lead, Compliance, Operações, Gestão de Riscos) e responsabilidades específicas de cada um no harness.

**Seção 7 — Critérios de Aprovação para Produção**  
Checklist em 3 blocos (técnico, processo, compliance) com critérios binários obrigatórios para promoção.

**Seção 8 — Riscos Cobertos e Lacunas Remanescentes**  
- Tabela de 8 riscos cobertos com mecanismo de cobertura
- 5 lacunas remanescentes identificadas com recomendações concretas:
  - L-1: ausência de monitoramento contínuo em produção estável
  - L-2: ausência de critério para inclusão de novos temas na base
  - L-3: FAQ-Atendimento sem processo de curadoria ou depreciação formal
  - L-4: ausência de SLA de resolução por categoria de feedback
  - L-5: ausência de teste de carga e latência para guardrails determinísticos

---

## 4. Rastreabilidade da Sessão

| Campo | Detalhe |
|-------|---------|
| **Modelo** | Claude Sonnet 4.6 |
| **Interface** | claude.ai (web) |
| **Data** | 25/06/2026 |
| **Documento insumo** | `guardrails-assistente-novatech.md` v1.0 — 14 guardrails, 4 seções |
| **Interações** | 2 (prompt principal + solicitação de extração de evidência) |
| **Outputs produzidos** | `harness.md` + este documento |
| **Papel assumido pelo modelo** | Product Specialist sênior — design de harness de governança |
| **Restrição aplicada** | Guardrails G-01 a G-14 preservados como restrição obrigatória em todo o documento |
| **Técnicas de prompt** | Role assignment, contextualização de estado, decomposição por subtarefas, restrição de fonte, instrução negativa, diferenciação de papéis, enquadramento de propósito, especificação de formato |
| **Valor entregue** | Documento operacional de governança pós-go-live com fluxo de feedback, suíte de testes, matriz de aprovação humana, separação de responsabilidades e identificação de lacunas — pronto para submissão como artefato do Cenário 3 da trilha AI First |

---

## 5. Análise Crítica do Output

- O modelo acertou sem ajuste a macroestrutura do entregável: converteu o prompt em um harness contínuo de governança, e não em um checklist genérico de release, mantendo os 8 blocos pedidos e a imutabilidade dos guardrails G-01 a G-14 como premissa central.
- O modelo também acertou ao transformar requisitos abstratos em critérios operacionais verificáveis, como zero tolerância para G-07, execução em camadas da suíte de regressão e matriz explícita de aprovação humana por tipo de mudança.
- O que precisou ser complementado manualmente foi a rastreabilidade com os artefatos arquiteturais do cenário anterior: o output estava forte do ponto de vista operacional, mas não fazia conexão explícita com ADR-0002 e ADR-0004, ligação que foi adicionada na revisão do `harness.md` para fechar o critério de integração entre cenários.
- Uma lacuna identificada independentemente pelo participante foi a inconsistência na numeração da taxonomia de feedback, com duplicidade do código F-04 na tabela de categorização. Esse ponto exigiu revisão manual porque afeta auditabilidade e referência cruzada ao longo do documento.
- A principal decisão de curadoria do participante foi preservar quase toda a espinha dorsal do texto gerado e intervir apenas em pontos de governança documental: rastreabilidade entre artefatos, consistência dos códigos e explicitação do julgamento humano sobre o que foi mantido, ajustado ou corrigido.

*Este documento registra a evidência de uso da IA de forma auditável, incluindo o insumo fornecido, o prompt integral utilizado e a estrutura completa da saída gerada.*
