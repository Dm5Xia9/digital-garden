*Представляет заказ, оформленный клиентом*

| Идентификатор             | Описание                                     | СУБД проекция         |
| ------------------------- | -------------------------------------------- | --------------------- |
| [[Идентификатор заказа]]  |                                              | UUID_v6               |
| [[Идентификатор клиента]] |                                              | UUID_v6, внешний ключ |
| items                     | Список товаров в заказе ([[OrderItem]])      | jsonb                 |
| totalAmount               | Общая сумма заказа                           | number                |
| status                    | Текущий статус заказа [[(Enum) OrderStatus]] | varchat(16)           |
| createdAt                 | Дата и время создания заказа                 | timestamp             |
|                           |                                              |                       |
|                           |                                              |                       |

## Сущности

- [[OrderItem]] - Товар заказа

## Объекты значения
- [[(Enum) OrderStatus]] - Статус заказа

## Инварианты агрегата

- `Order` должен содержать хотя бы один `OrderItem`.
- `Order` не может быть оплачен, если его статус не `Pending`.
- После оплаты заказ нельзя изменять.

## Взаимодействие с другими агрегатами
* `Order` ссылается на `Customer` для понимания кто именно совершил заказ
* `OrderItem` ссылается на `Product` для привязки позиции заказа, к конкретному товару