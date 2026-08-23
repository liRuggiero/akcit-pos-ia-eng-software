---
feature: sistema-gestao-eventos
version: 1.0
status: ready-for-development
updated: 2026-08-23
---

# Requisitos Não Funcionais

| ID | Categoria | Requisito | Métrica / Critério | Prioridade |
|---|---|---|---|---|
| RNF-001 | Performance | O sistema deve responder às operações de login, inscrição, consulta de evento e emissão de certificado dentro do tempo máximo estabelecido. | Até 5 segundos. | Alta |
| RNF-002 | Disponibilidade | O sistema deve manter a disponibilidade mínima definida para operação. | Acima de 95%, medida diariamente. | Alta |
| RNF-003 | Atualização de dados | A informação de vagas ocupadas deve ser atualizada dentro do intervalo máximo definido. | Até 1 hora, exibindo data e hora da última atualização. | Alta |
| RNF-004 | Manutenção | Uma manutenção programada poderá indisponibilizar o sistema somente dentro do limite estabelecido. | Máximo de 15 minutos de indisponibilidade por janela de manutenção. | Alta |
| RNF-005 | Manutenção | Durante uma janela de manutenção, o sistema deve apresentar uma tela informando que está em manutenção. | Tela de manutenção durante a indisponibilidade; notificação prévia não é exigida. | Alta |
| RNF-006 | Backup | O sistema deve possuir rotina de backup. | Backup diário. | Alta |
| RNF-007 | Segurança | O acesso deve utilizar autenticação de dois fatores. | Segundo fator por e-mail ou SMS, escolhido pelo usuário. | Alta |
| RNF-008 | Segurança | O código do segundo fator deve expirar após o período definido. | 3 minutos. | Alta |
| RNF-009 | Segurança | O sistema deve bloquear o acesso após o limite de tentativas inválidas de segundo fator. | 3 tentativas inválidas; bloqueio por 24 horas. | Alta |
| RNF-010 | Auditoria | Registros de alterações sensíveis devem ser preservados. | Até 5 anos. | Alta |
| RNF-011 | Privacidade | Dados pessoais devem observar a política de retenção definida pela Eventus e as obrigações aplicáveis. | Até 5 anos, observada a validação jurídica aplicável. | Alta |
