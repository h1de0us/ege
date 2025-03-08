---
layout: note
title: "задание 24: регулярные выражения"
---

[читлист](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)

[пример задачи на сложение и умножение](https://education.yandex.ru/ege/task/d1b752f8-587f-47c2-8187-7975a7684715)

```python
import re

line = open('24.txt', 'r').readline()

number = r'[1-9]\d*' # цифра от одного до девяти [1-9] + одна любая цифра \d сколько угодно раз (*)
# нулевое произведение: something * 0, 0 * something, something * 0 * something
# 0 обязательно должен быть

# делаем something слева:
# {number}\* - ненулевое число (number) и звездочка (\*)
# {number}\* | 0\* - ненулевое число и звездочка ИЛИ ноль и звездочка (перед звездочками стоит \)
# круглые скобки и ?: означают группу - (?:{number}\*|0\*)
# (?:{number}\*|0\*)* - повторяем группу сколько угодно раз (последняя звездочка без *)

# добавляем 0 в середину: (?:{number}\*|0\*)*0

# делаем something справа
# \*{number} - звездочка и ненулевое число
# \*{number} | \*0 - звездочка и ненулевое число ИЛИ звездочка и ноль
# скобки и ?: -  (?:\*{number}|\*0) - сделали группу
# (?:\*{number}|\*0)* - повторяем группу сколько угодно раз (последняя звездочка без *)

zero_product = rf'(?:{number}\*|0\*)*0(?:\*{number}|\*0)*'

# делаем сумму из нулевых произведений:
# сумма = zero_product и несколько плюсов вместо с zero_product (опционально)

zero_sum = rf'{zero_product}(?:\+{zero_product})*'

results = re.findall(zero_sum, line)

answer = 0
for res in results:
    answer = max(answer, len(res))
print(answer)
```

[пример задачи на вычитание и умножение](https://education.yandex.ru/ege/task/60136ef5-552a-4bc0-9224-2914e321d93b)

```python
line = open('24.txt', 'r').readline()

import re

number = r'[1-9]\d*'
product = rf'-?{number}(?:\*{number}|\*0)*'
diff = rf'{product}(?:-{product}|-0)*'

results = re.findall(diff, line)
answer = 0
for res in results:
    answer = max(answer, len(res))
print(answer)
```

[пример задачи про цепочку из повторяющихся фрагментов](https://education.yandex.ru/ege/task/543670ff-52fb-4d26-86f8-c82350ecb28b)

```python
import re

line = open('24.txt', 'r').readline()
pattern = r'(?:XA|XY|TXA)*'
parts = re.findall(pattern, line)
print(max(len(part) for part in parts))
```

[пример задачи про тройки символов](https://education.yandex.ru/ege/task/6f3cbabe-5d81-415e-8f8f-2f6c1c3ff308)

```python
import re

line = open('24.txt', 'r').readline()
pattern = r'(?:[1-9]\d[A-Za-z])*'
parts = re.findall(pattern, line)
print(max(len(part) for part in parts) // 3)
```

[пример задачи про пары символов](https://education.yandex.ru/ege/task/f05f88d6-f4c2-4bc5-84bd-d9e7ffffb9bd)
```python
import re

line = open('24.txt', 'r').readline()
pattern = r'(?:[A-C][A-C])*'
parts = re.findall(pattern, line)
print(max(len(part) for part in parts) // 2)
```

[задача про yandex](https://education.yandex.ru/ege/task/01dfb7d5-1b71-470d-adf4-612e4adaaf7e)

```python
import re

f = open('24.txt', 'r')
line = f.readline()

regex = r'(?:y(?:4|a)nd(?:3|e)x)+'

pattern = re.compile(regex)
span_start = 0
res = True
answer = 0
end = 0
while res:
    res = pattern.search(line, pos=span_start)
    if res:
        if len(res[0]) > answer:
            answer = max(len(res[0]), answer)
            end = res.end()
        span_start = res.start() + 1
print(answer, end)
print(line[end:end + 6])
``` 

[задача про число, не соответствующее маскам](https://education.yandex.ru/ege/task/a7d40d31-e448-4fe4-bcbd-6fb94af87a43)

```python
import re

mask1 = r'\d*0\d3\d*'
mask2 = r'\d*4\d2'
mask3 = r'\d*1\d*'

count = 0
for n in range(700000 + 1, 10 ** 10):
    parts1 = re.findall(mask1, str(n))
    parts2 = re.findall(mask2, str(n))
    parts3 = re.findall(mask3, str(n))
    if parts1 and parts1[0] == str(n):
        continue
    if parts2 and parts2[0] == str(n):
        continue
    if parts3 and parts3[0] == str(n):
        continue

    if n % 13 == 0:
        count += 1
        print(n, sum(int(digit) for digit in str(n)), sep='\t')

    if count == 5:
        break
```