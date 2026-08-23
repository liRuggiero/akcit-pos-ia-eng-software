---
feature: sistema-gestao-eventos
version: 1.0
status: ready-for-development
updated: 2026-08-23
---

# Critérios de Aceite

## CA-001 — Autenticação com segundo fator

História relacionada: US-001  
Requisitos relacionados: RF-001, RF-002  
Caso de uso relacionado: não aplicável

### Cenário 1 — Segundo fator válido
Given um usuário com credenciais válidas  
When ele escolher receber o código por e-mail ou SMS e informar um código válido dentro de 3 minutos  
Then o sistema deve permitir a continuidade da autenticação.

### Cenário 2 — Código expirado
Given um código de segundo fator emitido há mais de 3 minutos  
When o usuário tentar utilizá-lo  
Then o sistema não deve aceitar o código.

### Cenário 3 — Limite de tentativas
Given um usuário em processo de autenticação  
When ocorrerem 3 tentativas inválidas de segundo fator  
Then o acesso deve ficar bloqueado por 24 horas.

---

## CA-002 — Inscrição única por CPF

História relacionada: US-003  
Requisitos relacionados: RF-006, RF-009  
Caso de uso relacionado: UC-001

### Cenário 1 — Primeira inscrição
Given um CPF sem inscrição no evento  
When o participante concluir a inscrição válida  
Then o sistema deve registrar a inscrição.

### Cenário 2 — Inscrição duplicada
Given um CPF que já possui inscrição no evento  
When for solicitada nova inscrição para o mesmo CPF  
Then o sistema deve impedir a duplicidade.

---

## CA-003 — Inscrição em evento gratuito

História relacionada: US-003  
Requisitos relacionados: RF-010  
Caso de uso relacionado: UC-001

### Cenário 1 — Evento gratuito com vaga
Given um evento gratuito com vaga disponível  
When o participante concluir a inscrição  
Then o sistema deve confirmar a inscrição imediatamente  
And não deve criar reserva de pagamento de 48 horas.

---

## CA-004 — Reserva de vaga em evento pago

História relacionada: US-003  
Requisitos relacionados: RF-011, RF-012  
Caso de uso relacionado: UC-001

### Cenário 1 — Inscrição paga aguardando pagamento
Given um evento pago com vaga disponível  
When a inscrição for concluída  
Then o sistema deve reservar uma vaga durante 48 horas  
And a vaga reservada deve contar como ocupada durante esse período.

---

## CA-005 — Pagamento da inscrição

História relacionada: US-004  
Requisitos relacionados: RF-013, RF-014, RF-015  
Caso de uso relacionado: UC-002

### Cenário 1 — Escolha Banco Inter
Given uma inscrição paga aguardando pagamento  
When o participante escolher Pix Banco Inter e o pagamento for confirmado  
Then o sistema deve confirmar a inscrição.

### Cenário 2 — Escolha Mercado Pago
Given uma inscrição paga aguardando pagamento  
When o participante escolher Mercado Pago  
Then as formas e taxas aplicáveis devem ser apresentadas pelo gateway Mercado Pago.

### Cenário 3 — Gateway indisponível
Given uma tentativa de pagamento  
When o gateway selecionado estiver indisponível  
Then o sistema deve informar a indisponibilidade  
And orientar o participante a tentar novamente posteriormente.

---

## CA-006 — Expiração da reserva

História relacionada: US-005  
Requisitos relacionados: RF-016, RF-017, RF-020, RF-035  
Caso de uso relacionado: UC-003

### Cenário 1 — Reserva sem pagamento
Given uma inscrição paga sem confirmação de pagamento  
When forem completadas 48 horas de reserva  
Then a inscrição deve ser cancelada automaticamente  
And a vaga deve ser liberada  
And o participante deve ser notificado da expiração e do cancelamento.

### Cenário 2 — Ausência de lista de espera
Given uma vaga liberada por expiração  
And não existir participante na lista de espera  
When a vaga for liberada  
Then ela deve voltar a ficar disponível ao público.

---

## CA-007 — Lista de espera

História relacionada: US-005  
Requisitos relacionados: RF-018, RF-019  
Caso de uso relacionado: UC-004

### Cenário 1 — Evento lotado
Given um evento com capacidade integralmente ocupada  
When uma nova inscrição for solicitada  
Then o participante deve ser incluído na lista de espera.

### Cenário 2 — Vaga liberada
Given uma lista de espera existente  
When uma vaga for liberada  
Then o próximo participante da lista deve ser notificado.

### Cenário 3 — Limite da oferta
Given um participante chamado da lista de espera  
When for determinado seu prazo para pagamento  
Then o prazo deve ser de até 48 horas ou até faltar 1 dia para o evento, prevalecendo o que ocorrer primeiro.

---

## CA-008 — Cancelamento pelo participante

História relacionada: US-006  
Requisitos relacionados: RF-021, RF-022, RF-024  
Caso de uso relacionado: UC-005

### Cenário 1 — Cancelamento elegível a reembolso
Given uma inscrição paga  
When o participante cancelar até 7 dias antes do evento  
Then o reembolso deve corresponder a 85% do valor efetivamente recebido pela Eventus  
And deve utilizar o mesmo meio do pagamento original.

### Cenário 2 — Cancelamento sem reembolso
Given uma inscrição paga  
When o participante cancelar após o período de até 7 dias antes do evento  
Then a inscrição deve ser cancelada  
And nenhum reembolso deve ser devido.

---

## CA-009 — Cancelamento do evento

História relacionada: US-002, US-007  
Requisitos relacionados: RF-005, RF-023, RF-024  
Caso de uso relacionado: UC-006

### Cenário 1 — Evento cancelado pela Eventus
Given um evento com participantes pagos  
When o organizador cancelar o evento  
Then os participantes pagos devem ter direito a reembolso integral.

---

## CA-010 — Pagamento tardio com vaga

História relacionada: US-007  
Requisitos relacionados: RF-025, RF-035  
Caso de uso relacionado: UC-007

### Cenário 1 — Há vaga disponível
Given uma reserva expirada  
And o gateway confirmar posteriormente o pagamento  
And ainda existir vaga disponível  
When o sistema processar a confirmação  
Then a inscrição deve ser reativada  
And o participante deve ser informado.

---

## CA-011 — Pagamento tardio sem vaga

História relacionada: US-007  
Requisitos relacionados: RF-026, RF-035  
Caso de uso relacionado: UC-007

### Cenário 1 — Não há vaga disponível
Given uma reserva expirada  
And o gateway confirmar posteriormente o pagamento  
And não existir vaga disponível  
When o sistema processar a confirmação  
Then o pagamento deve ser estornado  
And o participante deve ser informado.

---

## CA-012 — Registro de presença

História relacionada: US-008  
Requisitos relacionados: RF-027, RF-028, RF-029  
Caso de uso relacionado: UC-008

### Cenário 1 — Registro pelo palestrante
Given uma palestra associada a um palestrante  
When o palestrante registrar a presença de um participante  
Then essa presença deve ser considerada no cálculo da quantidade de palestras frequentadas.

### Cenário 2 — Ajuste pelo organizador
Given uma presença registrada  
When o organizador corrigir o registro  
Then o sistema deve utilizar o registro corrigido para o cálculo de presença.

---

## CA-013 — Emissão do certificado

História relacionada: US-009  
Requisitos relacionados: RF-030, RF-031, RF-032  
Caso de uso relacionado: UC-008

### Cenário 1 — Presença suficiente
Given um participante com presença em pelo menos 75% da quantidade de palestras do evento  
When o sistema calcular sua participação  
Then o certificado deve ser gerado automaticamente  
And enviado por e-mail.

### Cenário 2 — Conteúdo do certificado
Given um certificado gerado  
Then ele deve conter nome do participante, CPF, nome do evento, data do evento, assinatura do responsável pelo evento e nome da empresa realizadora.

### Cenário 3 — Presença insuficiente
Given um participante com presença inferior a 75% da quantidade de palestras  
When o sistema calcular sua participação  
Then o certificado não deve ser emitido.

---

## CA-014 — Atualização de vagas ocupadas

História relacionada: US-010  
Requisitos relacionados: RF-033, RF-034  
Caso de uso relacionado: não aplicável

### Cenário 1 — Exibição da ocupação
Given um evento com inscrições registradas  
When um usuário autorizado consultar as vagas ocupadas  
Then a informação apresentada deve possuir atualização não superior a 1 hora  
And a tela deve apresentar data e hora da última atualização.

---

## CA-015 — Notificações do participante

História relacionada: US-011  
Requisitos relacionados: RF-019, RF-031, RF-035  
Caso de uso relacionado: UC-003, UC-004, UC-007, UC-008

### Cenário 1 — Eventos obrigatórios
Given um participante relacionado a uma inscrição  
When ocorrer um dos eventos de notificação definidos  
Then o sistema deve gerar a respectiva notificação.

### Cenário 2 — Lembretes do evento
Given uma inscrição confirmada  
When faltarem 2 dias, 1 dia ou 15 minutos para o início do evento  
Then o sistema deve gerar a notificação correspondente em cada marco.

---

## CA-016 — Controle de acesso às informações

História relacionada: US-012  
Requisitos relacionados: RF-036, RF-037, RF-038, RF-039, RF-040  
Caso de uso relacionado: não aplicável

### Cenário 1 — Participante consulta pagamento
Given um participante autenticado  
When consultar pagamentos  
Then deve visualizar apenas os próprios pagamentos.

### Cenário 2 — Participante consulta reembolso
Given um participante autenticado  
When consultar reembolsos  
Then deve visualizar apenas os próprios reembolsos.

### Cenário 3 — Lista completa de participantes
Given um usuário com perfil organizador, financeiro ou palestrante  
When consultar a lista de participantes  
Then o sistema deve permitir a visualização da lista completa.

### Cenário 4 — Participante tenta acessar lista completa
Given um usuário com perfil participante  
When tentar acessar a lista completa de participantes  
Then o sistema não deve permitir o acesso.

---

## CA-017 — Auditoria de alterações sensíveis

História relacionada: US-013  
Requisitos relacionados: RF-041  
Caso de uso relacionado: não aplicável

### Cenário 1 — Alteração sensível
Given um usuário autorizado  
When realizar edição de evento, ajuste de presença, cancelamento, reembolso ou alteração de dados  
Then o sistema deve registrar a ação para auditoria.

---

## CA-018 — Performance

Histórias relacionadas: US-001, US-003, US-009  
Requisitos relacionados: RNF-001  
Caso de uso relacionado: UC-001, UC-008

### Cenário 1 — Operações medidas
Given o sistema em operação  
When forem executadas operações de login, inscrição, consulta de evento ou emissão de certificado  
Then a operação deve responder em até 5 segundos.

---

## CA-019 — Disponibilidade

Requisitos relacionados: RNF-002  
Caso de uso relacionado: não aplicável

### Cenário 1 — Disponibilidade diária
Given um dia de operação do sistema  
When a disponibilidade for apurada  
Then o resultado deve ser superior a 95%.

---

## CA-020 — Janela de manutenção

Requisitos relacionados: RNF-004, RNF-005  
Caso de uso relacionado: não aplicável

### Cenário 1 — Manutenção em andamento
Given uma janela de manutenção  
When o sistema estiver indisponível pela manutenção  
Then a indisponibilidade não deve superar 15 minutos  
And uma tela de manutenção deve ser apresentada.

---

## CA-021 — Backup

Requisitos relacionados: RNF-006  
Caso de uso relacionado: não aplicável

### Cenário 1 — Rotina de backup
Given o sistema em operação  
When for verificada a política de backup  
Then deve existir execução diária de backup.