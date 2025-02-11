## **Общее назначение**

`Example.EntityFramework.csproj` – это инфраструктурный проект, реализующий **слой работы с базой данных** через **ORM Entity Framework (EF Core)**.

Этот проект принадлежит к **Infrastructure Layer** в **Чистой архитектуре** и предназначен для **хранения и извлечения данных** без нарушения принципов разделения ответственности (**SRP**).

В **DDD** инфраструктурный слой **не содержит доменной логики**, а лишь предоставляет механизмы для работы с хранилищем данных.

---

## **Архитектурные принципы**

### **1. Явное указание используемой технологии**

Проект **называется `EntityFramework`**, потому что **структура приложения должна явно указывать на используемые технологии**.

Даже если **Entity Framework** будет заменен на другую ORM (например, **Dapper, NHibernate**), структура проекта должна отражать этот выбор. Это помогает:  
✅ Улучшить читаемость кода.  
✅ Сделать инфраструктурный слой **самодокументируемым**.  
✅ Упростить сопровождение проекта.

---

## **Структура проекта**

Классический **EF Core** проект содержит следующие компоненты:

### **1. `Migrations/` – Миграции базы данных**

- Здесь хранятся файлы миграций (`*.cs`), которые описывают изменения схемы БД.
- Управляются через **EF Core Migrations** (`dotnet ef migrations`).
- Позволяют безопасно обновлять структуру базы данных **без потери данных**.

```bash
dotnet ef migrations add Init
dotnet ef database update
```

---

### **2. `ModelBuilders/` – Конфигурация маппинга (Fluent API)**

- Содержит классы **конфигурации маппинга** между **реляционной схемой** и **объектной моделью**.
- Используется `EntityTypeConfiguration<T>` для настройки свойств таблиц, индексов, связей (`HasOne`, `HasMany`).

```csharp
public class OrderConfiguration : IEntityTypeConfiguration<OrderStorageObject>
{
    public void Configure(EntityTypeBuilder<OrderStorageObject> builder)
    {
        builder.ToTable("Orders");
        builder.HasKey(o => o.Id);
        builder.Property(o => o.Amount).HasColumnType("decimal(18,2)");
    }
}
```

---

### **3. `StorageObjects/` – Объекты хранения данных (internal)**

- Представляют собой **сущности БД**, соответствующие таблицам.
- Используются **только внутри инфраструктурного слоя**, **не должны быть доступны в других слоях**.
- Все **StorageObjects** помечаются как **internal**, чтобы **не утекали за границы инфраструктуры**.

```csharp
internal class OrderStorageObject
{
    public Guid Id { get; set; }
    public decimal Amount { get; set; }
    public DateTime CreatedAt { get; set; }
}
```

---

### **4. `Repositories/` – Реализации репозиториев**

- **Репозитории обеспечивают доступ к данным** через **EF Core**.
- Они реализуют **интерфейсы доменного слоя**, инкапсулируя логику взаимодействия с БД.
[[Репозитории (Repositories)]]

---

### **5. `UnitOfWorks/` – Реализация паттерна Unit of Work**

- `Unit of Work` координирует работу репозиториев и управляет транзакциями.
[[UnitOfWork (UoW)]]

---

### **6. `EntityFrameworkModule.cs` – Конфигурация DI-контейнера**

```csharp
public static class EntityFrameworkModule
{
    public static IServiceCollection AddEntityFramework(this IServiceCollection services, string connectionString)
    {
        services.AddDbContext<HrmDbContext>(options =>
            options.UseSqlServer(connectionString));

        services.AddScoped<IOrderRepository, OrderRepository>();
        services.AddScoped<IUnitOfWork, UnitOfWork>();

        return services;
    }
}
```

---

### **7. `HrmDbContext.cs` – Основной `DbContext`**

```csharp
public class HrmDbContext : DbContext
{
    public DbSet<OrderStorageObject> Orders { get; set; }

    public HrmDbContext(DbContextOptions<HrmDbContext> options) : base(options) { }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfigurationsFromAssembly(typeof(HrmDbContext).Assembly);
    }
}
```

---

### **8. `Specifications/` – Спецификации для фильтрации данных**

```csharp
public class OrdersByCustomerIdSpecification : Specification<OrderStorageObject>
{
    public OrdersByCustomerIdSpecification(Guid customerId)
        : base(o => o.CustomerId == customerId) { }
}
```