# Методы последовательностей
 
## Обычные функции (вспоминаем)(29.09.2020)

<https://youtu.be/DV5uBNx4R1A>

 
```
function Sum(x, y: real) := x + y;

begin
  var (a, b) := (1, 2);
  sum(a, b).println;
  println(sum(10.2, 20 * pi));
  print(sum(a + b, sum(2 * a, b - a)));
end.
```
 
```
function CountElems(k: BigInteger): BigInteger := k * (k + 1) div 2;

begin
  var n := ReadlnBigInteger;
  CountElems(n).println;
end.
```
 
```
// Параметры по умолчанию
function Square(x: real := 1; y: real := 2) := x * y ;

begin
  var (a, b) := (3, 4);
  Square().print
end.
```
 
```
// Переменное количество параметров подпрограммы
function Sum(params a: array of integer): integer;
begin
  result := 0;
  foreach var x in a do
    result += x;
end;

begin
  sum(1, 2, 3, 2, 6, 7, 3, 5, 3, 6).print
end.
```
 
```
// ACMP: Мышка
begin
   var (W, H, R) := ReadInteger3;
   var d := 2 * R;
   print(if (d in 0..W) and (d in 0..H) then 'YES' else 'NO')
end. 
```

Д/з: С клавиатуры вводится последовательность целых чисел, ограниченная нулем (ноль не входит в последовательность). Получить и вывести квадраты нечетных чисел, не превышающих 15.


## Лямбды и последовательности (06.10.2020)

### Лямбды (вспоминаем)
```
uses GraphWPF;

//function f(x: real) := x * x;

begin
  DrawGraph(x → abs(sin(x)),-6, 6);  // ->  →
end. 
```
 
```
begin
  var a := ArrRandom;
  a.Println;
  a.OrderDescending.Println;
end. 
```
 
```
begin
  var a := Arr(('Петров', 11), ('Андреев', 11), ('Сидоров', 10), 
               ('Иванов', 11), ('Сергеев', 10), ('Петровский', 10 ));
  a.Println;   
  a.OrderBy(x -> x.Item2).ThenByDescending(x -> x.Item1).Println;
end. 
```


### Последовательности


```
//Sequence of (string, real);
begin
  var s := Seq(1, 2, 3);
  s.Println;
  s := 10 + s + 12;
  s.Println;
  s := SeqRandom(5);
  s.Println; // Ленивость
  s.Println; // Элементы последовательности не хранятся в памяти.
             // Хранится только правило получения элементов.
  var a := ReadSeqInteger(3).ToArray;
  a.PrintLn;
  a.Println;
end. 
```

```
begin
  //Перебор элементов последовательности
  var a := SeqRandom(5);
  foreach var x in a do
    x.print;   
  println;
  foreach var x in a do
    x.print;   
end.
```

```
// LINQ
begin //condition -- условие
  var a := ReadSeqIntegerWhile(x → x > 0);
  a.Println;
end.
```

```
begin
  var a := seq(2, 1, 4, -3, 0, 9);
  // Проецирование:  Select
  a.Select(x -> sqrt(x)).Println;
  a.Select((x, i) -> x + i).Println;
  // Фильтрация:  Where
  a.Where(x -> x > 0).Println;
  a.Where(x -> x.IsEven).Println;
  a.Where((x, i) -> i.IsOdd)
   .Println
   .Where(x -> x > 0)
   .Println
   .Select(x -> x * x)
   .Println;
end.
```

## Методы последовательностей №1 (13.10.2020)

<https://youtu.be/oL7h__B8NWM>

```
{С клавиатуры вводится последовательность целых чисел, 
ограниченная нулем (ноль не входит в последовательность). 
Получить и вывести квадраты нечётных чисел, не превышающих 15.}
begin
  var k := 0;
  repeat
    k := ReadInteger;
    if (k < 16) and k.IsEven and (k <> 0) then
      print(k * k);
  until k = 0;   
end.
```

То же методами последовательностей:

```
begin
    ReadSeqIntegerWhile(x -> x <> 0)
   .Where(x -> (x < 16) and x.IsEven)  // filter
   .Select(x -> x * x)  //map
   .Println;
end.
```

### Генераторы последовательностей

```
// Генераторы последовательностей
begin
  //var a := Range('а', 'я', 4).Println;
  //var a := SeqFill(5, 7).Println; 
  // var a := SeqRandomInteger(7, 1, 10).Println;
  var n := 5;
  //n.To(10).Print;
  //n.Times.Println; // [0; n - 1]
  //n.Range.Println; // [1; n]
  n.Downto(1).Println
end.
```

```
begin
  (-10).To(10)
    .Println
    .Where(a -> a.IsOdd)
    .Println
    .Select(t -> t * t)
    .Println('===');
end.
```

```
begin
  var a := PartitionPoints(1, 4, 13).Println;
end.
```

```
begin
  //var a := SeqGen(10, i -> i + 1).Print;
  //var a := SeqGen(10, i -> i * i, 5).Print;
  //var a := SeqGen(10, 0, x -> x + 2).Print;
  // Числа Фибоначчи
  //var a := SeqGen(15, 0, 1, (a, b) -> a + b).Print;
  //var a := SeqWhile(1, x -> x * 2, x -> x < 12000).Print;
  var a := SeqWhile(0, 1, (a, b) -> a + b, x -> x < 10000).Print; 
end.
```

## Методы последовательностей №2. Понятие исключения (20.10.2020)

```
var a: sequence of integer;
a.First   -- первый элемент последовательности
a.Last    -- последний элемент последовательности
```

Методы `First` и `Last` могут вызываться с параметром-предикатом, определяющим условие отбора для элементов:

```
a.First(x -> x.IsEven); // найти первый чётный элемент в а
a.Last(x -> x > 10); // найти последний элемент в а, больший 10
```

Если последовательность пуста, может возникнуть **исключение** (см. ниже). Тогда надо пользоваться методами:

```
a.FirstOrDefault  -- первый элемент, или значение по умолчанию, если последовательность пуста
a.LastOrDefault  -- последний элемент, или значение по умолчанию, если последовательность пуста
```

Как вообще может возникнуть пустая последовательность? Например, после фильтрации элементов, если ни один не прошёл фильтр.

```
var a: sequence of integer;
a.Count   -- количество элементов (длина) последовательности
a.Sum     -- сумма элементов последовательности
```

Метод `Count` может вызываться с параметром-предикатом, определяющим условие отбора для подсчитываемых элементов:

```
a.Count(x -> x > 0); // подсчитать количество положительных элементов в а
```

Метод `Sum` может вызываться с параметром-селектором, определяющим действие, применяемое к каждому элементу перед суммированием:

```
a.Sum(x -> x * x); // найти сумму квадратов элементов последовательности
```

### Понятие об исключениях

Когда во время выполнения программы происходит ошибка, генерируется так называемое исключение, которое можно перехватить и обработать. Исключение представляет собой объект класса, производного от класса Exception, создающийся при возникновении исключительной ситуации.

Если исключение не обработать, то программа завершится с ошибкой. Для обработки исключений используется оператор `try ... except`. 


```
begin
  var a: integer;
  try
    a := ReadInteger;
	print(2 * a);
  except
    print('Error');
  end;
 
end.
```


```

```

