---
layout: note
title: "задание 24: регулярные выражения"
---

[читлист](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)

[пример задачи](https://education.yandex.ru/ege/task/d1b752f8-587f-47c2-8187-7975a7684715)

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