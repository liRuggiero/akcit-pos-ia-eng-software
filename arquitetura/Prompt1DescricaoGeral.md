# Prompt 1 — Descrição geral do negócio

## Papel

Atue como Arquiteto de Software Sênior, com experiência em engenharia reversa, C4 Model, sistemas distribuídos, integração e soluções com IA Generativa.

## Contexto

Precisamos modelar o **Sistema de Ajuste no Datavis com IA Generativa**.

O sistema deve apoiar engenheiros e analistas na elaboração e no ajuste de estimativas de demandas de dados. O usuário registra a demanda, informa o contexto, inclui documentos, planilhas, imagens, áudios ou vídeos e solicita uma proposta de estimativa.

A solução deve:

- organizar os dados da demanda e seus anexos;
- extrair texto dos arquivos e transcrever mídias quando necessário;
- usar IA Generativa para interpretar o contexto e sugerir jornadas, atividades, premissas e estimativa em T-shirt sizing;
- calcular uma visão financeira a partir das jornadas estimadas e de parâmetros de custo;
- permitir que o engenheiro revise, altere, aprove ou rejeite a sugestão;
- enviar a demanda aprovada para uma ferramenta corporativa de gestão de projetos;
- manter histórico, status e rastreabilidade da estimativa.

O sistema apoia o planejamento, mas não executa pipelines de dados, não realiza deploy e não substitui a aprovação humana.

## Instruções

Antes de propor a arquitetura:

1. Separe claramente fatos informados, hipóteses, lacunas, recomendações e decisões arquiteturais.
2. Identifique de 6 a 10 lacunas que possam alterar arquitetura, segurança, custo, operação, prazo ou qualidade.
3. Elabore de 5 a 8 perguntas de esclarecimento e explique, em uma frase, por que cada resposta altera a arquitetura.
4. Reescreva o roteiro técnico delimitando responsabilidades e fora de escopo.
5. Gere um diagrama C4 no nível de Containers em PlantUML.
6. Não exponha endpoints, classes ou métodos; mantenha o nível de abstração de Containers.
7. Marque sistemas externos com o estereótipo `<<external>>`.
8. Quando uma tecnologia não estiver definida, use um nome genérico e registre a escolha como hipótese.
9. Inclua um checklist objetivo para revisão do diagrama em Pull Request.

## Formato esperado

1. Fatos informados
2. Lacunas arquiteturais
3. Perguntas de esclarecimento
4. Hipóteses adotadas para a primeira modelagem
5. Roteiro revisado
6. Código PlantUML
7. Checklist de revisão

