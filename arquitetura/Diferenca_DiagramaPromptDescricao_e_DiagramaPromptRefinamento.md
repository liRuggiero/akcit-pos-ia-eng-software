# Diferença entre os diagramas de descrição e refinamento

## Visão geral

A principal diferença é que o **DiagramaPromptDescricao** representa a arquitetura inicial, ainda com tecnologias e integrações genéricas, enquanto o **DiagramaPromptRefinamento** incorpora as definições e restrições confirmadas no segundo prompt.

O segundo diagrama não propõe uma arquitetura completamente diferente. Ele apresenta uma **especialização mais concreta e governada do primeiro modelo**.

## Comparação

| Elemento | DiagramaPromptDescricao | DiagramaPromptRefinamento |
|---|---|---|
| Finalidade | Visão arquitetural inicial | Visão atualizada após o refinamento |
| Gestão de projetos | Ferramenta de Projetos genérica | **JIRA — Atlassian** |
| IA Generativa | Provedor de IA genérico | **LLM hospedada em infraestrutura privada** |
| Banco de dados | Banco Relacional genérico | **PostgreSQL** |
| Arquivos e mídias | Armazenamento de Objetos genérico | **Armazenamento compatível com S3** |
| Mensageria | Barramento de Eventos ou Filas | **Message Broker** |
| Transcrição | Serviço de Transcrição genérico | **Serviço STT** para áudio e vídeo |
| Orquestrador de IA | Organização do contexto, prompt e validação da resposta | Inclui **anonimização, minimização e saída estruturada** |
| Integração | Publicação da estimativa aprovada em uma ferramenta genérica | Criação **idempotente** do item no JIRA |
| Aprovação | Revisão humana representada de forma geral | **Aprovação humana obrigatória** antes da integração |
| Governança | Controles ainda tratados como lacunas ou hipóteses | Anonimização, versionamento, rastreabilidade e idempotência explicitados |

## DiagramaPromptDescricao

O **DiagramaPromptDescricao** apresenta a primeira interpretação arquitetural do sistema com base na descrição geral do negócio.

Sua finalidade é:

- identificar os principais containers da solução;
- delimitar as responsabilidades de cada componente;
- representar o fluxo entre interface, serviços, dados, IA e integrações;
- evidenciar tecnologias e decisões ainda não confirmadas;
- apoiar a identificação de lacunas arquiteturais.

Por isso, utiliza nomes genéricos como:

- Banco Relacional;
- Armazenamento de Objetos;
- Barramento de Eventos ou Filas;
- Provedor de IA;
- Ferramenta de Projetos.

Esses elementos são hipóteses de modelagem e não decisões tecnológicas definitivas.

## DiagramaPromptRefinamento

O **DiagramaPromptRefinamento** atualiza o primeiro modelo com as restrições informadas no segundo prompt.

As principais especializações são:

- substituição da ferramenta genérica de projetos pelo **JIRA**;
- definição do **PostgreSQL** para persistência estruturada;
- definição de armazenamento de arquivos compatível com **S3**;
- confirmação do **Message Broker** para processamento assíncrono;
- confirmação do serviço de **Speech-to-Text — STT**;
- utilização de uma **LLM em infraestrutura privada**;
- anonimização e minimização do conteúdo antes do processamento pela LLM;
- geração de uma resposta estruturada com jornadas, riscos, premissas e T-shirt sizing;
- aprovação humana obrigatória antes da criação do item no JIRA;
- criação idempotente do item, evitando duplicidades em reprocessamentos;
- registro da chave retornada pelo JIRA para garantir rastreabilidade.

## Síntese

- O primeiro diagrama responde: **“Quais capacidades e componentes o sistema precisa possuir?”**
- O segundo diagrama responde: **“Como essas capacidades ficam organizadas após a confirmação das restrições técnicas e de governança?”**

Assim, o **DiagramaPromptDescricao** é conceitual e exploratório, enquanto o **DiagramaPromptRefinamento** é mais específico, rastreável e aderente ao cenário definido para a solução.
