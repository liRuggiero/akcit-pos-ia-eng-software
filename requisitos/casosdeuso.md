---
feature: sistema-gestao-eventos
version: 1.0
status: ready-for-development
updated: 2026-08-23
---

# Casos de Uso

## UC-001 — Realizar inscrição em evento

### Objetivo
Registrar a inscrição de um participante em um evento.

### Ator principal
Participante

### Pré-condições
- O evento está disponível para inscrição.
- O participante está autenticado.

### Gatilho
O participante solicita inscrição no evento.

### Fluxo principal
1. O participante inicia a inscrição.
2. O sistema solicita os dados obrigatórios e consentimentos.
3. O participante informa os dados e manifesta os consentimentos obrigatórios.
4. O sistema verifica se o CPF já possui inscrição no evento.
5. O sistema verifica a disponibilidade de vaga.
6. O sistema registra a inscrição.
7. Se o evento for gratuito, o sistema confirma a inscrição imediatamente.
8. Se o evento for pago, o sistema reserva a vaga por 48 horas aguardando pagamento.
9. O sistema notifica o participante sobre a inscrição efetuada.

### Fluxos alternativos

#### FA-01 — Evento lotado
1. O sistema identifica que não há vaga disponível.
2. O participante é incluído na lista de espera.

#### FA-02 — Evento pago
1. O sistema apresenta as opções de pagamento previstas.
2. O participante escolhe Mercado Pago ou Pix Banco Inter.
3. A confirmação da inscrição fica condicionada ao pagamento dentro da reserva.

### Exceções

#### EX-01 — CPF já inscrito
1. O sistema identifica inscrição existente para o mesmo CPF no evento.
2. O sistema impede nova inscrição.

### Pós-condições
- A inscrição gratuita fica confirmada; ou
- A inscrição paga fica reservada aguardando pagamento; ou
- O participante fica em lista de espera.

### Requisitos relacionados
- RF-006
- RF-007
- RF-008
- RF-009
- RF-010
- RF-011
- RF-012

### Regras relacionadas
- RB-003
- RB-004
- RB-005
- RB-006
- RB-007

### História relacionada
- US-003

---

## UC-002 — Efetuar pagamento da inscrição

### Objetivo
Permitir o pagamento de uma inscrição em evento pago.

### Ator principal
Participante

### Atores secundários
- Mercado Pago
- Banco Inter

### Pré-condições
- Existe inscrição paga aguardando pagamento.
- A reserva encontra-se válida.

### Gatilho
O participante inicia o pagamento.

### Fluxo principal
1. O participante escolhe o meio de pagamento.
2. O sistema direciona a operação para o gateway correspondente.
3. O gateway processa a transação.
4. O sistema recebe a confirmação do pagamento.
5. O sistema confirma a inscrição.
6. O participante é notificado da confirmação.

### Fluxos alternativos

#### FA-01 — Mercado Pago
1. O Mercado Pago apresenta ao participante as formas e taxas disponíveis.
2. O participante escolhe uma das condições apresentadas pelo gateway.

### Exceções

#### EX-01 — Gateway indisponível
1. O sistema identifica indisponibilidade do gateway.
2. O sistema informa o participante.
3. O participante é orientado a tentar novamente posteriormente.

### Pós-condições
- Inscrição confirmada quando o pagamento for aprovado.

### Requisitos relacionados
- RF-013
- RF-014
- RF-015
- RF-043

### Regras relacionadas
- RB-008
- RB-009
- RB-024

### História relacionada
- US-004

---

## UC-003 — Expirar reserva e liberar vaga

### Objetivo
Liberar uma vaga reservada cuja inscrição não foi paga no prazo.

### Ator principal
Sistema

### Pré-condições
- Existe inscrição paga aguardando pagamento.
- O prazo da reserva atingiu 48 horas.

### Gatilho
Expiração do prazo da reserva.

### Fluxo principal
1. O sistema identifica a expiração.
2. O sistema cancela a inscrição.
3. O sistema libera a vaga.
4. O sistema notifica o participante sobre a expiração e o cancelamento.
5. Havendo lista de espera, o sistema notifica o próximo participante.
6. Não havendo lista de espera, a vaga volta a ficar disponível ao público.

### Pós-condições
- A reserva expirada não ocupa mais a vaga.
- A vaga é direcionada à lista de espera ou disponibilizada ao público.

### Requisitos relacionados
- RF-016
- RF-017
- RF-019
- RF-020
- RF-035

### Regras relacionadas
- RB-010
- RB-011
- RB-020

### História relacionada
- US-005

---

## UC-004 — Atender participante da lista de espera

### Objetivo
Permitir que o próximo participante da lista de espera utilize uma vaga liberada.

### Ator principal
Participante

### Pré-condições
- O participante está na lista de espera.
- Uma vaga foi liberada.

### Gatilho
Liberação de vaga.

### Fluxo principal
1. O sistema seleciona o próximo participante da lista.
2. O sistema notifica o participante.
3. O participante possui até 48 horas para efetivar o pagamento ou até faltar 1 dia para o evento, prevalecendo o prazo que ocorrer primeiro.
4. O pagamento confirmado dentro do prazo confirma a inscrição.

### Pós-condições
- Inscrição confirmada quando a condição de pagamento for atendida.

### Requisitos relacionados
- RF-018
- RF-019

### Regras relacionadas
- RB-011
- RB-012

### História relacionada
- US-005

---

## UC-005 — Cancelar inscrição e processar reembolso

### Objetivo
Cancelar uma inscrição e aplicar a regra de reembolso.

### Ator principal
Participante

### Atores secundários
- Financeiro

### Pré-condições
- Existe uma inscrição do participante.

### Gatilho
O participante solicita cancelamento.

### Fluxo principal
1. O participante solicita o cancelamento.
2. O sistema calcula a antecedência em relação ao evento.
3. Se o cancelamento ocorrer até 7 dias antes do evento, o sistema determina reembolso de 85% do valor efetivamente recebido pela Eventus.
4. O financeiro processa o reembolso pelo mesmo meio do pagamento original.
5. A inscrição é cancelada.
6. O participante é notificado.

### Fluxos alternativos

#### FA-01 — Cancelamento fora do prazo de reembolso
1. O cancelamento ocorre após o período elegível.
2. A inscrição é cancelada.
3. Não há reembolso.

### Pós-condições
- Inscrição cancelada.
- Reembolso processado quando aplicável.

### Requisitos relacionados
- RF-021
- RF-022
- RF-023
- RF-024
- RF-035

### Regras relacionadas
- RB-013
- RB-020

### História relacionada
- US-006
- US-007

---

## UC-006 — Cancelar evento

### Objetivo
Cancelar integralmente um evento.

### Ator principal
Organizador

### Atores secundários
- Financeiro

### Pré-condições
- O evento existe.

### Gatilho
O organizador solicita o cancelamento do evento.

### Fluxo principal
1. O organizador cancela o evento.
2. O sistema identifica as inscrições pagas.
3. Os participantes pagos tornam-se elegíveis a reembolso integral.
4. O financeiro processa os reembolsos.
5. Os participantes são notificados do cancelamento.

### Pós-condições
- Evento cancelado.
- Participantes pagos com reembolso integral aplicável.

### Requisitos relacionados
- RF-005
- RF-023
- RF-024
- RF-035

### Regras relacionadas
- RB-014
- RB-020

### História relacionada
- US-002
- US-007

---

## UC-007 — Tratar pagamento confirmado após expiração

### Objetivo
Tratar confirmação tardia de pagamento de uma reserva já expirada.

### Ator principal
Sistema

### Atores secundários
- Financeiro

### Pré-condições
- A reserva expirou.
- O gateway confirmou posteriormente o pagamento.

### Gatilho
Recebimento da confirmação tardia.

### Fluxo principal
1. O sistema verifica se ainda existe vaga disponível.
2. Havendo vaga, o sistema reativa a inscrição.
3. O sistema informa o participante sobre a reativação.

### Fluxos alternativos

#### FA-01 — Sem vaga
1. O sistema identifica que não existe mais vaga.
2. O pagamento é estornado.
3. O participante é informado sobre o estorno.

### Pós-condições
- Inscrição reativada ou pagamento estornado.

### Requisitos relacionados
- RF-025
- RF-026
- RF-035

### Regras relacionadas
- RB-015
- RB-020

### História relacionada
- US-007
- US-011

---

## UC-008 — Registrar presença e emitir certificado

### Objetivo
Registrar presença nas palestras e emitir certificado para participante elegível.

### Ator principal
Palestrante

### Atores secundários
- Organizador
- Participante

### Pré-condições
- O participante possui inscrição no evento.
- A palestra existe.

### Gatilho
Registro de presença em uma palestra.

### Fluxo principal
1. O palestrante registra a presença dos participantes em sua palestra.
2. O sistema acumula a quantidade de palestras frequentadas por participante.
3. O sistema calcula o percentual de presença.
4. Ao atingir pelo menos 75% das palestras do evento, o participante torna-se elegível ao certificado.
5. O sistema gera o certificado automaticamente.
6. O sistema envia o certificado por e-mail.

### Fluxos alternativos

#### FA-01 — Correção pelo organizador
1. O organizador registra ou corrige a presença.
2. O sistema recalcula a participação do participante.

### Pós-condições
- Presenças atualizadas.
- Certificado emitido quando o percentual mínimo for alcançado.

### Requisitos relacionados
- RF-027
- RF-028
- RF-029
- RF-030
- RF-031
- RF-032

### Regras relacionadas
- RB-016
- RB-017
- RB-018

### História relacionada
- US-008
- US-009
