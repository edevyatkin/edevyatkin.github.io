---
layout: post
title: "Anvent of Code 2023. День 1"
date: 2024-01-14 18:00 +0300
categories: advent-of-code
published: true
---
## Anvent of Code 2023. День 1

[Условие задачи](https://adventofcode.com/2023/day/1)

В первый день нас вводят в курс дела: какие-то проблемы возникли с производством снега и поэтому эльфам нужна наша помощь. Нас хотят отправить на облака с помощью требушета и для этого требуются некие калибровочные значения в виде целых чисел. Эти числа нужно сложить, что и будет ответом на задачу. 

На вход подаётся набор строк, состоящий из латинских букв и цифр.

### Часть 1
В первой части для восстановления калибровочного значения необходимо взять первую и последнюю цифру в каждой строке из входа.

Для этого запустим два цикла: с начала строки и с конца строки. Если нам попадается цифра - цикл прерывается. Получившиеся две цифры объединяем в число и возвращаем результат.

### Часть 2
Во второй части усложнение в том, что число может быть "написано" в виде подстроки по-английски: "one", "two" и так далее.

Общий подход для решения обоих частей одинаков.

Здесь функция GetNum реализует описанный выше алгоритм.

```csharp
private static int GetNum(string line, bool isPartTwo)
{
    var num = 0;

    for (var i = 0; i < line.Length; i++)
    {
        if (TryGetNum(line, i, isPartTwo, out var n))
        {
            num = n;
            break;
        }
    }

    for (var i = line.Length - 1; i >= 0; i--)
    {
        if (TryGetNum(line, i, isPartTwo, out var n))
        {
            num = num * 10 + n;
            break;
        }
    }

    return num;
}
```

Функция TryGetNum, начиная с определенного символа в строке, пробует получить либо число (а точнее цифру), либо найти одно из слов по-английски, означающих какое-либо число.

```csharp
private static bool TryGetNum(string line, int i, bool isPartTwo, out int num)
{
    var numDict = new Dictionary<string, int> {
        ["zero"] = 0,
        ["one"] = 1,
        ["two"] = 2,
        ["three"] = 3,
        ["four"] = 4,
        ["five"] = 5,
        ["six"] = 6,
        ["seven"] = 7,
        ["eight"] = 8,
        ["nine"] = 9,
    };
    
    if (char.IsDigit(line[i]))
    {
        num = line[i] - '0';
        return true;
    }
    if (isPartTwo)
    {
        foreach (var key in numDict.Keys)
        {
            if (i + key.Length <= line.Length && line[i..(i + key.Length)] == key)
            {
                num = numDict[key];
                return true;
            }
        }
    }

    num = -1;
    return false;
}
```

[Полное решение на C#](https://github.com/edevyatkin/AdventOfCode/blob/master/AdventOfCode2023/Day1.cs)
