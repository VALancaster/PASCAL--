# Лабораторная работа №2: Среда исполнения программ на языке Pascal--

---

## Техническое задание

### 1. Цель проекта

Разработка интегрированной среды разработки и исполнения программ на языке **Pascal--**, поддерживающей:

- написание и анализ кода;
- синтаксическую проверку;
- выполнение программ с пользовательским вводом и базовой логикой.

### 2. Функциональные требования

#### 2.1. Работа с текстом программы

- Представление текста в виде иерархического списка (приближённо — абстрактное синтаксическое дерево).
- Поддержка базовых конструкций Pascal--:
  - `const`, `var`, `begin` ... `end`, `if ... then ... else`, `Read`, `Write`.
  - Арифметические и логические выражения.
  - Вложенные блоки и условные операторы.

#### 2.2. Таблица идентификаторов

Хранение переменных и констант в одной из структур (на выбор):

- упорядоченная таблица;
- дерево поиска;
- хеш-таблица.

Каждая запись должна содержать:

- имя;
- тип (`integer`, `double`);
- значение.

#### 2.3. Вычисления и исполнение

- Выражения (арифметические и условные) преобразуются в постфиксную форму (Reverse Polish Notation).
- Исполнение производится с использованием стека.

Поддерживаемые операторы:

- Арифметические: `+`, `-`, `*`, `/`, `div`, `mod`.
- Логические: `=`, `<>`, `<`, `>`, `<=`, `>=`.

Исполняемые действия:

- Построчное выполнение с учётом вложенных блоков;
- `Read` — пользовательский ввод;
- `Write` — вывод значений.

#### 2.4. Синтаксический анализ

- Сборка синтаксического дерева.
- Проверка структуры программы:
  - отсутствие `begin/end`;
  - несоответствие типов;
  - недопустимые символы или конструкции;
  - неверные имена переменных и констант.

#### 2.5. Интерфейс пользователя

- Консольный (редактирование необязательно) или графический интерфейс (по желанию).
- Возможности:
  - загрузка и отображение программ;
  - запуск и вывод результатов;
  - отображение ошибок.

### 3. Пример работы

Пример исходной программы:

```pascal
const a = 5;
var x, y;
begin
  Read(x);
  y := a * x + 3;
  if y > 10 then
    Write(y)
  else
    Write(a)
end.
```
Ввод: x = 2  
Результат: Write -> 13  
(так как y = 5 * 2 + 3 = 13 > 10, выводится 13)

Ввод: x = 1  
Результат: Write -> 5  
(так как y = 8, выполняется else, выводится a = 5)

### 4. Нефункциональные требования

- Язык реализации: **C++**.
- Поддержка сборки в **Visual Studio**.
- Подключение **Google Test** для модульного тестирования.
- Структура проекта:
  - `Parser/` — синтаксический анализ;
  - `Executor/` — выполнение программ;
  - `SymbolTable/` — таблица идентификаторов;
  - `RPN/` — постфиксная форма и стек.

### 5. Распределение работ

| Участник           | Задачи                                                                 |
|--------------------|------------------------------------------------------------------------|
| Долов Вячеслав     | Таблица идентификаторов, структура программы |
| Константинов Семён  | Интерпретатор, пользовательский интерфейс         |
| Кутергин Валентин  | Постфиксный калькулятор, синтаксический разбор          |


### 6. План разработки

| Неделя | Задачи                                                                        |
|--------|--------------------------------------------------------------------------------|
| 1      | Подготовка технического задания, создание репозитория                         |
| 2      | Анализ предметной области, список объектов и алгоритмов                       |
| 3      | Проектирование классов, структура программы                                   |
| 4      | Заготовки классов, пустые юнит-тесты                                          |
| 5–7    | Реализация компонентов, тестирование, интеграция                              |
| 8      | Финальная проверка, документация, сдача проекта                               |

### 7. Требования к тестированию

Тестирование должно включать:

- Проверку всех операций с идентификаторами:
  - Добавление, поиск, извлечение;
  - Поведение при дублирующихся именах;
- Корректность вычислений:
  - Арифметические и логические выражения;
  - Преобразование в постфиксную форму;
  - Вычисление выражений, включая вложенные;
- Проверку интерпретации:
  - Выполнение вложенных блоков;
  - Обработка `Read` и `Write`;
- Обнаружение ошибок:
  - Деление на ноль;
  - Неинициализированные переменные;
  - Ошибки синтаксиса (`:=`, `;`, `begin/end`);
  - Недопустимые идентификаторы и символы.

### 8. Сдача проекта

- **Исходный код** — в репозитории **GitHub**.
- **Документация** (файл `README.md`) — с инструкцией по сборке и запуску.
- **Примеры программ** — для демонстрации возможностей.
- **Отчёт о тестировании** — с описанием покрытых случаев.

---

## Список объектов и основных алгоритмов


---

### 1. Класс `PostfixExecutor`

**Описание:** Выполняет выражения в постфиксной форме через стек.

**Методы:**

- `double Evaluate(const std::vector<Token>& postfix)` — вычисление значения;
- Поддержка переменных (доступ к таблице символов);
- Обработка ошибок: деление на ноль, неизвестная переменная и т.п.

---

### 2. Класс `HierarchicalList`

**Описание:** Представляют иерархический текст программы.

**Методы:**

- `void AddChild(ASTNode*)`;
- `void Execute()` — рекурсивное выполнение;
- `void CheckSyntax()` — проверка синтаксической корректности.

---

### 3. Класс `ProgramExecutor`

**Описание:** Главный интерпретатор — обходит дерево программы и выполняет действия.

**Методы:**

- `void Run(ASTNode* root)` — запуск исполнения;
- `void ExecuteBlock(ASTNode*)`, `void ExecuteIf(ASTNode*)` и т.п.;
- Использует `RPNExecutor` и `SymbolTable`.

### 4. Класс `Lexer`

**Описание:** Отвечает за лексический анализ программы — разбиение текста на лексемы.

**Типы токенов:**


enum class TokenType {
  Keyword, Identifier, Number, Operator, Separator, StringLiteral, EndOfFile, ...
};

### 5. Класс `Parser`

**Описание:** Выполняет парсинг и проверку на корректность грамматики.

**Методы:**

- `ASTNode* BuildAST(const std::vector<Token>& tokens)`
- `std::vector<Token> GenerateRPN(ASTNode* node)`

