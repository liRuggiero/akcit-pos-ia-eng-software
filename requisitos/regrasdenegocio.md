---
feature: sistema-gestao-eventos
version: 1.0
status: ready-for-development
updated: 2026-08-23
---

# Regras de Negócio

| ID | Regra | Descrição | Origem | Requisitos relacionados |
|---|---|---|---|---|
| RB-001 | Autenticação em dois fatores | Todos os perfis devem utilizar autenticação em dois fatores. O usuário escolhe receber o código por e-mail ou SMS. O código é válido por 3 minutos. Após 3 tentativas inválidas, o acesso fica bloqueado por 24 horas. | PO | RF-001, RF-002 |
| RB-002 | Administração do evento | Somente o organizador pode criar e editar eventos. | PO | RF-003, RF-004 |
| RB-003 | Unicidade da inscrição | Um CPF pode possuir no máximo uma inscrição no mesmo evento. | PO | RF-006, RF-009 |
| RB-004 | Evento gratuito | A inscrição em evento gratuito é confirmada imediatamente quando houver vaga e não utiliza reserva de 48 horas para pagamento. | PO | RF-006, RF-010 |
| RB-005 | Dados obrigatórios | A inscrição deve conter nome, e-mail, CPF, telefone, escolaridade, como o participante ficou sabendo do evento, cidade e CEP. | PO | RF-007 |
| RB-006 | Consentimentos | O consentimento para armazenamento das informações pessoais e para fotos tiradas durante o evento é obrigatório para conclusão da inscrição. Conforme decisão de negócio informada, o consentimento de fotos não poderá ser revogado posteriormente, sujeito à validação Jurídico/DPO antes da produção. | PO | RF-008, RF-042 |
| RB-007 | Reserva de vaga | Em evento pago, uma inscrição concluída reserva e consome uma vaga por 48 horas enquanto aguarda confirmação de pagamento. | PO | RF-011, RF-012, RF-015 |
| RB-008 | Escolha do pagamento | O participante de evento pago pode escolher entre Mercado Pago e Pix Banco Inter. | PO | RF-013 |
| RB-009 | Taxa Mercado Pago | Quando Mercado Pago for escolhido, as opções e taxas aplicáveis são apresentadas pelo próprio gateway. O custo da taxa apresentado pelo Mercado Pago é incluído no valor desembolsado pelo participante. | PO | RF-014 |
| RB-010 | Expiração da reserva | Se o pagamento não for confirmado dentro das 48 horas, a inscrição é cancelada automaticamente e a vaga é liberada. | PO | RF-015, RF-016, RF-017 |
| RB-011 | Destino da vaga liberada | Quando uma vaga for liberada, o próximo participante da lista de espera deve ser priorizado. Não havendo lista de espera, a vaga volta a ser disponibilizada ao público. | PO | RF-017, RF-018, RF-019, RF-020 |
| RB-012 | Prazo da lista de espera | O participante chamado da lista de espera terá até 48 horas para efetivar o pagamento ou até faltar 1 dia para o evento, prevalecendo o prazo que ocorrer primeiro. | PO | RF-019 |
| RB-013 | Cancelamento pelo participante | Cancelamento solicitado até 7 dias antes do evento gera reembolso de 85% do valor efetivamente recebido pela Eventus. Após esse período não há reembolso. O reembolso deve ocorrer pelo mesmo meio do pagamento original. | PO | RF-021, RF-022, RF-023, RF-024 |
| RB-014 | Cancelamento do evento | Se o organizador cancelar o evento, os participantes de eventos pagos devem receber reembolso integral. | PO | RF-005, RF-023, RF-024 |
| RB-015 | Pagamento confirmado após expiração | Se um pagamento for confirmado após a reserva expirar, a inscrição deve ser reativada se ainda existir vaga. Caso não exista vaga, o pagamento deve ser estornado. O participante deve ser informado em ambos os casos. | PO | RF-025, RF-026, RF-035 |
| RB-016 | Elegibilidade do certificado | O participante tem direito ao certificado quando possuir presença registrada em pelo menos 75% da quantidade de palestras do evento. | PO | RF-027, RF-028, RF-029, RF-030, RF-031 |
| RB-017 | Palestrante da palestra | Cada palestra possui um único palestrante. O palestrante pode registrar presença em sua palestra. O organizador também pode registrar ou corrigir presença. | PO | RF-027, RF-028 |
| RB-018 | Conteúdo do certificado | O certificado deve conter nome do participante, CPF, nome do evento, data do evento, assinatura do responsável pelo evento e nome da empresa que realizou o evento. | PO | RF-032 |
| RB-019 | Vagas ocupadas | A informação de vagas ocupadas deve estar disponível com atualização de até 1 hora e apresentar a data e hora da última atualização. | PO | RF-033, RF-034 |
| RB-020 | Notificações obrigatórias | O participante deve ser notificado sobre inscrição efetuada, inscrição confirmada, inscrição cancelada, expiração da reserva, chamada da lista de espera, pagamento tardio com reativação ou estorno, 2 dias antes do evento, 1 dia antes do evento, 15 minutos antes do evento e emissão/envio do certificado. | PO | RF-019, RF-031, RF-035 |
| RB-021 | Permissões de acesso | Organizador e financeiro visualizam pagamentos; participante visualiza apenas os próprios. Financeiro processa reembolsos; organizador e financeiro acompanham reembolsos; participante acompanha apenas os próprios. Organizador, financeiro e palestrante podem visualizar a lista completa de participantes. | PO | RF-023, RF-036, RF-037, RF-038, RF-039, RF-040 |
| RB-022 | Auditoria | Alterações sensíveis, incluindo edição de evento, ajuste de presença, cancelamento, reembolso e alteração de dados, devem possuir registro de auditoria mantido por até 5 anos. | PO | RF-041 |
| RB-023 | Retenção e direitos sobre dados | Dados pessoais poderão ser mantidos por até 5 anos. O participante pode solicitar exclusão ou anonimização, ressalvadas informações que precisem permanecer armazenadas pelo período aplicável em razão de obrigações financeiras, fiscais ou legais. | PO | RF-042 |
| RB-024 | Indisponibilidade de gateway | Quando Mercado Pago ou Banco Inter estiver indisponível, o participante deve ser informado da indisponibilidade e orientado a tentar novamente mais tarde. | PO | RF-043 |
| RB-025 | Indicador de negócio | O resultado esperado para o evento é atingir percentual de inscrições superior a 60% até 7 dias antes da realização. | PO | RF-033 |
