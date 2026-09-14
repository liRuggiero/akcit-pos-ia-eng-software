# Prompt 2 — Refinamento das restrições

Considere a análise e o diagrama produzidos no Prompt 1. Atualize a arquitetura com as restrições confirmadas abaixo, sem alterar desnecessariamente as responsabilidades já definidas.

## Restrições confirmadas

1. A ferramenta corporativa de gestão de projetos é o **JIRA, da Atlassian**.
2. A LLM é hospedada em **infraestrutura privada** e não deve ser representada como serviço público de terceiros.
3. Dados estruturados de demanda, estimativa, aprovação e auditoria são persistidos em **PostgreSQL**.
4. Documentos e mídias são armazenados em um serviço de objetos **compatível com S3**.
5. Áudios e vídeos passam por um **serviço de Speech-to-Text (STT)** antes da consolidação do contexto.
6. O processamento de arquivos e a geração da estimativa devem ocorrer de forma assíncrona por meio de um **Message Broker**.
7. Antes de enviar conteúdo à LLM, o sistema deve aplicar minimização e anonimização de dados.
8. A estimativa da IA deve incluir jornadas, premissas, riscos e **T-shirt sizing**.
9. A aprovação humana é obrigatória; somente estimativas aprovadas podem gerar item no JIRA.
10. A integração com o JIRA deve devolver a chave criada, por exemplo `DATA-1234`, e registrá-la no histórico da demanda.

## Solicitação

1. Apresente as alterações realizadas em relação ao primeiro modelo.
2. Atualize o diagrama C4 no nível de Containers em PlantUML.
3. Gere um diagrama de sequência em Mermaid cobrindo as quatro etapas:
   - abertura e inclusão;
   - processamento via LLM;
   - aprovação humana;
   - integração com o JIRA.
4. Evidencie interações assíncronas e o retorno de status ao usuário.
5. Não invente SLA, volumetria, autenticação, produto de broker, fornecedor S3 ou tecnologia interna da LLM.
6. Liste decisões ainda pendentes e riscos residuais.

