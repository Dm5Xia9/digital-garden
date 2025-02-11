
## Что такое доменный сервис?

Доменные сервисы используются, когда бизнес-логика не принадлежит конкретной сущности или объекту-значению. Они представляют собой операции, относящиеся к предметной области, но не укладывающиеся в границы одной сущности.

## Когда использовать доменные сервисы?

1. **Операция затрагивает несколько сущностей** — если логика не может быть отнесена к одной сущности.
2. **Отсутствует естественный владелец логики** — если метод не вписывается в существующую сущность.
3. **Бизнес-логика должна быть изолирована** — например, сложные расчеты или валидация данных.

## Пример доменного сервиса: Расчет стоимости заказа

```csharp
public interface IOrderPricingService
{
    decimal CalculateTotalPrice(IEnumerable<Order> orders);
}

public class OrderPricingService : IOrderPricingService
{
    public decimal CalculateTotalPrice(IEnumerable<Order> orders)
    {
        return orders.Sum(order => order.Items.Sum(item => item.Price * item.Quantity));
    }
}
```

## Советы по проектированию доменных сервисов

- Доменные сервисы **не должны хранить состояние**.
- Ориентируйтесь на **бизнес-логику**, а не на технические детали.
- Используйте **интерфейсы** для удобства тестирования и внедрения зависимостей.

## Дополнительные материалы

- [Eric Evans — Domain-Driven Design](https://www.domainlanguage.com/ddd/reference/)
- [Vaughn Vernon — Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)