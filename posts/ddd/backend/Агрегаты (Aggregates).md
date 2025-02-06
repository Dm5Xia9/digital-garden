
## Что такое агрегат?

Агрегат — это группа объектов, объединенных в одно логическое целое, управляемое через один корневой объект (Aggregate Root). Агрегаты обеспечивают согласованность инвариантов и управляют целостностью данных в предметной области.

## Принципы агрегатов

1. **Единая точка доступа** — внешний код взаимодействует только с корневой сущностью агрегата.
2. **Границы согласованности** — изменения внутри агрегата должны быть атомарными.
3. **Минимизация связности** — агрегаты слабо связаны между собой и обмениваются данными через доменные события.
4. **Размер агрегата** — агрегаты должны быть небольшими, чтобы не перегружать систему.

## Пример агрегата: Заказ (Order)

```csharp
[DevEnvironment(TypeModifier.AggregateRoot)]
public class Order
{
    public Guid OrderId { get; private set; }
    public Guid CustomerId { get; private set; }
    private List<OrderItem> _items = new();
    
    public IReadOnlyCollection<OrderItem> Items => _items.AsReadOnly();

    public Order(Guid customerId)
    {
        OrderId = Guid.NewGuid();
        CustomerId = customerId;
    }

    public void AddItem(Guid productId, int quantity)
    {
        var item = new OrderItem(productId, quantity);
        _items.Add(item);
    }

    public int TotalItems()
    {
        return _items.Sum(item => item.Quantity);
    }
}

public class OrderItem
{
    public Guid ProductId { get; private set; }
    public int Quantity { get; private set; }

    public OrderItem(Guid productId, int quantity)
    {
        ProductId = productId;
        Quantity = quantity;
    }
}
```

Здесь `Order` является корневой сущностью агрегата, а `OrderItem` — вложенной сущностью.

## Советы по проектированию агрегатов

- Используйте **инварианты**, чтобы гарантировать корректное состояние агрегата.
- Определите **граничные контексты (Bounded Contexts)**, чтобы минимизировать пересечения между агрегатами.
- Разделяйте **команды и запросы (CQRS)** — не загружайте агрегат данными, если они не нужны для бизнес-логики.

## Дополнительные материалы

- [Eric Evans — Оригинальная книга по DDD](https://www.domainlanguage.com/ddd/reference/)
- [Vaughn Vernon — Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)