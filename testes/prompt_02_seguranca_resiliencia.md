# Prompt 02 — Segurança, erros e resiliência da integração

## Objetivo
Gerar casos negativos que investiguem falhas, controles de acesso, limites de autorização, violações de contrato e tratamento de erros na federação de agentes.

## Cenário considerado
O mesmo fluxo hipotético do Prompt 01 — **Agente Orquestrador → Agente de Dados → skill `consultar-api-rest` → API REST** — acrescido de um **Serviço de Autorização**, caso esse componente seja previsto na arquitetura real.

## Prompt para executar na LLM

> Atue como engenheiro(a) de QA com experiência em segurança, resiliência e testes de integração de sistemas multiagentes.
>
> Use este cenário **hipotético**: um Agente Orquestrador delega a um Agente de Dados uma consulta; o agente chama a skill `consultar-api-rest`; a skill consulta uma API REST; há validação de permissões por um Serviço de Autorização, ainda sujeito a confirmação no desenho de arquitetura.
>
> Gere **8 casos de teste negativos de integração**, um para cada tema:
> 1. Falha de autenticação;
> 2. Usuário sem permissão para os dados;
> 3. Timeout da API;
> 4. Indisponibilidade do serviço;
> 5. Resposta que viola o contrato da API;
> 6. Retorno de dado sensível não autorizado;
> 7. Falha na comunicação ou perda de contexto entre agentes;
> 8. Tentativa de ação fora das permissões concedidas à skill.
>
> Para cada caso, apresente em uma tabela: **ID**, **risco coberto**, **pré-condições**, **entrada/falha simulada**, **componentes integrados**, **passos de execução**, **comportamento esperado**, **critério objetivo de aprovação**, **evidência a coletar**.
>
> Regras: utilize apenas dados **sintéticos**; priorize **menor privilégio**, negação segura, mascaramento quando aplicável, trilhas de auditoria e proteção contra vazamento; não assuma políticas de retry, timeout, mascaramento ou códigos HTTP não informados — quando necessário, marque como **premissa a validar**. Proponha formas de simular falhas sem afetar serviços de produção. Não confunda testes de integração com testes unitários ou com avaliação genérica de qualidade da resposta da LLM.
>
> Ao final, aponte os riscos não cobertos e quais definições de arquitetura/política seriam necessárias para automatizar os testes.

## Como registrar o experimento
- **LLM/modelo utilizado:** preencher após a execução.
- **Data:** preencher após a execução.
- **Saída:** salvar em `casos-de-teste/casos_seguranca_resiliencia.md`.
- **Ajustes realizados no prompt:** registrar o que foi alterado e por quê.
- **Observações:** verificar se os comportamentos esperados são objetivamente mensuráveis e não dependem de políticas inventadas.
