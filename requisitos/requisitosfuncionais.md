---
feature: sistema-gestao-eventos
version: 1.0
status: ready-for-development
updated: 2026-08-23
---

# Requisitos Funcionais

## Objetivo da entrega

Centralizar a gestão dos eventos da Eventus, substituindo o controle realizado por formulários on-line e planilhas, permitindo gerenciar eventos, inscrições, vagas, pagamentos, cancelamentos, lista de espera, presença e certificados.

## Requisitos

| ID | Descrição | Regra associada | Prioridade | Origem |
|---|---|---|---|---|
| RF-001 | O sistema deve permitir autenticação dos usuários dos perfis participante, organizador, financeiro e palestrante utilizando duplo fator de autenticação. | RB-001 | Alta | PO |
| RF-002 | O sistema deve permitir ao usuário escolher receber o código de segundo fator por e-mail ou SMS. | RB-001 | Alta | PO |
| RF-003 | O sistema deve permitir ao organizador criar e editar eventos. | RB-002 | Alta | PO |
| RF-004 | O sistema deve permitir configurar informações do evento, incluindo datas, local, capacidade, lotes, tipos de ingresso e período de inscrições. | RB-002 | Alta | PO |
| RF-005 | O sistema deve permitir ao organizador cancelar um evento. | RB-014 | Alta | PO |
| RF-006 | O sistema deve permitir a inscrição de participantes em eventos. | RB-003, RB-004 | Alta | PO |
| RF-007 | O sistema deve coletar na inscrição nome, e-mail, CPF, telefone, escolaridade, origem de conhecimento do evento, cidade e CEP. | RB-005 | Alta | PO |
| RF-008 | O sistema deve exigir o consentimento definido para armazenamento das informações pessoais e para fotos tiradas durante o evento antes da conclusão da inscrição. | RB-006 | Alta | PO |
| RF-009 | O sistema deve impedir mais de uma inscrição do mesmo CPF no mesmo evento. | RB-003 | Alta | PO |
| RF-010 | O sistema deve confirmar imediatamente a inscrição em eventos gratuitos quando houver vaga disponível. | RB-004 | Alta | PO |
| RF-011 | O sistema deve reservar uma vaga durante 48 horas para uma inscrição concluída em evento pago enquanto aguarda confirmação do pagamento. | RB-007 | Alta | PO |
| RF-012 | O sistema deve considerar a vaga reservada de uma inscrição aguardando pagamento na capacidade ocupada do evento. | RB-007 | Alta | PO |
| RF-013 | O sistema deve permitir ao participante escolher entre Mercado Pago e Pix Banco Inter para pagamento de eventos pagos. | RB-008 | Alta | PO |
| RF-014 | O sistema deve utilizar as opções de pagamento disponibilizadas pelo Mercado Pago quando esse gateway for selecionado. | RB-009 | Alta | PO |
| RF-015 | O sistema deve receber e registrar a confirmação do resultado do pagamento realizado pelos gateways integrados. | RB-007, RB-010 | Alta | PO |
| RF-016 | O sistema deve cancelar automaticamente uma inscrição não paga quando o prazo de reserva expirar. | RB-010 | Alta | PO |
| RF-017 | O sistema deve liberar uma vaga após a expiração de uma reserva não paga. | RB-010, RB-011 | Alta | PO |
| RF-018 | O sistema deve manter lista de espera quando a capacidade do evento estiver esgotada. | RB-011 | Alta | PO |
| RF-019 | O sistema deve notificar o próximo participante da lista de espera quando uma vaga for disponibilizada. | RB-011, RB-012 | Alta | PO |
| RF-020 | O sistema deve disponibilizar novamente a vaga ao público quando uma vaga for liberada e não houver participante em lista de espera. | RB-011 | Alta | PO |
| RF-021 | O sistema deve permitir ao participante solicitar o cancelamento da própria inscrição. | RB-013 | Alta | PO |
| RF-022 | O sistema deve calcular o valor de reembolso aplicável ao cancelamento de uma inscrição conforme as regras estabelecidas. | RB-013 | Alta | PO |
| RF-023 | O sistema deve permitir ao financeiro processar reembolsos. | RB-013, RB-014 | Alta | PO |
| RF-024 | O sistema deve processar reembolso pelo mesmo meio utilizado no pagamento original. | RB-013, RB-014 | Alta | PO |
| RF-025 | O sistema deve tratar confirmação de pagamento recebida após a expiração da reserva, reativando a inscrição quando ainda houver vaga. | RB-015 | Alta | PO |
| RF-026 | O sistema deve estornar pagamento confirmado após a expiração da reserva quando não houver mais vaga disponível. | RB-015 | Alta | PO |
| RF-027 | O sistema deve permitir ao palestrante associado a uma palestra registrar a presença dos participantes nessa palestra. | RB-016, RB-017 | Alta | PO |
| RF-028 | O sistema deve permitir ao organizador registrar ou corrigir presença dos participantes nas palestras. | RB-016 | Alta | PO |
| RF-029 | O sistema deve calcular a participação do inscrito considerando a quantidade de palestras em que teve presença registrada. | RB-016 | Alta | PO |
| RF-030 | O sistema deve gerar automaticamente certificado para participante que atingir o percentual mínimo de presença. | RB-016 | Alta | PO |
| RF-031 | O sistema deve enviar o certificado emitido ao participante por e-mail. | RB-016 | Alta | PO |
| RF-032 | O certificado deve conter nome e CPF do participante, nome e data do evento, assinatura do responsável pelo evento e nome da empresa realizadora. | RB-018 | Alta | PO |
| RF-033 | O sistema deve apresentar aos usuários autorizados a quantidade de vagas ocupadas do evento. | RB-019 | Alta | PO |
| RF-034 | O sistema deve apresentar a data e hora da última atualização da informação de vagas ocupadas. | RB-019 | Alta | PO |
| RF-035 | O sistema deve notificar o participante sobre os eventos de negócio definidos nas regras de notificação. | RB-020 | Alta | PO |
| RF-036 | O sistema deve permitir ao organizador e ao financeiro visualizar pagamentos dos participantes. | RB-021 | Alta | PO |
| RF-037 | O sistema deve permitir ao participante visualizar apenas os próprios pagamentos. | RB-021 | Alta | PO |
| RF-038 | O sistema deve permitir ao organizador e ao financeiro acompanhar reembolsos. | RB-021 | Alta | PO |
| RF-039 | O sistema deve permitir ao participante acompanhar apenas os próprios reembolsos. | RB-021 | Alta | PO |
| RF-040 | O sistema deve permitir ao organizador, financeiro e palestrante visualizar a lista completa de participantes. | RB-021 | Alta | PO |
| RF-041 | O sistema deve registrar auditoria das alterações sensíveis realizadas pelos usuários. | RB-022 | Alta | PO |
| RF-042 | O sistema deve permitir solicitação de exclusão ou anonimização de dados pessoais do participante, respeitadas as informações que devam ser preservadas pelo prazo aplicável. | RB-023 | Alta | PO |
| RF-043 | O sistema deve informar indisponibilidade de Mercado Pago ou Banco Inter quando não for possível concluir a operação e orientar o participante a tentar novamente posteriormente. | RB-024 | Alta | PO |
