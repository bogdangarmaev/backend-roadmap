# Python 3.9
Мои конспекты по [документации python 3.9](https://docs.python.org/3/).

Здесь записываю наиболее редко встречающиеся тонкости и то что любят спрашивать на собеседовании ;)

_________
# Кодировка
По умолчанию Python использует наиболее популярную кодировку UTF-8. Она пригодна для использования
в большинстве языков. Чтобы объявить иную кодировку используйте следующий синтаксис:
```python
# -*- coding: encoding -*-
```
Например, для использования кодировки Windows-1252, первая линия документа должна быть написана 
следующим образом:
```python
# -*- coding: cp1252 -*-
```
Единственным исключением является тот случай, когда мы напрямую хотим сделать файл исполняемым
в Unix - подобных системах:
```python
#!/usr/bin/env python3
# -*- coding: cp1252 -*-
```
В Windows не требуется этого делать, так как файлы .py автоматически ассоциируются с интерпретатором.
_________________
# Синтаксис комментирования кода:
```python
# первый комментарий
spam = 1  # второй комментарий
          # третий комментарий
text = "# данный текст не является комментарием"
```
___________________
# Некоторые особенности работы python с некоторыми типами данных.
В том случае если в математических операциях присутствует float и int числа, для проведениях корректных
операций python будет приводит int к float:
```jupyterpython
>>> 4 * 3.75 - 1
14.0
```
В интерактивном режиме последнее значение, которое было выведено на печать сохраняется в переменную _,
данная переменная предназначена только для чтения:
```jupyterpython
>>> tax = 12.5 / 100
>>> price = 100.50
>>> price * tax
12.5625
>>> price + _
113.0625
>>> round(_, 2)
113.06
```
Строковые данные можно писать в несколько строк можно писать оборачивая в тройные кавычки:
```jupyterpython
print("""\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
```
либо писать каждую строчку в отдельных кавычках, они автоматически сконкатенируются:
```jupyterpython
>>> text = ('Put several strings within parentheses '
...         'to have them joined together.')
>>> text
'Put several strings within parentheses to have them joined together.'
```
Это связано с тем, что две строки сподряд автоматически конкатенируются:
```jupyterpython
>>> 'Py' 'thon'
'Python'
```
Можно менять отдельные срезы в листах: 
```jupyterpython
>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> letters
['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> # заменим значения в срезе
>>> letters[2:5] = ['C', 'D', 'E']
>>> letters
['a', 'b', 'C', 'D', 'E', 'f', 'g']
>>> # удалим срез значений в середине
>>> letters[2:5] = []
>>> letters
['a', 'b', 'f', 'g']
>>> # полностью очистим лист
>>> letters[:] = []
>>> letters
[]
```
Код, который меняет список по которому итерируется, может работать неверно. Чтобы не допустить 
этого необходимо скопировать список или создать новый список:
```jupyterpython
# Способ итерации по скопированному листу
for user, status in users.copy().items():
    if status == 'inactive':
        del users[user]

# Создание новой коллекции 
active_users = {}
for user, status in users.items():
    if status == 'active':
        active_users[user] = status
```
В python существуют итерабельные объекты, то есть это объекты которые могут выдавать какую-либо
последовательность, например функция range() возвращает итерабельный объект.
```jupyterpython
>>> sum(range(4))  # 0 + 1 + 2 + 3
6
```
Дефолтные значения функции вычисляются единожды и в месте и области определения функции:
```jupyterpython
i = 5
def f(arg=i):
    print(arg)
i = 6
f()
5
```
Данное свойство может вызывать интересное поведение когда дефолтное значение мутабельное:
```jupyterpython
def f(L = [])
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))
[1]
[1, 2]
[1, 2, 3]

```
Аргументы функции можно обозначить как только позиционные или только ключ-значение.
Для достижение данного эффекта необходимо поместить все только позиционные аргументы перед
знаком /, аргументы после данного знака могут быть любыми кроме position-only. Для
обозначения аргументов которые могут быть только ключ-значение необходимо поместить их после 
знака *.
```jupyterpython
>>> def standard_arg(arg):
...     print(arg)
...
>>> def pos_only_arg(arg, /):
...     print(arg)
...
>>> def kwd_only_arg(*, arg):
...     print(arg)
...
>>> def combined_example(pos_only, /, standard, *, kwd_only):
...     print(pos_only, standard, kwd_only)
```
В python можно легко создать очереди, для этого необходимо использовать collections.deque.
```jupyterpython
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])
```
При использовании list comprehension можно использовать оператор walrus (оператор моржа) для 
запуска выражения с одновременным назначением выходного значения переменной:
```jupyterpython
>>> import random
>>> def get_weather_data():
...     return random.randrange(90, 110)
>>> hot_temps = [temp for _ in range(20) if (temp := get_weather_data()) >= 100]
>>> hot_temps
[107, 102, 109, 104, 107, 109, 108, 101, 104]
```
Вообще оператор моржа начиная с python 3.8 можно использовать для присвоения значения
переменной в выражении:
```jupyterpython
while chunk := fp.read(200):
   print(chunk)
```
Отличие дандер - метода класса __new__ от __init__. При создании экземпляра класса
сначала вызывается метод __new__, который возвращает экземпляр класса, после чего
вызывается метод __init__ в который передаются аргументы. На основе данного механизма
можно выделить примеры использования метода __new__. Реализация паттерна синглтон:
```jupyterpython
class Singleton(object):
    _instance = None  # Keep instance reference 
    
    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = object.__new__(cls, *args, **kwargs)
        return cls._instance
```
Таким образом, при первом создании экземпляра класса он будет записан в свойство 
класса и при следующих созданиях экземпляров будет возвращаться созданный в первый
раз инстанс.
\
Создать класс можно с использованием функции type:
```jupyterpython
>>> type('A', (object,), {})
<class '__main__.A'>
```
Чтобы создать экземпляр класса необходимо вызвать этот самый класс. Но для создания 
класса Python в свою очередь должен вызвать метакласс. Таким образом, мы можем создать
свой метакласс использовав подкласс type.
```jupyterpython
>>> class A(object):
...     __metaclass__ = Meta
```
Данный код равносилен:
```jupyterpython
A = Meta('A', (object,), {})
```
Сборщиком мусора (Garbage collector) можно управлять напрямую с помощью модуля gc.


