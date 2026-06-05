# Entrega Final - Exercicio 1.2

### Tarefa 1 - jornada do atendente em formato textual

**Script:**

- **Documento de entrada:** `entrega-final.md` e `analise-de-inconsistencia-vs-FAQ.md`. 

- **Prompt:**

    ```
    Você é Product Specialist no projeto NovaTech e precisa desenhar a jornada do atendente usando um assistente de IA com RAG, para apresentação interna e ao cliente.

    Seu objetivo é gerar uma jornada textual clara, estruturada e orientada a operação real do atendimento.

    - Contexto do cenário: A NovaTech é uma empresa de logística com grande volume de documentação espalhada em SharePoint, wiki interna e planilhas. O time de atendimento hoje consulta em média 4 fontes por chamado e gasta cerca de 12 minutos buscando respostas. O objetivo do assistente é reduzir esse tempo para menos de 2 minutos, sempre respondendo com base documental e indicação de fonte.

    - Dados do discovery: Os atendentes hoje abrem em média 4 fontes diferentes por chamado. As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%), política de devolução (20%) e outros (20%). Em 15% dos casos, o atendente não encontra resposta e escala para o supervisor.

    - Considere estes riscos e restrições disponíveis no documento `entrega-final.md`:
    - Considere o documento 'analise-de-inconsistencia-vs-FAQ.md` como insumo para o fluxo de fallback, feedbac e guardrails

    - Sua tarefa: Produza a jornada do atendente com os seguintes blocos obrigatórios:
        1. Visão geral da jornada: Explique em 5 a 8 linhas qual é o papel do assistente dentro do fluxo do atendente.

        2. Fluxo principal: Descreva passo a passo o caminho feliz, considerando:
            - atendente recebe dúvida do cliente;
            - atendente consulta o assistente;
            - assistente interpreta a pergunta;
            - assistente recupera conteúdo relevante;
            - assistente responde com fonte;
            - atendente usa a resposta no atendimento.
        Apresente em formato sequencial, com passos numerados.
        
        3. Fluxo de fallback: Descreva o que deve acontecer quando:
            - o assistente não encontra resposta confiável;
            - há conflito entre documentos;
            - a resposta encontrada é incompleta;
            - o atendente discorda da resposta apresentada;
            - o caso exige validação humana.
        Mostre claramente para onde o caso deve ser encaminhado, por exemplo: supervisor, Compliance, Comercial ou Operações, conforme o tipo de dúvida.

        4. Fluxo de feedback: Descreva como o atendente informa que a resposta estava:
            - errada;
            - desatualizada;
            - incompleta;
            - sem fonte suficiente;
            - em conflito com a prática operacional.
        Explique como esse feedback pode alimentar melhoria contínua da base de conhecimento e do assistente.

        5. Guardrails obrigatórios do assistente: Defina pelo menos 2 guardrails específicos ao contexto da NovaTech. Não quero guardrails genéricos. Eles devem ser aplicáveis ao domínio de logística, atendimento e RAG documental.

        Exemplos do tipo de especificidade esperada:
        - nunca inventar prazo de entrega sem base documental;
        - nunca escolher silenciosamente entre versões contraditórias de procedimento;
        - nunca usar FAQ informal como única fonte quando houver política formal conflitante.

        Requisitos de qualidade
        - Não invente capacidades técnicas não descritas.
        - Não use conhecimento externo.
        - Seja específico e operacional.
        - Priorize linguagem clara para negócio e delivery.
        - A jornada deve ser compreensível por pessoas não técnicas.
        - O conteúdo deve estar pronto para virar um diagrama visual depois.

        Formato obrigatório de saída: Entregue em Markdown, com esta estrutura:
        - Jornada do Atendente com Assistente de IA
            1. Visão geral da jornada
            2. Fluxo principal
            3. Fluxo de fallback
            4. Fluxo de feedback
            5. Guardrails do assistente

    ```

- **Output:** Arquivo `jornada-atendente-assistente-ia` - jornada do atendente em formato textual

- **Evidência do uso de IA: ** ![Print do Claude](<Evidência do uso da IA - Print do Claude.png>)

--- 

### Tarefa 2 - Diagrama - jornada do atendente

**Script:**

- **Documento de entrada:** `jornada-atendente-assistente-ia`. 

- **Prompt:**

    ```
    Você é Product Specialist no projeto NovaTech e precisa transformar a jornada textual do atendente com assistente de IA em um diagrama visual de fluxo para apresentação ao time interno e ao cliente.

    - Objetivo: Criar um diagrama visual que represente de forma clara e legível a operação do atendimento com assistente de IA, mostrando os 3 caminhos obrigatórios:
        - fluxo principal
        - fluxo de fallback
        - fluxo de feedback

    - Fonte de verdade: Use exclusivamente como base a jornada textual produzida na Tarefa 1. Não invente etapas, decisões, áreas envolvidas ou capacidades técnicas que não estejam descritas nessa jornada.

    - Instruções de construção do diagrama
        O diagrama deve:
            - representar o processo de ponta a ponta, desde o recebimento da dúvida do cliente até o encerramento ou escalonamento do caso;
            - destacar visualmente os 3 caminhos: principal, fallback e feedback;
            - mostrar os pontos de decisão do fluxo;
            - indicar quando o atendente segue com a resposta ao cliente e quando precisa escalar o caso;
            - deixar explícitos os possíveis destinos do fallback, como supervisor, Compliance, Comercial ou Operações, conforme a jornada textual;
            - mostrar como o feedback do atendente retorna para melhoria da base e do assistente.

    - Requisitos de qualidade
        - Não usar conhecimento externo.
        - Não inventar capacidades técnicas não descritas.
        - Ser específico, operacional e fácil de entender.
        - Priorizar linguagem clara para negócio e delivery.
        - Ser legível para público não técnico.
        - Manter coerência total com a jornada textual da Tarefa 1.

    - Estrutura esperada do diagrama: Organize o fluxo com:
        - início do atendimento;
        - consulta ao assistente;
        - decisão sobre confiança e suficiência da resposta;
        - caminho principal quando a resposta é confiável;
        - caminho de fallback quando há conflito, baixa confiança, ausência de resposta ou necessidade de validação humana;
        - caminho de feedback quando a resposta estiver errada, desatualizada, incompleta ou sem fonte suficiente;
        - encerramento do atendimento ou encaminhamento para tratativa posterior.

    - Formato de saída: Entregue:
        1. um diagrama visual de fluxo com os 3 caminhos claramente diferenciados;
        2. um título para o diagrama;
        3. uma legenda curta explicando o significado de cada caminho, se necessário.

    - Título sugerido: Jornada do Atendente com Assistente de IA - Fluxo Principal, Fallback e Feedback

    ```

- **Output:** Arquivo `Jornada do Atendente com Assistente de IA — Fluxo.pdf` - jornada do atendente em formato textual

- **Evidência do uso de IA: ** ![Print do Claude Design](<Evidência do uso da IA - Print do Claude Design.png>)