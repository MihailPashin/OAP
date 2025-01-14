<table style="width: 100%;"><tr><td style="width: 40%;">
<a href="../articles/wpf_filtering.md">Фильтрация данных
</a></td><td style="width: 20%;">
<a href="../readme.md">Содержание
</a></td><td style="width: 40%;">
<a href="../articles/wpf_filtering.md">Фильтрация данных
</a></td><tr></table>

# Поиск, сортировка

В этой теме мы познакомимся еще с двумя визуальными элементами: **TextBox** (ввод строки для поиска) и **RadioButton** (сортировка по одному полю)

## Поиск

1. В разметке окна (в элемент WrapPanel) добавляем элемент для ввода теста - TextBox

    ```xml
    <Label 
        Content="искать" 
        VerticalAlignment="Center"/>
    <TextBox
        Width="200"
        VerticalAlignment="Center"
        x:Name="SearchFilterTextBox" 
        KeyUp="SearchFilter_KeyUp"/>
    ```    

2. В коде окна создаем переменную для хранения строки поиска и запоминаем её в обработчике ввода текста (SearchFilter_KeyUp)

    ```cs
    private string SearchFilter = ""; 

    private void SearchFilter_KeyUp(object sender, KeyEventArgs e)
    {
        SearchFilter = SearchFilterTextBox.Text;
        Invalidate();
    }
    ```

3. Дорабатываем геттер списка кошек, чтобы после фильтра по возрасту срабатывал ещё и фильтр по кличке

    ```cs
    get
    {
        // сохраняем во временную переменную полный список
        var res = _CatList;

        // фильтруем по возрасту
        res = res.Where(c=>(c.Age>=SelectedAge.AgeFrom && c.Age<SelectedAge.AgeTo));

        // если фильтр не пустой, то ищем ВХОЖДЕНИЕ подстроки поиска в кличке без учета регистра
        if(SearchFilter != "")
            res = res.Where(c => c.Name.IndexOf(SearchFilter, StringComparison.OrdinalIgnoreCase) >= 0);

        return res;
    }
    ```

## Сортировка

>Мы, в рамках знакомства с визуальными элементами, будем использовать радио-кнопки, но, если вариантов сортировки более одного, то лучше использовать тот-же выпадающий список

Мы будем сортировать по возрасту

1. В разметке добавляем радиокнопки

    ```xml
    <Label 
        Content="Возраст:" 
        VerticalAlignment="Center"/>
    <RadioButton
        GroupName="Age"
        Tag="1"
        Content="по возрастанию"
        IsChecked="True"
        Checked="RadioButton_Checked"
        VerticalAlignment="Center"/>
    <RadioButton
        GroupName="Age"
        Tag="2"
        Content="по убыванию"
        Checked="RadioButton_Checked"
        VerticalAlignment="Center"/>
    ```

    У группы радио-кнопок одновременно может быть выбран только один вариант. Группа задается атрибутом **GroupName** 

2. В коде добавляем переменную для хранения варианта сортировки и обработчик смены варианта сортировки

    ```cs
    private bool SortAsc = true;

    private void RadioButton_Checked(object sender, RoutedEventArgs e)
    {
        SortAsc = (sender as RadioButton).Tag.ToString() == "1";
        Invalidate();
    }
    ```

3. И дорабатываем геттер списка кошек

    ```cs
    ...
    if (SortAsc) res = res.OrderBy(c=>c.Age);
    else res = res.OrderByDescending(c => c.Age);

    return res;
    ```