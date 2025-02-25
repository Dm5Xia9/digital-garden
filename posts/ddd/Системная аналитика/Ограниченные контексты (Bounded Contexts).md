Тоже самое с точки зрения бекенд-разработчика: [[ddd/Бекенд/Ограниченные контексты (Bounded Contexts)|Ограниченные контексты (Bounded Contexts)]]

## Что такое ограниченный контекст?

Ограниченный контекст (Bounded Context) — это четко определенная область внутри предметной модели, в которой используются единые термины, правила и логика. Этот принцип помогает аналитикам разделять систему на управляемые части и снижать сложность взаимодействий.

## Роль аналитика в работе с ограниченными контекстами

1. **Определение границ** — анализировать бизнес-процессы и определять логические границы между контекстами.
2. **Документирование терминологии** — создавать глоссарии и поддерживать единое понимание предметной области внутри контекста.
3. **Выявление зависимостей** — анализировать взаимодействие контекстов и фиксировать возможные конфликты терминологии.
4. **Сопровождение контекстных карт** — визуализировать взаимосвязи между контекстами и отслеживать их изменения.

## Примеры ограниченных контекстов

Рассмотрим пример системы электронной коммерции:

- **Контекст заказов (Orders Context)** — определяет процесс оформления и обработки заказов.
- **Контекст клиентов (Customers Context)** — включает управление учетными записями и профилями пользователей.
- **Контекст каталога (Catalog Context)** — отвечает за описание товаров, их характеристики и цены.

Аналитик должен определить, где начинаются и заканчиваются эти контексты, какие данные они используют и какие события генерируют.

## Пример описания ограниченных контекстов: "Заказы в магазине"

### Контекст заказов (Orders Context)

**Описание:** Контекст отвечает за процесс оформления, обработки и выполнения заказов. Включает статусы заказов, способы оплаты и историю покупок.

**Основные сущности:**

- `Заказ (Order)` — содержит список товаров, сумму, статус и покупателя.
- `Позиция заказа (OrderItem)` — конкретный товар в заказе.
- `Оплата (Payment)` — информация о способе оплаты и статусе транзакции.

**Взаимодействие с другими контекстами:**

- Получает данные о товарах из **Контекста каталога**.
- Передает данные о клиентах в **Контекст клиентов**.
- Отправляет уведомления о заказах в **Контекст уведомлений**.

### Контекст клиентов (Customers Context)

**Описание:** Содержит информацию о пользователях, их профилях, контактных данных и предпочтениях.

**Основные сущности:**

- `Клиент (Customer)` — учетная запись пользователя, включающая личные данные.
- `Адрес доставки (ShippingAddress)` — сохраненные адреса для заказов.

**Взаимодействие с другими контекстами:**

- Получает данные о заказах из **Контекста заказов**.
- Используется для аутентификации и авторизации пользователей.

### Контекст каталога (Catalog Context)

**Описание:** Содержит информацию о товарах, их характеристиках, наличии и ценах.

**Основные сущности:**

- `Товар (Product)` — описание товара, цена, параметры.
- `Категория (Category)` — группировка товаров по определенным признакам.

**Взаимодействие с другими контекстами:**

- Передает данные о товарах в **Контекст заказов**.
- Используется для формирования рекомендаций в **Контексте маркетинга**.

## Инструменты аналитика для работы с ограниченными контекстами

- **Контекстные карты (Context Maps)** — диаграммы взаимодействий между контекстами.
- **Глоссарии терминов** — справочники с единым набором понятий и их значений.
- **Диаграммы потоков данных** — схемы движения информации между контекстами.
- **Бизнес-процессы (BPMN, UML)** — визуализация процессов для выявления границ контекстов.

## Рекомендации по проектированию ограниченных контекстов

- Четко определяйте **владельцев контекстов** и их ответственность.
- Фиксируйте различия в **терминологии** между контекстами.
- Используйте **контрактное взаимодействие** для обмена данными между контекстами.
- Регулярно актуализируйте **контекстные карты** при изменении бизнес-процессов.

## Дополнительные материалы

- [Eric Evans — Domain-Driven Design](https://www.domainlanguage.com/ddd/reference/)
- [Vaughn Vernon — Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)