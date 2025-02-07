
## Что такое сущность?

Сущность — это объект в предметной области, который обладает уникальной идентичностью и изменяемыми характеристиками. В отличие от объектов-значений, сущности не определяются только своими атрибутами.

## Основные характеристики сущностей

1. **Уникальный идентификатор** — сущность должна иметь стабильный и неизменяемый идентификатор.
2. **Изменяемые свойства** — состояние сущности может меняться с течением времени.
3. **Бизнес-логика** — сущности содержат логику, относящуюся к их поведению в предметной области.
4. **Сравнение по идентификатору** — две сущности считаются равными, если их идентификаторы совпадают.

## Пример сущности: Пользователь (User)

```csharp
[DevEnvironment(TypeModifier.Entity)]
public class User: Entity<User.Identifier>
{
    public string Name { get; private set; }
    public string Email { get; private set; }

    public User(Guid id, string name, string email)
    {
        Id = id;
        Name = name;
        Email = email;
    }

    public void UpdateName(string newName)
    {
        if (string.IsNullOrWhiteSpace(newName))
        {
            throw new ArgumentException("Имя пользователя не может быть пустым.");
        }
        Name = newName;
    }
    
    public record Identifier(Guid Value) : IIdentifier
    {
        public Identifier() : this(Guid.NewGuid()) { }
    }
}
```

В данном примере `User` является сущностью, так как обладает уникальным идентификатором `Id`, а также изменяемыми атрибутами (`Name` и `Email`).

## Советы по проектированию сущностей

- Используйте **Value Objects** для неизменяемых данных внутри сущности.
- Избегайте **сущностей-анемиков**, добавляйте бизнес-логику в методы сущности.
- Ограничивайте доступ к изменяемым данным, используя `private set` или инкапсуляцию.

## Дополнительные материалы

- [Eric Evans — Domain-Driven Design](https://www.domainlanguage.com/ddd/reference/)
- [Vaughn Vernon — Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)