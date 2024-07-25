# ForExam

Ответ на вопрос 4.

Создание класса неизменяемого списка. Создание конструкторов - только там можно изменить массив list. Список может содержать различные типы данных.
```c#
class Unchange<T>
{
    private T[] list = new T[0];

    public Unchange(T[] list)
    {
        this.list = list;
    }

    public Unchange(T item)
    {
        list = new T[1] { item };
    }

    public Unchange()
    {
        list = new T[0];
    }
```
Нельзя менять содержимое списка `list`. Однако должны присутствовать операции добавления и удаления. Следовательно, операции должны возвращать новый список с добавленным или удаленным элементом.  

Ниже представлена функция добавления элемента по индексу. 
```c#
    private bool wait = false;

    public T[] InsertAndGetArray(T item, int index) //добавление на заданный индекс
    {
        while (wait) { }
        wait = true;

        //если список пустой, просто добавляем в него элемент и возвращаем
        if (list.Length == 0 && index == 0)
        {
            wait = false;
            return new T[1] { item };
        }
        //проверка правильности индекса
        if (index < 0 || index > list.Length)
        {
            Console.WriteLine("Индекс за пределами списка");
            wait = false;
            return list;
        }

        T[] result = new T[list.Length + 1];

        if (index == list.Length) // добавление в самый конец 
        {
            for (int i = 0; i < list.Length; i++)
            {
                result[i] = list[i];
            }

            result[list.Length] = item;
            wait = false;
            return result;
        }

        for (int i = 0; i < index; i++)
        {
            result[i] = list[i];
        }

        result[index] = item;

        for (int i = index; i < list.Length; i++)
        {
            result[i + 1] = list[i];
        }

        wait = false;
        return result;
    }
```
Ниже представлена функция для удаления элемента из списка по индексу.
```c#
    public T[] DeleteAndGetArray(int index) // удаление элемента на заданном индексе
    {
        while (wait) { }
        wait = true;

        //если список пуст, ничего не происходит
        if (list.Length == 0)
        {
            Console.WriteLine("Список пуст");
            wait = false;
            return list;
        }

        //проверка правильности индекса
        if (index < 0 || index >= list.Length)
        {
            Console.WriteLine("Индекс за пределами списка");
            wait = false;
            return list;
        }

        //удаление элемента
        T[] result = new T[list.Length - 1];

        for (int i = 0; i < index; i++)
        {
            result[i] = list[i];
        }

        for (int i = index + 1; i < list.Length; i++)
        {
            result[i - 1] = list[i];
        }

        wait = false;
        return result;
    }
```
Получение элемента по индексу. Количество элементов в списке 
```c#
    //получение элемента по позиции
    public T this[int index]
    {
        get
        {
            return list[index];
        }
    }

    //получение количества элементов
    public int Count
    {
        get
        {
            return list.Length;
        }
    }
}
```
Проверка реализации `Unchange`. 
```c#
class Program
{
    public static void Main(string[] args)
    {
        Unchange<int> primary_list = new Unchange<int>(new int[3] {1, 2, 3});

        //добавление элемента "4" на 1 позицию primary_list
        Unchange<int> newarr = new Unchange<int>(primary_list.InsertAndGetArray(4, 1));

        //вывод на экран
        for (int i = 0; i < newarr.Count; i++)
        {
            Console.WriteLine(newarr[i]);   //получение элемента по позиции
        }

        //удаление элемента "4" с 1 позиции newarr
        newarr = new Unchange<int>(newarr.DeleteAndGetArray(1));
        Console.WriteLine();

        //вывод на экран
        for (int i = 0; i < newarr.Count; i++)
        {
            Console.WriteLine(newarr[i]);   //получение элемента по позиции
        }
    }
}
```
Результат работы программы для исходных данных в Main
```
1
4
2
3

1
2
3
```
