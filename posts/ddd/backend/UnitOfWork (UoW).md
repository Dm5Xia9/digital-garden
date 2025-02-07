## Что такое Unit of Work?

**Unit of Work** — это паттерн проектирования, который управляет изменениями в бизнес-объектах, объединяя операции над несколькими репозиториями в одну транзакцию. Он отслеживает изменения, которые происходят в сущностях, и обеспечивает их сохранение в базе данных в рамках одной транзакции.

### Основные цели Unit of Work

1. **Координация изменений** — позволяет группировать изменения в различных репозиториях.
2. **Управление транзакциями** — обеспечивает атомарность операций, гарантируя, что все изменения будут применены или отменены.
3. **Отслеживание состояния объектов** — позволяет отслеживать новые, измененные и удаленные объекты.

### Пример использования Unit of Work

Предположим, у нас есть два репозитория: `IOrderRepository` и `ICustomerRepository`. Мы хотим создать новый заказ и обновить информацию о клиенте в рамках одной транзакции.

```csharp
public interface IUnitOfWork : IDisposable
{
    IOrderRepository Orders { get; }
    ICustomerRepository Customers { get; }
    int SaveChanges();
}
```

### Реализация Unit of Work

```csharp
public class UnitOfWork : IUnitOfWork
{
    private readonly AppDbContext _context;

    public UnitOfWork(AppDbContext context)
    {
        _context = context;
        Orders = new OrderRepository(_context);
        Customers = new CustomerRepository(_context);
    }

    public IOrderRepository Orders { get; private set; }
    public ICustomerRepository Customers { get; private set; }

    public int SaveChanges()
    {
        return _context.SaveChanges();
    }

    public void Dispose()
    {
        _context.Dispose();
    }
}
```

### Пример использования в сервисе

```csharp
public class OrderService
{
    private readonly IUnitOfWork _unitOfWork;

    public OrderService(IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }

    public void CreateOrder(Order order, Customer customer)
    {
        _unitOfWork.Customers.Update(customer);
        _unitOfWork.Orders.Add(order);
        _unitOfWork.SaveChanges();
    }
}
```