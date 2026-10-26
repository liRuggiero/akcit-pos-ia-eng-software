# Prompt 01 — Cenários funcionais de integração

## Objetivo
Gerar casos de teste que validem a comunicação entre o agente orquestrador, o agente de dados, uma skill reutilizável e uma API REST, incluindo fluxo de sucesso, contrato e preservação de contexto.

## Cenário considerado
Arquitetura de referência **hipotética** para a atividade acadêmica:

1. Um usuário solicita um indicador de negócio ao **Agente Orquestrador**.
2. O orquestrador encaminha a solicitação ao **Agente de Dados**.
3. O agente utiliza a skill `consultar-api-rest` para consultar uma **API REST**.
4. A resposta estruturada retorna ao agente de dados e, em seguida, ao orquestrador.

Os contratos e endpoints reais ainda deverão ser definidos. Não assumir funcionalidades não descritas.

## Prompt para executar na LLM

> Atue como engenheiro(a) de QA especializado(a) em testes de integração de sistemas multiagentes.
>
> Considere o cenário hipotético: um Agente Orquestrador recebe um pedido de indicador; delega a um Agente de Dados; este utiliza uma skill reutilizável `consultar-api-rest` para fazer uma chamada a uma API REST; o resultado retorna ao orquestrador.
>
> Gere **6 casos de teste de integração funcional** que cubram: (a) roteamento ao agente correto; (b) parâmetros de entrada; (c) preservação do contexto/correlation ID; (d) chamada da skill à API; (e) validação do contrato de resposta; (f) retorno ao orquestrador.
>
> Para cada caso, apresente em uma tabela: **ID**, **objetivo**, **pré-condições**, **entrada/dados de teste**, **componentes integrados**, **passos de execução**, **resultado esperado**, **critério objetivo de aprovação**.
>
> Regras: diferencie explicitamente **premissas** de requisitos informados; proponha dados **fictícios**; não invente endpoints, campos obrigatórios nem autenticação como se já estivessem implementados; destaque dependências que precisam ser confirmadas com o time. Evite testes puramente unitários ou exclusivamente de avaliação textual da LLM.
>
> Ao final, liste oportunidades de automatização e lacunas de cobertura.

## Como registrar o experimento
- **LLM/modelo utilizado:** preencher após a execução.
- **Data:** preencher após a execução.
- **Saída:** salvar em `casos-de-teste/casos_funcionais.md`.
- **Ajustes realizados no prompt:** registrar o que foi alterado e por quê.
- **Observações:** registrar casos redundantes, critérios pouco verificáveis e premissas indevidas.
