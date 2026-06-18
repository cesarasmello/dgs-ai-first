
ACHADO 1 — CRÍTICO
Item: RF-02 contradiz a seção 5.1 e o próprio RF-04 ao impor uma versão como padrão sem resolução formal do conflito
Evidência: RF-02 determina que o assistente use "exclusivamente a PROC-042-v2 para chamados novos." A seção 5.1 diz que entre dois PROCs "prevalece o de data de emissão mais recente com responsável identificado." A seção 5.2 diz que o assistente "não escolhe nem sugere uma versão como provavelmente correta." O gap G-01 reconhece que a questão de qual versão é vigente ainda não foi resolvida.
Impacto: RF-02 implanta uma escolha de versão que a seção 5 proíbe e que G-01 admite não ter resposta. Se o PROC-042-v2 não for formalmente declarado vigente antes do go-live, o assistente estará aplicando uma regra de negócio não ratificada. QA não tem como testar aprovação do que ainda não foi decidido.
Pergunta de validação: A Diretoria Comercial já formalizou que a PROC-042-v2 substitui integralmente a v1 para todos os contratos? Se sim, esse ato de aprovação precisa ser registrado como pré-condição de go-live no próprio PRD.

ACHADO 2 — CRÍTICO
Item: "Nível de confiança" é conceito central sem definição operacional em nenhuma seção
Evidência: A seção 2.1 lista "nível de confiança (alto / médio / baixo)" como elemento obrigatório da resposta. Os RNF-02, RNF-03 e RNF-05 usam "confiança alta ou média" como critério de filtro para cobertura, alucinação e rastreabilidade. Em nenhum ponto do documento é definido o que determina cada nível.
Impacto: Os critérios de aceitação de RNF-02, RNF-03 e RNF-05 são inteiramente dependentes dessa definição. Sem ela, QA não consegue classificar uma resposta como "confiança alta" nem reprovar o assistente por estar abaixo de 80% de cobertura. O requisito mais importante de não-alucinação (RNF-03) fica sem base de medição.
Pergunta de validação: Quais critérios objetivos — número de chunks recuperados, score de similaridade mínimo, presença de documento formal vs. FAQ — determinam se uma resposta é alta, média ou baixa confiança?

ACHADO 3 — CRÍTICO
Item: RF-04 e RF-08 exigem que o assistente "identifique" conflito sem especificar como o mecanismo de detecção funciona
Evidência: RF-04 diz "o assistente deve identificar e sinalizar conflito quando dois documentos indexados contiverem regras incompatíveis." RF-08 exige que o assistente "indique se há outra versão indexada na base." O critério de aceitação de RF-04 pressupõe que "o pipeline recupera chunks de ambas as versões" — mas não especifica como o assistente sabe que dois chunks são sobre o mesmo tema e são contraditórios.
Impacto: A detecção de conflito pode ser implementada de formas radicalmente diferentes (metadado, lógica de prompt, comparação semântica), com resultados muito distintos de precisão. Se o mecanismo não for especificado, QA não pode reprovar uma implementação que detecta apenas 30% dos conflitos reais — porque não há critério mínimo de recall definido.
Pergunta de validação: O requisito de detecção de conflito deve ser resolvido por metadado de versão no índice (abordagem determinística) ou por inferência do LLM sobre o conteúdo dos chunks (abordagem probabilística)? Qual taxa mínima de detecção de conflito é aceitável?

ACHADO 4 — ALTO
Item: Comportamento do assistente para perguntas multi-domínio (prazo + carga perigosa + frete especial simultaneamente) não está especificado
Evidência: Todos os RF e BDD tratam de perguntas sobre um único tema. O mapa de cobertura do Anexo B (citado indiretamente pelos chunks de referência) menciona explicitamente o cenário "prazo de devolução + carga perigosa + frete especial" como pergunta multi-domínio, sem que o PRD defina o comportamento esperado nesse caso.
Impacto: Uma pergunta como "qual o prazo e o custo de devolver uma carga perigosa de 800kg enviada para o Norte?" exige comportamento simultâneo de RF-01 (devolução), RF-02 (frete especial), RF-06 (restrição de carga perigosa) e possivelmente RF-04 (conflito de versão). O documento não define se o assistente responde parcialmente, aciona fallback completo ou combina as respostas por seção.
Pergunta de validação: Quando uma pergunta aciona simultaneamente uma política restritiva (RF-06) e um requisito de cálculo (RF-02), qual comportamento prevalece — resposta parcial com o que é possível responder, ou fallback total por causa da parte não coberta?

ACHADO 5 — ALTO
Item: SLA de atualização da base (RNF-04) não define o que conta como "publicação no repositório oficial"
Evidência: RNF-04 estabelece "novo documento indexado e disponível para consulta em até 24 horas após publicação no repositório oficial (SharePoint/wiki interna)." Não há definição de: quem publica, em qual caminho/área do SharePoint, com quais metadados mínimos para que o pipeline reconheça o documento como válido para indexação.
Impacto: Um documento publicado na área errada do SharePoint, sem metadados de versão ou sem responsável formal, pode não ser reconhecido pelo pipeline — e o SLA de 24h seria formalmente cumprido (o documento está "publicado") enquanto o assistente continuaria sem a informação. O critério de validação do RNF-04 ("verificar disponibilidade no assistente às 09h do dia seguinte") não captura esse cenário.
Pergunta de validação: Existe um padrão formal de publicação (caminho, metadados obrigatórios, responsável aprovador) que define quando um documento está "publicado" para fins de disparo do SLA de indexação?

ACHADO 6 — ALTO
Item: Matriz de rastreabilidade (seção 7) referencia um documento externo ao PRD como fonte de RNF
Evidência: A linha "RNF-01 a 08" na matriz de rastreabilidade aponta como fonte "jornada-atendente-assistente-ia.md — Seções 3 (fallback) e 4 (feedback)." Esse documento não é parte formalmente integrante desta especificação — não está listado no escopo, não tem versão controlada mencionada e não é referenciado nos próprios requisitos não funcionais (seção 4).
Impacto: Se o documento "jornada-atendente-assistente-ia.md" for atualizado ou descontinuado, os RNF perdem rastreabilidade sem que o PRD sinalize a mudança. Além disso, QA não tem como verificar se os RNF são derivados fielmente desse documento sem acesso a ele.
Pergunta de validação: O documento "jornada-atendente-assistente-ia.md" deve ser formalmente incorporado como anexo desta especificação, ou os requisitos nele baseados devem ser transcritos diretamente no PRD?

ACHADO 7 — ALTO
Item: G-07 é um gap de go-live sem critério de decisão — e o PRD pressupõe a resposta ao redigir RF-02
Evidência: G-07 pergunta "Como o assistente deve tratar chamados abertos antes de 01/12/2023, ainda em processamento em 2026?" e classifica como pendência "antes do go-live." Simultaneamente, RF-02 já define o comportamento para chamados abertos "após 01/12/2023" — deixando implícito, mas sem especificar, o que acontece com chamados anteriores a essa data.
Impacto: Se G-07 não for resolvido antes do go-live, o assistente terá comportamento indefinido para uma classe de chamados identificada no próprio PRD. O atendente não saberá qual tabela aplicar para esses casos, e o QA não terá como testar esse fluxo.
Pergunta de validação: Em junho/2026, ainda existem chamados abertos antes de 01/12/2023 em processamento? Se sim, qual o volume estimado e qual versão de tabela deve ser aplicada?

ACHADO 8 — ALTO
Item: O critério de "confiança baixa" como gatilho de fallback (seção 6.1) não tem threshold definido
Evidência: A tabela 6.1 define como gatilho de fallback de ausência: "nenhum documento indexado cobre o tema ou a confiança de recuperação é baixa." O que constitui "baixo" não está definido em nenhuma seção — nem em termos de score de similaridade, nem de número de chunks recuperados, nem de ausência de documento formal.
Impacto: Sem threshold, a implementação decide arbitrariamente o que é "baixo." Uma configuração permissiva pode levar o assistente a responder com confiança em perguntas sem cobertura real; uma conservadora pode acionar fallback para perguntas bem cobertas. Os RNF-02 e RNF-03 ficam em conflito potencial: maximizar cobertura (RNF-02 ≥ 80%) empurra o threshold para baixo; minimizar alucinação (RNF-03 = zero) empurra para cima.
Pergunta de validação: Qual score mínimo de similaridade semântica, ou qual combinação de critérios, define que uma resposta tem confiança suficiente para ser apresentada sem fallback?

ACHADO 9 — MÉDIO
Item: BDD-06 pressupõe que o assistente "saberá" usar a PROC-042-v2, mas RF-04 exige que conflito seja sempre sinalizado — os dois cenários são contraditórios para a mesma pergunta
Evidência: BDD-06 espera que para a pergunta sobre prazo de frete especial, a resposta "deve indicar PROC-042 v2 como documento fonte" e informar "+3 dias úteis." Mas RF-04 e RNF-07 determinam que qualquer recuperação simultânea de chunks de v1 e v2 deve acionar alerta de conflito. A pergunta sobre prazo de frete especial é exatamente o tipo que recuperaria chunks de ambas as versões (PROC-042-C e PROC-042v2-C).
Impacto: QA não consegue passar simultaneamente em BDD-06 (resposta direta com v2 e menção à v1 como "referência") e RNF-07 (alerta de conflito obrigatório). Os dois requisitos exigem comportamentos opostos para a mesma entrada. Um dos dois precisa ser revisto ou o cenário de BDD-06 precisa descrever um estado pós-resolução do conflito (G-01 resolvido).
Pergunta de validação: BDD-06 pressupõe que G-01 já foi resolvido (v2 é formalmente vigente) ou descreve o comportamento enquanto o conflito ainda existe? Se o segundo, o BDD deveria ser idêntico ao BDD-02.

ACHADO 10 — MÉDIO
Item: RNF-01 mede "primeiro token exibido" — mas a seção 2.1 exige apresentação de até 5 elementos de rastreabilidade, e o tempo para completar a resposta não é especificado
Evidência: RNF-01 define SLA de 10s medido até o "primeiro token exibido." A seção 2.1 exige que toda resposta inclua: orientação em linguagem clara, trecho do documento fonte, nome e versão do documento, data de vigência e nível de confiança.
Impacto: Uma implementação pode cumprir o SLA de 10s exibindo o primeiro token e completando a resposta completa em 45s — e ser aprovada por RNF-01 enquanto a experiência real do atendente é inaceitável. O requisito de performance está desalinhado com o requisito de completude da resposta.
Pergunta de validação: O SLA de 10s deve ser medido até o primeiro token ou até a resposta completa (incluindo fonte, versão e trecho citado) estar disponível para o atendente?

ACHADO 11 — MÉDIO
Item: RF-09 define feedback como coletado "ao final de cada consulta" — mas o fluxo de discordância (seção 6.1) descreve acionamento durante a consulta, gerando inconsistência de UX
Evidência: RF-09 diz "o assistente deve coletar feedback do atendente ao final de cada consulta." A seção 6.1 descreve fallback de discordância acionado quando "o atendente reconhece que a resposta contraria a prática operacional" — o que implica acionamento imediato, não ao final. O BDD-07 pressupõe acionamento pós-recebimento da resposta, antes do fim da consulta.
Impacto: Se o mecanismo de feedback for implementado apenas ao "final da consulta" (ex: após o atendente fechar o chamado), o fluxo de discordância imediata descrito em 6.1 não terá ponto de captura. Consultas em que o atendente descarta a resposta e responde por conta própria nunca gerarão feedback.
Pergunta de validação: O feedback é coletado uma vez ao final da consulta, ou pode ser acionado a qualquer momento durante a resposta? Se ambos, os dois mecanismos precisam ser especificados separadamente.

ACHADO 12 — MÉDIO
Item: Seção 2.2 (não escopo) exclui o FAQ como fonte primária, mas seção 5.1 o posiciona como nível 3 de precedência — o que implica que ele é indexado e usado, contradizendo o não escopo
Evidência: Seção 2.2: "FAQ-Atendimento como fonte primária indexada — este documento pode ser referenciado como contexto de segundo nível, mas não como fonte normativa." Seção 5.1 define o FAQ como nível 3 da hierarquia de precedência, entre PROC e conteúdos sem data. Para estar em uma hierarquia de precedência, o FAQ precisa estar indexado.
Impacto: O documento não esclarece se o FAQ é ou não indexado. Se não for indexado, a seção 5.1 se refere a uma fonte que o assistente nunca vê, tornando o nível 3 da hierarquia inoperante. Se for indexado, o não escopo da seção 2.2 está incorreto.
Pergunta de validação: O FAQ-Atendimento será ou não indexado no pipeline de RAG? Se sim, com qual metadado de tipo que o assistente usará para aplicar a precedência de nível 3?

ACHADO 13 — BAIXO
Item: RNF-02 define conjunto de 50 perguntas-teste sem especificar quem as elabora, quando, com qual critério de representatividade e quem aprova
Evidência: RNF-02: "Aplicar conjunto de 50 perguntas-teste derivadas dos temas documentados. Classificar respostas por nível de confiança." Nenhuma seção define responsável pela elaboração desse conjunto, critério de seleção (cobertura uniforme por tema? ponderada pela frequência de 35%/25%/20%?), ou processo de aprovação.
Impacto: O principal instrumento de validação de qualidade do produto pode ser elaborado de forma não representativa. 50 perguntas focadas em um único tema cobririam mal os outros dois — e o assistente poderia ser aprovado sem ter sido testado adequadamente em frete especial ou SLA.
Pergunta de validação: Quem é responsável por elaborar e aprovar o conjunto de 50 perguntas-teste, e qual critério garante representatividade proporcional aos temas mais consultados (35% prazos, 25% frete, 20% devolução)?
