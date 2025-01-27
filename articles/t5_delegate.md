<table style="width: 100%;"><tr><td style="width: 40%;">
<a href="../articles/t5_function.md">Методы
</a></td><td style="width: 20%;">
<a href="../readme.md">Содержание
</a></td><td style="width: 40%;">
<a href="../articles/t5_exception.md">Исключения.
</a></td><tr></table>

# Делегаты, события и лямбды.

* [Делегаты](#Делегаты)
* [Применение делегатов](#Применение-делегатов)
* [Лямбды](#Лямбды)

## Делегаты

Делегаты представляют такие объекты, которые указывают на методы. То есть делегаты - это указатели на методы и с помощью делегатов мы можем вызвать данные методы.

### Определение делегатов

Для объявления делегата используется ключевое слово **delegate**, после которого идет возвращаемый тип, название и параметры. Например:

```cs
delegate void Message();
```

Делегат Message в качестве возвращаемого типа имеет тип void (то есть ничего не возвращает) и не принимает никаких параметров. Это значит, что этот делегат может указывать на любой метод, который не принимает никаких параметров и ничего не возвращает.

Рассмотрим примение этого делегата:

```cs
class Program
{
    delegate void Message(); // 1. Объявляем делегат
 
    static void Main(string[] args)
    {
        Message mes; // 2. Создаем переменную делегата
        if (DateTime.Now.Hour < 12)
        {
            mes = GoodMorning; // 3. Присваиваем этой переменной адрес метода
        }
        else
        {
            mes = GoodEvening;
        }
        mes(); // 4. Вызываем метод
        Console.ReadKey();
    }
    private static void GoodMorning()
    {
        Console.WriteLine("Good Morning");
    }
    private static void GoodEvening()
    {
        Console.WriteLine("Good Evening");
    }
}
```

Здесь сначала мы определяем делегат:

```cs
delegate void Message(); // 1. Объявляем делегат
```

В данном случае делегат определяется внутри класса, но также можно определить делегат вне класса внутри пространства имен.

Для использования делегата объявляется переменная этого делегата:

```cs
Message mes; // 2. Создаем переменную делегата
```

С помощью свойства *DateTime.Now.Hour* получаем текущий час. И в зависимости от времени в делегат передается адрес определенного метода. Обратите внимание, что методы эти имеют то же возвращаемое значение и тот же набор параметров (в данном случае отсутствие параметров), что и делегат.

```cs
mes = GoodMorning; // 3. Присваиваем этой переменной адрес метода
```

Затем через делегат вызываем метод, на который ссылается данный делегат:

```cs
mes(); // 4. Вызываем метод
```

Вызов делегата производится подобно вызову метода.

Посмотрим на примере другого делегата:

```cs
class Program
{
    delegate int Operation(int x, int y);
     
    static void Main(string[] args)
    {
        // присваивание адреса метода через контруктор
        Operation del = Add; // делегат указывает на метод Add
        int result = del(4,5); // фактически Add(4, 5)
        Console.WriteLine(result);
 
        del = Multiply; // теперь делегат указывает на метод Multiply
        result = del(4, 5); // фактически Multiply(4, 5)
        Console.WriteLine(result);
 
        Console.Read();
    }
    private static int Add(int x, int y)
    {
        return x+y;
    }
    private static int Multiply (int x, int y)
    {
        return x * y;
    }
}
```

В данном случае делегат Operation возвращает значение типа int и имеет два параметра типа int. Поэтому этому делегату соответствует любой метод, который возвращает значение типа int и принимает два параметра типа int. В данном случае это методы Add и Multiply. То есть мы можем присвоить переменной делегата любой из этих методов и вызывать.

Поскольку делегат принимает два параметра типа int, то при его вызове необходимо передать значения для этих параметров: del(4,5).

Делегаты необязательно могут указывать только на методы, которые определены в том же классе, где определена переменная делегата. Это могут быть также методы из других классов и структур.

```cs
class Math
{
    public int Sum(int x, int y) { return x + y; }
}
class Program
{
    delegate int Operation(int x, int y);
 
    static void Main(string[] args)
    {
        Math math = new Math();
        Operation del = math.Sum;
        int result = del(4, 5);     // math.Sum(4, 5)
        Console.WriteLine(result);  // 9
 
        Console.Read();
    }
}
```

### Присвоение ссылки на метод

Выше переменной делегата напрямую присваивался метод. Есть еще один способ - создание объекта делегата с помощью конструктора, в который передается нужный метод:

```cs
class Program
{
    delegate int Operation(int x, int y);
 
    static void Main(string[] args)
    {
        Operation del = Add;
        Operation del2 = new Operation(Add);
 
        Console.Read();
    }
    private static int Add(int x, int y) { return x + y; }
}
```

Оба способа равноценны.

### Соответствие методов делегату

Как было написано выше, методы соответствуют делегату, если они имеют один и тот же возвращаемый тип и один и тот же набор параметров. Но надо учитывать, что во внимание также принимаются модификаторы ref и out. Например, пусть у нас есть делегат:

```cs
delegate void SomeDel(int a, double b);
```

Этому делегату соответствует, например, следующий метод:

```cs
void SomeMethod1(int g, double n) { }
```

А следующие методы НЕ соответствуют:

```cs
int SomeMethod2(int g, double n) { }
void SomeMethod3(double n, int g) { }
void SomeMethod4(ref int g, double n) { }
void SomeMethod5(out int g, double n) { g = 6; }
```

Здесь метод SomeMethod2 имеет другой возвращаемый тип, отличный от типа делегата. SomeMethod3 имеет другой набор параметров. Параметры SomeMethod4 и SomeMethod5 также отличаются от параметров делегата, поскольку имеют модификаторы ref и out.

### Добавление методов в делегат

В примерах выше переменная делегата указывала на один метод. В реальности же делегат может указывать на множество методов, которые имеют ту же сигнатуру и возвращаемые тип. Все методы в делегате попадают в специальный список - список вызова или invocation list. И при вызове делегата все методы из этого списка последовательно вызываются. И мы можем добавлять в этот спиок не один, а несколько методов:

```cs
class Program
{
    delegate void Message();
 
    static void Main(string[] args)
    {
        Message mes1 = Hello;
        mes1 += HowAreYou;  // теперь mes1 указывает на два метода
        mes1(); // вызываются оба метода - Hello и HowAreYou
        Console.Read();
    }
    private static void Hello()
    {
        Console.WriteLine("Hello");
    }
    private static void HowAreYou()
    {
        Console.WriteLine("How are you?");
    }
}
```

В данном случае в список вызова делегата mes1 добавляются два метода - Hello и HowAreYou. И при вызове mes1 вызываются сразу оба этих метода.

Для добавления делегатов применяется операция +=. Однако стоит отметить, что в реальности будет происходить создание нового объекта делегата, который получит методы старой копии делегата и новый метод, и новый созданный объект делеагата будет присвоен переменной mes1.

При добавлении делегатов следует учитывать, что мы можем добавить ссылку на один и тот же метод несколько раз, и в списке вызова делегата тогда будет несколько ссылок на один и то же метод. Соответственно при вызове делегата добавленный метод будет вызываться столько раз, сколько он был добавлен:

```cs
Message mes1 = Hello;
mes1 += HowAreYou;
mes1 += Hello;
mes1 += Hello;
 
mes1();
```

Консольный вывод:

```
Hello
How are you?
Hello
Hello
```

Подобным образом мы можем удалять методы из делегата с помощью операции -=:

```cs
static void Main(string[] args)
{
    Message mes1 = Hello;
    mes1 += HowAreYou;
    mes1(); // вызываются все методы из mes1
    mes1 -= HowAreYou;  // удаляем метод HowAreYou
    mes1(); // вызывается метод Hello
     
    Console.Read();
}
```

При удалении методов из делегата фактически будет создаватья новый делегат, который в списке вызова методов будет содержать на один метод меньше.

При удалении следует учитывать, что если делегат содержит несколько ссылок на один и тот же метод, то операция -= начинает поиск с конца списка вызова делегата и удаляет только первое найденное вхождение. Если подобного метода в списке вызова делегата нет, то операция -= не имеет никакого эффекта.

### Объединение делегатов

Делегаты можно объединять в другие делегаты. Например:

```cs
class Program
{
    delegate void Message();
 
    static void Main(string[] args)
    {
        Message mes1 = Hello;
        Message mes2 = HowAreYou;
        Message mes3 = mes1 + mes2; // объединяем делегаты
        mes3(); // вызываются все методы из mes1 и mes2
         
        Console.Read();
    }
    private static void Hello()
    {
        Console.WriteLine("Hello");
    }
    private static void HowAreYou()
    {
        Console.WriteLine("How are you?");
    }
}
```

В данном случае объект mes3 представляет объединение делегатов mes1 и mes2. Объединение делегатов значит, что в список вызова делегата mes3 попадут все методы из делегатов mes1 и mes2. И при вызове делегата mes3 все эти методы одновременно будут вызваны.

### Вызов делегата

В примерах выше делегат вызывался как обычный метод. Если делегат принимал параметры, то при ее вызове для параметров передавались необходимые значения:

```cs
class Program
{
    delegate int Operation(int x, int y);
    delegate void Message();
 
    static void Main(string[] args)
    {
        Message mes = Hello;
        mes();
        Operation op = Add;
        op(3, 4);
        Console.Read();
    }
    private static void Hello() { Console.WriteLine("Hello"); }
    private static int Add(int x, int y) { return x + y; }
}
```

Другой способ вызова делегата представляет метод Invoke():

```cs
class Program
{
    delegate int Operation(int x, int y);
    delegate void Message();
 
    static void Main(string[] args)
    {
        Message mes = Hello;
        mes.Invoke();
        Operation op = Add;
        op.Invoke(3, 4);
        Console.Read();
    }
    private static void Hello() { Console.WriteLine("Hello"); }
    private static int Add(int x, int y) { return x + y; }
}
```

Если делегат принимает параметры, то в метод Invoke передаются значения для этих параметров.

Следует учитывать, что если делегат пуст, то есть в его списке вызова нет ссылок ни на один из методов (то есть делегат равен Null), то при вызове такого делегата мы получим исключение, как, например, в следующем случае:

```cs
Message mes = null;
//mes();        // ! Ошибка: делегат равен null
 
Operation op = Add;
op -= Add;      // делегат op пуст
op(3, 4);       // !Ошибка: делегат равен null
```

Поэтому при вызове делегата всегда лучше проверять, не равен ли он null. Либо можно использовать метод Invoke и оператор условного null:

```cs
Message mes = null;
mes?.Invoke();        // ошибки нет, делегат просто не вызывается
 
Operation op = Add;
op -= Add;          // делегат op пуст
op?.Invoke(3, 4);   // ошибки нет, делегат просто не вызывается
```

Если делегат возвращает некоторое значение, то возвращается значение последнего метода из списка вызова (если в списке вызова несколько методов). Например:

```cs
class Program
{
    delegate int Operation(int x, int y);
     
    static void Main(string[] args)
    {
        Operation op = Subtract;
        op += Multiply;
        op += Add;
        Console.WriteLine(op(7, 2));    // Add(7,2) = 9
        Console.Read();
    }
    private static int Add(int x, int y) { return x + y; }
    private static int Subtract(int x, int y) { return x - y; }
    private static int Multiply(int x, int y) { return x * y; }
}
```

### Делегаты как параметры методов

Также делегаты могут быть параметрами методов:

```cs
class Program
{
    delegate void GetMessage();
 
    static void Main(string[] args)
    {
        if (DateTime.Now.Hour < 12)
        {
            Show_Message(GoodMorning);
        }
        else
        {
             Show_Message(GoodEvening);
        }
        Console.ReadLine();
    }
    private static void Show_Message(GetMessage _del)
    {
        _del?.Invoke();
    }
    private static void GoodMorning()
    {
        Console.WriteLine("Good Morning");
    }
    private static void GoodEvening()
    {
        Console.WriteLine("Good Evening");
    }
}
```

### Обобщенные делегаты

Делегаты могут быть обобщенными, например:

```cs
delegate T Operation<T, K>(K val);
 
class Program
{
    static void Main(string[] args)
    {
        Operation<decimal, int> op = Square;
 
        Console.WriteLine(op(5));
        Console.Read();
    }
 
    static decimal Square(int n)
    {
        return n * n;
    }
}
```

## Применение делегатов

Выще подробно были рассмотрены делегаты. Однако данные примеры, возможно, не показывают истинной силы делегатов, так как нужные нам методы в данном случае мы можем вызвать и напрямую без всяких делегатов. Однако наиболее сильная сторона делегатов состоит в том, что они позволяют делегировать выполнение некоторому коду извне. И на момент написания программы мы можем не знать, что за код будет выполняться. Мы просто вызываем делегат. А какой метод будет непосредственно выполняться при вызове делегата, будет решаться потом. Например, наши классы будут распространяться в виде отдельной библиотеки классов, которая будет подключаться в проект другого разработчика. И этот разработчик захочет определить какую-то свою логику обработки, но изменить исходный код нашей библиотеки классов он не может. И делегаты как раз предоставляют возможность вызвать некое действие, которое задается извне и которое на момент написания кода может быть неизвестно.

Рассмотрим подробный пример. Пусть у нас есть класс, описывающий счет в банке:

```cs
class Account
{
    int _sum; // Переменная для хранения суммы
 
    public Account(int sum)
    {
        _sum = sum;
    }
 
    public int CurrentSum
    {
        get { return _sum; }
    }
 
    public void Put(int sum)
    {
        _sum += sum;
    }
 
    public void Withdraw(int sum)
    {
        if (sum <= _sum)
        {
            _sum -= sum;
        }
    }
}
```

Допустим, в случае вывода денег с помощью метода Withdraw нам надо как-то уведомлять об этом самого клиента и, может быть, другие объекты. Для этого создадим делегат AccountStateHandler. Чтобы использовать делегат, нам надо создать переменную этого делегата, а затем присвоить ему метод, который будет вызываться делегатом.

Итак, добавим в класс Account следующие строки:

```cs
class Account
{
    // Объявляем делегат
    public delegate void AccountStateHandler(string message);
    // Создаем переменную делегата
    AccountStateHandler _del;
 
    // Регистрируем делегат
    public void RegisterHandler(AccountStateHandler del)
    {
        _del = del;
    }
     
    // Далее остальные строки класса Account
```

Здесь фактически проделываются те же шаги, что были выше, и есть практически все кроме вызова делегата. В данном случае у нас делегат принимает параметр типа string. Теперь изменим метод Withdraw следующим образом:

```cs
public void Withdraw(int sum)
{
    if (sum <= _sum)
    {
         _sum -= sum;
 
        if (_del != null)
            _del($"Сумма {sum} снята со счета");
    }
    else
    {
        if (_del != null)
            _del("Недостаточно денег на счете");
    }
}
```

Теперь при снятии денег через метод Withdraw мы сначала проверяем, имеет ли делегат ссылку на какой-либо метод (иначе он имеет значение null). И если метод установлен, то вызываем его, передавая соответствующее сообщение в качестве параметра.

Теперь протестируем класс в основной программе:

```cs
class Program
{
    static void Main(string[] args)
    {
        // создаем банковский счет
        Account account = new Account(200);
        // Добавляем в делегат ссылку на метод Show_Message
        // а сам делегат передается в качестве параметра метода RegisterHandler
        account.RegisterHandler(new Account.AccountStateHandler(Show_Message));
        // Два раза подряд пытаемся снять деньги
        account.Withdraw(100);
        account.Withdraw(150);
        Console.ReadLine();
    }
    private static void Show_Message(String message)
    {
        Console.WriteLine(message);
    }
}  
```

Запустив программу, мы получим два разных сообщения:

```
Сумма 100 снята со счета
Недостаточно денег на счете
```

Таким образом, мы создали механизм обратного вызова для класса Account, который срабатывает в случае снятия денег. Поскольку делегат объявлен внутри класса Account, то чтобы к нему получить доступ, используется выражение Account.AccountStateHandler.

Опять же может возникнуть вопрос: почему бы в коде метода Withdraw() не выводить сообщение о снятии денег? Зачем нужно задействовать какой-то делегат?

Дело в том, что не всегда у нас есть доступ к коду классов. Например, часть классов может создаваться и компилироваться одним человеком, который не будет знать, как эти классы будут использоваться. А использовать эти классы будет другой разработчик.

Так, здесь мы выводим сообщение на консоль. Однако для класса Account не важно, как это сообщение выводится. Классу Account даже не известно, что вообще будет делаться в результате списания денег. Он просто посылает уведомление об этом через делегат.

В результате, если мы создаем консольное приложение, мы можем через делегат выводить сообщение на консоль. Если мы создаем графическое приложение Windows Forms или WPF, то можно выводить сообщение в виде графического окна. А можно не просто выводить сообщение. А, например, записать при списании информацию об этом действии в файл или отправить уведомление на электронную почту. В общем любыми способами обработать вызов делегата. И способ обработки не будет зависеть от класса Account.

Хотя в примере наш делегат принимал адрес на один метод, в действительности он может указывать сразу на несколько методов. Кроме того, при необходимости мы можем удалить ссылки на адреса определенных методов, чтобы они не вызывались при вызове делегата. Итак, изменим в классе Account метод RegisterHandler и добавим новый метод UnregisterHandler, который будет удалять методы из списка методов делегата:

```cs
// Регистрируем делегат
public void RegisterHandler(AccountStateHandler del)
{
    _del += del; // добавляем делегат
}
// Отмена регистрации делегата
public void UnregisterHandler(AccountStateHandler del)
{
    _del -= del; // удаляем делегат
}
```

В первом методе объединяет делегаты _del и del в один, который потом присваивается переменной _del. Во втором методе удаляется делегат del. Теперь перейдем к основной программе:

```cs
class Program
{
    static void Main(string[] args)
    {
        Account account = new Account(200);
        Account.AccountStateHandler colorDelegate = new Account.AccountStateHandler(Color_Message);
 
        // Добавляем в делегат ссылку на методы
        account.RegisterHandler(new Account.AccountStateHandler(Show_Message));
        account.RegisterHandler(colorDelegate);
        // Два раза подряд пытаемся снять деньги
        account.Withdraw(100);
        account.Withdraw(150);
 
        // Удаляем делегат
        account.UnregisterHandler(colorDelegate);
        account.Withdraw(50);
             
        Console.ReadLine();
    }
    private static void Show_Message(String message)
    {
        Console.WriteLine(message);
    }
    private static void Color_Message(string message)
    {
        // Устанавливаем красный цвет символов
        Console.ForegroundColor = ConsoleColor.Red;
        Console.WriteLine(message);
        // Сбрасываем настройки цвета
        Console.ResetColor();
    }
}  
```

В целях тестирования мы создали еще один метод - Color_Message, который выводит то же самое сообщение только красным цветом. Для первого делегата создается отдельная переменная. Но большой разницы между передачей обоих в метод account.RegisterHandler нет: просто в одном случае мы сразу передаем объект, создаваемый конструктором account.RegisterHandler(new Account.AccountStateHandler(Show_Message));

Во втором случае создаем переменную и ее уже передаем в метод account.RegisterHandler(colorDelegate);.

В строке account.UnregisterHandler(colorDelegate); этот метод удаляется из списка вызовов делегата, поэтому этот метод больше не будет срабатывать. Консольный вывод будет иметь следующую форму:

```
Сумма 100 снята со счета
Сумма 100 снята со счета
Недостаточно денег на счете
Недостаточно денег на счете
Сумма 50 снята со счета
```

## Лямбды

Лямбда-выражения представляют упрощенную запись анонимных методов. Лямбда-выражения позволяют создать ёмкие лаконичные методы, которые могут возвращать некоторое значение и которые можно передать в качестве параметров в другие методы.

Ламбда-выражения имеют следующий синтаксис: слева от лямбда-оператора `=>` определяется список параметров, а справа блок выражений, использующий эти параметры: `(список_параметров) => выражение`. Например:

```cs
class Program
{
    delegate int Operation(int x, int y);
    static void Main(string[] args)
    {
        Operation operation = (x, y) => x + y;
        Console.WriteLine(operation(10, 20));       // 30
        Console.WriteLine(operation(40, 20));       // 60
        Console.Read();
    }
}
```

Здесь код `(x, y) => x + y;` представляет собой лямбда-выражение, где x и y - это параметры, а x + y - выражение. При этом нам не надо указывать тип параметров, а при возвращении результата не надо использовать оператор return.

При этом надо учитывать, что каждый параметр в лямбда-выражении неявно преобразуется в соответствующий параметр делегата, поэтому типы параметров должны быть одинаковыми. Кроме того, количество параметров должно быть таким же, как и у делегата. И возвращаемое значение лямбда-выражений должно быть тем же, что и у делегата. То есть в данном случае использованное лямбда-выражение соответствует делегату Operation как по типу возвращаемого значения, так и по типу и количеству параметров.

Если лямбда-выражение принимает один параметр, то скобки вокруг параметра можно опустить:

```cs
class Program
{
    delegate int Square(int x); // объявляем делегат, принимающий int и возвращающий int
    static void Main(string[] args)
    {
        Square square = i => i * i; // объекту делегата присваивается лямбда-выражение
 
        int z = square(6); // используем делегат
        Console.WriteLine(z); // выводит число 36
        Console.Read();
    }
}
```

Бывает, что параметров не требуется. В этом случае вместо параметра в лямбда-выражении используются пустые скобки. Также бывает, что лямбда-выражение не возвращает никакого значения:

```cs
class Program
{
    delegate void Hello(); // делегат без параметров
    static void Main(string[] args)
    {
        Hello hello1 = () => Console.WriteLine("Hello");
        Hello hello2 = () => Console.WriteLine("Welcome");
        hello1();       // Hello
        hello2();       // Welcome
        Console.Read();
    }
}
```

В данном случае лямда-выражение ничего не возвращает, так как после лямбда-оператора идет действие, которое ничего не возвращает.

Как видно, из примеров выше, нам необязательно указывать тип параметров у лямбда-выражения. Однако, нам обязательно нужно указывать тип, если делегат, которому должно соответствовать лямбда-выражение, имеет параметры с модификаторами ref и out:

```cs
class Program
{
    delegate void ChangeHandler(ref int x);
    static void Main(string[] args)
    {
        int x = 9;
        ChangeHandler ch = (ref int n) => n = n * 2;
        ch(ref x);
        Console.WriteLine(x);   // 18
        Console.Read();
    }
}
```

Лямбда-выражения также могут выполнять другие методы:

```cs
class Program
{
    delegate void Hello(); // делегат без параметров
    static void Main(string[] args)
    {
        Hello message = () => Show_Message();
        message();
    }
    private static void Show_Message()
    {
        Console.WriteLine("Привет мир!");
    }
}
```

### Лямбда-выражения как аргументы методов

Как и делегаты, лямбда-выражения можно передавать в качестве аргументов методу для тех параметров, которые представляют делегат, что довольно удобно:

```cs
class Program  
{
    delegate bool IsEqual(int x);
     
    static void Main(string[] args)
    {
        int[] integers = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
         
        // найдем сумму чисел больше 5
        int result1 = Sum(integers, x => x > 5);
        Console.WriteLine(result1); // 30
         
        // найдем сумму четных чисел
        int result2 = Sum(integers, x => x % 2 == 0);
        Console.WriteLine(result2);  //20
         
        Console.Read();
    }
 
    private static int Sum (int[] numbers, IsEqual func)
    {
        int result = 0;
        foreach(int i in numbers)
        {
            if (func(i))
                result += i;
        }
        return result;
    }
}
```

Метод Sum принимает в качестве параметра массив чисел и делегат IsEqual и возвращает сумму чисел массива в виде объекта int. В цикле проходим по всем числам и складываем их. Причем складываем только те числа, для которых делегат IsEqual func возвращает true. То есть делегат IsEqual здесь фактически задает условие, которому должны соответствовать значения массива. Но на момент написания метода Sum нам неизвестно, что это за условие.

При вызове метода Sum ему передается массив и лямбда-выражение:

```cs
int result1 = Sum(integers, x => x > 5);
```

То есть параметр x здесь будет представлять число, которое передается в делегат:

```cs
if (func(i))
```

А выражение x > 5 представляет условие, которому должно соответствовать число. Если число соответствует этому условию, то лямбда-выражение возвращает true, а переданное число складывается с другими числами.

Подобным образом работает второй вызов метода Sum, только здесь уже идет проверка числа на четность, то есть если остаток от деления на 2 равен нулю:

```cs
int result2 = Sum(integers, x => x % 2 == 0);
```

<table style="width: 100%;"><tr><td style="width: 40%;">
<a href="../articles/t5_function.md">Методы
</a></td><td style="width: 20%;">
<a href="../readme.md">Содержание
</a></td><td style="width: 40%;">
<a href="../articles/t5_exception.md">Исключения.
</a></td><tr></table>