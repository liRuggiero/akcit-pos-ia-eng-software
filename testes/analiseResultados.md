# Análise dos resultados — Testes de integração em uma Federação de Agentes de IA

> **Status:** análise preliminar, pronta para complementar após a execução dos prompts. Os 14 cenários abaixo são uma **matriz de cobertura proposta**, não resultados observados de uma rodada documentada da LLM nem evidências de testes executados.

## 1. Objetivo do experimento

Comparar como dois tipos de prompt orientam uma LLM na elaboração de casos de teste de integração para uma arquitetura hipotética de **Federação de Agentes**, composta por **Agente Orquestrador → Agente de Dados → skill `consultar-api-rest` → API REST** e, nos cenários de controle de acesso, um **Serviço de Autorização**.

O estudo investiga **qualidade** (clareza, critérios verificáveis e aderência aos contratos) e **diversidade** (variedade de fluxos, riscos e componentes exercitados). O objetivo não é avaliar a qualidade textual das respostas dos agentes, e sim a integração entre componentes.

## 2. Materiais e delimitações

| Artefato | Papel no estudo |
|---|---|
| [`prompts/prompt_01_cenarios_funcionais.md`](prompts/prompt_01_cenarios_funcionais.md) | Solicita 6 cenários funcionais de integração. |
| [`prompts/prompt_02_seguranca_resiliencia.md`](prompts/prompt_02_seguranca_resiliencia.md) | Solicita 8 cenários negativos, de segurança e resiliência. |
| [`contratos/openapi-indicadores.yaml`](contratos/openapi-indicadores.yaml) | Exemplo de especificação OpenAPI do recurso `GET /indicadores/{id}`. |
| [`contratos/requisicao-consulta.json`](contratos/requisicao-consulta.json) | Envelope ilustrativo da delegação entre agentes, **não** body de HTTP GET. |
| [`contratos/resposta-sucesso.json`](contratos/resposta-sucesso.json) | Exemplo de payload de sucesso. |
| [`contratos/resposta-erro.json`](contratos/resposta-erro.json) | Exemplo de erro por acesso negado. |
| [`contratos/resultado-autorizacao.json`](contratos/resultado-autorizacao.json) | Exemplo de decisão de autorização. |

Todos os dados são sintéticos. Os contratos descrevem uma **arquitetura de referência hipotética**, não uma integração corporativa em produção. A validação da identidade no ponto de execução é necessária; um campo de identidade no envelope não substitui autenticação confiável.

## 3. Estratégia de prompting e efeito esperado

### Prompt 01 — Cenários funcionais

**Foco:** roteamento, passagem de parâmetros, propagação de `correlation_id`, invocação da skill, aderência ao contrato de resposta e devolução do resultado ao orquestrador.

**Contribuição esperada para a qualidade:** testes mais específicos sobre a sequência de chamadas e os dados transportados; melhor delimitação das interfaces e dos resultados esperados quando os contratos são fornecidos como contexto.

**Contribuição esperada para a diversidade:** cobertura de diferentes etapas do *happy path*. Por si só, tende a explorar menos falhas, abuso de permissões e degradação de serviços.

### Prompt 02 — Segurança e resiliência

**Foco:** autenticação, autorização, timeout, indisponibilidade, violação de contrato, resposta com dados sensíveis, perda de contexto e extrapolação das permissões da skill.

**Contribuição esperada para a qualidade:** explicitação de critérios de bloqueio, observabilidade, isolamento e tratamento de falhas, especialmente quando o prompt exige evidências verificáveis.

**Contribuição esperada para a diversidade:** ampliação da cobertura para exceções e ameaças não contempladas pelo prompt funcional. Sem contratos e políticas definidos, há risco de a LLM presumir códigos HTTP, retries, timeouts e mascaramento inexistentes.

**Comparação preliminar:** os prompts são **complementares**, não concorrentes. O primeiro avalia se a integração funciona no fluxo previsto; o segundo, se falha de forma controlada e segura.

## 4. Matriz de cobertura proposta — 14 casos

A tabela a seguir representa os **temas exigidos pelos prompts**, que deverão ser confrontados com os casos efetivamente produzidos pela LLM.

| ID proposto | Origem | Cenário | Verificação objetiva a exigir da LLM |
|---|---|---|---|
| F-01 | Prompt 01 | Roteamento de solicitação | A solicitação chega ao Agente de Dados correto. |
| F-02 | Prompt 01 | Parâmetros de entrada | O identificador `IND-001` é preservado até a chamada da API. |
| F-03 | Prompt 01 | Propagação de contexto | O `correlation_id` é mantido nas mensagens e no header `X-Correlation-ID`. |
| F-04 | Prompt 01 | Invocação da skill | A skill realiza `GET /indicadores/IND-001` conforme contrato. |
| F-05 | Prompt 01 | Validação do retorno REST | O payload recebido atende ao schema de sucesso do OpenAPI. |
| F-06 | Prompt 01 | Resposta ao orquestrador | O dado retornado corresponde ao indicador consultado, sem perda de contexto. |
| N-01 | Prompt 02 | Falha de autenticação | A tentativa sem credencial válida não obtém dados protegidos; verificar status previsto no contrato. |
| N-02 | Prompt 02 | Ausência de autorização | Uma decisão de acesso negado impede a consulta e o retorno de dados. |
| N-03 | Prompt 02 | Timeout | A falha é identificada sem espera indefinida; prazo e eventual retry são **premissas a definir**. |
| N-04 | Prompt 02 | Serviço indisponível | Erro `503`, quando aplicável, é tratado sem inventar resultado de negócio. |
| N-05 | Prompt 02 | Resposta incompatível com contrato | Campo obrigatório ausente provoca rejeição/erro de validação. |
| N-06 | Prompt 02 | Campo sensível inesperado | Um campo como `cpf` não é exposto no resultado, conforme controle de schema e política definida. |
| N-07 | Prompt 02 | Falha/perda de contexto entre agentes | Ausência ou divergência do `correlation_id` é sinalizada segundo regra de integração definida. |
| N-08 | Prompt 02 | Skill excede permissões | Tentativa fora do escopo de leitura é negada e deixa evidência auditável. |

**Nota:** `F-*` e `N-*` são IDs sugeridos para esta análise. Caso a LLM gere outros identificadores, preservar os IDs reais nos arquivos de resultado e mapear a equivalência aqui.

## 5. Ajustes de prompt recomendados

Os itens abaixo são **refinamentos planejados e justificativas**, não um histórico de iterações já realizadas.

| Problema a evitar | Ajuste sugerido | Benefício esperado |
|---|---|---|
| Casos genéricos, sem referência à integração | Informar os componentes e os contratos de exemplo | Maior aderência ao cenário concreto. |
| Resultados esperados vagos (“funciona corretamente”) | Exigir critério de aprovação observável para cada caso | Melhor testabilidade e potencial de automação. |
| Confusão entre mensagem entre agentes e requisição HTTP | Explicitar que `requisicao-consulta.json` é envelope de delegação, e que a API usa `GET` | Evita casos tecnicamente incorretos. |
| Suposições indevidas sobre autenticação e respostas | Solicitar distinção entre especificação fornecida e **premissa a validar** | Reduz requisitos inventados. |
| Foco apenas em fluxo de sucesso | Usar um segundo prompt voltado a ameaças e falhas | Melhora diversidade e cobertura de riscos. |
| Testes impossíveis de reproduzir | Pedir entradas sintéticas, passos, *mock* de dependências e evidências | Facilita reprodução local sem impactar produção. |
| Redundância entre cenários | Exigir um cenário principal distinto para cada ID e justificar diferenças | Melhora a utilidade do conjunto de testes. |

### Complemento recomendado para uma segunda rodada

Adicionar ao final de **ambos** os prompts:

> Use explicitamente os contratos da pasta `contratos/` como referência. Para cada caso, cite o campo, header, schema ou status HTTP verificado, quando aplicável. Não presuma a implementação de um endpoint de autorização, política de retry ou mascaramento sem evidência. Assinale lacunas como **premissa a validar**. Evite cenários duplicados e descreva como reproduzir o teste com mocks e dados sintéticos.

Registrar essa versão como segunda rodada somente **depois de executá-la** e salvar as respectivas saídas.

## 6. Critérios de avaliação das respostas da LLM

Atribuir uma nota de **0 a 2** a cada critério em cada conjunto de casos: **0 = ausente ou incorreto; 1 = parcial; 2 = adequado**.

| Critério | O que observar |
|---|---|
| Aderência ao escopo | Exercita integração entre componentes, não apenas uma função isolada. |
| Precisão contratual | Respeita endpoint, headers, campos, status e schemas documentados. |
| Verificabilidade | Expõe pré-condições, passos, resultado esperado e aprovação objetiva. |
| Variedade | Cobre tipos distintos de fluxo, risco e falha, sem duplicações artificiais. |
| Segurança e rastreabilidade | Considera menor privilégio, negação de acesso e `correlation_id`. |
| Gestão de premissas | Não transforma políticas ou comportamentos hipotéticos em fatos implementados. |

**Pontuação máxima por conjunto: 12 pontos.** A nota é uma rubrica sugerida pelo projeto, não um padrão externo de QA.

| Medida | Prompt 01 | Prompt 02 |
|---|---|---|
| Casos solicitados | 6 | 8 |
| Casos efetivamente retornados | _A preencher_ | _A preencher_ |
| Casos únicos e aproveitáveis | _A preencher_ | _A preencher_ |
| Nota de qualidade (0–12) | _A preencher_ | _A preencher_ |
| Problemas recorrentes | _A preencher_ | _A preencher_ |
| Refinamentos efetivamente testados | _A preencher_ | _A preencher_ |

## 7. Registro da execução do experimento

Preencher os campos abaixo em cada rodada real e salvar a saída integral da LLM, sem edições silenciosas.

| Campo | Rodada 1 — funcional | Rodada 1 — segurança/resiliência |
|---|---|---|
| Data e hora | _A preencher_ | _A preencher_ |
| LLM/modelo (e versão, se disponível) | _A preencher_ | _A preencher_ |
| Prompt/commit utilizado | _A preencher_ | _A preencher_ |
| Contratos anexados ou transcritos no contexto | _A preencher_ | _A preencher_ |
| Arquivo com saída integral | `casos-de-teste/casos_funcionais.md` | `casos-de-teste/casos_seguranca_resiliencia.md` |
| Ajustes realizados após revisão | _A preencher_ | _A preencher_ |

Para demonstrar melhoria entre versões, repetir a geração com os mesmos contratos e configuração do modelo, sempre que possível, alterando somente a redação do prompt. Comparar cobertura, duplicações, premissas indevidas e nota da rubrica.

## 8. Limitações e próximos passos

1. **Ainda não há evidências de execução:** esta análise organiza hipóteses, matriz de cobertura, critérios e método de registro; não afirma que uma LLM produziu efetivamente os 14 casos.
2. **Ainda não há execução automatizada dos casos:** gerar um caso de teste não equivale a executá-lo ou comprovar que o sistema passou no teste.
3. **Contratos são ilustrativos:** alguns controles (por exemplo, autorização entre agentes, timeout e tratamento de dados sensíveis) precisam de decisões explícitas no protótipo.
4. **Próximo passo:** executar os prompts, salvar as duas saídas, pontuar os resultados, documentar refinamentos efetivamente aplicados e atualizar a conclusão com evidências observadas.

## 9. Conclusão preliminar para o fórum

O experimento foi estruturado para comparar dois estilos de prompt aplicados a testes de integração de uma Federação de Agentes. **Como hipótese**, o prompt funcional tende a produzir maior detalhamento dos fluxos previstos e contratos, enquanto o prompt de segurança e resiliência tende a expandir a diversidade de situações de erro, controle de acesso e falha. O uso combinado permite avaliar o funcionamento esperado e o comportamento seguro da integração. A confirmação dessas contribuições depende da execução documentada dos prompts e da revisão dos casos efetivamente produzidos.

**Repositório GitHub:** _inserir URL após a publicação_.
