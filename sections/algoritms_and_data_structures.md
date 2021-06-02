# Алгоритмы и структуры данных 
### Алгоритмы
##### Сортировки
Квадратичные сортировки - сортировки требующие затрат O(n ** 2)
- Сортировка вставками (insert sort)\
Берутся минимальные срезы элементов слева, и в данные срезы вставляются элементы
  справа и смещаются влево, до тех пор пор пока не встретят меньшее себе значение.
```python
l = [3, 5, 6, 1, 4, 2]
for index, item in enumerate(l):
    # итерируемся по элементам находящимся слева, справа налево
    for index_of_comparing_item in range(index, 0, -1):
        index_of_item_to_compare = index_of_comparing_item - 1
        if item < l[index_of_item_to_compare]:
            # сдвигаем вправо элемент, если число сравниваемое число больше
            l.insert(index_of_item_to_compare, l.pop(index_of_item_to_compare))
        else:
            # прерываем цикл, если наткнулись на меньшее число
            break

```
- Сортировка выбором(choice sort)\
Заключается в сравнени каждого элемента со всеми элементами. Берутся элементы 
  слева направо и при сравнении на место данного элемента ставится меньший элемент
  пока список не закончиться.
  
```python
l = [3, 5, 6, 1, 4, 2]
for index, item in enumerate(l):
    # итерируемся по элементам слева направо
    for index_of_comparing_item in range(index + 1, len(l)):
        # если текущий элемент больше сравниваемого, то меняем их местами
        if item > l[index_of_comparing_item]:
            l[index] = l[index_of_comparing_item]
            l[index_of_comparing_item] = item
```
