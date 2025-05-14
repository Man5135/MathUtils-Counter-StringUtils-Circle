Вопрос 1
Ошибка заключается в том, что классы Person и Employee объявлены дважды: один раз внутри пространства имён HelloApp, а второй раз — вне его.

Чтобы исправить, нужно оставить только одно объявление этих классов. Например, можно удалить дублирующиеся объявления в конце файла.
Правильный вариант:
```
using System;

namespace HelloApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Person tom = new Employee();
            Console.ReadKey();
        }
    }

    internal class Person
    {
    }

    public class Employee : Person
    {
    }
}
```
Вопрос 2
При создании объекта Employee следующим образом:
Employee tom = new Employee("Tom", "Microsoft");
Будут вызваны конструкторы в таком порядке:

Employee(string name, string company)
Это основной конструктор, который ты вызываешь.

Person(string name) (через base(name))
Этот конструктор вызывается из Employee, так как указан base(name).

Person(string name, int age) (через this(name, 18))
Конструктор Person(string name) делегирует выполнение сюда, передавая age = 18 по умолчанию.

Инициализация полей Employee (company = "Microsoft")
После завершения цепочки конструкторов базового класса (Person) выполняется присваивание поля company в Employee.

Итоговая последовательность:
Employee("Tom", "Microsoft")

→ Person("Tom")

→ Person("Tom", 18) (устанавливает name = "Tom", age = 18)

→ company = "Microsoft" (в Employee)

Таким образом, в итоге объект tom будет иметь:

name = "Tom" (наследуется от Person),

age = 18 (значение по умолчанию из цепочки конструкторов Person),

company = "Microsoft" (устанавливается в Employee).

Вопрос 3
В C# можно запретить наследование от класса, сделав его sealed (запечатанным).
```
public sealed class Person  // Теперь от Person нельзя наследоваться
{
    public string Name { get; set; }
}

// Попытка наследования вызовет ошибку компиляции:
public class Employee : Person  //  Ошибка: "Cannot derive from sealed type 'Person'"
{
    public string Company { get; set; }
}
```
Вопрос 4
Программа выведет на консоль:
Грузовик с грузоподъемностью 1.1 тонн

Почему ?
Создание объекта Truck

При вызове new Truck(2, 1.1m) сначала вызывается конструктор Truck(int seats, decimal capacity).

Внутри него явно присваиваются значения свойствам:

Seats = seats (наследуется от Auto),

Capacity = capacity (свойство самого Truck).

Конструктор базового класса Auto

Хотя у Auto есть конструктор Auto(int seats), он не вызывается автоматически, потому что в Truck нет явного вызова base(seats).

Вместо этого Seats устанавливается напрямую через присваивание (Seats = seats).

Вывод на консоль

Значение truck.Capacity (1.1) подставляется в строку, и результат выводится.

Вопрос 5
Программа выведет на консоль следующий результат:
Auto has been created
Truck has been created
Truck with capacity 1.1

Пошаговое объяснение:
1.Создание объекта Truck
Вызывается конструктор Truck(decimal capacity).
Поскольку не указан явный вызов base(...), сначала автоматически вызывается конструктор по умолчанию базового класса Auto() (без параметров).
Он выводит сообщение: "Auto has been created".

2.Выполнение конструктора Truck
После завершения конструктора Auto() выполняется тело конструктора Truck(decimal capacity):
Seats = 2 – устанавливает значение унаследованного свойства.
Capacity = capacity – присваивает значение 1.1m.
Выводит сообщение: "Truck has been created".

3.Вывод информации в Main
Console.WriteLine($"Truck with capacity {truck.Capacity}") подставляет значение truck.Capacity (1.1) и выводит:
"Truck with capacity 1.1".

Почему не вызвался Auto(int seats)?
Конструктор Auto(int seats) не вызывается, потому что:
В Truck нет явного указания base(seats).

Если не указать base(...), компилятор автоматически вызывает конструктор по умолчанию базового класса (Auto()).
Вопрос 6
Программа выведет на консоль:
Sam

Пошаговое объяснение:
1.Инициализация свойства Name в классе Person
Свойству Name присваивается значение по умолчанию "Ben" (при создании объекта).
Но это значение перезаписывается дальше!
Вызов конструктора Employee
При создании объекта Employee вызывается конструктор:

2.Вызов конструктора Employee
При создании объекта Employee вызывается конструктор:
Параметры:
name = "Tom" (но он не используется в конструкторе Employee).
company = "Microsoft".

3.Вызов базового конструктора Person через base("Bob")

В конструкторе Employee явно вызывается base("Bob"), поэтому:

В конструкторе Person(string name) параметр name принимает значение "Bob".

Внутри конструктора Person происходит:
Name = "Tim";  // Свойство Name перезаписывается на "Tim" (вместо "Bob").

4.Инициализация Company в Employee

После выполнения base("Bob") управление возвращается в Employee, где:
Company = company;  // company = "Microsoft".

5.Инициализатор объекта { Name = "Sam" }

В самом конце выполняется инициализатор свойства Name:
{ Name = "Sam" }  // Перезаписывает предыдущее значение "Tim".
