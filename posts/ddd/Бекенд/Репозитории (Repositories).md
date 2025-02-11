
## Что такое репозиторий?

Репозиторий — это паттерн, предоставляющий абстракцию над механизмами хранения данных. Он служит для управления доступом к сущностям и агрегатам, обеспечивая удобный интерфейс для работы с данными в предметной области.

## Основные принципы репозиториев

1. **Инкапсуляция доступа к данным** — репозиторий скрывает детали хранения и выборки данных.
2. **Работа с доменными объектами** — репозиторий управляет сущностями и агрегатами, а не просто таблицами базы данных.
3. **Чистый интерфейс** — операции в репозитории должны отражать бизнес-логику, а не технические детали.
4. **Использование интерфейсов** — внедрение зависимости через интерфейсы позволяет легко заменять реализацию хранилища.

## Пример интерфейса репозитория

```csharp
public interface IOrderRepository
{
    Order GetById(Guid orderId);
    void Add(Order order);
    void Update(Order order);
    void Remove(Order order);
}
```

## Реализация репозитория с использованием Entity Framework Core

```csharp
public class OrderRepository : IOrderRepository
{
    private readonly AppDbContext _context;

    public OrderRepository(AppDbContext context)
    {
        _context = context;
    }

    public Order GetById(Guid orderId)
    {
        return _context.Orders.Include(o => o.Items).FirstOrDefault(o => o.OrderId == orderId);
    }

    public void Add(Order order)
    {
        _context.Orders.Add(order);
    }

    public void Update(Order order)
    {
        _context.Orders.Update(order);
    }

    public void Remove(Order order)
    {
        _context.Orders.Remove(order);
    }
}
```

## Советы по проектированию репозиториев

- Используйте **интерфейсы** для упрощения тестирования и замены реализаций.
- Предпочитайте **спецификации** (Specification Pattern) для сложных запросов.
- Ограничивайте операции в репозитории **CRUD-методами**, избегая сложной логики внутри.
- Если агрегат содержит коллекцию вложенных сущностей, учитывайте **каскадное сохранение**.
- Не завершайте транзакцию через вызов **SaveChanges** внутри методов репозитория. Область транзакции определяется **UnitOfWork**
## Дополнительные материалы

- [Eric Evans — Domain-Driven Design](https://www.domainlanguage.com/ddd/reference/)
- [Vaughn Vernon — Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)