# Resposta 2 — Refinamento

## 1. Alterações em relação ao primeiro modelo

| Elemento | Modelo inicial | Modelo refinado |
|---|---|---|
| Gestão de projetos | Ferramenta genérica | JIRA (Atlassian) |
| IA Generativa | Provedor não definido | LLM em infraestrutura privada |
| Dados estruturados | Banco relacional genérico | PostgreSQL |
| Anexos | Armazenamento de objetos genérico | Armazenamento compatível com S3 |
| Processamento | Barramento/filas genérico | Message Broker, produto ainda não definido |
| Mídias | Transcrição como hipótese | Serviço STT confirmado |
| Proteção de dados | Lacuna | Minimização e anonimização antes da LLM |
| Decisão final | Revisão humana | Aprovação humana obrigatória |
| Integração | Envio genérico | Criação no JIRA e registro da chave retornada |

## 2. Arquitetura refinada

### Responsabilidades

- **Aplicação Web:** cria demandas, envia anexos, acompanha status e apresenta a proposta para revisão.
- **API Gateway:** controla a entrada e encaminha as solicitações aos serviços internos.
- **Serviço de Gestão de Estimativas:** mantém demanda, versões, estados, jornadas, custos, aprovação e histórico.
- **Serviço de Ingestão e Processamento:** valida anexos, armazena objetos, extrai texto e coordena transcrição.
- **Orquestrador de IA:** consolida o contexto, minimiza e anonimiza dados, monta o prompt, chama a LLM e valida a resposta estruturada.
- **Serviço de Integração JIRA:** converte a estimativa aprovada no contrato esperado pelo JIRA, cria o item e registra a chave retornada.
- **Message Broker:** desacopla ingestão, processamento, geração e integração.
- **PostgreSQL:** persiste dados estruturados, versões, estados e auditoria.
- **Armazenamento S3:** persiste documentos e mídias.
- **Serviço STT:** transcreve áudio e vídeo.
- **LLM Privada:** produz a proposta de estimativa a partir de contexto minimizado.
- **JIRA:** recebe exclusivamente a estimativa aprovada.

## 3. Diagrama C4 — Containers atualizado

Fonte: [`../diagramas/Diagrama2Containers.puml`](../diagramas/Diagrama2Containers.puml).

```plantuml
@startuml
left to right direction
actor "Engenheiro / Analista" as User
rectangle "Sistema de Ajuste no Datavis" {
  rectangle "Aplicação Web\n[Container]" as Web
  rectangle "API Gateway\n[Container]" as API
  rectangle "Gestão de Estimativas\n[Container]" as Est
  rectangle "Ingestão e Processamento\n[Container]" as Ing
  rectangle "Orquestrador de IA\n[Container]" as AI
  rectangle "Integração JIRA\n[Container]" as Adapter
  queue "Message Broker\n[Container]" as MB
  database "PostgreSQL\n[Container]" as DB
  database "Armazenamento S3\n[Container]" as S3
}
rectangle "Identidade Corporativa\n<<external>>" as IdP
rectangle "Serviço STT\n<<external>>" as STT
rectangle "LLM — Infraestrutura Privada\n<<external>>" as LLM
rectangle "JIRA — Atlassian\n<<external>>" as JIRA
User --> Web : Usa
Web --> IdP : Autentica
Web --> API : Comandos e consultas
API --> Est : Demanda e aprovação
API --> Ing : Anexos
Est --> DB : Workflow e versões
Ing --> S3 : Armazena arquivos
Ing --> STT : Solicita transcrição
Ing --> MB : ContextoPreparado
MB --> AI : Entrega evento
AI --> DB : Busca contexto
AI --> LLM : Contexto minimizado
LLM --> AI : Proposta estruturada
AI --> Est : Registra proposta
Est --> MB : EstimativaAprovada
MB --> Adapter : Entrega evento
Adapter --> JIRA : Cria item
Adapter --> Est : Registra chave
@enduml
```

O arquivo `.puml` indicado contém a mesma arquitetura com estilos, descrições e legenda completos.

## 4. Diagrama de sequência

Fonte: [`../diagramas/DiagramaSequencia.mmd`](../diagramas/DiagramaSequencia.mmd).

```mermaid
sequenceDiagram
    autonumber
    actor Eng as Engenheiro
    participant Web as Aplicação Web
    participant Est as Gestão de Estimativas
    participant Ing as Ingestão
    participant S3 as Armazenamento S3
    participant STT as Serviço STT
    participant MB as Message Broker
    participant IA as Orquestrador de IA
    participant DB as PostgreSQL
    participant LLM as LLM Privada
    participant Jira as Integração JIRA
    participant JIRA as JIRA

    rect rgb(235, 243, 255)
        Note over Eng,MB: Etapa 1 — Abertura e inclusão
        Eng->>Web: Cria demanda e envia anexos
        Web->>Est: Registra demanda
        Est->>DB: Persiste demanda e status RECEBIDA
        Web->>Ing: Envia anexos
        Ing->>S3: Armazena arquivos
        opt Áudio ou vídeo
            Ing->>STT: Solicita transcrição
            STT-->>Ing: Texto transcrito
        end
        Ing-)MB: Publica ContextoPreparado
        Web-->>Eng: Exibe processamento em andamento
    end

    rect rgb(244, 238, 255)
        Note over MB,LLM: Etapa 2 — Processamento via LLM
        MB-)IA: Entrega ContextoPreparado
        IA->>DB: Busca demanda e parâmetros vigentes
        IA->>IA: Minimiza e anonimiza o contexto
        IA->>LLM: Solicita proposta estruturada
        LLM-->>IA: Jornadas, premissas, riscos e sizing
        IA->>IA: Valida formato e calcula custos
        IA->>Est: Registra proposta gerada
        Est->>DB: Persiste versão e status EM_REVISAO
    end

    rect rgb(235, 243, 255)
        Note over Eng,DB: Etapa 3 — Aprovação humana
        Eng->>Web: Consulta e ajusta a estimativa
        Web->>Est: Salva alterações
        Est->>DB: Persiste nova versão
        Eng->>Web: Aprova estimativa
        Web->>Est: Solicita aprovação
        Est->>DB: Registra aprovador e status APROVADA
        Est-)MB: Publica EstimativaAprovada
    end

    rect rgb(244, 238, 255)
        Note over MB,JIRA: Etapa 4 — Integração JIRA
        MB-)Jira: Entrega EstimativaAprovada
        Jira->>JIRA: Cria item com a estimativa aprovada
        JIRA-->>Jira: Retorna chave DATA-1234
        Jira->>Est: Registra resultado da integração
        Est->>DB: Salva chave e status CONCLUIDA
        Web->>Est: Consulta status
        Est-->>Web: CONCLUIDA e DATA-1234
        Web-->>Eng: Exibe confirmação
    end
```

## 5. Estados mínimos da demanda

`RASCUNHO → RECEBIDA → EM_PROCESSAMENTO → EM_REVISAO → APROVADA → EM_INTEGRACAO → CONCLUIDA`

Estados de exceção recomendados: `REJEITADA`, `FALHA_PROCESSAMENTO` e `FALHA_INTEGRACAO`. A política de retentativa continua pendente.

## 6. Decisões ainda pendentes

- provedor de identidade, RBAC e segregação de funções;
- produto e topologia do Message Broker;
- produto compatível com S3 e políticas de retenção;
- tecnologia do serviço STT e tratamento de idiomas;
- contrato de saída estruturada da LLM e estratégia de versionamento de prompts;
- regra oficial para jornadas, custos e validade dos parâmetros;
- limites de arquivo, volumetria, SLA e disponibilidade;
- estratégia de retentativa, idempotência e fila de mensagens não processadas;
- requisitos de backup, recuperação, monitoramento e suporte.

## 7. Riscos residuais e controles sugeridos

| Risco | Controle sugerido |
|---|---|
| Resposta incorreta ou inconsistente da LLM | Saída estruturada, validação determinística e aprovação humana. |
| Exposição de informação confidencial | Classificação, minimização, anonimização, criptografia e controle de acesso. |
| Duplicidade de itens no JIRA | Chave de idempotência por versão aprovada. |
| Falha silenciosa em fluxo assíncrono | Correlação, métricas, logs, alertas e fila de mensagens não processadas. |
| Estimativa irreproduzível | Versionar prompt, modelo, parâmetros de custo, entradas e saída. |
| Arquivo malicioso ou formato inválido | Validação de tipo/tamanho e verificação antimalware antes do processamento. |
