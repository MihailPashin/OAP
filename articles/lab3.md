# Тема циклы

Перед началом лабораторной верните свой репозиторий на ветку *master*:

```
git checkout master
```

>Если вы находитесь за другим компьютером, то предварительно склонируйте свой репозиторий командой:
>```
>git clone URL_вашего_репозитория
>```

Создайте новую ветку в репозитории:

```
git checkout -b lab4_3
```

>Если явно не указано другое, то данные вводятся с консоли

1. Найдите наибольшее число в массиве (массив инициализировать случайными значениями)

    ```cs
    // объявление массива
    var Massiv = new int[10];

    // генератор случайных чисел
    Random Rand = new Random();

    // заполняем массив случайными числами 0..100
    for (int i = 0; i < Massiv.Length; i++)
        Massiv[i] = Rand.Next(100);
    ```

2. Найти сумму и произведение всех чисел массива (массив инициализировать случайными значениями)

3. Вывести на экран ряд натуральных чисел от минимума до максимума с шагом. Например, если минимум 10, максимум 35, шаг 5, то вывод должен быть таким: 10 15 20 25 30 35. Минимум, максимум и шаг указываются пользователем (считываются из консоли).

4. Вычислить факториал числа

5. Посчитать четные и нечетные числа массива (массив инициализировать случайными значениями)

6. Ряд Фибоначчи (каждое следующее число = сумме двух предыдущих). Вывести на экран столько элементов ряда Фибоначчи, сколько указал пользователь (в консоли). 

7. Найти сумму **n** элементов (**n** ввести с консоли) следующего ряда чисел:
`1 -0.5 0.25 -0.125 ...`

8. Угадать случайное число.

    В программе генерируется случайное целое число от 0 до 100. Пользователь должен его отгадать не более чем за 10 попыток. После каждой неудачной попытки должно сообщаться больше или меньше введенное пользователем число, чем то, что загадано. Если за 10 попыток число не отгадано, то вывести загаданное число.

9. Симпатичный узор

    На днях Иван у себя в прихожей выложил кафель, состоящий из квадратных черных и белых плиток. Прихожая Ивана имеет квадратную форму 4х4, вмещающую 16 плиток. Теперь Иван переживает, что узор из плиток, который у него получился, может быть не симпатичным. С точки зрения дизайна симпатичным узором считается тот, который не содержит в себе квадрата 2х2, состоящего из плиток одного цвета.

    ![](../img/04018.png)

    По расположению плиток в прихожей Ивана требуется определить: является ли выполненный узор симпатичным (инициализировать массив случайными числами 0 и 1 и вывести в консоль узор и результат).

10. Табло

    На хоккейном стадионе в одном большом городе расположено большое прямоугольное табло. Оно имеет n строк и m столбцов (то есть состоит из n x m ячеек). Во время хоккейного матча это табло служит для отображения счета и времени, прошедшего с начала тайма, а в перерывах на нем показывают различную рекламу.

    В связи с этим возникла задача проверки возможности показа на этом табло определенной рекламной заставки. Заставка также, как и табло, имеет размер n строк на m столбцов. Каждая из ячеек заставки окрашена в один из четырех цветов - трех основных: красный - R, зеленый - G, синий - B и черный - .(точка).

    Каждая из ячеек табло характеризуется своими цветопередаточными возможностями. Любая из ячеек табло может отображать черный цвет - это соответствует тому, что на нее вообще не подается напряжение. Также каждая из ячеек может отображать некоторое подмножество множества основных цветов. В этой задаче эти подмножества будут кодироваться следующим образом:

    0 - ячейка может отображать только черный цвет;
    1 - ячейка может отображать только черный и синий цвета;
    2 - ячейка может отображать только черный и зеленый цвета;
    3 - ячейка может отображать только черный, зеленый и синий цвета;
    4 - ячейка может отображать только черный и красный цвета;
    5 - ячейка может отображать только черный, красный и синий цвета;
    6 - ячейка может отображать только черный, красный и зеленый цвета;
    7 - ячейка может отображать только черный, красный, зеленый и синий цвета.

    Напишите программу, которая по описанию табло и заставки определяет: возможно ли на табло отобразить эту заставку.

    Пример описаний (получать из консоли)

    ```
    33
    .GB
    R.B
    RG.
    012
    345
    670
    ```

    ```
    23
    RGB
    .G.
    777
    777
    ```

