# Typing, аннотации типов

Как вы заметили, Python позволяет писать код, не заботясь об указании типов переменных, аргументов, возвращаемых значений и пр. 

Это отличная фича, но если ей активно пользоваться, то большой проект, с кучей классов, методов и функций через какое-то время станет невозможно поддерживать. 

Разберемся, как с этим бороться.

## Введение:

Существует самая обычная функция:

```python

def sum_val(a, b):
    return a + b

```

Очевидно, функция складывает 2 числа и возвращает результат. Представим ситуацию, когда очевидно это только для вас, а другой разработчик воспользуется функцией не по назначению:

```python
a = sum_val('a', 'b')
print(a) # -> 'ab'
```

или еще хуже:

```python
a = sum_val([False], [True])
print(a) # -> '[False, True]'
```

И будет думать, что так и задумывалось. Давайте явно укажем, что функция предполагает операции с числами (int):

```python
def sum_val(a: int, b: int) -> int:
    return a + b
```

Отлично! Что же мы сделали в примере выше? Аргументы `a, b` принимают значение типа `int` и возвращают `int`. Конечно, мы все еще может передать и получать любые значения, но IDE и статические анализаторы (о них позже) укажут нам на ошибку.

## Как аннотировать?

### Функции и методы

```python
def func(argument: type) -> return_type:
    ...
```

```python
def find_max_int(numbers: list[int]) -> int:
    return max(numbers)
```

### Аннотации переменных:

```python
a: str = 'Hello World!'
b: int = 123 + 2 * 2 - 11
c: bool = 12 > 5
d: list[str] = ['a', 'b', 'c', 'd']
e: set[str] = set(['a', 'a', 'b', 'a'])
f: dict[str, str] = {'name': 'Nikolas', 'OS': 'MacOS', 'language': 'Python'}
```

### Аннотации в классах:

```python
class User:
    def __init__(self, username: str, age: int, coder: bool) -> None:
        self.username = username
        self.age = age
        self.coder = coder

def get_random_user() -> User:
    return User(username='timofeev41', age=20, coder=True)

user: User = get_random_user()
```

## Сложные аннотации

Представим, что нам нужно хранить словарь, кортеж или список элементов:

```python
user = {'name': 'Nikolas', 'age': 20, 'coder': True}

calculations = (123.2, 22, 12.3)

male_names = ['Nikolas', 'Eldar', 'Rawman']

```

### Аннотации словарей:

Для аннотации можно использовать `dict` (python 3.8+) или `typing.Dict` (остальные случаи)

Рассмотрим конкретные примеры:

```python
a: dict[str, str] = {}
b: dict[str, int] = {}
c: dict[str, str | int] = {}
```

Эта аннотация означает, что в словаре `a` ключи и значения принимают значения типа `str`. В словаре `b` ключи принимают значения типа `str`, а значения имеют тип `int`, в словаре `c` примере мы указываем, что значения могут быть как целыми числами (`int`), так и строками (`str`).

```python
d: dict[str, dict[str, str]] = {'timofeev41': {'name': 'Nikolas'}}
```

Не стоит забывать, что значения могут быть и словарями! Как в примере выше.


### Аннотации кортежей:

Для каждой позиции нужно указывать тип значения (как на примере ниже)

```python
point: tuple(int, int, int) = tuple(1, 2, 3)
```

### Аннотации списков:

Со списками все совсем просто, ниже приведет пример списка строк:

```python
male_names: list[str] = ['Nikolas', 'Eldar', 'Rawman']
```


### Аннотации множеств:

Аналогично `list`, только `set`:

```python
unique_names: set[str] = set(male_names)
```

### Аннотации None:

Представим, что наша функция может возвращать значение или None, как расставить аннотации в таком случае? Пример:

```python
def func(a: int):
    if a > 100:
        return 'Success'
    return None
```

Видим, что функция возвращает строку, в случае если `a > 100`, в противном случае мы получим `None`. Аннотация будет выглядеть так:

```python
def func(a: int) -> str | None:
    if a > 100:
        return 'Success'
    return None
```

`-> str | None` указывает, что в качестве возвращаемого значения может быть либо строка, либо None.

Другой пример:

```python
username = requests.get('https://data.com/somedata').json()['username']

def process_user(username) -> None:
    if not username:
        return None
    save_user(username.lower())
```

Допустим, с сервера вместо `username` может прилететь None вместо строки с именем пользователя. Укажем это в аннотации:

```python
def process_user(username: str | None) -> None:
    if not username:
        return None
    save_user(username.lower())
```

Теперь `if not username` выглядит вполне логично.


### Аннотации typing.Any:

Иногда мы не знаем, какие значения можно получить. В таких случаях можно либо не ставить аннотацию, либо явно указать тип typing.Any

