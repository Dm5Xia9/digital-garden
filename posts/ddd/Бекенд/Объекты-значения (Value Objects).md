## Что такое объект-значение?

Объект-значение (Value Object) — это объект, который определяется своими атрибутами, а не уникальной идентичностью. Они неизменяемы и не имеют собственного жизненного цикла.

## Основные характеристики объектов-значений

1. **Отсутствие идентичности** — два объекта-значения считаются равными, если их значения совпадают.
2. **Неизменяемость** — после создания объект-значение не должен изменяться.
3. **Самодостаточность** — объект-значение инкапсулирует логику валидации и поведения.

## Пример объекта-значения: Адрес (Address)

```csharp
[DevEnvironment(TypeModifier.ValueObject)]
public record Address
{
    public string Street { get; init; }
    public string City { get; init; }
    public string ZipCode { get; init; }

    public Address(string street, string city, string zipCode)
    {
        if (string.IsNullOrWhiteSpace(street)) throw new ArgumentException("Улица не может быть пустой");
        if (string.IsNullOrWhiteSpace(city)) throw new ArgumentException("Город не может быть пустым");
        if (string.IsNullOrWhiteSpace(zipCode)) throw new ArgumentException("Почтовый индекс не может быть пустым");
        
        Street = street;
        City = city;
        ZipCode = zipCode;
    }
}
```

В данном примере `Address` является объектом-значением, так как определяется своими атрибутами и не имеет уникального идентификатора.

## Советы по проектированию объектов-значений

- Используйте **неизменяемые свойства** (`readonly` или `get` без `set`).
- Реализуйте **методы сравнения** (`Equals` и `GetHashCode`).
- Инкапсулируйте **валидацию данных** внутри конструктора.
- Если объект-значение становится слишком сложным, возможно, он должен быть сущностью.

## Дополнительные материалы

- [Eric Evans — Domain-Driven Design](https://www.domainlanguage.com/ddd/reference/)
- [Vaughn Vernon — Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)