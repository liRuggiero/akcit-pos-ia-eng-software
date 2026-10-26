# Casos de teste — Segurança e resiliência

8 casos negativos referentes ao Prompt 02.

> **Status:** casos de teste **propostos pela LLM neste exercício**, não executados em software real. Resultados observados e evidências de execução estão pendentes. Arquitetura e APIs são fictícias.

## Referências

- [`../contratos/openapi-indicadores.yaml`](../contratos/openapi-indicadores.yaml)
- [`../contratos/requisicao-consulta.json`](../contratos/requisicao-consulta.json)
- [`../contratos/resposta-sucesso.json`](../contratos/resposta-sucesso.json)
- [`../contratos/resposta-erro.json`](../contratos/resposta-erro.json)
- [`../contratos/resultado-autorizacao.json`](../contratos/resultado-autorizacao.json)

## CT07 — Autenticação inválida

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Skill e API mock exigem Bearer JWT; observabilidade ativada.
- **Entrada / falha simulada:** GET sem token ou com token inválido.
- **Passos:** Acionar consulta simulada; coletar HTTP e invocação do agente.
- **Resultado esperado:** Resposta 401 e falha propagada sem exposição de credencial.
- **Critério objetivo de aprovação:** HTTP 401 e `codigo=NAO_AUTENTICADO`; zero dados de indicador retornados.
- **Evidência a coletar:** HTTP 401, payload de erro e logs sem token.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT08 — Usuário sem permissão

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Mock de autorização com política de leitura e interceptação da chamada REST.
- **Entrada / falha simulada:** Principal de teste sem `indicadores:leitura`; `permitido=false` (variação sintética).
- **Passos:** Solicitar `IND-001`; capturar decisão e chamadas subsequentes.
- **Resultado esperado:** Federação bloqueia consulta; se houver resposta HTTP, retorna 403 padronizado.
- **Critério objetivo de aprovação:** Decisão negativa; zero GETs ao recurso após negativa; sem dados do indicador.
- **Evidência a coletar:** Decisão de autorização, contagem zero de GET e erro 403 quando aplicável.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT09 — Timeout da API

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Mock permite atrasar respostas; limite de timeout do cliente definido antes do teste.
- **Entrada / falha simulada:** Delay acima do limite configurado.
- **Passos:** Configurar atraso; executar fluxo; medir tempo e capturar resultado.
- **Resultado esperado:** Skill interrompe espera e devolve erro controlado ao orquestrador.
- **Critério objetivo de aprovação:** Operação termina dentro de limite acordado e não retorna sucesso falso.
- **Evidência a coletar:** Tempos e traces; requisito de timeout **a definir**.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT10 — Indisponibilidade da API

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Mock REST responde 503; nenhuma infraestrutura real usada.
- **Entrada / falha simulada:** HTTP 503 e schema `Erro`, código `SERVICO_INDISPONIVEL`.
- **Passos:** Forçar 503; chamar via skill; capturar propagação ao orquestrador.
- **Resultado esperado:** Serviço indisponível comunicado sem converter resposta em indicador válido.
- **Critério objetivo de aprovação:** HTTP 503 reconhecido; payload sensível ausente; sem retorno de valor de indicador.
- **Evidência a coletar:** Resposta 503, payload e trace; política de retry **a definir**.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT11 — Violação do contrato de retorno

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Validador de schema OpenAPI na fronteira API/skill.
- **Entrada / falha simulada:** HTTP 200 com `valor="noventa"` ou `referencia` ausente.
- **Passos:** Configurar resposta inválida; executar consulta e validar payload.
- **Resultado esperado:** Resposta rejeitada como inválida; indicador não tratado como confiável.
- **Critério objetivo de aprovação:** Pelo menos um erro de schema detectado; nenhum resultado inválido entregue como sucesso.
- **Evidência a coletar:** Relatório do validador e registro de falha.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT12 — Dado sensível não autorizado na resposta

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Campo sensível fictício identificado na classificação de teste.
- **Entrada / falha simulada:** HTTP 200 com `cpf="000.000.000-00"` além dos campos permitidos.
- **Passos:** Simular resposta com campo adicional; verificar validação e retorno do agente.
- **Resultado esperado:** Campo extra rejeitado pelo schema (`additionalProperties: false`); conteúdo não repassado.
- **Critério objetivo de aprovação:** Nenhum `cpf` no retorno ou logs; resposta sinalizada como não conforme.
- **Evidência a coletar:** Payload mock, relatório schema e logs higienizados.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT13 — Falha na passagem de contexto entre agentes

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Trace distribuído ou stub registra IDs por etapa.
- **Entrada / falha simulada:** Mensagem delegada sem `correlation_id` ou com ID diferente.
- **Passos:** Injetar envelope adulterado; observar execução e registros.
- **Resultado esperado:** Inconsistência detectada; correlação não é silenciosamente substituída por ID arbitrário.
- **Critério objetivo de aprovação:** Falha rastreável; nenhuma resposta indevidamente vinculada à solicitação original.
- **Evidência a coletar:** Traces de origem/destino e evento de inconsistência; política **a definir**.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT14 — Ação fora do escopo da skill

- **Tipo:** Negativo / segurança e resiliência
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Skill de consulta configurada somente para leitura; política de autorização instrumentada.
- **Entrada / falha simulada:** Pedido sintético `acao=excluir_indicador` para `IND-001`.
- **Passos:** Enviar pedido inadequado; observar roteamento, autorização e chamadas HTTP.
- **Resultado esperado:** Operação não autorizada bloqueada antes de qualquer mutação.
- **Critério objetivo de aprovação:** Zero chamadas POST/PUT/PATCH/DELETE; decisão de bloqueio registrada.
- **Evidência a coletar:** Contagem por método HTTP e decisão de autorização.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## Premissas e ajustes pendentes

- As interfaces entre agentes e a decisão de autorização são ilustrativas, sem contrato formal próprio.
- O contrato OpenAPI disponível define apenas o endpoint GET, seus códigos HTTP e os schemas de resposta; não especifica política de retry, tempo limite nem formato de mensagens internas.
- Antes da execução real, confirmar autenticação, observabilidade, mascaramento, tratamento de erros e como a skill valida o contrato.
- Utilizar somente mocks, stubs e dados sintéticos. Não apontar testes para produção.

## Registro do experimento

- **Modelo / versão:** GPT-6 (geração inicial dos casos neste chat; confirmar na documentação acadêmica conforme ferramenta efetivamente usada).
- **Data:** 2026-10-26.
- **Prompt de referência:** [`../prompts/prompt_02_seguranca_resiliencia.md`](../prompts/prompt_02_seguranca_resiliencia.md).
- **Nota metodológica:** esta geração utiliza também os contratos já disponíveis como contexto, portanto não deve ser apresentada como execução literal e isolada do texto do prompt.
