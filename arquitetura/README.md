# Modelagem arquitetural do Sistema de Ajuste no Datavis

## Sobre este trabalho

Este repositório registra uma atividade prática de **Diagrams as Code** aplicada à engenharia reversa e à modelagem arquitetural do **Sistema de Ajuste no Datavis com IA Generativa**.

A atividade utiliza dois prompts encadeados. O primeiro transforma a descrição do negócio em uma arquitetura inicial e explicita as informações que ainda precisam ser confirmadas. O segundo incorpora novas restrições, especializa os componentes e acrescenta uma visão comportamental da solução.

O resultado demonstra como a IA Generativa pode apoiar o trabalho arquitetural sem substituir a análise crítica, a validação das hipóteses ou a decisão humana.

## Resultado em uma frase

> Uma descrição inicial do negócio evolui para uma arquitetura documentada e rastreável, representada por imagens que facilitam a comunicação e a análise da solução.

## Evolução da atividade

1. A descrição geral do sistema é submetida ao primeiro prompt.
2. A IA identifica lacunas, hipóteses e perguntas arquiteturais.
3. A primeira resposta consolida a visão inicial e o diagrama de Containers.
4. O segundo prompt acrescenta as restrições confirmadas.
5. A resposta refinada atualiza a arquitetura e detalha o comportamento do sistema.
6. As imagens finais registram visualmente a evolução entre os dois modelos.

### Primeira rodada — descoberta e estruturação

O `Prompt1DescricaoGeral.md` solicita que a IA assuma a perspectiva de um Arquiteto de Software Sênior. Antes de desenhar a solução, ela deve separar fatos, hipóteses e lacunas, além de formular perguntas que possam alterar custo, risco, segurança, operação ou qualidade.

A resposta dessa rodada apresenta:

- requisitos conhecidos e limites do sistema;
- lacunas arquiteturais relevantes;
- perguntas para refinamento do escopo;
- hipóteses necessárias para a modelagem inicial;
- roteiro técnico revisado;
- diagrama C4 no nível de Containers;
- checklist para revisão em Pull Request.

### Segunda rodada — especialização e comportamento

O `Prompt2Refinamento.md` incorpora as restrições confirmadas e atualiza o modelo sem modificar desnecessariamente a divisão de responsabilidades definida anteriormente.

O refinamento passa a representar explicitamente:

- JIRA como ferramenta de gestão de demandas;
- LLM hospedada em infraestrutura privada;
- PostgreSQL para persistência estruturada;
- armazenamento de objetos compatível com S3;
- serviço de Speech-to-Text para áudio e vídeo;
- Message Broker para processamento assíncrono;
- anonimização e minimização antes do uso da LLM;
- aprovação humana obrigatória;
- criação idempotente do item no JIRA;
- registro da chave retornada para fins de rastreabilidade.

## Como interpretar os dois modelos

| Aspecto | Modelo inicial | Modelo refinado |
|---|---|---|
| Objetivo | Descobrir capacidades e responsabilidades | Aplicar as restrições confirmadas |
| Tecnologias | Predominantemente genéricas | PostgreSQL, S3, JIRA e LLM privada |
| Integração final | Ferramenta de projetos não definida | JIRA — Atlassian |
| Proteção das informações | Identificada como ponto de análise | Minimização e anonimização explícitas |
| Processamento | Assíncrono por barramento ou filas | Assíncrono por Message Broker |
| Decisão humana | Revisão prevista | Aprovação obrigatória antes do JIRA |
| Visão comportamental | Não contemplada | Sequência completa em quatro etapas |

O segundo modelo não substitui a lógica do primeiro. Ele representa sua **evolução**, tornando concretas as decisões que antes apareciam como hipóteses ou lacunas.

## Visão funcional do sistema

O Sistema de Ajuste no Datavis apoia a elaboração e a revisão de estimativas para demandas de dados.

Em seu fluxo principal:

1. o engenheiro registra a demanda e envia os anexos;
2. documentos e mídias são armazenados e processados;
3. áudios e vídeos são transcritos quando necessário;
4. o contexto é consolidado, minimizado e anonimizado;
5. a LLM privada sugere jornadas, premissas, riscos e T-shirt sizing;
6. o sistema calcula e registra a visão de custos;
7. o engenheiro revisa e aprova ou rejeita a estimativa;
8. somente após aprovação, a demanda é criada no JIRA;
9. a chave retornada pelo JIRA é registrada no histórico.

O sistema apoia a estimativa e a tomada de decisão, mas não executa pipelines de dados, não realiza deploy e não aprova demandas autonomamente.

## Arquivos do projeto

Todos os arquivos são mantidos no mesmo nível do repositório, sem separação em pastas:

- `README.md`
- `Prompt1DescricaoGeral.md`
- `Resposta1DescricaoGeral.md`
- `DiagramaPromptDescricao.png`
- `Prompt2Refinamento.md`
- `Resposta2Refinamento.md`
- `DiagramaPromptRefinamento.png`

Os prompts preservam as instruções utilizadas nas duas rodadas, enquanto as respostas registram as análises, lacunas, hipóteses, decisões pendentes e riscos. Os diagramas são disponibilizados somente como imagens PNG, facilitando sua consulta sem exigir ferramentas adicionais.

## Entregáveis

| Arquivo | Conteúdo |
|---|---|
| `Prompt1DescricaoGeral.md` | Descrição inicial e instruções para discovery arquitetural |
| `Resposta1DescricaoGeral.md` | Lacunas, perguntas, roteiro, modelo inicial e checklist |
| `DiagramaPromptDescricao.png` | Imagem do modelo inicial |
| `Prompt2Refinamento.md` | Restrições confirmadas para a segunda rodada |
| `Resposta2Refinamento.md` | Arquitetura refinada, sequência, pendências e riscos |
| `DiagramaPromptRefinamento.png` | Imagem do modelo refinado |

## Convenções visuais

- **Azul:** containers internos da aplicação.
- **Roxo:** capacidades relacionadas à IA.
- **Cinza:** persistência, armazenamento e mensageria.
- **Contorno tracejado:** sistemas ou serviços externos.
- **Setas:** direção e finalidade das interações.

## Imagens dos diagramas

Os dois diagramas são entregues exclusivamente em formato PNG:

- `DiagramaPromptDescricao.png`: apresenta o modelo arquitetural inicial, com componentes e tecnologias ainda genéricos;
- `DiagramaPromptRefinamento.png`: apresenta o modelo atualizado, incluindo JIRA, PostgreSQL, armazenamento S3, Message Broker, serviço STT e LLM privada.

As imagens constituem os artefatos visuais finais da atividade e podem ser consultadas diretamente, sem instalação de PlantUML, Mermaid ou extensões adicionais.

## Aprendizados principais

A atividade evidencia que a produção do diagrama é apenas uma parte da modelagem. O valor arquitetural está também em:

- explicitar o que foi informado e o que ainda é hipótese;
- evitar que decisões tecnológicas sejam inventadas a partir de lacunas;
- delimitar responsabilidades e itens fora do escopo;
- representar controles humanos, segurança e rastreabilidade;
- manter as imagens e as decisões arquiteturais documentadas junto aos demais artefatos do projeto.

Assim, a combinação entre IA Generativa e Diagrams as Code torna o processo mais rápido e reproduzível, desde que as recomendações continuem sujeitas a validação técnica e decisão humana.

---

> **Nota sobre os nomes:** o descritivo original utilizava variações como `RespostaPromptDescricao.md`, `Resposta-Prompt-1.md` e `Resposta1DescricaoGeral.md`. Para preservar consistência, este repositório utiliza o padrão `Prompt1DescricaoGeral.md`, `Prompt2Refinamento.md`, `Resposta1DescricaoGeral.md` e `Resposta2Refinamento.md`.
