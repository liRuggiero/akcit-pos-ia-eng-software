---
feature: sistema-gestao-eventos
version: 1.0
status: ready-for-development
updated: 2026-08-23
---

# Dúvidas e Pontos em Aberto

## Pendências

| ID | Tema | Dúvida / Ponto em aberto | Impacto | Responsável sugerido | Status |
|---|---|---|---|---|---|
| PA-001 | LGPD / Jurídico | Validar antes da entrada em produção as regras de retenção, exclusão/anonimização e consentimento obrigatório e não revogável de fotos. | Não bloqueante para desenvolvimento; bloqueante para produção | Jurídico / DPO Eventus | Aberto |
| PA-002 | Recuperação | Não foi definida métrica de tempo máximo de recuperação do serviço em caso de falha ou desastre. | Não bloqueante | Infraestrutura / PO | Aberto |
| PA-003 | Acessibilidade | Não foram definidos requisitos ou padrão de acessibilidade para esta versão. | Não bloqueante | PO | Aberto |
| PA-004 | 2FA | Não foi definido limite específico para quantidade ou frequência de reenvios do código de segundo fator. | Não bloqueante | PO / Segurança | Aberto |

## Premissas

Nenhuma premissa não confirmada foi utilizada para definir comportamento funcional ou regra de negócio.

## Decisões tomadas

| ID | Decisão | Responsável / Origem |
|---|---|---|
| DEC-001 | Utilizar Mercado Pago e Pix Banco Inter como opções de pagamento, escolhidas pelo participante. | PO |
| DEC-002 | Utilizar reserva de vaga por 48 horas para inscrições pagas aguardando pagamento. | PO |
| DEC-003 | Adotar lista de espera quando a capacidade do evento for atingida. | PO |
| DEC-004 | Eventos gratuitos terão confirmação imediata da inscrição. | PO |
| DEC-005 | Cancelamentos elegíveis feitos pelo participante terão reembolso de 85% do valor efetivamente recebido pela Eventus. | PO |
| DEC-006 | Cancelamento do evento pela Eventus gera reembolso integral. | PO |
| DEC-007 | Certificados exigem presença em pelo menos 75% da quantidade de palestras. | PO |
| DEC-008 | Autenticação utilizará dois fatores com escolha entre e-mail e SMS. | PO |
| DEC-009 | Disponibilidade deverá permanecer acima de 95%, medida diariamente. | PO |
| DEC-010 | A janela de manutenção poderá causar no máximo 15 minutos de indisponibilidade e apresentará tela de manutenção, sem necessidade de aviso prévio. | PO |
| DEC-011 | A validação Jurídico/DPO é obrigatória antes da entrada em produção. | PO |
