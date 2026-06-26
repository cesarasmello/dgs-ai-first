# Registro da Conversa — Mockup Assistente NovaTech no Microsoft Teams

> Documento de rastreabilidade da sessão de trabalho.
> Projeto: **Projeto NovaTech** · Papel: Product Specialist UX/UI
> Data de referência: 10/06/2026
> Ferramenta usada: Claude design

---

## 1. Entradas / Documentos fornecidos

| # | Arquivo | Tipo | Conteúdo |
|---|---------|------|----------|
| 1 | `uploads/requirements.md` | Especificação (SDD) | Requisitos do **Query Endpoint** do assistente NovaTech — outcomes (OUT-01…07), scope boundaries (SB-01…06 / BC1–BC6), constraints (CN-01…12), prior decisions (ADR-0001…0004), critérios de verificação (CV-01…13), non-goals, open questions e assumptions. |
| 2 | `uploads/analise-de-ambiguidade.md` | Revisão técnica (Tech Lead) | Revisão crítica dos artefatos: 19 achados (3 críticos, 7 altos, 6 médios, 3 baixos), top-5 ambiguidades (AMB-01…05) e decisões pendentes (DP-01…08). |
| 3 | `uploads/mockup-formato-PDF-v1.pdf` | Mockup de referência (PDF) | Versão em PDF da interface Teams (4 estados) revisada pelo Tech Lead. |

---

## 2. Prompts / Solicitações do usuário

### Prompt 1 — Criação do mockup
Criar um **mockup da resposta do assistente no Microsoft Teams**, coerente com a especificação do query endpoint, mostrando: pergunta do atendente; resposta principal; fonte obrigatória (documento + seção); indicador de confiança; estado de baixa confiança com sugestão de escalar ao supervisor; caso de conflito entre versões (priorizar vigente e avisar versão anterior); caso sem evidência suficiente.
Regras: linguagem formal em português; não inventar regras fora das entradas; interface clara para decisão em até 30 segundos.
Saída: apenas o mockup com anotações curtas de rastreabilidade requisito → elemento da interface.

### Prompt 2 — Exportação standalone (v1)
Salvar o mockup como **HTML standalone** (arquivo único, offline).

### Prompt 3 — Ajuste a partir da revisão técnica
Ajustar **somente o mockup** com base nos achados do Tech Lead: corrigir ambiguidades de interface, rastreabilidade de fonte e estados críticos, **sem alterar requisitos de produto**, regras de negócio, guardrails ou critérios de aceite. Manter todos os itens obrigatórios do mockup.
Saída: mockup ajustado.

### Prompt 4 — Exportação standalone (v2)
Salvar o mockup ajustado como **HTML standalone** (arquivo único, offline).

### Prompt 5 — Este registro
Extrair a conversa em um arquivo markdown com as entradas, prompts, documentos fornecidos e, como saída, o nome de cada arquivo gerado.

---

## 3. Saídas / Arquivos gerados

| # | Arquivo gerado | Origem | Descrição |
|---|----------------|--------|-----------|
| 1 | `Mockup Assistente NovaTech - Teams.html` | Prompt 1 | Mockup v1 — interface Teams (light) com a resposta do bot em Adaptive Cards; 4 estados + gutter de rastreabilidade requisito → elemento. |
| 2 | `Mockup Assistente NovaTech - Teams (standalone).html` | Prompt 2 | Versão v1 autocontida (offline), com splash/thumbnail. |
| 3 | `Mockup Assistente NovaTech - Teams v2.html` | Prompt 3 | Mockup v2 ajustado pós-revisão técnica (apenas interface). |
| 4 | `Mockup Assistente NovaTech - Teams v2 (standalone).html` | Prompt 4 | Versão v2 autocontida (offline), com splash/thumbnail. |
| 5 | `Registro da Conversa - Mockup NovaTech.md` | Prompt 5 | Este documento. |

> Observação: os arquivos `Mockup Assistente NovaTech - Teams-print.html` e `Mockup Assistente NovaTech - Teams v2-print.html` presentes no projeto foram gerados a partir do fluxo de impressão/visualização, não pelo processo de design descrito acima.

---

## 4. Resumo dos ajustes da v1 → v2 (escopo de interface)

Todos os ajustes foram **exclusivamente de interface** — nenhum requisito de produto, regra de negócio ou critério de aceite foi alterado.

| Achado | Ajuste de interface aplicado |
|--------|------------------------------|
| **A-11** | Removido o botão "Comparar versões" do estado de conflito (sem requisito correspondente); mantido apenas "Abrir PROC-042-v2". |
| **A-15 / AMB-02** | Distinção visual e textual reforçada entre **Confiança baixa** (documento aplicável existe, porém ausente/incompleto no corpus → escalar) e **Sem evidência** (nenhum documento aplicável; tema fora do escopo → encaminhar). |
| **A-06** | Estado de escalonamento clarificado (quando aparece e o que faz). |
| **OUT-01 / CN-06** | Adicionada a linha "Ação recomendada" em todos os estados, dando o próximo passo explícito para decisão em < 30 s. |

Itens obrigatórios mantidos em ambas as versões: resposta principal, fonte (documento + seção), indicador de confiança, baixa confiança com escalonamento, conflito priorizando a versão vigente + aviso da anterior, e estado sem evidência suficiente.

**Sinalizado (não decidido):** critérios formais de atribuição dos estados de confiança permanecem como decisão de produto pendente (**DP-02**); especificação do botão de escalonamento (**DP-03**). Ambos anotados no mockup, fora do escopo de ajuste de interface.
