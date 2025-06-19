# Введение в информационные технологии

### Часть 1. Базовые конструкции языка С++

#### 1. Структура программы на С++, функция main(…), директивы препроцессора и комментарии. Многофайловые приложения. Заголовочные файлы. Недопущение повторного включения.

**Определение**: Программа на С++ состоит из исходных файлов, содержащих функции, классы, переменные и другие элементы. Основной точкой входа является функция `main()`. Программа обрабатывается препроцессором, компилятором, компоновщиком и выполняется.

- **Функция main()**: Точка входа программы. Возвращает `int` (код завершения: 0 — успех, ненулевое значение — ошибка). Может принимать аргументы для обработки параметров командной строки.
  ```cpp
  int main() {
      return 0; // Успешное завершение
  }
  int main(int argc, char* argv[]) { // argc — кол-во аргументов, argv — массив строк
      for (int i = 0; i < argc; ++i) {
          std::cout << argv[i] << std::endl;
      }
      return 0;
  }
  ```

- **Директивы препроцессора**: Инструкции, начинающиеся с `#`, выполняются до компиляции. Примеры:
  - `#include <iostream>`: Подключает библиотеку.
  - `#define PI 3.14`: Определяет макрос.
  - `#ifdef DEBUG ... #endif`: Условная компиляция.
  ```cpp
  #include <iostream>
  #define SQUARE(x) ((x) * (x))
  int main() {
      std::cout << SQUARE(5) << std::endl; // 25
      return 0;
  }
  ```

- **Комментарии**: Пояснения в коде, игнорируются компилятором.
  - Однострочные: `// Комментарий`.
  - Многострочные: `/* Комментарий */`.
  ```cpp
  // Это однострочный комментарий
  /* Это
     многострочный
     комментарий */
  ```

- **Многофайловые приложения**: Программа делится на файлы `.cpp` (реализация) и `.h` (заголовки). Заголовки содержат объявления функций, классов, констант.
  - Пример структуры:
    - `main.cpp`: Содержит `main()`.
    - `utils.h`: Объявления функций.
    - `utils.cpp`: Реализация функций.
  ```cpp
  // utils.h
  #ifndef UTILS_H
  #define UTILS_H
  int add(int a, int b);
  #endif

  // utils.cpp
  #include "utils.h"
  int add(int a, int b) { return a + b; }

  // main.cpp
  #include <iostream>
  #include "utils.h"
  int main() {
      std::cout << add(2, 3) << std::endl; // 5
      return 0;
  }
  ```

- **Недопущение повторного включения**: Защищает от многократного включения заголовочного файла.
  - Используется `#ifndef`, `#define`, `#endif` или `#pragma once`.
  ```cpp
  // utils.h с защитой
  #ifndef UTILS_H
  #define UTILS_H
  // Код заголовка
  #endif

  // Или с #pragma once
  #pragma once
  // Код заголовка
  ```

#### 2. Область видимости и время жизни переменных. Классификатор extern. Области памяти программы.

**Определение**: Область видимости определяет, где переменная доступна. Время жизни — период существования переменной в памяти.

- **Область видимости**:
  - **Локальная**: Внутри блока `{}` (например, в функции).
  - **Глобальная**: Вне всех функций, доступна во всем файле.
  - **Файловая**: С `static` в файле, видима только в этом файле.
  - **Пространство имен**: Внутри `namespace`.
  ```cpp
  int global = 10; // Глобальная
  namespace ns {
      int ns_var = 20; // Пространство имен
  }
  static int file_var = 30; // Файловая
  int main() {
      int local = 40; // Локальная
      std::cout << global << " " << ns::ns_var << " " << file_var << " " << local << std::endl;
      return 0;
  }
  ```

- **Время жизни**:
  - **Автоматические**: Существуют в блоке, уничтожаются при выходе.
  - **Статические**: Существуют весь период работы программы.
  - **Динамические**: Создаются с `new`, уничтожаются с `delete`.
  ```cpp
  void func() {
      static int count = 0; // Статическая, сохраняет значение
      ++count;
      std::cout << count << std::endl;
  }
  int main() {
      func(); // 1
      func(); // 2
      int* ptr = new int(5); // Динамическая
      delete ptr; // Освобождение
      return 0;
  }
  ```

- **Классификатор extern**: Объявляет переменную, определенную в другом файле, без выделения памяти.
  ```cpp
  // file1.cpp
  int shared = 100;

  // file2.cpp
  extern int shared; // Объявление
  int main() {
      std::cout << shared << std::endl; // 100
      return 0;
  }
  ```

- **Области памяти**:
  - **Стек**: Локальные переменные, параметры функций, быстрый доступ, ограниченный размер.
  - **Куча**: Динамические объекты, управляется вручную.
  - **Сегмент данных**: Глобальные и статические переменные (инициализированные и неинициализированные).
  - **Сегмент кода**: Машинный код программы.
  ```cpp
  int global = 1; // Сегмент данных
  int main() {
      int local = 2; // Стек
      int* heap = new int(3); // Куча
      delete heap;
      return 0;
  }
  ```

#### 3. Приведение простых типов. Размер типов. Суффиксы литералов. Литералы интегральных типов. Функция sizeof().

**Определение**: Приведение типов изменяет интерпретацию данных. Размер типов зависит от платформы. Литералы задают значения с явным типом.

- **Приведение типов**:
  - **Явное**: Используется `(тип)` или `static_cast<тип>`.
  - **Неявное**: Выполняется автоматически при совместимых типах.
  ```cpp
  int main() {
      double d = 3.14;
      int i = (int)d; // Явное, i = 3
      int j = d; // Неявное, j = 3
      std::cout << i << " " << j << std::endl;
      return 0;
  }
  ```

- **Размер типов**: Зависит от архитектуры (обычно 32/64 бит).
  - `char`: 1 байт.
  - `int`: 4 байта.
  - `double`: 8 байт.
  - `long long`: 8 байт.
  ```cpp
  int main() {
      std::cout << sizeof(char) << " " << sizeof(int) << " " << sizeof(double) << std::endl;
      return 0;
  }
  ```

- **Суффиксы литералов**: Указывают тип константы.
  - `u`: `unsigned int` (например, `123u`).
  - `l`: `long` (например, `123l`).
  - `f`: `float` (например, `3.14f`).
  ```cpp
  int main() {
      unsigned int u = 123u;
      long l = 123l;
      float f = 3.14f;
      std::cout << u << " " << l << " " << f << std::endl;
      return 0;
  }
  ```

- **Литералы интегральных типов**:
  - Десятичные: `123`.
  - Восьмеричные: `0123` (начинаются с 0).
  - Шестнадцатеричные: `0x7B` (начинаются с `0x`).
  - Двоичные (C++14): `0b1111011` (начинаются с `0b`).
  ```cpp
  int main() {
      int dec = 123;
      int oct = 0123; // 83 в десятичной
      int hex = 0x7B; // 123 в десятичной
      int bin = 0b1111011; // 123 в десятичной
      std::cout << dec << " " << oct << " " << hex << " " << bin << std::endl;
      return 0;
  }
  ```

- **sizeof()**: Возвращает размер типа или объекта в байтах.
  ```cpp
  int main() {
      int x = 5;
      std::cout << sizeof(x) << " " << sizeof(int) << std::endl; // 4 4
      return 0;
  }
  ```

#### 4. Объявление нового типа. Структуры. Особенности хранения в памяти.

**Определение**: Новый тип создается для удобства работы с данными. Структуры объединяют данные в единый объект.

- **Объявление типа**:
  - `typedef`: Создает псевдоним типа.
  - `using`: Современный способ (C++11).
  ```cpp
  typedef unsigned int uint;
  using uint = unsigned int;
  int main() {
      uint x = 5;
      std::cout << x << std::endl;
      return 0;
  }
  ```

- **Структуры**: Определяются с `struct`, содержат поля (переменные).
  ```cpp
  struct Point {
      int x, y;
  };
  int main() {
      Point p = {1, 2};
      std::cout << p.x << " " << p.y << std::endl;
      return 0;
  }
  ```

- **Хранение в памяти**:
  - Поля структуры располагаются последовательно.
  - **Выравнивание памяти (padding)**: Компилятор добавляет "пустые" байты для оптимизации доступа.
  ```cpp
  struct Example {
      char c; // 1 байт
      int i;  // 4 байта, но из-за выравнивания добавляется 3 байта padding
  };
  int main() {
      std::cout << sizeof(Example) << std::endl; // 8 (1 + 3 + 4)
      return 0;
  }
  ```

#### 5. Базовые типы данных. Объявление переменных. Классификатор const. Операции.

**Определение**: Базовые типы — фундаментальные типы данных для хранения значений. Переменные объявляются с указанием типа. Операции выполняют вычисления.

- **Базовые типы**:
  - **Целые**: `int`, `short`, `long`, `long long`, `unsigned` модификаторы.
  - **Вещественные**: `float`, `double`, `long double`.
  - **Символьные**: `char`, `wchar_t`.
  - **Логический**: `bool`.
  ```cpp
  int main() {
      int i = 5;
      double d = 3.14;
      char c = 'A';
      bool b = true;
      std::cout << i << " " << d << " " << c << " " << b << std::endl;
      return 0;
  }
  ```

- **Объявление переменных**: `тип имя = значение;`.
  ```cpp
  int x = 10;
  double y = 2.5;
  ```

- **const**: Запрещает изменение переменной.
  ```cpp
  const int MAX = 100;
  // MAX = 200; // Ошибка
  ```

- **Операции**:
  - **Арифметические**: `+`, `-`, `*`, `/`, `%`.
  - **Логические**: `&&`, `||`, `!`.
  - **Битовые**: `&`, `|`, `^`, `~`, `<<`, `>>`.
  ```cpp
  int main() {
      int a = 5, b = 3;
      std::cout << a + b << " " << (a && b) << " " << (a & b) << std::endl; // 8 1 1
      return 0;
  }
  ```

#### 6. Операторы условного перехода. Перечислимый тип.

**Определение**: Условные операторы управляют выполнением кода на основе условий. Перечислимый тип задает набор именованных констант.

- **Условные операторы**:
  - `if`: Проверяет условие.
  - `switch`: Выбирает ветку по значению.
  - Тернарный оператор: `условие ? выражение1 : выражение2`.
  ```cpp
  int main() {
      int x = 5;
      if (x > 0) {
          std::cout << "Positive" << std::endl;
      } else {
          std::cout << "Non-positive" << std::endl;
      }
      switch (x) {
          case 5: std::cout << "Five" << std::endl; break;
          default: std::cout << "Other" << std::endl;
      }
      std::cout << (x > 0 ? "Positive" : "Non-positive") << std::endl;
      return 0;
  }
  ```

- **Перечислимый тип**:
  - `enum`: Простой перечислитель.
  - `enum class`: Типобезопасный (C++11).
  ```cpp
  enum Color { RED, GREEN, BLUE };
  enum class Status : int { OK = 1, ERROR = -1 };
  int main() {
      Color c = RED;
      Status s = Status::OK;
      std::cout << c << " " << static_cast<int>(s) << std::endl; // 0 1
      return 0;
  }
  ```

#### 7. Организация циклов.

**Определение**: Циклы повторяют выполнение кода до выполнения условия.

- **Типы циклов**:
  - `for`: Для известного числа итераций.
  - `while`: Пока условие истинно.
  - `do-while`: Выполняется хотя бы раз.
  ```cpp
  int main() {
      for (int i = 0; i < 3; ++i) {
          std::cout << i << " ";
      } // 0 1 2
      int j = 0;
      while (j < 3) {
          std::cout << j << " ";
          ++j;
      } // 0 1 2
      int k = 0;
      do {
          std::cout << k << " ";
          ++k;
      } while (k < 3); // 0 1 2
      std::cout << std::endl;
      return 0;
  }
  ```

- **Управление**:
  - `break`: Выход из цикла.
  - `continue`: Переход к следующей итерации.
  ```cpp
  int main() {
      for (int i = 0; i < 5; ++i) {
          if (i == 2) continue;
          if (i == 4) break;
          std::cout << i << " ";
      } // 0 1 3
      std::cout << std::endl;
      return 0;
  }
  ```

#### 8. Стек и куча. Динамическая память. Операторы «.» и «->».

**Определение**: Стек и куча — области памяти для хранения данных. Динамическая память управляется вручную.

- **Стек**: Хранит локальные переменные, параметры функций. Быстрый, ограниченный размер.
- **Куча**: Хранит динамически выделенные объекты. Медленнее, больший объем.
  ```cpp
  int main() {
      int stack_var = 5; // Стек
      int* heap_var = new int(10); // Куча
      std::cout << stack_var << " " << *heap_var << std::endl;
      delete heap_var; // Освобождение
      return 0;
  }
  ```

- **Операторы**:
  - `.`: Доступ к членам объекта.
  - `->`: Доступ к членам через указатель.
  ```cpp
  struct Point {
      int x, y;
  };
  int main() {
      Point p = {1, 2};
      Point* ptr = &p;
      std::cout << p.x << " " << ptr->x << std::endl; // 1 1
      return 0;
  }
  ```

#### 9. Указатели и ссылочные переменные. Особенности работы. Классификатор const.

**Определение**: Указатели хранят адрес памяти, ссылки — псевдонимы переменных.

- **Указатели**:
  - Хранят адрес: `тип* имя = &переменная;`.
  - Разыменование: `*указатель`.
  - Арифметика: `ptr + n` смещает на `n * sizeof(тип)` байт.
  ```cpp
  int main() {
      int x = 5;
      int* ptr = &x;
      *ptr = 10;
      std::cout << x << " " << *ptr << std::endl; // 10 10
      return 0;
  }
  ```

- **Ссылки**:
  - Псевдоним: `тип& имя = переменная;`.
  - Нельзя переназначить, не занимают отдельной памяти.
  ```cpp
  int main() {
      int x = 5;
      int& ref = x;
      ref = 10;
      std::cout << x << " " << ref << std::endl; // 10 10
      return 0;
  }
  ```

- **const**:
  - `const int*`: Указатель на константу.
  - `int* const`: Константный указатель.
  - `const int&`: Ссылка на константу.
  ```cpp
  int main() {
      const int x = 5;
      const int* ptr = &x; // Указатель на константу
      // *ptr = 10; // Ошибка
      int y = 10;
      int* const ptr2 = &y; // Константный указатель
      *ptr2 = 15; // OK
      // ptr2 = &x; // Ошибка
      const int& ref = x; // Ссылка на константу
      std::cout << *ptr << " " << *ptr2 << " " << ref << std::endl;
      return 0;
  }
  ```

#### 10. Статические массивы. Объявление, инициализация, хранение, обращение, передача. Две схемы хранения матриц.

**Определение**: Статические массивы — фиксированные по размеру наборы данных одного типа.

- **Объявление и инициализация**:
  ```cpp
  int main() {
      int arr[5] = {1, 2, 3, 4, 5};
      int arr2[5] = {0}; // Все элементы 0
      std::cout << arr[0] << " " << arr2[0] << std::endl;
      return 0;
  }
  ```

- **Хранение**: Непрерывный участок памяти.
- **Обращение**: Через индекс `arr[i]`.
- **Передача**: Массив преобразуется в указатель на первый элемент.
  ```cpp
  void printArray(int arr[], int size) {
      for (int i = 0; i < size; ++i) {
          std::cout << arr[i] << " ";
      }
  }
  int main() {
      int arr[3] = {1, 2, 3};
      printArray(arr, 3); // 1 2 3
      std::cout << std::endl;
      return 0;
  }
  ```

- **Схемы хранения матриц в динамической памяти**:
  1. **Массив указателей**:
     ```cpp
     int** matrix = new int*[rows];
     for (int i = 0; i < rows; ++i) {
         matrix[i] = new int[cols];
     }
     // Освобождение
     for (int i = 0; i < rows; ++i) {
         delete[] matrix[i];
     }
     delete[] matrix;
     ```
  2. **Непрерывный массив**:
     ```cpp
     int* matrix = new int[rows * cols];
     // Доступ: matrix[i * cols + j]
     delete[] matrix;
     ```

#### 11. Адресная арифметика. Приведение типов указателей. Тип void*.

**Определение**: Адресная арифметика позволяет манипулировать адресами памяти. `void*` — универсальный указатель.

- **Адресная арифметика**: `ptr + n` смещает указатель на `n * sizeof(тип)` байт.
  ```cpp
  int main() {
      int arr[3] = {1, 2, 3};
      int* ptr = arr;
      std::cout << *(ptr + 1) << std::endl; // 2
      return 0;
  }
  ```

- **Приведение типов**:
  ```cpp
  int main() {
      int x = 5;
      void* vptr = &x;
      int* iptr = static_cast<int*>(vptr);
      std::cout << *iptr << std::endl; // 5
      return 0;
  }
  ```

- **void***: Хранит адрес без типа, требует приведения перед использованием.
  ```cpp
  int main() {
      int x = 5;
      void* ptr = &x;
      // std::cout << *ptr; // Ошибка
      std::cout << *static_cast<int*>(ptr) << std::endl; // 5
      return 0;
  }
  ```

#### 12. Статические и динамические массивы. Строки в стиле С.

**Определение**: Массивы бывают фиксированного (статические) и изменяемого (динамические) размера. Строки в стиле С — массивы `char` с завершающим `\0`.

- **Статические массивы**:
  ```cpp
  char str[10] = "Hello";
  ```

- **Динамические массивы**:
  ```cpp
  char* str = new char[10];
  strcpy(str, "Hello");
  delete[] str;
  ```

- **Строки C-style**:
  ```cpp
  int main() {
      char str[] = "Hello";
      std::cout << str << " " << strlen(str) << std::endl; // Hello 5
      return 0;
  }
  ```

- **sizeof()**: Для массива — размер массива, для указателя — размер указателя.
  ```cpp
  int main() {
      char str[] = "Hello";
      char* ptr = str;
      std::cout << sizeof(str) << " " << sizeof(ptr) << std::endl; // 6 8
      return 0;
  }
  ```

#### 13. Прототипы и описание функций. Рекурсия. Стек вызова. Имя функции как указатель.

**Определение**: Функции выполняют задачи, имеют прототипы (объявления) и определения. Рекурсия — вызов функции из самой себя.

- **Прототип и определение**:
  ```cpp
  int add(int a, int b); // Прототип
  int main() {
      std::cout << add(2, 3) << std::endl; // 5
      return 0;
  }
  int add(int a, int b) { // Определение
      return a + b;
  }
  ```

- **Рекурсия**:
  ```cpp
  int factorial(int n) {
      if (n <= 1) return 1;
      return n * factorial(n - 1);
  }
  int main() {
      std::cout << factorial(5) << std::endl; // 120
      return 0;
  }
  ```

- **Стек вызова**: Хранит кадры активации (аргументы, локальные переменные, адрес возврата).
- **Имя функции**: Указатель на функцию.
  ```cpp
  int main() {
      int (*func)(int, int) = add;
      std::cout << func(2, 3) << std::endl; // 5
      return 0;
  }
  ```

#### 14. Способы передачи аргументов. rvalue и lvalue. Ссылки. Продление жизни.

**Определение**: Аргументы передаются по значению, указателю или ссылке. `lvalue` — объект с адресом, `rvalue` — временный объект.

- **Передача**:
  - **По значению**: Копия.
  - **По указателю**: Адрес.
  - **По ссылке**: Псевдоним.
  ```cpp
  void byValue(int x) { x = 10; }
  void byPointer(int* x) { *x = 10; }
  void byReference(int& x) { x = 10; }
  int main() {
      int a = 5, b = 5, c = 5;
      byValue(a);
      byPointer(&b);
      byReference(c);
      std::cout << a << " " << b << " " << c << std::endl; // 5 10 10
      return 0;
  }
  ```

- **lvalue и rvalue**:
  - `lvalue`: Переменная (`int x;`).
  - `rvalue`: Временный объект (`5`, `func()`).
  ```cpp
  int main() {
      int x = 5; // x — lvalue, 5 — rvalue
      x = 10; // OK
      // 5 = 10; // Ошибка
      return 0;
  }
  ```

- **Ссылки**:
  - Левосторонние (`&`) — для `lvalue`.
  - Правосторонние (`&&`) — для `rvalue`.
  - `const T&` продлевает жизнь `rvalue`.
  ```cpp
  int main() {
      const int& ref = 5; // Продление жизни rvalue
      std::cout << ref << std::endl; // 5
      return 0;
  }
  ```

#### 15. Функции с аргументами по умолчанию.

**Определение**: Аргументы по умолчанию задаются в протоколе функции.

- **Пример**:
  ```cpp
  void print(int x, int y = 0) {
      std::cout << x << " " << y << std::endl;
  }
  int main() {
      print(5); // 5 0
      print(5, 10); // 5 10
      return 0;
  }
  ```

- **Ограничения**:
  - Аргументы по умолчанию задаются справа налево.
  - Нельзя задавать в определении и прототипе одновременно.
  ```cpp
    // utils.h
    void func(int x, int y = 0;);
    // utils.cpp
   void func(int x, int y) { … } // Ошибка, если y имеет default в определении```


#### 16. Перегрузка функций.

**Определение**: Функции с одинаковыми именами, но разными параметрами.

- **Пример**:
  ```cpp
  int add(int a, int b) { return a + b; }
  double add(double a, double b) { return a + b; }
  int main() {
      std::cout << add(2, 3) << " " << add(2.5, 3.5) << std::endl; // 5 6
      return 0;
  }
  ```

- **Ограничения**:
  - Нельзя различать по возвращаемому типу.
  - Нельзя перегружать по `const` для аргументов по значению.

#### 17. Автоматический вывод типа: auto, decltype.

**Определение**: `auto` и `decltype` упрощают работу с типами.

- **auto**: Компилятор выводит тип.
- **decltype**: Определяет тип выражения.
  ```cpp
  int main() {
      auto x = 5; // int
      auto y = 3.14; // double
      decltype(x) z = 10; // int
      std::cout << x << " " << z << std::endl; // 5 10
      return 0;
  }
  ```

- **Синонимы типов**:
  ```cpp
  using uint = unsigned int;
  ```

#### 18. Обработка ошибок.

**Определение**: Ошибки обрабатываются кодами или исключениями.

- **Коды ошибок**:
  ```cpp
  int divide(int a, int b, int* result) {
      if (b == 0) return -1;
      *result = a / b;
      return 0;
  }
  int main() {
      int res;
      if (divide(10, 2, &res) == 0) {
          std::cout << res << std::endl; // 5
      } else {
          std::cerr << "Ошибка" << std::endl;
      }
      return 0;
  }
  ```

- **Исключения**:
  ```cpp
  int main() {
      try {
          throw std::runtime_error("Ошибка");
      } catch (const std::exception& e) {
          std::cerr << e.what() << std::endl;
      }
      return 0;
  }
  ```

---
### Часть 2. Введение в объектно-ориентированное программирование и смежные вопросы

Ниже приведены развернутые пояснения для каждого пункта второй части с подробными определениями, примерами кода, объяснением концепций и дополнительными деталями для полного понимания. Каждый пункт включает теоретическую основу, практическое применение и важные аспекты реализации в С++.

---

#### 1. Смысл понятий «инкапсуляция», «наследование», «полиморфизм».

**Определение**:  
Эти три концепции являются основными принципами объектно-ориентированного программирования (ООП), которые позволяют создавать гибкие, модульные и масштабируемые программы.

- **Инкапсуляция**:  
  Инкапсуляция заключается в объединении данных (полей) и методов, работающих с этими данными, в одном классе, а также в ограничении прямого доступа к данным, предоставляя контролируемый интерфейс. Это защищает внутреннее состояние объекта от несанкционированного изменения и упрощает поддержку кода.

  **Цели инкапсуляции**:
  - Скрытие деталей реализации.
  - Обеспечение целостности данных через проверку в методах.
  - Упрощение интерфейса для пользователей класса.

  **Пример**:
  ```cpp
  #include <iostream>
  class BankAccount {
  private:
      double balance; // Скрытое поле
      std::string accountNumber;
  public:
      BankAccount(const std::string& number, double initialBalance) 
          : accountNumber(number), balance(initialBalance) {
          if (initialBalance < 0) balance = 0; // Проверка
      }
      void deposit(double amount) { // Контролируемый доступ
          if (amount > 0) balance += amount;
      }
      double getBalance() const { // Только чтение
          return balance;
      }
      std::string getAccountNumber() const {
          return accountNumber;
      }
  };

  int main() {
      BankAccount acc("12345", 1000.0);
      acc.deposit(500.0);
      std::cout << "Account: " << acc.getAccountNumber() 
                << ", Balance: " << acc.getBalance() << std::endl; // Account: 12345, Balance: 1500
      // acc.balance = -100; // Ошибка: balance private
      return 0;
  }
  ```
  **Пояснение**:  
  Поля `balance` и `accountNumber` скрыты (`private`), а доступ к ним предоставляется через методы `deposit` и `getBalance`, которые включают проверки. Это предотвращает некорректное использование объекта.

- **Наследование**:  
  Наследование позволяет создавать новый класс (производный) на основе существующего (базового), унаследовав его поля и методы. Это способствует повторному использованию кода и созданию иерархий классов.

  **Типы наследования в С++**:
  - `public`: Публичные и защищенные члены базового класса сохраняют свои модификаторы доступа.
  - `protected`: Публичные члены базового становятся защищенными.
  - `private`: Все члены базового становятся приватными.

  **Пример**:
  ```cpp
  #include <iostream>
  class Vehicle {
  protected:
      std::string brand;
      int speed;
  public:
      Vehicle(const std::string& b, int s) : brand(b), speed(s) {}
      void display() const {
          std::cout << "Brand: " << brand << ", Speed: " << speed << std::endl;
      }
  };

  class Car : public Vehicle {
  private:
      int doors;
  public:
      Car(const std::string& b, int s, int d) : Vehicle(b, s), doors(d) {}
      void display() const {
          Vehicle::display();
          std::cout << "Doors: " << doors << std::endl;
      }
  };

  int main() {
      Car car("Toyota", 200, 4);
      car.display(); // Brand: Toyota, Speed: 200, Doors: 4
      return 0;
  }
  ```
  **Пояснение**:  
  Класс `Car` наследует `Vehicle`, добавляя поле `doors`. Метод `display` переопределяется, но базовая версия вызывается через `Vehicle::display`. Наследование позволяет расширить функциональность без дублирования кода.

- **Полиморфизм**:  
  Полиморфизм позволяет объектам разных типов обрабатываться через общий интерфейс, обеспечивая разное поведение в зависимости от типа объекта. В С++ различают:
  - **Динамический полиморфизм**: Реализуется через виртуальные функции и наследование.
  - **Статический полиморфизм**: Реализуется через перегрузку функций/операторов и шаблоны.

  **Пример динамического полиморфизма**:
  ```cpp
  #include <iostream>
  class Shape {
  public:
      virtual double area() const = 0; // Чисто виртуальная функция
      virtual ~Shape() = default; // Виртуальный деструктор
  };

  class Circle : public Shape {
  private:
      double radius;
  public:
      Circle(double r) : radius(r) {}
      double area() const override {
          return 3.14159 * radius * radius;
      }
  };

  class Rectangle : public Shape {
  private:
      double width, height;
  public:
      Rectangle(double w, double h) : width(w), height(h) {}
      double area() const override {
          return width * height;
      }
  };

  int main() {
      Shape* shapes[] = { new Circle(5.0), new Rectangle(4.0, 6.0) };
      for (const Shape* s : shapes) {
          std::cout << "Area: " << s->area() << std::endl;
          delete s;
      }
      return 0;
  }
  ```
  **Вывод**:
  ```
  Area: 78.5397
  Area: 24
  ```
  **Пояснение**:  
  Указатели на `Shape` вызывают правильные реализации `area` для `Circle` и `Rectangle` благодаря виртуальным функциям. Это позволяет работать с разными типами через единый интерфейс.

  **Пример статического полиморфизма**:
  ```cpp
  #include <iostream>
  template<typename T>
  T max(T a, T b) { // Шаблон
      return a > b ? a : b;
  }

  int max(int a, int b) { // Перегрузка
      std::cout << "Int version\n";
      return a > b ? a : b;
  }

  int main() {
      std::cout << max(5, 3) << std::endl; // Int version, 5
      std::cout << max(5.5, 3.3) << std::endl; // 5.5
      return 0;
  }
  ```
  **Пояснение**:  
  Статический полиморфизм определяется на этапе компиляции, что делает его более эффективным, но менее гибким, чем динамический.

---

#### 2. Шаблонные функции. Определение, явная специализация. Особенности компиляции.

**Определение**:  
Шаблонная функция — это функция, параметризованная типами или значениями, позволяющая писать обобщенный код, который работает с разными типами данных. Шаблоны повышают переиспользуемость кода и типобезопасность.

**Компоненты шаблонной функции**:
- Ключевое слово `template`.
- Параметры шаблона: типы (`typename T`) или значения (`int N`).
- Тело функции, использующее параметры шаблона.

**Пример**:
```cpp
#include <iostream>
template<typename T>
T max(T a, T b) {
    return a > b ? a : b;
}

int main() {
    std::cout << max(5, 3) << std::endl; // 5
    std::cout << max(5.5, 3.3) << std::endl; // 5.5
    std::cout << max('a', 'b') << std::endl; // b
    return 0;
}
```
**Пояснение**:  
Компилятор генерирует версии функции для каждого используемого типа (`int`, `double`, `char`) при инстанцировании.

**Явная специализация**:  
Специализация позволяет определить конкретную реализацию шаблонной функции для определенного типа, переопределяя общий шаблон.

**Пример специализации**:
```cpp
#include <iostream>
#include <cstring>
template<typename T>
T max(T a, T b) {
    return a > b ? a : b;
}

// Явная специализация для const char*
template<>
const char* max<const char*>(const char* a, const char* b) {
    return strcmp(a, b) > 0 ? a : b;
}

int main() {
    std::cout << max(5, 3) << std::endl; // 5
    std::cout << max("apple", "banana") << std::endl; // banana
    return 0;
}
```
**Пояснение**:  
Для `const char*` используется специальная реализация с `strcmp`, так как общий шаблон не подходит для строк.

**Особенности компиляции**:
- **Инстанцирование**: Код шаблона не компилируется, пока не используется с конкретным типом. Компилятор создает экземпляр функции для каждого типа.
- **Расположение кода**: Определение шаблонной функции обычно помещается в заголовочный файл, так как компилятор должен видеть полный код при инстанцировании.
- **Ошибки компиляции**: Ошибки в шаблонах выявляются только при инстанцировании, что может усложнить отладку.
- **Code bloat**: Многократное инстанцирование для разных типов увеличивает размер исполняемого файла.

**Пример ошибки**:
```cpp
template<typename T>
void func(T x) {
    x.someMethod(); // Ошибка, если T не имеет someMethod
}
```
Ошибка возникнет только при вызове, например, `func<int>(5)`.

**Пояснение**:  
Шаблонные функции — мощный инструмент для обобщенного программирования, но требуют осторожности из-за особенностей компиляции и потенциального увеличения размера кода.

---

#### 3. Шаблон класса. Шаблонные функции внутри шаблонного класса. Особенности компиляции.

**Определение**:  
Шаблон класса — это класс, параметризованный типами или значениями, позволяющий создавать обобщенные структуры данных. Шаблонные функции внутри таких классов также могут быть параметризованы.

**Компоненты шаблона класса**:
- Объявление шаблона: `template<typename T>`.
- Поля и методы, использующие параметры шаблона.

**Пример шаблона класса**:
```cpp
#include <iostream>
template<typename T>
class Box {
private:
    T value;
public:
    Box(T v) : value(v) {}
    void set(T v) { value = v; }
    T get() const { return value; }
};

int main() {
    Box<int> intBox(42);
    Box<double> doubleBox(3.14);
    std::cout << intBox.get() << " " << doubleBox.get() << std::endl; // 42 3.14
    return 0;
}
```
**Пояснение**:  
Класс `Box` создается для каждого типа (`int`, `double`) при инстанцировании.

**Шаблонные функции внутри шаблонного класса**:
Шаблонный класс может содержать шаблонные методы, которые имеют собственные параметры шаблона.

**Пример**:
```cpp
#include <iostream>
template<typename T>
class Container {
private:
    T data;
public:
    Container(T d) : data(d) {}
    // Шаблонный метод
    template<typename U>
    void combine(U other) {
        std::cout << data << " + " << other << std::endl;
    }
};

int main() {
    Container<int> c(10);
    c.combine(5.5); // 10 + 5.5
    c.combine("text"); // 10 + text
    return 0;
}
```
**Пояснение**:  
Метод `combine` принимает любой тип `U`, что делает класс более гибким.

**Явная специализация класса**:
Аналогично функциям, класс можно специализировать для конкретного типа.

**Пример**:
```cpp
#include <iostream>
template<typename T>
class Storage {
public:
    void store(T x) { std::cout << "Generic: " << x << std::endl; }
};

// Специализация для const char*
template<>
class Storage<const char*> {
public:
    void store(const char* x) { std::cout << "String: " << x << std::endl; }
};

int main() {
    Storage<int> intStore;
    Storage<const char*> strStore;
    intStore.store(42); // Generic: 42
    strStore.store("Hello"); // String: Hello
    return 0;
}
```

**Особенности компиляции**:
- Аналогично шаблонным функциям, код шаблонного класса компилируется при инстанцировании.
- Определения методов шаблонного класса обычно включаются в заголовочный файл.
- Возможен **частичный специализация** для групп типов:
  ```cpp
  template<typename T>
  class Pair<T, T> { /* Специализация для одинаковых типов */ };
  ```
- Ошибки компиляции возникают только при использовании конкретного типа.

**Пояснение**:  
Шаблоны классов широко используются в стандартной библиотеке (например, `std::vector`, `std::map`). Они требуют тщательной работы с заголовочными файлами и понимания инстанцирования.

---

#### 4. Конструктор и деструктор. Конструкторы по умолчанию, копирования, оператор присваивания. Различный синтаксис инициализации объекта (a=, a(), a{}). Однородная инициализация структур. Отличие присваивания и инициализации.

**Определение**:  
- **Конструктор**: Специальный метод, вызываемый при создании объекта для инициализации его состояния.
- **Деструктор**: Специальный метод, вызываемый при уничтожении объекта для освобождения ресурсов.
- **Оператор присваивания**: Метод, определяющий поведение присваивания одного объекта другому.

**Типы конструкторов**:
1. **Конструктор по умолчанию**: `Class()`. Создает объект без аргументов.
2. **Конструктор с параметрами**: `Class(T arg)`. Инициализирует объект с заданными значениями.
3. **Конструктор копирования**: `Class(const Class& other)`. Создает копию существующего объекта.
4. **Конструктор перемещения** (C++11): `Class(Class&& other) noexcept`. Переносит ресурсы.

**Пример конструкторов и деструктора**:
```cpp
#include <iostream>
class Resource {
private:
    int* data;
public:
    // Конструктор по умолчанию
    Resource() : data(new int(0)) {
        std::cout << "Default constructor\n";
    }
    // Конструктор с параметром
    Resource(int v) : data(new int(v)) {
        std::cout << "Parameterized constructor\n";
    }
    // Конструктор копирования
    Resource(const Resource& other) : data(new int(*other.data)) {
        std::cout << "Copy constructor\n";
    }
    // Конструктор перемещения
    Resource(Resource&& other) noexcept : data(other.data) {
        other.data = nullptr;
        std::cout << "Move constructor\n";
    }
    // Деструктор
    ~Resource() {
        delete data;
        std::cout << "Destructor\n";
    }
    // Оператор присваивания копирования
    Resource& operator=(const Resource& other) {
        if (this != &other) {
            delete data;
            data = new int(*other.data);
        }
        std::cout << "Copy assignment\n";
        return *this;
    }
    int get() const { return data ? *data : 0; }
};

int main() {
    Resource r1; // Default constructor
    Resource r2(42); // Parameterized constructor
    Resource r3 = r2; // Copy constructor
    r1 = r3; // Copy assignment
    Resource r4 = std::move(r2); // Move constructor
    std::cout << r3.get() << std::endl; // 42
    return 0; // Деструкторы для всех объектов
}
```

**Синтаксис инициализации**:
1. **Копирование**: `Class a = b;` или `Class a(b);`.
2. **Прямая инициализация**: `Class a(args);`.
3. **Однородная инициализация** (C++11): `Class a{args};`.
   - Для структур: `struct S { int x, y; }; S s{1, 2};`.
4. **Инициализация по умолчанию**: `Class a{};`.

**Пример синтаксиса**:
```cpp
#include <iostream>
struct Point {
    int x, y;
    Point(int a = 0, int b = 0) : x(a), y(b) {}
    void print() const { std::cout << x << ", " << y << std::endl; }
};

int main() {
    Point p1 = Point(1, 2); // Копирование
    Point p2(3, 4); // Прямая
    Point p3{5, 6}; // Однородная
    Point p4{}; // По умолчанию (0, 0)
    p1.print(); // 1, 2
    p2.print(); // 3, 4
    p3.print(); // 5, 6
    p4.print(); // 0, 0
    return 0;
}
```

**Однородная инициализация структур**:
- Позволяет инициализировать поля структуры в порядке их объявления.
- Защищает от ошибок, связанных с неявным преобразованием типов.
```cpp
struct Data {
    int id;
    double value;
};
Data d{1, 3.14}; // id = 1, value = 3.14
```

**Отличие присваивания и инициализации**:
- **Инициализация**: Создание объекта с заданным начальным состоянием (вызывается конструктор).
  - Пример: `Resource r(42);` — вызывает конструктор.
- **Присваивание**: Изменение состояния существующего объекта (вызывается `operator=`).
  - Пример: `r = Resource(10);` — вызывает конструктор для временного объекта и оператор присваивания.
- Инициализация более эффективна, так как избегает временных объектов в некоторых случаях.

**Пояснение**:  
Конструкторы и деструкторы управляют жизненным циклом объекта. Однородная инициализация (C++11) унифицирует синтаксис и повышает безопасность. Правильная реализация специальных функций (правило пяти) критически важна для классов с ресурсами.

---

#### 5. Правосторонние ссылки, назначение, перегрузка функций с их применением. Перемещение. Конструктор перемещения. Оператор присваивания с перемещением. Функция std::move.

**Определение**:  
- **Правосторонняя ссылка** (`T&&`): Ссылка, связывающаяся с временными объектами (rvalue). Введена в C++11 для поддержки семантики перемещения.
- **Семантика перемещения**: Перенос ресурсов (например, указателей) из одного объекта в другой без копирования, что повышает производительность.
- **std::move**: Утилита, приводящая объект к rvalue, позволяя использовать перемещение.

**Назначение правосторонних ссылок**:
- Оптимизация операций с временными объектами.
- Реализация перемещающих конструкторов и операторов присваивания.
- Уменьшение ненужного копирования в функциях и контейнерах.

**Пример правосторонних ссылок**:
```cpp
#include <iostream>
void process(int& lvalue) { std::cout << "lvalue: " << lvalue << std::endl; }
void process(int&& rvalue) { std::cout << "rvalue: " << rvalue << std::endl; }

int main() {
    int x = 5;
    process(x); // lvalue: 5
    process(10); // rvalue: 10
    process(std::move(x)); // rvalue: 5
    return 0;
}
```

**Перегрузка функций**:
Правосторонние ссылки позволяют перегрузить функции для обработки lvalue и rvalue отдельно.

**Пример перегрузки**:
```cpp
#include <iostream>
#include <string>
class String {
private:
    char* data;
public:
    String(const char* s) {
        data = new char[strlen(s) + 1];
        strcpy(data, s);
    }
    // Перегрузка для lvalue
    void append(const String& other) {
        std::cout << "Copy append\n";
        char* newData = new char[strlen(data) + strlen(other.data) + 1];
        strcpy(newData, data);
        strcat(newData, other.data);
        delete[] data;
        data = newData;
    }
    // Перегрузка для rvalue
    void append(String&& other) {
        std::cout << "Move append\n";
        delete[] data;
        data = other.data;
        other.data = nullptr;
    }
    ~String() { delete[] data; }
    const char* get() const { return data; }
};

int main() {
    String s1("Hello");
    String s2("World");
    s1.append(s2); // Copy append
    s1.append(String("!")); // Move append
    std::cout << s1.get() << std::endl; // HelloWorld!
    return 0;
}
```

**Конструктор перемещения**:
Переносит ресурсы из временного объекта, обнуляя исходный.

**Оператор присваивания с перемещением**:
Аналогично перемещает ресурсы, освобождая старые.

**Пример**:
```cpp
#include <iostream>
#include <utility>
class Resource {
private:
    int* data;
public:
    Resource(int v = 0) : data(new int(v)) { std::cout << "Constructor\n"; }
    Resource(const Resource& other) : data(new int(*other.data)) {
        std::cout << "Copy constructor\n";
    }
    // Конструктор перемещения
    Resource(Resource&& other) noexcept : data(other.data) {
        other.data = nullptr;
        std::cout << "Move constructor\n";
    }
    // Оператор присваивания с перемещением
    Resource& operator=(Resource&& other) noexcept {
        if (this != &other) {
            delete data;
            data = other.data;
            other.data = nullptr;
            std::cout << "Move assignment\n";
        }
        return *this;
    }
    ~Resource() {
        delete data;
        std::cout << "Destructor\n";
    }
    int get() const { return data ? *data : 0; }
};

int main() {
    Resource r1(10);
    Resource r2 = std::move(r1); // Move constructor
    std::cout << r2.get() << std::endl; // 10
    Resource r3;
    r3 = std::move(r2); // Move assignment
    std::cout << r3.get() << std::endl; // 10
    return 0;
}
```

**std::move**:
- Приводит lvalue к rvalue, сигнализируя, что объект можно перемещать.
- Не выполняет фактического перемещения, а лишь позволяет компилятору выбрать соответствующую перегрузку.

**Пример std::move**:
```cpp
#include <vector>
#include <utility>
int main() {
    std::vector<int> v1 = {1, 2, 3};
    std::vector<int> v2 = std::move(v1); // Перемещение
    // v1 пустой, v2 содержит {1, 2, 3}
    return 0;
}
```

**Пояснение**:  
Семантика перемещения и правосторонние ссылки революционизировали производительность в С++11, особенно для контейнеров и классов с ресурсами. `noexcept` важен для оптимизации.

---

#### 6. Инкапсуляция данных. Уровни доступа к данным и функциям класса. Указатель this.

**Определение**:  
Инкапсуляция данных — это сокрытие внутренней реализации класса и предоставление контролируемого доступа через публичный интерфейс. Уровни доступа и указатель `this` играют ключевую роль в реализации.

**Уровни доступа**:
1. **public**: Доступен всем.
2. **protected**: Доступен внутри класса и в производных классах.
3. **private**: Доступен только внутри класса.

**Пример**:
```cpp
#include <iostream>
class Employee {
private:
    std::string name; // Доступ только внутри класса
    double salary;
protected:
    int id; // Доступ в производных классах
public:
    Employee(const std::string& n, double s, int i) : name(n), salary(s), id(i) {}
    void setSalary(double s) { // Контролируемый доступ
        if (s > 0) salary = s;
    }
    double getSalary() const { return salary; }
    std::string getName() const { return name; }
};

class Manager : public Employee {
public:
    Manager(const std::string& n, double s, int i) : Employee(n, s, i) {}
    void setId(int newId) { id = newId; } // Доступ к protected
    // void changeName() { name = "New"; } // Ошибка: name private
};

int main() {
    Manager m("Alice", 5000.0, 101);
    m.setSalary(6000.0);
    m.setId(102);
    std::cout << m.getName() << ": " << m.getSalary() << std::endl; // Alice: 6000
    return 0;
}
```

**Указатель this**:
- `this` — указатель на текущий объект, доступный в нестатических методах класса.
- Используется для:
  - Разрешения конфликтов имен (например, поле vs параметр).
  - Возврата `*this` для цепного вызова.
  - Передачи объекта в другие функции.

**Пример с this**:
```cpp
#include <iostream>
class Counter {
private:
    int value;
public:
    Counter(int v = 0) : value(v) {}
    Counter& increment() {
        ++value;
        return *this; // Для цепного вызова
    }
    Counter& setValue(int value) {
        this->value = value; // Разрешение конфликта имен
        return *this;
    }
    int getValue() const { return value; }
};

int main() {
    Counter c;
    c.setValue(5).increment().increment();
    std::cout << c.getValue() << std::endl; // 7
    return 0;
}
```

**Пояснение**:  
Инкапсуляция защищает данные, а `this` упрощает работу с объектом внутри класса. Уровни доступа позволяют гибко управлять видимостью членов.

---

#### 7. Композиция классов. Изложение классов в памяти и порядок инициализации/зачистки. Преамбула конструктора. Принцип действия автоматически создаваемых компилятором конструктора копирования, конструктора по умолчанию, копирующего оператора присваивания.

**Определение**:  
Композиция классов — это включение объектов одного класса как полей другого класса, создавая отношение "имеет". Это альтернатива наследованию для построения сложных объектов.

**Пример композиции**:
```cpp
#include <iostream>
class Engine {
private:
    int power;
public:
    Engine(int p) : power(p) { std::cout << "Engine created\n"; }
    ~Engine() { std::cout << "Engine destroyed\n"; }
    int getPower() const { return power; }
};

class Car {
private:
    Engine engine; // Композиция
    std::string model;
public:
    Car(const std::string& m, int p) : engine(p), model(m) {
        std::cout << "Car created\n";
    }
    ~Car() { std::cout << "Car destroyed\n"; }
    void display() const {
        std::cout << model << " with " << engine.getPower() << " hp\n";
    }
};

int main() {
    Car car("Toyota", 200);
    car.display(); // Toyota with 200 hp
    return 0;
}
```
**Вывод**:
```
Engine created
Car created
Toyota with 200 hp
Car destroyed
Engine destroyed
```

**Изложение в памяти**:
- Объект `Car` содержит:
  - Поля `engine` (включая его поля, например, `power`).
  - Поле `model`.
- Поля располагаются последовательно с учетом выравнивания (padding).
- Размер `Car` = размер `Engine` + размер `std::string` + padding.

**Порядок инициализации**:
- Поля инициализируются в порядке их **объявления** в классе, а не в порядке в преамбуле конструктора.
- В примере: сначала `engine`, затем `model`.
- Преамбула конструктора (`: engine(p), model(m)`) задает значения для инициализации.

**Порядок зачистки**:
- Деструкторы вызываются в **обратном порядке** объявления полей.
- В примере: сначала `~Car`, затем `~Engine`.

**Преамбула конструктора**:
- Список инициализации (`: field(value)`) используется для инициализации полей и базовых классов.
- Более эффективна, чем присваивание в теле конструктора, так как избегает лишних операций.

**Автоматически сгенерированные функции**:
1. **Конструктор по умолчанию**:
   - Инициализирует поля значениями по умолчанию (для встроенных типов — не определено).
   - Пример: `Car() : engine(), model() {}`.
2. **Конструктор копирования**:
   - Копирует каждое поле, вызывая копирующие конструкторы для объектов.
   - Пример: `Car(const Car& other) : engine(other.engine), model(other.model) {}`.
3. **Копирующий оператор присваивания**:
   - Копирует поля, вызывая операторы присваивания для объектов.
   - Пример: `Car& operator=(const Car& other) { engine = other.engine; model = other.model; return *this; }`.

**Пример автоматических функций**:
```cpp
#include <iostream>
class AutoResource {
public:
    AutoResource() { std::cout << "Default\n"; }
    AutoResource(const AutoResource&) { std::cout << "Copy\n"; }
    AutoResource& operator=(const AutoResource&) {
        std::cout << "Copy assign\n";
        return *this;
    }
    ~AutoResource() { std::cout << "Destructor\n"; }
};

int main() {
    AutoResource r1; // Default
    AutoResource r2 = r1; // Copy
    r1 = r2; // Copy assign
    return 0; // Destructor x2
}
```

**Пояснение**:  
Композиция создает тесную связь между классами, где время жизни компонента зависит от владельца. Автоматические функции удобны, но требуют ручной реализации для классов с ресурсами.

---

#### 8. Наследование классов. Изложение классов в памяти и порядок инициализации/зачистки. Преамбула конструктора. Принцип действия автоматически создаваемых компилятором конструктора копирования, конструктора по умолчанию, копирующего оператора присваивания.

**Определение**:  
Наследование позволяет производному классу унаследовать поля и методы базового класса, расширяя или переопределяя их.

**Пример наследования**:
```cpp
#include <iostream>
class Base {
protected:
    int baseData;
public:
    Base(int d) : baseData(d) { std::cout << "Base created\n"; }
    ~Base() { std::cout << "Base destroyed\n"; }
    void display() const { std::cout << "Base: " << baseData << std::endl; }
};

class Derived : public Base {
private:
    int derivedData;
public:
    Derived(int b, int d) : Base(b), derivedData(d) {
        std::cout << "Derived created\n";
    }
    ~Derived() { std::cout << "Derived destroyed\n"; }
    void display() const {
        Base::display();
        std::cout << "Derived: " << derivedData << std::endl;
    }
};

int main() {
    Derived d(10, 20);
    d.display();
    return 0;
}
```
**Вывод**:
```
Base created
Derived created
Base: 10
Derived: 20
Derived destroyed
Base destroyed
```

**Изложение в памяти**:
- Объект `Derived` содержит:
  - Поля базового класса `Base` (`baseData`).
  - Поля производного класса (`derivedData`).
- Поля `Base` располагаются перед полями `Derived` в памяти.
- Размер `Derived` = размер `Base` + размер `Derived` + padding.

**Порядок инициализации**:
- Базовый класс инициализируется **первым** (в преамбуле конструктора `Derived`).
- Затем инициализируются поля производного класса в порядке их объявления.
- В примере: `Base::baseData`, затем `derivedData`.

**Порядок зачистки**:
- Деструкторы вызываются в **обратном порядке**:
  - Сначала `~Derived`.
  - Затем `~Base`.

**Преамбула конструктора**:
- Используется для вызова конструктора базового класса и инициализации полей.
- Пример: `: Base(b), derivedData(d)`.

**Автоматически сгенерированные функции**:
1. **Конструктор по умолчанию**:
   - Вызывает конструктор по умолчанию базового класса и инициализирует поля производного.
   - Пример: `Derived() : Base() {}`.
2. **Конструктор копирования**:
   - Копирует базовый класс (через его копирующий конструктор) и поля производного.
   - Пример: `Derived(const Derived& other) : Base(other), derivedData(other.derivedData) {}`.
3. **Копирующий оператор присваивания**:
   - Копирует базовый класс и поля производного.
   - Пример: `Derived& operator=(const Derived& other) { Base::operator=(other); derivedData = other.derivedData; return *this; }`.

**Пример**:
```cpp
#include <iostream>
class AutoBase {
public:
    AutoBase() { std::cout << "Base default\n"; }
    AutoBase(const AutoBase&) { std::cout << "Base copy\n"; }
    AutoBase& operator=(const AutoBase&) {
        std::cout << "Base assign\n";
        return *this;
    }
    ~AutoBase() { std::cout << "Base destructor\n"; }
};

class AutoDerived : public AutoBase {};

int main() {
    AutoDerived d1; // Base default
    AutoDerived d2 = d1; // Base copy
    d1 = d2; // Base assign
    return 0; // Base destructor x2
}
```

**Пояснение**:  
Наследование создает иерархию, где порядок инициализации и зачистки строго определен. Автоматические функции работают корректно для простых классов, но требуют ручной реализации для сложных.

---

#### 9. Доступ к полям и методам родительского класса при public наследовании, смысл модификаторов доступа private, public, protected.

**Определение**:  
При `public` наследовании производный класс сохраняет доступ к `public` и `protected` членам базового класса, но модификаторы доступа определяют, кто может использовать эти члены.

**Модификаторы доступа**:
1. **public**: Члены доступны всем.
2. **protected**: Члены доступны внутри класса и в производных классах.
3. **private**: Члены доступны только внутри класса.

**Правила при public наследовании**:
- `public` члены базового остаются `public` в производном.
- `protected` члены остаются `protected`.
- `private` члены недоступны в производном классе напрямую.

**Пример**:
```cpp
#include <iostream>
class Base {
private:
    int privateData = 1; // Недоступно в производном
protected:
    int protectedData = 2; // Доступно в производном
public:
    int publicData = 3; // Доступно всем
    void display() const {
        std::cout << privateData << " " << protectedData << " " << publicData << std::endl;
    }
};

class Derived : public Base {
public:
    void modify() {
        // privateData = 10; // Ошибка
        protectedData = 20; // OK
        publicData = 30; // OK
    }
};

int main() {
    Derived d;
    d.modify();
    d.display(); // 1 20 30
    std::cout << d.publicData << std::endl; // 30
    // std::cout << d.protectedData; // Ошибка
    return 0;
}
```

**Пояснение**:  
`public` наследование сохраняет интерфейс базового класса, но ограничивает доступ к `private` данным. `protected` позволяет производным классам работать с данными базового класса, сохраняя инкапсуляцию.

---

#### 10. Различные значения классификатора const при описании класса.

**Определение**:  
Ключевое слово `const` в классах используется для обеспечения неизменяемости объектов, методов или возвращаемых значений, предотвращая нежелательные модификации.

**Значения const**:
1. **const методы**: Не изменяют объект (`void func() const;`).
   - Вызываются для `const` объектов.
2. **const поля**: Неизменяемые после инициализации (`const int x;`).
3. **const объекты**: Могут вызывать только `const` методы (`const Class obj;`).
4. **const в возвращаемом значении**: Защищает возвращаемые данные (`const T& get() const;`).
5. **const указатели/ссылки**: Ограничивают модификацию через указатель или ссылку.

**Пример**:
```cpp
#include <iostream>
class Example {
private:
    const int id; // const поле
    int value;
public:
    Example(int i, int v) : id(i), value(v) {}
    // const метод
    int getId() const { return id; }
    // const возвращаемое значение
    const int& getValue() const { return value; }
    void setValue(int v) { value = v; }
};

int main() {
    const Example e(1, 10); // const объект
    std::cout << e.getId() << " " << e.getValue() << std::endl; // 1 10
    // e.setValue(20); // Ошибка: const объект
    // e.getValue() = 20; // Ошибка: const возвращаемое значение
    return 0;
}
```

**Пояснение**:  
`const` повышает безопасность и читаемость кода, явно указывая, что не изменяется. `mutable` позволяет обойти физическую константность для логической.

---

#### 11. Виртуальные функции. Виртуальные деструкторы. Абстрактные классы.

**Определение**:  
- **Виртуальные функции**: Функции, объявленные с `virtual`, обеспечивают динамическое связывание (выбор реализации во время выполнения).
- **Виртуальный деструктор**: Гарантирует вызов деструктора производного класса при удалении через указатель на базовый.
- **Абстрактный класс**: Класс с хотя бы одной чисто виртуальной функцией (`virtual void func() = 0;`), не может быть инстанцирован.

**Пример**:
```cpp
#include <iostream>
class Base {
public:
    virtual void func() { std::cout << "Base func\n"; }
    virtual void pureFunc() = 0; // Чисто виртуальная
    virtual ~Base() { std::cout << "Base destructor\n"; }
};

class Derived : public Base {
public:
    void func() override { std::cout << "Derived func\n"; }
    void pureFunc() override { std::cout << "Derived pureFunc\n"; }
    ~Derived() override { std::cout << "Derived destructor\n"; }
};

int main() {
    Base* ptr = new Derived();
    ptr->func(); // Derived func
    ptr->pureFunc(); // Derived pureFunc
    delete ptr; // Derived destructor, Base destructor
    // Base b; // Ошибка: абстрактный класс
    return 0;
}
```

**Пояснение**:  
Виртуальные функции используют таблицу виртуальных функций (vtable) для динамического связывания. Виртуальный деструктор обязателен в полиморфных иерархиях. Абстрактные классы задают интерфейс.

---

#### 12. Наследование классов. Сокрытие функций. Автоматическое приведение типов указателей и ссылок к родительскому классу.

**Определение**:  
- **Сокрытие функций**: Метод производного класса с тем же именем, но другой сигнатурой, скрывает метод базового.
- **Автоматическое приведение**: Указатели и ссылки на производный класс приводятся к базовому без явного приведения.

**Пример сокрытия**:
```cpp
#include <iostream>
class Base {
public:
    void func(int x) { std::cout << "Base: " << x << std::endl; }
};

class Derived : public Base {
public:
    void func(double x) { std::cout << "Derived: " << x << std::endl; }
};

int main() {
    Derived d;
    d.func(5.5); // Derived: 5.5
    // d.func(5); // Ошибка: func(int) скрыт
    d.Base::func(5); // Base: 5
    return 0;
}
```

**Пример приведения типов**:
```cpp
#include <iostream>
class Base {
public:
    virtual void func() { std::cout << "Base\n"; }
};

class Derived : public Base {
public:
    void func() override { std::cout << "Derived\n"; }
};

int main() {
    Derived d;
    Base* ptr = &d; // Автоматическое приведение
    Base& ref = d; // Автоматическое приведение
    ptr->func(); // Derived
    ref.func(); // Derived
    return 0;
}
```

**Пояснение**:  
Сокрытие может привести к ошибкам, если не использовать `override`. Приведение к базовому классу безопасно и используется в полиморфизме.

---

#### 13. Перегрузка операторов членов класса. Бинарные и унарные операторы. Ограничения при перегрузке операторов членов класса.

**Определение**:  
Перегрузка операторов позволяет определить поведение операторов (`+`, `-`, `=`) для пользовательских типов. Операторы-члены определяются внутри класса.

**Типы операторов**:
- **Бинарные**: Два операнда (`a + b`).
- **Унарные**: Один операнд (`-a`, `++a`).

**Пример**:
```cpp
#include <iostream>
class Complex {
private:
    double real, imag;
public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}
    // Бинарный оператор +
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }
    // Унарный оператор -
    Complex operator-() const {
        return Complex(-real, -imag);
    }
    void print() const {
        std::cout << real << " + " << imag << "i\n";
    }
};

int main() {
    Complex a(1, 2), b(3, 4);
    Complex c = a + b; // operator+
    c.print(); // 4 + 6i
    Complex d = -a; // operator-
    d.print(); // -1 + -2i
    return 0;
}
```

**Ограничения**:
- Нельзя перегрузить: `::`, `.`, `.*`, `sizeof`, `typeid`.
- Нельзя изменить приоритет или ассоциативность.
- Один операнд должен быть объектом класса.
- Нельзя создавать новые операторы.

**Пояснение**:  
Перегрузка операторов делает код интуитивным, но требует соблюдения семантики операторов (например, `+` должно быть коммутативным).

---

#### 14. Перегрузка операторов в виде внешних функций. Бинарные и унарные операторы.

**Определение**:  
Внешние операторы определяются вне класса, часто как `friend` функции, чтобы иметь доступ к `private` членам. Используются, когда первый операнд не является объектом класса.

**Пример**:
```cpp
#include <iostream>
class Complex {
private:
    double real, imag;
public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}
    friend Complex operator+(double x, const Complex& c); // Внешний бинарный
    friend Complex operator-(const Complex& c); // Внешний унарный
    void print() const { std::cout << real << " + " << imag << "i\n"; }
};

Complex operator+(double x, const Complex& c) {
    return Complex(x + c.real, c.imag);
}

Complex operator-(const Complex& c) {
    return Complex(-c.real, -c.imag);
}

int main() {
    Complex c(1, 2);
    Complex d = 5.0 + c; // Внешний +
    d.print(); // 6 + 2i
    Complex e = -c; // Внешний -
    e.print(); // -1 + -2i
    return 0;
}
```

**Пояснение**:  
Внешние операторы полезны для симметричных операций или когда требуется доступ к не-классовым типам. `friend` предоставляет доступ к приватным данным.

---

#### 15. Перегрузка арифметических операторов в виде внешних функций. Оптимизация вычисления арифметических выражений с пользовательскими типами с применением перемещения.

**Определение**:  
Перегрузка арифметических операторов как внешних функций позволяет оптимизировать выражения, особенно с использованием семантики перемещения для временных объектов.

**Пример с перемещением**:
```cpp
#include <iostream>
#include <utility>
class Matrix {
private:
    int* data;
    size_t size;
public:
    Matrix(size_t s) : size(s), data(new int[s]) {
        std::cout << "Constructor\n";
    }
    Matrix(const Matrix& other) : size(other.size), data(new int[size]) {
        std::copy(other.data, other.data + size, data);
        std::cout << "Copy constructor\n";
    }
    Matrix(Matrix&& other) noexcept : size(other.size), data(other.data) {
        other.data = nullptr;
        other.size = 0;
        std::cout << "Move constructor\n";
    }
    ~Matrix() {
        delete[] data;
        std::cout << "Destructor\n";
    }
    friend Matrix operator+(Matrix a, const Matrix& b) { // Копирование a
        for (size_t i = 0; i < a.size; ++i) {
            a.data[i] += b.data[i];
        }
        return a; // Возвращаем a, оптимизируется RVO
    }
    friend Matrix operator+(Matrix&& a, const Matrix& b) { // Перемещение a
        std::cout << "Move + operator\n";
        for (size_t i = 0; i < a.size; ++i) {
            a.data[i] += b.data[i];
        }
        return std::move(a); // Перемещаем a
    }
};

int main() {
    Matrix m1(2), m2(2);
    Matrix m3 = m1 + m2; // Копирование
    Matrix m4 = Matrix(2) + m2; // Перемещение
    return 0;
}
```

**Оптимизация**:
- Перегрузка для `Matrix&&` избегает копирования временного объекта.
- `std::move` сигнализирует, что объект можно перемещать.
- RVO дополнительно устраняет копирование возвращаемого значения.

**Пояснение**:  
Перемещение снижает накладные расходы для временных объектов, что особенно важно для больших структур данных, таких как матрицы.

---

#### 16. Дружественные функции класса. Перегрузка оператора потокового вывода.

**Определение**:  
Дружественная функция (`friend`) имеет доступ к `private` и `protected` членам класса, но не является его членом. Используется для операций, требующих доступа к внутренней реализации.

**Пример оператора вывода**:
```cpp
#include <iostream>
class Point {
private:
    int x, y;
public:
    Point(int a, int b) : x(a), y(b) {}
    // Дружественная функция
    friend std::ostream& operator<<(std::ostream& os, const Point& p);
};

std::ostream& operator<<(std::ostream& os, const Point& p) {
    os << "(" << p.x << ", " << p.y << ")";
    return os;
}

int main() {
    Point p(1, 2);
    std::cout << p << std::endl; // (1, 2)
    return 0;
}
```

**Пояснение**:  
Дружественные функции упрощают реализацию операторов ввода-вывода, сохраняя инкапсуляцию. Оператор `<<` возвращает `ostream&` для цепного вызова.

---

#### 17. Функторы как обобщение функций.

**Определение**:  
Функтор — это объект класса с перегруженным `operator()`, который можно использовать как функцию. Функторы хранят состояние, что делает их более гибкими, чем обычные функции.

**Пример**:
```cpp
#include <iostream>
#include <algorithm>
#include <vector>
struct Adder {
    int offset;
    Adder(int o) : offset(o) {}
    int operator()(int x) const { return x + offset; }
};

int main() {
    std::vector<int> v = {1, 2, 3};
    Adder add(10);
    std::transform(v.begin(), v.end(), v.begin(), add);
    for (int x : v) {
        std::cout << x << " "; // 11 12 13
    }
    std::cout << std::endl;
    return 0;
}
```

**Пояснение**:  
Функторы используются в STL-алгоритмах, где требуется настраиваемое поведение с сохранением состояния. Они более мощные, чем указатели на функции.

---

#### 18. Лямбда-функции и захват переменных. Применение со стандартными алгоритмами.

**Определение**:  
Лямбда-функция — это анонимная функция, определяемая в месте использования. Синтаксис: `[захват](параметры) { тело }`. Лямбды упрощают написание коротких функций.

**Захват переменных**:
- По значению: `[x]`.
- По ссылке: `[&x]`.
- Все по значению: `[=]`.
- Все по ссылке: `[&]`.

**Пример**:
```cpp
#include <iostream>
#include <algorithm>
#include <vector>
int main() {
    std::vector<int> v = {5, 2, 8, 1};
    int offset = 10;
    // Лямбда с захватом
    std::for_each(v.begin(), v.end(), [offset](int& x) { x += offset; });
    for (int x : v) {
        std::cout << x << " "; // 15 12 18 11
    }
    std::cout << std::endl;
    // Сортировка с лямбдой
    std::sort(v.begin(), v.end(), [](int a, int b) { return a < b; });
    for (int x : v) {
        std::cout << x << " "; // 11 12 15 18
    }
    std::cout << std::endl;
    return 0;
}
```

**Пояснение**:  
Лямбды заменяют функторы в простых случаях, интегрируясь с STL-алгоритмами. Захват позволяет использовать внешние переменные, но требует осторожности с временем их жизни.

---

#### 19. Итераторы. Классификация итераторов. Цикл range for.

**Определение**:  
Итератор — это объект, предоставляющий интерфейс для обхода элементов контейнера. Итераторы абстрагируют доступ к данным, позволяя работать с разными структурами.

**Классификация итераторов**:
1. **Входные (Input)**: Только чтение, однократный проход (`std::istream_iterator`).
2. **Выходные (Output)**: Только запись (`std::ostream_iterator`).
3. **Прямого доступа (Forward)**: Чтение/запись, односторонний проход (`std::forward_list`).
4. **Двунаправленные (Bidirectional)**: Чтение/запись, проход вперед/назад (`std::list`).
5. **Случайного доступа (Random Access)**: Чтение/запись, произвольный доступ (`std::vector`, массивы).

**Пример**:
```cpp
#include <iostream>
#include <vector>
int main() {
    std::vector<int> v = {1, 2, 3};
    // Использование итератора
    for (std::vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
        *it += 10;
        std::cout << *it << " "; // 11 12 13
    }
    std::cout << std::endl;
    return 0;
}
```

**Цикл range for**:
- Упрощает обход контейнеров: `for (auto& x : container)`.
- Требует у контейнера методов `begin()` и `end()`.

**Пример range for**:
```cpp
#include <iostream>
#include <vector>
int main() {
    std::vector<int> v = {1, 2, 3};
    for (int& x : v) {
        x += 10;
        std::cout << x << " "; // 11 12 13
    }
    std::cout << std::endl;
    return 0;
}
```

**Пояснение**:  
Итераторы унифицируют доступ к контейнерам, а range for упрощает синтаксис. Случайный доступ наиболее гибок, но менее универсален.

---

#### 20. Стандартная библиотека std. Потоки ввода-вывода. Использование файловых потоков для работы с текстовыми файлами. Перегрузка оператора потокового вывода и ввода (для файлов и потоков cin, cout).

**Определение**:  
Потоки ввода-вывода в STL (`std::iostream`) предоставляют унифицированный интерфейс для работы с консолью, файлами и строками. Основные классы: `std::istream`, `std::ostream`, `std::iostream`.

**Основные потоки**:
- `std::cin`: Ввод с клавиатуры.
- `std::cout`: Вывод на консоль.
- `std::cerr`: Вывод ошибок.

**Файловые потоки**:
- `std::ifstream`: Чтение из файла.
- `std::ofstream`: Запись в файл.
- `std::fstream`: Чтение и запись.

**Пример файловых потоков**:
```cpp
#include <iostream>
#include <fstream>
#include <string>
int main() {
    // Запись в файл
    std::ofstream out("output.txt");
    if (out.is_open()) {
        out << "Hello, file!\n";
        out.close();
    }
    // Чтение из файла
    std::ifstream in("output.txt");
    if (in.is_open()) {
        std::string line;
        while (std::getline(in, line)) {
            std::cout << line << std::endl; // Hello, file!
        }
        in.close();
    }
    return 0;
}
```

**Перегрузка операторов ввода-вывода**:
```cpp
#include <iostream>
class Point {
private:
    int x, y;
public:
    Point(int a = 0, int b = 0) : x(a), y(b) {}
    friend std::ostream& operator<<(std::ostream& os, const Point& p);
    friend std::istream& operator>>(std::istream& is, Point& p);
};

std::ostream& operator<<(std::ostream& os, const Point& p) {
    os << "(" << p.x << ", " << p.y << ")";
    return os;
}

std::istream& operator>>(std::istream& is, Point& p) {
    is >> p.x >> p.y;
    return is;
}

int main() {
    Point p;
    std::cin >> p; // Ввод: 3 4
    std::cout << p << std::endl; // (3, 4)
    std::ofstream out("point.txt");
    out << p;
    return 0;
}
```

**Пояснение**:  
Потоки обеспечивают гибкость и расширяемость. Перегрузка `<<` и `>>` делает пользовательские типы совместимыми с STL.

---

#### 21. Стандартная библиотека std. Класс vector, особенности выделения памяти. Основные функции члены класса. Работа с алгоритмами стандартной библиотеки на примере класса vector.

**Определение**:  
`std::vector` — это динамический массив, автоматически управляющий памятью. Он предоставляет быстрый доступ по индексу и гибкое изменение размера.

**Особенности выделения памяти**:
- **size**: Текущая длина вектора (число элементов).
- **capacity**: Выделенная память (может быть больше `size`).
- При добавлении элементов (`push_back`) вектор увеличивает `capacity` (обычно в 1.5–2 раза), копируя данные в новую область.
- `reserve` позволяет заранее выделить память, избегая перевыделений.

**Основные методы**:
- `push_back(T)`: Добавляет элемент в конец.
- `pop_back()`: Удаляет последний элемент.
- `size()`: Возвращает текущий размер.
- `capacity()`: Возвращает выделенную память.
- `resize(n)`: Изменяет размер, добавляя/удаляя элементы.
- `reserve(n)`: Выделяет память для `n` элементов.
- `operator[]`: Доступ по индексу (без проверки).
- `at(n)`: Доступ с проверкой границ.
- `begin()`, `end()`: Итераторы.

**Пример**:
```cpp
#include <iostream>
#include <vector>
int main() {
    std::vector<int> v;
    v.reserve(10); // Выделяем память
    v.push_back(1);
    v.push_back(2);
    std::cout << "Size: " << v.size() << ", Capacity: " << v.capacity() << std::endl; // Size: 2, Capacity: 10
    v.resize(5, 0); // 1, 2, 0, 0, 0
    std::cout << v.at(3) << std::endl; // 0
    return 0;
}
```

**Работа с алгоритмами**:
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
int main() {
    std::vector<int> v = {5, 2, 8, 1};
    // Сортировка
    std::sort(v.begin(), v.end());
    for (int x : v) {
        std::cout << x << " "; // 1 2 5 8
    }
    std::cout << std::endl;
    // Поиск
    auto it = std::find(v.begin(), v.end(), 5);
    if (it != v.end()) {
        std::cout << "Found: " << *it << std::endl; // Found: 5
    }
    // Применение функции
    std::for_each(v.begin(), v.end(), [](int& x) { x *= 2; });
    for (int x : v) {
        std::cout << x << " "; // 2 4 10 16
    }
    std::cout << std::endl;
    return 0;
}
```

**Пояснение**:  
`std::vector` — один из самых универсальных контейнеров, идеально подходящий для последовательного хранения. Алгоритмы STL повышают его функциональность, минимизируя ручной код.

---

### Часть 3. Подробные ответы на вопросы из задачи 1

Ниже приведены развернутые ответы на каждый вопрос из третьей части с определениями, пояснениями, примерами кода и дополнительными деталями, чтобы обеспечить полное понимание.

---

#### 1. Что такое цепной вызов функций и операторов? Как его обеспечить для пользовательского типа? Приведите два примера цепного вызова операторов для встроенных типов.

**Определение**:  
Цепной вызов (method chaining) — это техника, при которой несколько методов или операторов вызываются последовательно в одной строке, используя возвращаемое значение предыдущего вызова как объект для следующего. Для этого метод или оператор должен возвращать ссылку на объект (`*this` для методов класса). Это улучшает читаемость и позволяет компактно записывать последовательные операции.

**Обеспечение для пользовательского типа**:  
Чтобы реализовать цепной вызов для пользовательского типа, методы класса должны возвращать ссылку на текущий объект (`Class&`). Это позволяет вызывать следующий метод на том же объекте. Для операторов перегрузка должна возвращать ссылку на объект, измененный операцией.

**Пример для пользовательского типа**:
```cpp
#include <iostream>
class Builder {
    int value = 0;
public:
    // Метод возвращает *this для цепного вызова
    Builder& add(int x) {
        value += x;
        return *this;
    }
    // Перегруженный оператор << для цепного вызова
    Builder& operator<<(int x) {
        value += x;
        return *this;
    }
    int getValue() const { return value; }
};

int main() {
    Builder b;
    b.add(10).add(20).add(30); // Цепной вызов методов
    std::cout << b.getValue() << std::endl; // 60
    b << 5 << 15; // Цепной вызов операторов
    std::cout << b.getValue() << std::endl; // 80
    return 0;
}
```

**Примеры цепного вызова для встроенных типов**:
1. **Поток вывода**:
   ```cpp
   std::cout << "Hello" << " " << "World" << std::endl;
   ```
   Здесь `operator<<` возвращает `std::ostream&`, позволяя продолжить цепочку.

2. **Присваивание с арифметикой**:
   ```cpp
   int a = 1, b = 2, c = 3;
   a += b += c; // b = b + c (5), a = a + b (6)
   ```
   Здесь `operator+=` возвращает ссылку на измененный объект, позволяя продолжить операцию.

**Пояснение**:  
Цепной вызов удобен для построения "флюентного" интерфейса, как в случае с потоками или строительными паттернами. Для пользовательских типов важно возвращать `*this` и учитывать константность методов, если объект не должен изменяться.

---

#### 2. Перечислите основные особенности ссылочных переменных. Как физически реализуются ссылки в большинстве современных компиляторов?

**Определение**:  
Ссылка в С++ — это псевдоним для существующей переменной, предоставляющий альтернативное имя для доступа к той же области памяти. Ссылки были введены для упрощения работы с передачей данных в функции и управления объектами.

**Основные особенности ссылочных переменных**:
1. **Инициализация обязательна**: Ссылка должна быть связана с объектом при создании и не может быть переназначена на другой объект.
   ```cpp
   int x = 5;
   int& ref = x; // OK
   // int& ref; // Ошибка: не инициализирована
   ```
2. **Не занимает дополнительной памяти**: Ссылка — это не отдельный объект, а альтернативное имя для существующего.
3. **Нельзя создать "нулевую" ссылку**: В отличие от указателей, ссылка всегда указывает на существующий объект.
4. **Прозрачность доступа**: Работа с ссылкой идентична работе с исходной переменной.
   ```cpp
   ref = 10; // Изменяет x
   std::cout << x << std::endl; // 10
   ```
5. **Поддержка `const`**: Ссылки на константы (`const T&`) могут связываться с временными объектами (rvalue), продлевая их жизнь.
   ```cpp
   const int& ref = 5; // Временный объект живет до конца области видимости ref
   ```
6. **Использование в функциях**: Ссылки позволяют изменять аргументы без копирования и без использования указателей.

**Физическая реализация в компиляторах**:  
В большинстве современных компиляторов (например, GCC, Clang, MSVC) ссылки не создают отдельной памяти и заменяются прямым доступом к адресу исходной переменной на этапе компиляции. Компилятор:
- Заменяет операции с ссылкой операциями с исходным объектом.
- Для аргументов функций ссылки могут реализоваться как указатели (внутренне), но это скрыто от программиста.
- Для `const T&` с временными объектами компилятор создает скрытую переменную для хранения временного объекта.

**Пример**:
```cpp
#include <iostream>
void modify(int& ref) {
    ref = 20;
}
int main() {
    int x = 5;
    int& ref = x;
    modify(ref);
    std::cout << x << " " << ref << std::endl; // 20 20
    return 0;
}
```
Компилятор заменяет `ref` на `x` в сгенерированном коде, а для `modify(ref)` может передать адрес `x` как указатель.

**Пояснение**:  
Ссылки упрощают синтаксис по сравнению с указателями, но их реализация прозрачна и эффективна, так как не требует дополнительной памяти.

---

#### 3. Что такое оптимизация возвращаемого значения? Укажите какой-либо случай, в котором эта оптимизация гарантированно выполняется.

**Определение**:  
Оптимизация возвращаемого значения (Return Value Optimization, RVO) — это техника компилятора, которая устраняет ненужное копирование объектов при возврате из функции. Вместо создания временной копии объекта компилятор размещает возвращаемый объект непосредственно в памяти вызывающей стороны.

**Как работает RVO**:
- Без RVO: Функция создает локальный объект, копирует его в возвращаемое значение, затем копирует в переменную вызывающей стороны.
- С RVO: Компилятор выделяет память для возвращаемого объекта в вызывающей стороне и конструирует объект напрямую, избегая копирования.

**Случай гарантированного выполнения**:  
RVO гарантированно выполняется для **Named Return Value Optimization (NRVO)**, когда функция возвращает локальную переменную, созданную в блоке `return`. Современные компиляторы (C++11 и выше) обязаны применять RVO в таких случаях, даже без флага оптимизации.

**Пример**:
```cpp
#include <iostream>
struct Data {
    int value;
    Data(int v) : value(v) { std::cout << "Constructor\n"; }
    Data(const Data&) { std::cout << "Copy constructor\n"; }
};

Data createData(int v) {
    Data d(v); // Локальный объект
    return d;  // NRVO: d конструируется прямо в вызывающей стороне
}

int main() {
    Data result = createData(42);
    std::cout << result.value << std::endl; // 42
    return 0;
}
```
Вывод:
```
Constructor
42
```
Копирующий конструктор не вызывается, так как RVO устраняет копирование.

**Пояснение**:  
RVO особенно важна для больших объектов, так как снижает накладные расходы. В C++17 стандарт гарантирует RVO для большинства случаев возврата временных объектов, делая код более эффективным.

---

#### 4. Почему в классах с виртуальными функциями деструктор следует делать виртуальным?

**Определение**:  
Виртуальный деструктор — это деструктор, объявленный с ключевым словом `virtual`, что позволяет вызывать деструктор производного класса при удалении объекта через указатель на базовый класс.

**Причина необходимости**:
- Если деструктор базового класса не виртуальный, а объект производного класса удаляется через указатель на базовый класс (`delete ptr`), вызывается только деструктор базового класса. Это приводит к:
  - Неопределенному поведению, если производный класс управляет ресурсами (например, динамической памятью).
  - Утечкам памяти, так как ресурсы производного класса не освобождаются.
- Виртуальный деструктор обеспечивает вызов деструктора производного класса, затем базового, гарантируя корректное освобождение всех ресурсов.

**Пример**:
```cpp
#include <iostream>
class Base {
public:
    // virtual ~Base() { std::cout << "Base destructor\n"; } // Виртуальный
    ~Base() { std::cout << "Base destructor\n"; } // Не виртуальный
};

class Derived : public Base {
public:
    int* ptr;
    Derived() : ptr(new int(10)) {}
    ~Derived() {
        delete ptr;
        std::cout << "Derived destructor\n";
    }
};

int main() {
    Base* ptr = new Derived();
    delete ptr; // Вызывает только Base destructor, если не виртуальный
    return 0;
}
```
Без `virtual`:
```
Base destructor
```
С `virtual`:
```
Derived destructor
Base destructor
```

**Пояснение**:  
Виртуальный деструктор обязателен в полиморфных иерархиях, где объекты производных классов могут удаляться через указатели на базовый класс. Это добавляет небольшую накладную стоимость (таблица виртуальных функций), но обеспечивает безопасность.

---

#### 5. Для чего предназначена идиома copy-move (copy-swap)? Какие она даёт гарантии?

**Определение**:  
Идиома copy-swap (или copy-and-swap) — это шаблон проектирования для реализации оператора присваивания, который использует конструктор копирования и функцию `swap` для обеспечения строгой исключительной безопасности и упрощения кода.

**Назначение**:
- Реализация оператора присваивания (`operator=`) с минимальным количеством кода.
- Обеспечение **строгой исключительной безопасности**: операция либо завершается успешно, либо не изменяет объект.
- Поддержка как копирующего, так и перемещающего присваивания в одной функции.

**Как работает**:
1. Создается копия аргумента (через конструктор копирования или перемещения).
2. Копия обменивается с текущим объектом через `swap`.
3. Копия (теперь содержащая старое состояние) автоматически уничтожается.

**Гарантии**:
- **Строгая исключительная безопасность**: Если копирование или `swap` выбросит исключение, исходный объект остается неизменным.
- **Корректная работа с самоприсваиванием**: Не требует явной проверки `if (this != &other)`.
- **Универсальность**: Поддерживает и копирование, и перемещение.

**Пример**:
```cpp
#include <iostream>
#include <utility>
class Resource {
    int* data;
public:
    Resource(int v = 0) : data(new int(v)) {}
    Resource(const Resource& other) : data(new int(*other.data)) {}
    Resource(Resource&& other) noexcept : data(other.data) { other.data = nullptr; }
    ~Resource() { delete data; }

    // Дружественная функция swap
    friend void swap(Resource& a, Resource& b) noexcept {
        std::swap(a.data, b.data);
    }

    // Copy-swap оператор присваивания
    Resource& operator=(Resource other) noexcept {
        swap(*this, other);
        return *this;
    }

    int get() const { return data ? *data : 0; }
};

int main() {
    Resource a(10), b(20);
    a = b; // Копирование
    std::cout << a.get() << std::endl; // 20
    a = Resource(30); // Перемещение
    std::cout << a.get() << std::endl; // 30
    return 0;
}
```

**Пояснение**:  
Идиома упрощает реализацию, объединяя копирующее и перемещающее присваивание. `noexcept` для `swap` и перемещения повышает эффективность, так как компилятор может оптимизировать код.

---

#### 6. В каких случаях автоматически вызывается деструктор? Как его вызвать вручную?

**Определение**:  
Деструктор — это специальный метод класса, вызываемый для освобождения ресурсов объекта перед его уничтожением.

**Случаи автоматического вызова деструктора**:
1. **Выход из области видимости**: Для автоматических (стековых) объектов деструктор вызывается, когда объект покидает область видимости.
   ```cpp
   {
       Resource r; // Деструктор вызывается при выходе из блока
   }
   ```
2. **Удаление динамического объекта**: При вызове `delete` для объекта, созданного с `new`.
   ```cpp
   Resource* r = new Resource();
   delete r; // Вызывает деструктор
   ```
3. **Уничтожение временного объекта**: Для rvalue объектов деструктор вызывается в конце выражения.
   ```cpp
   Resource(); // Деструктор вызывается сразу
   ```
4. **Уничтожение элементов контейнера**: При удалении элементов из `std::vector`, `std::list` и т.д.
   ```cpp
   std::vector<Resource> v(1);
   v.clear(); // Вызывает деструктор для элемента
   ```
5. **Иерархия наследования**: Деструкторы производных и базовых классов вызываются в обратном порядке.
   ```cpp
   Derived* d = new Derived();
   delete d; // Derived::~Derived(), затем Base::~Base()
   ```

**Ручной вызов деструктора**:
- Деструктор можно вызвать явно с помощью `obj.~Class()`. Это редко используется и требуется в специфических случаях, например, при управлении памятью через `placement new`.
- После ручного вызова деструктора объект считается уничтоженным, но память не освобождается (для динамических объектов нужен `delete`).

**Пример**:
```cpp
#include <iostream>
class Resource {
public:
    Resource() { std::cout << "Constructor\n"; }
    ~Resource() { std::cout << "Destructor\n"; }
};

int main() {
    {
        Resource r; // Автоматический вызов деструктора при выходе
    }
    Resource* ptr = new Resource();
    ptr->~Resource(); // Ручной вызов
    delete ptr; // Память освобождается, но деструктор не вызывается повторно
    return 0;
}
```
Вывод:
```
Constructor
Destructor
Constructor
Destructor
```

**Пояснение**:  
Ручной вызов деструктора опасен, так как требует осторожного управления памятью. Автоматический вызов предпочтителен, так как встроен в семантику языка.

---

#### 7. Для чего предназначен кадр стека вызова функций?

**Определение**:  
Кадр стека вызова (stack frame) — это участок памяти в стеке, выделяемый для каждой функции во время ее выполнения. Он хранит данные, необходимые для работы функции и возврата управления.

**Назначение кадра стека**:
1. **Хранение локальных переменных**: Все автоматические переменные функции размещаются в кадре.
2. **Хранение аргументов функции**: Параметры, переданные в функцию, копируются в кадр.
3. **Хранение адреса возврата**: Адрес инструкции, куда вернуться после завершения функции.
4. **Хранение состояния регистров**: Сохранение значений регистров процессора для восстановления после вызова.
5. **Управление стеком**: Указатели на начало и конец кадра (stack pointer и base pointer).
6. **Поддержка рекурсии**: Каждый вызов функции создает новый кадр, позволяя сохранять состояние.

**Пример**:
```cpp
#include <iostream>
void inner(int x) {
    int y = x + 1; // Локальная переменная в кадре
    std::cout << y << std::endl;
}
void outer() {
    int z = 10; // Локальная переменная
    inner(z); // Новый кадр для inner
}

int main() {
    outer(); // Кадр для outer
    return 0;
}
```

**Структура кадра (упрощенно)**:
- Для функции `inner`:
  - Аргумент `x`.
  - Локальная переменная `y`.
  - Адрес возврата в `outer`.
  - Сохраненные регистры.

**Пояснение**:  
Кадр стека обеспечивает изоляцию данных каждой функции, поддерживает рекурсию и позволяет компилятору эффективно управлять памятью. Переполнение стека (stack overflow) происходит при слишком глубокой рекурсии.

---

#### 8. Что такое полиморфизм? Перечислите основные способы реализации динамического полиморфизма в С++. Какие средства в С++ предназначены для поддержки статического полиморфизма?

**Определение**:  
Полиморфизм — это способность объектов вести себя по-разному в зависимости от их типа, несмотря на использование общего интерфейса. В С++ различают **динамический** (выполняется во время выполнения) и **статический** (выполняется на этапе компиляции) полиморфизм.

**Динамический полиморфизм**:
- Реализуется через наследование и виртуальные функции.
- Основные способы:
  1. **Виртуальные функции**: Используются с ключевым словом `virtual`. Вызов функции определяется типом объекта во время выполнения через таблицу виртуальных функций (vtable).
     ```cpp
     class Base {
     public:
         virtual void func() { std::cout << "Base\n"; }
     };
     class Derived : public Base {
     public:
         void func() override { std::cout << "Derived\n"; }
     };
     int main() {
         Base* ptr = new Derived();
         ptr->func(); // Derived
         delete ptr;
         return 0;
     }
     ```
  2. **Абстрактные классы**: Содержат чисто виртуальные функции (`virtual void func() = 0;`).
     ```cpp
     class Abstract {
     public:
         virtual void func() = 0;
     };
     ```
  3. **Переопределение методов**: Производные классы предоставляют собственные реализации виртуальных методов.

**Статический полиморфизм**:
- Реализуется на этапе компиляции, не требует накладных расходов на vtable.
- Средства:
  1. **Перегрузка функций и операторов**: Разные функции с одинаковым именем, но разными параметрами.
     ```cpp
     int add(int a, int b) { return a + b; }
     double add(double a, double b) { return a + b; }
     ```
  2. **Шаблоны**: Параметризация кода типами.
     ```cpp
     template<typename T>
     T max(T a, T b) { return a > b ? a : b; }
     ```
  3. **CRTP (Curiously Recurring Template Pattern)**:
     ```cpp
     template<typename Derived>
     class Base {
     public:
         void func() { static_cast<Derived*>(this)->impl(); }
     };
     class Derived : public Base<Derived> {
     public:
         void impl() { std::cout << "Derived\n"; }
     };
     ```

**Пояснение**:  
Динамический полиморфизм гибок, но имеет накладные расходы (vtable). Статический полиморфизм эффективен, но менее гибок, так как типы определяются на этапе компиляции.

---

#### 9. Объявление класса foo имеет вид: `class foo {};`. Какие функции класса foo компилятор сгенерирует автоматически? Выпишите сигнатуру этих функций. Как запретить их генерацию (покажите на примере любой функции)? Как явно указать компилятору сгенерировать какую-либо из них?

**Определение**:  
При объявлении класса компилятор автоматически генерирует специальные функции-члены, если они не определены явно. Это упрощает работу с классами, но может быть нежелательно в некоторых случаях.

**Автоматически сгенерированные функции** для `class foo {}`:
1. **Конструктор по умолчанию**: `foo();`
   - Создает объект с инициализацией полей по умолчанию (для встроенных типов — не определено).
2. **Конструктор копирования**: `foo(const foo&);`
   - Копирует все поля побитно или с использованием их копирующих конструкторов.
3. **Оператор присваивания копирования**: `foo& operator=(const foo&);`
   - Копирует все поля аналогично конструктору копирования.
4. **Деструктор**: `~foo();`
   - Уничтожает объект, вызывая деструкторы для полей.
5. **Конструктор перемещения** (C++11+): `foo(foo&&) noexcept;`
   - Перемещает ресурсы, если поля поддерживают перемещение.
6. **Оператор присваивания перемещения** (C++11+): `foo& operator=(foo&&) noexcept;`
   - Перемещает ресурсы аналогично конструктору перемещения.

**Сигнатуры**:
```cpp
class foo {
public:
    foo();                            // Конструктор по умолчанию
    foo(const foo&);                  // Конструктор копирования
    foo& operator=(const foo&);       // Оператор присваивания копирования
    ~foo();                           // Деструктор
    foo(foo&&) noexcept;              // Конструктор перемещения
    foo& operator=(foo&&) noexcept;   // Оператор присваивания перемещения
};
```

**Запрет генерации**:
- Используется `= delete` для любой функции.
- Пример:
  ```cpp
  class foo {
  public:
      foo(const foo&) = delete; // Запрещает копирование
  };
  ```

**Явная генерация**:
- Используется `= default` для указания компилятору сгенерировать функцию по умолчанию.
- Пример:
  ```cpp
  class foo {
  public:
      foo() = default; // Явно генерировать конструктор по умолчанию
  };
  ```

**Пример полного контроля**:
```cpp
#include <iostream>
class foo {
public:
    foo() { std::cout << "Default constructor\n"; }
    foo(const foo&) = delete; // Запрещаем копирование
    foo& operator=(const foo&) = delete;
    ~foo() = default; // Явно используем стандартный деструктор
};

int main() {
    foo f; // OK
    // foo f2 = f; // Ошибка: копирование запрещено
    return 0;
}
```

**Пояснение**:  
Автоматическая генерация упрощает написание кода, но может быть опасной для классов с ресурсами (например, указателями). `= delete` и `= default` дают точный контроль над поведением класса.

---

#### 10. Каким минимальным требованиям должен удовлетворять контейнер для модификации его элементов при помощи диапазонного for?

**Определение**:  
Диапазонный `for` (range-based for loop, C++11+) — это синтаксическая конструкция для обхода элементов контейнера: `for (auto& x : container) {}`. Контейнер должен поддерживать итерацию.

**Минимальные требования**:
1. **Методы `begin()` и `end()`**: Контейнер должен предоставлять функции `begin()` и `end()`, возвращающие итераторы на начало и конец последовательности.
   - Или: глобальные функции `begin(container)` и `end(container)` (для массивов или пользовательских типов).
2. **Итераторы**: Возвращаемые итераторы должны поддерживать:
   - Оператор разыменования (`*`) для доступа к элементу.
   - Оператор инкремента (`++`) для перехода к следующему элементу.
   - Оператор сравнения (`!=`) для проверки конца.
3. **Доступ к элементам**: Для модификации элементов в цикле `for (auto& x : container)` итератор должен возвращать ссылку на элемент.

**Пример пользовательского контейнера**:
```cpp
#include <iostream>
class MyContainer {
    int data[3] = {1, 2, 3};
public:
    class Iterator {
        int* ptr;
    public:
        Iterator(int* p) : ptr(p) {}
        int& operator*() { return *ptr; }
        Iterator& operator++() { ++ptr; return *this; }
        bool operator!=(const Iterator& other) const { return ptr != other.ptr; }
    };
    Iterator begin() { return Iterator(data); }
    Iterator end() { return Iterator(data + 3); }
};

int main() {
    MyContainer c;
    for (auto& x : c) {
        x *= 2; // Модификация
        std::cout << x << " ";
    } // 2 4 6
    std::cout << std::endl;
    return 0;
}
```

**Пояснение**:  
Диапазонный `for` работает с любым контейнером, реализующим минимальный интерфейс итерации. Для модификации элементов ссылка (`auto&`) обязательна, иначе цикл работает с копиями.

---

#### 11. Чем статические переменные отличаются от глобальных? В чём особенность жизненного цикла статических и глобальных переменных?

**Определение**:  
- **Глобальные переменные**: Объявлены вне функций, доступны во всей программе (или файле с `static`).
- **Статические переменные**: Объявлены с ключевым словом `static`, имеют ограниченную видимость, но сохраняют значение между вызовами.

**Отличия**:
1. **Область видимости**:
   - Глобальные: Видимы во всех файлах (если `extern`) или в файле (если `static`).
   - Статические: Видимы только в области объявления (файл, функция, класс).
2. **Место объявления**:
   - Глобальные: Вне функций или классов.
   - Статические: В функциях, файлах или классах.
3. **Доступ**:
   - Глобальные: Могут быть доступны из других файлов (с `extern`).
   - Статические: Ограничены областью (например, локальная статическая переменная видна только в функции).

**Жизненный цикл**:
- **Глобальные переменные**:
  - Создаются при запуске программы.
  - Уничтожаются при завершении программы.
  - Инициализируются до вызова `main()` (статическая инициализация).
- **Статические переменные**:
  - **Глобальные статические**: Аналогичны глобальным, но ограничены файлом.
  - **Локальные статические**: Создаются при первом вызове функции, уничтожаются при завершении программы.
  - Инициализация выполняется один раз (ленивая для локальных).

**Пример**:
```cpp
#include <iostream>
int global = 10; // Глобальная
static int file_static = 20; // Статическая, видна только в файле

void func() {
    static int local_static = 30; // Локальная статическая
    std::cout << global << " " << file_static << " " << local_static++ << std::endl;
}

int main() {
    func(); // 10 20 30
    func(); // 10 20 31
    return 0;
}
```

**Пояснение**:  
Статические переменные полезны для ограничения видимости и сохранения состояния, тогда как глобальные обеспечивают широкий доступ, что может усложнить отладку.

---

#### 12. На какие секции делится память, доступная программе, что в них размещается? Для переменных из каких секций предусмотрен автоматический вызов деструктора?

**Определение**:  
Память программы делится на секции, каждая из которых предназначена для определенных данных. Это обеспечивает эффективное управление ресурсами и изоляцию.

**Секции памяти**:
1. **Стек (Stack)**:
   - Хранит: Локальные переменные, параметры функций, адреса возврата.
   - Управление: Автоматическое, LIFO (Last In, First Out).
   - Особенности: Быстрый доступ, ограниченный размер.
2. **Куча (Heap)**:
   - Хранит: Динамически выделенные объекты (`new`).
   - Управление: Ручное (`new/delete`) или автоматическое (умные указатели).
   - Особенности: Большой объем, медленнее стека.
3. **Сегмент данных (Data Segment)**:
   - **Инициализированные данные**: Глобальные и статические переменные с явной инициализацией.
   - **Неинициализированные данные (BSS)**: Глобальные и статические переменные без инициализации (нулевые).
   - Хранит: Постоянные данные, доступные всю программу.
4. **Сегмент кода (Text Segment)**:
   - Хранит: Машинный код программы, константы (например, строковые литералы).
   - Особенности: Только для чтения.

**Автоматический вызов деструктора**:
- **Стек**: Деструкторы вызываются для автоматических объектов при выходе из области видимости.
  ```cpp
  {
      Resource r; // Деструктор вызывается здесь
  }
  ```
- **Куча**: Деструкторы вызываются при вызове `delete` для динамических объектов.
  ```cpp
  Resource* r = new Resource();
  delete r; // Деструктор
  ```
- **Сегмент данных**: Деструкторы вызываются для глобальных и статических объектов при завершении программы.
  ```cpp
  Resource global; // Деструктор вызывается при завершении
  ```

**Пример**:
```cpp
#include <iostream>
class Resource {
public:
    ~Resource() { std::cout << "Destructor\n"; }
};

Resource global; // Сегмент данных
int main() {
    Resource stack; // Стек
    Resource* heap = new Resource(); // Куча
    delete heap; // Деструктор для heap
    return 0;
} // Деструкторы для stack и global
```

**Пояснение**:  
Автоматический вызов деструкторов упрощает управление ресурсами, но для объектов на куче требуется явное освобождение.

---

#### 13. Что такое анонимный объект? Какова продолжительность его жизни и как и до какого момента можно её продлить?

**Определение**:  
Анонимный объект (временный объект, rvalue) — это объект, созданный выражением, не связанный с именованной переменной. Он используется в вычислениях и обычно уничтожается сразу после использования.

**Продолжительность жизни**:
- Анонимный объект существует до конца **полного выражения**, в котором он создан.
- Пример:
  ```cpp
  Resource(); // Создается и сразу уничтожается
  ```

**Продление жизни**:
- Жизнь анонимного объекта продлевается, если он связывается с **константной ссылкой** (`const T&`) или **правосторонней ссылкой** (`T&&`).
- Продление длится до конца области видимости ссылки.
- Нельзя продлить жизнь через неконстантную левостороннюю ссылку (`T&`), так как она не связывается с rvalue.

**Пример**:
```cpp
#include <iostream>
class Resource {
public:
    Resource() { std::cout << "Constructor\n"; }
    ~Resource() { std::cout << "Destructor\n"; }
};

int main() {
    {
        const Resource& ref = Resource(); // Продление жизни
        std::cout << "Still alive\n";
    } // Деструктор вызывается здесь
    return 0;
}
```
Вывод:
```
Constructor
Still alive
Destructor
```

**Пояснение**:  
Продление жизни полезно для временных объектов, возвращаемых функциями, чтобы избежать копирования. Однако это не применимо к неконстантным левым ссылкам.

---

#### 14. Что такое инкапсуляция? Назовите средства обеспечения инкапсуляции в С++.

**Определение**:  
Инкапсуляция — это принцип ООП, заключающийся в объединении данных и методов, работающих с ними, в одном классе и ограничении прямого доступа к данным, предоставляя контролируемый интерфейс.

**Средства обеспечения инкапсуляции в С++**:
1. **Модификаторы доступа**:
   - `private`: Доступ только внутри класса.
   - `protected`: Доступ внутри класса и в производных классах.
   - `public`: Доступ всем.
2. **Геттеры и сеттеры**: Методы для чтения и записи данных.
3. **Константные методы**: Методы, не изменяющие объект (`const`).
4. **Интерфейсы**: Абстрактные классы для определения контракта.
5. **Пространства имен**: Ограничение видимости глобальных имен.

**Пример**:
```cpp
#include <iostream>
class Data {
private:
    int value; // Скрытые данные
public:
    void setValue(int v) { // Сеттер
        if (v >= 0) value = v; // Контроль
    }
    int getValue() const { // Геттер, константный метод
        return value;
    }
};

int main() {
    Data d;
    d.setValue(42);
    std::cout << d.getValue() << std::endl; // 42
    // d.value = 10; // Ошибка: private
    return 0;
}
```

**Пояснение**:  
Инкапсуляция повышает безопасность и модульность, позволяя скрыть реализацию и предоставить только необходимый интерфейс.

---

#### 15. Что такое функция? Какие преимущества даёт использование функций?

**Определение**:  
Функция — это именованный блок кода, выполняющий определенную задачу, который может принимать аргументы и возвращать значение. Функции позволяют структурировать программу.

**Преимущества использования функций**:
1. **Модульность**: Разделение кода на логические блоки упрощает разработку и поддержку.
2. **Переиспользование**: Функции можно вызывать многократно, избегая дублирования кода.
3. **Читаемость**: Именованные функции делают код понятнее.
4. **Управление сложностью**: Функции скрывают детали реализации, упрощая высокоуровневую логику.
5. **Тестирование**: Легче тестировать отдельные функции.
6. **Рекурсия**: Функции поддерживают рекурсивные алгоритмы.

**Пример**:
```cpp
#include <iostream>
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

int main() {
    std::cout << factorial(5) << std::endl; // 120
    return 0;
}
```

**Пояснение**:  
Функции — основа структурированного программирования, позволяющая создавать масштабируемые и поддерживаемые программы.

---

#### 16. Память под объекты классов из иерархии с абстрактным базовым классом выделяется в куче, а указатели на них хранятся как указатели на базовый класс в стандартном векторе. Является ли такая схема эффективной с точки зрения использования кэш-памяти? Поясните ответ.

**Определение**:  
Кэш-память — это быстрая память процессора для временного хранения часто используемых данных. Эффективность использования кэша зависит от **локальности данных** (пространственной и временной).

**Схема**:
- Объекты производных классов создаются в куче (`new Derived()`).
- Указатели на базовый класс (`Base*`) хранятся в `std::vector<Base*>`.

**Эффективность для кэш-памяти**:
- **Неэффективно**, так как:
  1. **Разбросанные данные**: Объекты в куче выделяются в произвольных местах памяти, что нарушает пространственную локальность. При доступе к объектам процессору приходится загружать разные кэш-линии.
  2. **Косвенный доступ**: Доступ через указатели требует дополнительной загрузки адреса, увеличивая промахи кэша.
  3. **Vtable**: Полиморфизм добавляет обращения к таблице виртуальных функций, что также снижает локальность.
- **Сравнение с альтернативой**:
  - Если бы объекты хранились непосредственно в `std::vector<Derived>` (невозможно для абстрактного базового класса), данные были бы непрерывными, что улучшило бы кэш-локальность.

**Пример**:
```cpp
#include <vector>
class Base {
public:
    virtual void func() = 0;
    virtual ~Base() = default;
};
class Derived : public Base {
public:
    int data = 0;
    void func() override {}
};

int main() {
    std::vector<Base*> v;
    for (int i = 0; i < 1000; ++i) {
        v.push_back(new Derived());
    }
    // Доступ к v[i]->data требует загрузки указателя и объекта
    for (Base* ptr : v) {
        delete ptr;
    }
    return 0;
}
```

**Пояснение**:  
Для повышения эффективности можно использовать **пулы памяти** или **контейнеры с непрерывным хранением** (например, `std::vector<std::unique_ptr<Derived>>`), но указатели на кучу всегда менее эффективны, чем непрерывные данные.

---

#### 17. Что такое кэш-память? В чём состоит основной принцип оптимизации данных и алгоритмов для эффективной работы с кэш-памятью?

**Определение**:  
Кэш-память — это высокоскоростная память, расположенная между процессором и оперативной памятью, для хранения часто используемых данных. Она состоит из уровней (L1, L2, L3), где L1 — самая быстрая и маленькая.

**Основной принцип оптимизации**:
- **Локальность данных**:
  1. **Пространственная локальность**: Данные, расположенные близко в памяти, загружаются в кэш одновременно (в одной кэш-линии, обычно 64 байта).
  2. **Временная локальность**: Данные, используемые недавно, вероятно, будут использованы снова.
- **Цель**: Минимизировать промахи кэша (cache misses), когда данные отсутствуют в кэше и загружаются из медленной оперативной памяти.

**Оптимизация**:
1. **Непрерывное хранение**: Использование массивов вместо связных списков.
2. **Линейный доступ**: Обход данных последовательно (например, по строкам матрицы).
3. **Минимизация указателей**: Уменьшение косвенных обращений.
4. **Компактные структуры**: Сокращение padding в классах/структурах.
5. **Повторное использование данных**: Выполнение всех операций над данными, пока они в кэше.

**Пример**:
```cpp
#include <vector>
int main() {
    std::vector<int> v(1000);
    // Эффективно: линейный доступ
    for (size_t i = 0; i < v.size(); ++i) {
        v[i] = i;
    }
    // Неэффективно: случайный доступ
    for (size_t i = 0; i < v.size(); i += 100) {
        v[i] = i;
    }
    return 0;
}
```

**Пояснение**:  
Оптимизация для кэша критически важна для высокопроизводительных приложений, так как промахи кэша значительно увеличивают время выполнения.

---

#### 18. Матрица хранится в виде сплошного массива по столбцам. Вектор хранится в виде сплошного массива. Укажите какой-либо оптимальный и неоптимальный порядок действий с точки зрения эффективного использования кэш-памяти для умножения матрицы на вектор слева. Поясните ответ.

**Определение**:  
Умножение матрицы на вектор слева: \( y = A \cdot x \), где \( A \) — матрица \( m \times n \), \( x \) — вектор \( n \times 1 \), \( y \) — вектор \( m \times 1 \). Матрица хранится по столбцам (column-major), т.е. элементы столбца располагаются последовательно в памяти.

**Формула**:
\[ y_i = \sum_{j=0}^{n-1} A_{i,j} \cdot x_j \]

**Хранение**:
- Матрица \( A \): Элемент \( A_{i,j} \) в сплошном массиве находится по индексу \( i + j \cdot m \).
- Вектор \( x \): Непрерывный массив \( x_0, x_1, \ldots, x_{n-1} \).

**Оптимальный порядок**:
- **Обход по строкам матрицы** (внешний цикл по \( i \), внутренний по \( j \)):
  - Для каждой строки \( i \) вычисляется \( y_i \).
  - Доступ к \( A_{i,j} \) нелинеен (по столбцам), но вектор \( x \) обходит последовательно.
  - Пространственная локальность для \( x \) сохраняется, так как \( x_j \) загружаются в кэш последовательно.

**Пример оптимального кода**:
```cpp
void multiply(const int* A, const int* x, int* y, int m, int n) {
    for (int i = 0; i < m; ++i) {
        y[i] = 0;
        for (int j = 0; j < n; ++j) {
            y[i] += A[i + j * m] * x[j];
        }
    }
}
```

**Неоптимальный порядок**:
- **Обход по столбцам матрицы** (внешний цикл по \( j \), внутренний по \( i \)):
  - Для каждого столбца \( j \) обновляются все \( y_i \).
  - Доступ к \( A_{i,j} \) линеен, но \( y_i \) и \( x_j \) вызывают частые промахи кэша, так как \( y \) обходит многократно.

**Пример неоптимального кода**:
```cpp
void multiply(const int* A, const int* x, int* y, int m, int n) {
    for (int j = 0; j < n; ++j) {
        for (int i = 0; i < m; ++i) {
            y[i] += A[i + j * m] * x[j];
        }
    }
}
```

**Пояснение**:
- Оптимальный вариант минимизирует промахи кэша для вектора \( x \), так как его элементы загружаются последовательно. Неоптимальный вариант чаще обращается к \( y \), нарушая временную локальность.

---

#### 19. Что такое итератор (активный)? В каких случаях в роли итератора может выступать указатель?

**Определение**:  
Итератор — это объект, предоставляющий интерфейс для обхода элементов контейнера. Он абстрагирует доступ к данным, позволяя работать с разными структурами (массивы, списки, деревья) единообразно. "Активный" итератор обычно означает итератор, указывающий на существующий элемент (не `end()`).

**Свойства итератора**:
- **Разыменование** (`*`): Доступ к элементу.
- **Инкремент** (`++`): Переход к следующему элементу.
- **Сравнение** (`==`, `!=`): Проверка равенства или окончания.

**Классификация**:
- Входные, выходные, прямого доступа, двунаправленные, случайного доступа.

**Указатель как итератор**:
- Указатель (`T*`) может выступать в роли итератора для **непрерывных структур данных**, таких как массивы или `std::vector`.
- Указатель поддерживает:
  - Разыменование: `*ptr`.
  - Инкремент: `++ptr`.
  - Сравнение: `ptr != end`.
  - Случайный доступ: `ptr + n`.

**Пример**:
```cpp
#include <vector>
#include <iostream>
int main() {
    int arr[] = {1, 2, 3};
    int* begin = arr;
    int* end = arr + 3;
    for (int* it = begin; it != end; ++it) {
        std::cout << *it << " "; // 1 2 3
    }
    std::cout << std::endl;

    std::vector<int> v = {4, 5, 6};
    for (std::vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
        std::cout << *it << " "; // 4 5 6
    }
    return 0;
}
```

**Пояснение**:  
Указатели — это простейшие итераторы, используемые для массивов. Для сложных контейнеров (например, `std::list`) нужны специализированные итераторы.

---

#### 20. Может ли объект класса при различной композиции класса из одного и того же набора членов иметь разный размер? Если да, то по какой причине? Поясните ответ примером.

**Определение**:  
Размер объекта класса определяется размером его полей, выравниванием памяти (padding) и дополнительными данными (например, vtable для виртуальных функций).

**Ответ**:  
**Да**, объект может иметь разный размер при одинаковом наборе членов из-за **выравнивания памяти**. Компилятор добавляет "пустые" байты (padding) между полями, чтобы выровнять их адреса для оптимизации доступа процессором.

**Причина**:
- Выравнивание зависит от порядка объявления полей и требований платформы (например, 4-байтное или 8-байтное выравнивание).
- Разные компиляторы или настройки могут влиять на padding.

**Пример**:
```cpp
#include <iostream>
struct A {
    char c; // 1 байт
    int i;  // 4 байта
};

struct B {
    int i;  // 4 байта
    char c; // 1 байт
};

int main() {
    std::cout << sizeof(A) << " " << sizeof(B) << std::endl; // 8 8
    return 0;
}
```
- **A**: `c` (1 байт) + 3 байта padding + `i` (4 байта) = 8 байт.
- **B**: `i` (4 байта) + `c` (1 байт) + 3 байта padding = 8 байт.
- Если изменить порядок, размер может быть меньше для `B` на некоторых платформах.

**Пояснение**:  
Оптимизация порядка полей (упаковка от большего к меньшему) может уменьшить padding и размер объекта.

---

#### 21. При проектировании класса foo возникла необходимость в ручной реализации оператора `foo& operator=(const foo&)`. Какие ещё пять функций вероятно необходимо реализовать в классе? Укажите их сигнатуру. Какова основная причина для ручной реализации этих функций?

**Определение**:  
Если класс управляет ресурсами (например, динамической памятью), стандартные функции, сгенерированные компилятором, могут быть некорректными. Это требует ручной реализации.

**Необходимость ручной реализации `operator=`**:
- Класс владеет ресурсом (например, указателем), и побитовое копирование приведет к двойному освобождению или утечкам.

**Пять функций (правило пяти)**:
1. **Конструктор копирования**: `foo(const foo&);`
2. **Конструктор перемещения**: `foo(foo&&) noexcept;`
3. **Оператор присваивания перемещения**: `foo& operator=(foo&&) noexcept;`
4. **Деструктор**: `~foo();`
5. **Конструктор по умолчанию** (иногда): `foo();`

**Причина**:
- **Управление ресурсами**: Класс, владеющий указателем, файлом или другим ресурсом, требует явного контроля над копированием, перемещением и уничтожением, чтобы избежать утечек или двойного освобождения.
- Побитовое копирование (стандартное поведение) создает два указателя на один ресурс, что приводит к ошибкам.

**Пример**:
```cpp
#include <iostream>
class foo {
    int* data;
public:
    foo() : data(new int(0)) {} // Конструктор по умолчанию
    foo(const foo& other) : data(new int(*other.data)) {} // Копирование
    foo(foo&& other) noexcept : data(other.data) { other.data = nullptr; } // Перемещение
    foo& operator=(const foo& other) { // Копирующее присваивание
        if (this != &other) {
            delete data;
            data = new int(*other.data);
        }
        return *this;
    }
    foo& operator=(foo&& other) noexcept { // Перемещающее присваивание
        if (this != &other) {
            delete data;
            data = other.data;
            other.data = nullptr;
        }
        return *this;
    }
    ~foo() { delete data; } // Деструктор
};

int main() {
    foo f1, f2;
    f2 = f1; // Копирование
    foo f3 = std::move(f1); // Перемещение
    return 0;
}
```

**Пояснение**:  
Правило пяти гарантирует корректное управление ресурсами. Если одна функция реализована вручную, остальные обычно тоже требуют реализации.

---

#### 22. Что такое физическая и логическая константность объектов классов? Перечислите средства обеспечения и обхода константности в С++?

**Определение**:  
- **Физическая константность**: Объект не изменяется в памяти (все поля неизменяемы).
- **Логическая константность**: Объект выглядит неизменным для внешнего наблюдателя, но внутренние данные могут изменяться (например, для оптимизации).

**Средства обеспечения константности**:
1. **Ключевое слово `const`**:
   - Для методов: `void func() const;`.
   - Для объектов: `const Class obj;`.
   - Для полей: `const int x;`.
2. **Константные ссылки и указатели**: `const T&`, `const T*`.
3. **Ограничение доступа**: `private` поля, доступ через `const` геттеры.

**Средства обхода константности**:
1. **Ключевое слово `mutable`**:
   - Позволяет изменять поля в `const` методах.
   ```cpp
   class Cache {
       mutable int cache;
   public:
       int get() const { return cache++; }
   };
   ```
2. **Приведение типов**:
   - `const_cast` для снятия `const` (опасно, может привести к UB).
   ```cpp
   const int x = 5;
   int& y = const_cast<int&>(x);
   y = 10; // UB
   ```
3. **Внутренние указатели**: Класс может хранить указатель на изменяемые данные.

**Пример**:
```cpp
#include <iostream>
class Example {
    mutable int counter = 0;
public:
    int getCounter() const {
        return ++counter; // OK из-за mutable
    }
};

int main() {
    const Example e;
    std::cout << e.getCounter() << " " << e.getCounter() << std::endl; // 1 2
    return 0;
}
```

**Пояснение**:  
Логическая константность позволяет оптимизировать классы (например, кэширование), сохраняя внешнюю неизменяемость. `mutable` и `const_cast` требуют осторожного использования.

---

#### 23. Что характеризуют и откуда происходят термины rvalue и lvalue? Какое основное отличие между rvalue и lvalue?

**Определение**:  
- **lvalue** (left value): Объект, имеющий адрес в памяти, который может быть присвоен (находится слева от `=`).
- **rvalue** (right value): Временный объект или значение, не имеющее постоянного адреса, обычно используемое справа от `=`.

**Происхождение терминов**:
- Термины происходят из синтаксиса присваивания: `lvalue = rvalue`.
- lvalue — то, что можно присвоить, rvalue — то, что присваивается.

**Характеристики**:
- **lvalue**:
  - Имеет адрес: `&x` допустимо.
  - Примеры: Переменные, ссылки, разыменованные указатели.
  - Может быть модифицировано (если не `const`).
- **rvalue**:
  - Не имеет постоянного адреса (временный объект).
  - Примеры: Литералы (`5`), результаты выражений (`x + y`), возвращаемые временные объекты.
  - Обычно не модифицируется.

**Основное отличие**:
- **lvalue** можно присвоить значение, **rvalue** нельзя (оно временное).
- Пример:
  ```cpp
  int x = 5; // x — lvalue, 5 — rvalue
  x = 10; // OK
  // 5 = 10; // Ошибка
  ```

**Пример с rvalue-ссылками**:
```cpp
#include <iostream>
void func(int&& r) { std::cout << "rvalue: " << r << std::endl; }
void func(int& l) { std::cout << "lvalue: " << l << std::endl; }

int main() {
    int x = 5;
    func(x); // lvalue: 5
    func(10); // rvalue: 10
    return 0;
}
```

**Пояснение**:  
rvalue-ссылки (`&&`) и lvalue-ссылки (`&`) позволяют эффективно обрабатывать временные и постоянные объекты, особенно в контексте перемещения.

---

#### 24. Для структуры foo определены только следующие операторы:  
```cpp
struct foo {
    foo& operator=(const foo&);
    foo operator/(foo, foo);
};
foo operator-(foo, foo);
```
Нарисуйте дерево синтаксического разбора для выражения `v = v – f/(v – f);`, где переменные `f`, `v` имеют тип `foo`. Запишите это выражение через явный вызов операторов.

**Определение**:  
Дерево синтаксического разбора (parse tree) показывает порядок выполнения операций в выражении с учетом приоритетов и ассоциативности операторов.

**Приоритеты**:
- `/` имеет более высокий приоритет, чем `-`.
- `=` имеет самый низкий приоритет.
- Операторы `/` и `-` левоассоциативны.

**Выражение**: `v = v – f/(v – f)`

**Дерево синтаксического разбора**:
```
         =
        / \
       v   -
          / \
         v   /
            / \
           f   -
              / \
             v   f
```

**Пошаговый разбор**:
1. `v – f`: Вычисляется первым в скобках, вызывает `operator-(v, f)`.
2. `f / (v – f)`: Деление, вызывает `operator/(f, result_of_step_1)`.
3. `v – result_of_step_2`: Вычитание, вызывает `operator-(v, result_of_step_2)`.
4. `v = result_of_step_3`: Присваивание, вызывает `v.operator=(result_of_step_3)`.

**Явный вызов операторов**:
```cpp
v.operator=(operator-(v, operator/(f, operator-(v, f))));
```

**Пример кода**:
```cpp
#include <iostream>
struct foo {
    int value; // Для демонстрации
    foo(int v = 0) : value(v) {}
    foo& operator=(const foo& other) {
        value = other.value;
        return *this;
    }
    friend foo operator/(foo a, foo b) { return foo(a.value / b.value); }
};
foo operator-(foo a, foo b) { return foo(a.value - b.value); }

int main() {
    foo v(10), f(2);
    v = v - f / (v - f); // v = 10 - 2 / (10 - 2) = 10 - 2 / 8 = 10 - 0 = 10
    std::cout << v.value << std::endl; // 10
    // Явный вызов:
    v.operator=(operator-(v, operator/(f, operator-(v, f))));
    std::cout << v.value << std::endl; // 10
    return 0;
}
```

**Пояснение**:  
Дерево отражает порядок операций, а явный вызов демонстрирует, как компилятор интерпретирует выражение. Ассоциативность и приоритеты операторов критически важны для корректного разбора.

---



