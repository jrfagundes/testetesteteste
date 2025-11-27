# testetesteteste


flowchart LR
    %% Direção geral
    %% LR = left to right

    %% --- Grupos (sistemas) ---
    subgraph EXT[Usuário Externo]
        extUser[Público externo\nabre ticket]
    end

    subgraph ZENDESK[Zendesk]
        zTicket[Ticket criado no Zendesk\nregistro, categorização e triagem]
    end

    subgraph INT[Usuários Internos / Monitoramento]
        intUser[Usuários internos\n& Ferramentas de monitoramento (NTDD/NTDR)]
        intIssue[Criação direta de issue\nno Jira ITSM]
    end

    subgraph JIRA[Jira ITSM - Sustentação]
        jIssue[Criação de issue vinculada\n(contém ID do ticket Zendesk)]
        work[N1/N2/N3 trabalha no Jira ITSM\n(análise, tratativas, atualizações)]
        resolve[Resolução / fechamento\nno Jira ITSM]
    end

    subgraph HUB[Integração Zendesk ↔ Jira]
        z2j[Disparo de integração\nZendesk → Jira ITSM\n(Webhook/API)]
        j2z[Sincronização Jira → Zendesk\nstatus, comentários, campos-chave]
    end

    subgraph RET[Retorno ao usuário]
        agentZendesk[Agente Zendesk\nacompanha SLA & responde usuário]
        userOk[Ticket encerrado e\ncomunicação final ao usuário]
    end

    %% --- Fluxo do público externo (Zendesk → Jira) ---
    extUser --> zTicket
    zTicket --> z2j
    z2j --> jIssue

    %% --- Fluxo interno / monitoramento (direto no Jira) ---
    intUser --> intIssue --> jIssue

    %% --- Trabalho da Sustentação no Jira ITSM ---
    jIssue --> work --> resolve

    %% --- Retorno do Jira para o Zendesk (sincronização) ---
    resolve --> j2z --> zTicket

    %% --- Agente Zendesk acompanha e fecha com usuário ---
    zTicket --> agentZendesk --> userOk
