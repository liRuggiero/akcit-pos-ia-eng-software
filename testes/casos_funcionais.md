# Casos de teste — Integração funcional

6 casos funcionais referentes ao Prompt 01.

> **Status:** casos de teste **propostos pela LLM neste exercício**, não executados em software real. Resultados observados e evidências de execução estão pendentes. Arquitetura e APIs são fictícias.

## Referências

- [`../contratos/openapi-indicadores.yaml`](../contratos/openapi-indicadores.yaml)
- [`../contratos/requisicao-consulta.json`](../contratos/requisicao-consulta.json)
- [`../contratos/resposta-sucesso.json`](../contratos/resposta-sucesso.json)
- [`../contratos/resposta-erro.json`](../contratos/resposta-erro.json)
- [`../contratos/resultado-autorizacao.json`](../contratos/resultado-autorizacao.json)

## CT01 — Roteamento ao agente de dados

- **Tipo:** Funcional / integração
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Orquestrador e agente de dados configurados; stub captura mensagens.
- **Entrada / falha simulada:** Envelope `requisicao-consulta.json` com `acao=consultar_indicador`.
- **Passos:** Submeter solicitação ao orquestrador; capturar delegação.
- **Resultado esperado:** Uma delegação ao `agente-dados`, com `skill=consultar-api-rest` e `acao=consultar_indicador`.
- **Critério objetivo de aprovação:** Destino, skill e ação correspondem ao envelope de referência; nenhuma delegação para agente incorreto.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT02 — Propagação dos parâmetros

- **Tipo:** Funcional / integração
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Roteamento ativo; API mock responde 200.
- **Entrada / falha simulada:** `parametros.id=IND-001` e token fictício com escopo de leitura.
- **Passos:** Enviar envelope; capturar chamada REST produzida pela skill.
- **Resultado esperado:** Skill produz `GET /v1/indicadores/IND-001` com Bearer token de teste e sem corpo JSON.
- **Critério objetivo de aprovação:** Método e path exatos; ID não modificado; nenhuma credencial registrada em logs.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT03 — Preservação do identificador de correlação

- **Tipo:** Funcional / integração
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Interceptadores nos dois agentes, skill e mock REST.
- **Entrada / falha simulada:** `correlation_id=123e4567-e89b-42d3-a456-426614174000`.
- **Passos:** Enviar solicitação; registrar mensagens e headers; comparar IDs.
- **Resultado esperado:** Mesmo ID no envelope, header `X-Correlation-ID`, header da resposta 200 e log de correlação dos agentes.
- **Critério objetivo de aprovação:** Todos os IDs observáveis idênticos e no formato UUID; ausência de divergência.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT04 — Consulta REST após autorização positiva

- **Tipo:** Funcional / integração
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Serviço de autorização mock disponível; credenciais de teste válidas.
- **Entrada / falha simulada:** `resultado-autorizacao.json` (`permitido=true`); ID `IND-001`.
- **Passos:** Executar fluxo; observar ordem autorização → GET.
- **Resultado esperado:** Autorização positiva precede a única consulta `GET`; resposta 200 recebida.
- **Critério objetivo de aprovação:** Uma decisão permitida antes da chamada; exatamente um GET; status 200.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT05 — Validação do schema de resposta

- **Tipo:** Funcional / integração
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Mock API com contrato OpenAPI do repositório; validador habilitado.
- **Entrada / falha simulada:** `resposta-sucesso.json`: `id`, `valor`, `unidade`, `referencia`.
- **Passos:** Efetuar GET; validar body e status contra `openapi-indicadores.yaml`.
- **Resultado esperado:** Status 200, payload com campos exigidos e tipos válidos; sem propriedades extras.
- **Critério objetivo de aprovação:** Zero erros de validação; `valor` numérico e `referencia` em formato de data.
- **Execução:** Não executado.
- **Resultado real:** Pendente.

## CT06 — Retorno ao orquestrador sem alteração dos dados

- **Tipo:** Funcional / integração
- **Componentes:** Orquestrador, Agente de Dados, skill `consultar-api-rest`, serviço de autorização (quando aplicável), mock da API REST.
- **Pré-condições:** Fluxo autorizado e stub do orquestrador instrumentado.
- **Entrada / falha simulada:** Payload `resposta-sucesso.json`.
- **Passos:** Solicitar indicador; capturar payload da API e retorno estruturado ao orquestrador.
- **Resultado esperado:** Valores e campos preservados no retorno, com referência à solicitação original.
- **Critério objetivo de aprovação:** Comparação estruturada de `id`, `valor`, `unidade`, `referencia` sem divergências.
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
- **Prompt de referência:** [`../prompts/prompt_01_cenarios_funcionais.md`](../prompts/prompt_01_cenarios_funcionais.md).
- **Nota metodológica:** esta geração utiliza também os contratos já disponíveis como contexto, portanto não deve ser apresentada como execução literal e isolada do texto do prompt.
