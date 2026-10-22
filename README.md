# Strings

Учебный проект на C# (.NET Framework 4.7.2): реализация алгоритмов поиска подстроки
(наивный метод и алгоритм Кнута — Морриса — Пратта), графическое приложение
на Windows Forms для их сравнения и набор unit-тестов.

## Возможности

- **Наивный поиск**: первое вхождение подстроки.
- **Алгоритм КМП**: первое вхождение подстроки.
- **Улучшенный алгоритм КМП**: все вхождения подстроки за один проход.
- Поиск с заданного индекса во всех методах.
- Графический интерфейс:
  - ввод исходной строки (по умолчанию — фрагмент «Войны и мира») и подстроки;
  - выбор индекса, с которого начинается поиск («Откуда вести поиск»);
  - выбор метода поиска и опция «Учитывать регистр»;
  - вывод позиции (или списка всех позиций) и времени поиска в миллисекундах;
  - поиск выполняется в фоновом потоке и отменяется при изменении входных данных.

## Структура проекта

| Проект | Описание |
| --- | --- |
| `ClassLibraryStrings` | Библиотека классов: статический класс `Strings` с алгоритмами поиска |
| `WindowsFormsStrings` | Приложение Windows Forms «String operations» |
| `UnitTestStrings` | Unit-тесты на MSTest для всех методов поиска |

## API библиотеки

Все методы находятся в статическом классе `ClassLibraryStrings.Strings`.

| Метод | Результат |
| --- | --- |
| `int IndexOfAny_Primitive(string source, string pattern, int start)` | Индекс первого вхождения `pattern` в `source` начиная с `start`, или `-1` |
| `int IndexOf_KMP(string source, string pattern, int start)` | Индекс первого вхождения (алгоритм КМП), или `-1` |
| `List<int> IndexOfKMP_Best(string source, string pattern, int start)` | Список индексов всех вхождений (пустой, если вхождений нет) |
| `int[] PrefixFunction(string pattern)` | Префикс-функция строки (вспомогательный метод КМП) |

Пример:

```csharp
using System.Collections.Generic;
using ClassLibraryStrings;

string text = "Лениво дышит полдень мглистый, Лениво катится река";

int first = Strings.IndexOf_KMP(text, "Лениво", 0);          // 0
int next  = Strings.IndexOfAny_Primitive(text, "Лениво", 1); // 31
List<int> all = Strings.IndexOfKMP_Best(text, "Лениво", 0);  // [0, 31]
```

Подстрока `pattern` должна быть непустой.

### Сложность

- Наивный метод: O(n·m) в худшем случае.
- КМП: O(n + m), где n — длина исходной строки, m — длина подстроки.

Улучшенный КМП строит префикс-функцию для строки `pattern + '\0' + source`
и находит все позиции, где её значение равно длине подстроки.

## Требования

- Windows
- .NET Framework 4.7.2
- Visual Studio 2017 или новее (NuGet-пакеты MSTest восстанавливаются автоматически)

## Сборка и запуск

1. Откройте `Strings.sln` в Visual Studio.
2. Назначьте `WindowsFormsStrings` запускаемым проектом.
3. Нажмите **F5**.

Сборка из командной строки (Developer Command Prompt):

```
nuget restore Strings.sln
msbuild Strings.sln
```

## Тесты

Запустите тесты через **Test Explorer** в Visual Studio или из командной строки:

```
vstest.console.exe UnitTestStrings\bin\Debug\UnitTestStrings.dll
```
