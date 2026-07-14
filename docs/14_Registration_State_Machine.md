# Registration State Machine

## Назначение

Registration State Machine описывает полный жизненный цикл участия пользователя в матче.

Все переходы между состояниями выполняются только согласно правилам бизнес-логики.

---

# Состояния регистрации

POSITION_RESERVED

↓

WAITING_PAYMENT

↓

CONFIRMED

↓

CHECKED_IN

↓

PLAYING

↓

FINISHED

---

Отдельные конечные состояния

CANCELLED

PAYMENT_EXPIRED

NO_SHOW

---

# BR-REG-STATE-001. POSITION_RESERVED

Игровое место успешно зарезервировано.

Начинается отсчёт времени резервирования.

Разрешено:

- выполнить оплату;
- изменить позицию;
- отменить регистрацию.

---

# BR-REG-STATE-002. WAITING_PAYMENT

Ожидается подтверждение оплаты.

Разрешено:

- завершить оплату;
- отменить регистрацию.

---

# BR-REG-STATE-003. CONFIRMED

Регистрация полностью подтверждена.

Игрок становится участником матча.

Разрешено:

- пройти Check-In;
- отменить участие только по правилам организатора.

---

# BR-REG-STATE-004. CHECKED_IN

Игрок подтвердил своё присутствие.

Игрок допускается к назначению игрового слота.

---

# BR-REG-STATE-005. PLAYING

Игрок принимает участие в матче.

Создаются:

- TeamSlotAssignment;
- MatchEvents.

---

# BR-REG-STATE-006. FINISHED

Матч завершён.

Регистрация становится частью истории пользователя.

---

# BR-REG-STATE-007. CANCELLED

Регистрация отменена.

Игрок исключается из матча.

Игровое место освобождается.

---

# BR-REG-STATE-008. PAYMENT_EXPIRED

Истекло время оплаты.

Регистрация автоматически отменяется.

Игровое место освобождается.

---

# BR-REG-STATE-009. NO_SHOW

Игрок не прошёл Check-In.

Игрок не допускается к участию.

Может быть приглашён игрок из листа ожидания.

---

# BR-REG-STATE-010. Бесплатная регистрация

Если участие в матче не требует оплаты, регистрация сразу переходит:

POSITION_RESERVED

↓

CONFIRMED

Состояние WAITING_PAYMENT пропускается.

---

# BR-REG-STATE-011. Освобождение места

После перехода регистрации в состояния:

- CANCELLED;
- PAYMENT_EXPIRED;
- NO_SHOW;

зарезервированное место освобождается.

Если существует лист ожидания, система может автоматически предложить место следующему игроку.

---

# Допустимые переходы

---

POSITION_RESERVED

→ WAITING_PAYMENT

→ CANCELLED

→ PAYMENT_EXPIRED

---

WAITING_PAYMENT

→ CONFIRMED

→ CANCELLED

→ PAYMENT_EXPIRED

---

CONFIRMED

→ CHECKED_IN

→ CANCELLED

→ NO_SHOW

---

CHECKED_IN

→ PLAYING

---

PLAYING

→ FINISHED
