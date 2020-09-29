# RegExp

Сайт для проверки: [https://regex101.com/](https://regex101.com/)<br />
Неплохой курс [регулярные выражения][1] в Python
[1]:(https://www.youtube.com/watch?v=1SWGdyVwN3E&list=PLA0M1Bcd0w8w8gtWzf9YkfAxFCgDb09pA)

`.` - любой одиночный символ (кроме `\n`)

`[]` - любой из символов, диапазон, набор символов (символы `"(", ")", ".", "?"` внутри квадратных скобок будут восприниматься как есть, кроме `\`)

`()` - группирующие сохраняющие скобки

`$` - конец строки

`^` - начало строки

`\` - экранирование

`\n` и `\t` - тоже используются

`\d` - любая цифра

`\D` - всё, кроме цифр

`\s` - любой пробельный символ (`" ", \t, \n, \r, \f, \v`) `[ \t\n\r\f\v]`

`\S` - любой не пробельный символ `[^ \t\n\r\f\v]`

`\w` - буква (любой символ слова)

`\W` - всё, кроме букв (любой не символ слова)

`\b` - граница слова

`\B` - не граница

## Квантификация (квантификаторы)

`{n,m}` - мажорный режим, ищет по возможности максимальное (n - минимальное количество совпадений, m - максимальное)

`{n,m}?` - минорный (жадный) режим, ищет по возможности минимальное (n - минимальное количество совпадений, m - максимальное)

`{m}` - короткая запись, эквивалент `{m,m}`

`{m,}` - от m и более раз

`{,n}` - не более n раз

`{m,}?` и `{,n}?` - тоже можно

`n{4}` - искать n 4 раза подряд

`n{4,6}` - искать n от 4 до 6 раз

`*` или `{0,}` - от 0 раз и выше (минорный: `*?`)

`+` или `{1,}` - от 1 и выше (минорный: `+?`)

`?` или `{0,1}` - 0 или 1 раз (минорный: `??`)

`^` - не (перед символами внутри [])

## Примеры

Все примеры будут **на Python**

Ищем **набор символов** в строке:

``` Python3
import re

text = "Карта map и объект bitmap - это разные вещи"

match = re.findall("map", text)
print(match)  # ['map', 'map']
```

Ищем **целое слово** в той же строке (*см. выше*):

``` Python3
match = re.findall(r"\bmap\b", text)
print(match)  # ['map']
```

**Экранируем символы** скобок:

``` Python3
import re

text = "map и (map) - это разные вещи"

match = re.findall(r"\(map\)", text)
print(match)  # ['(map)']
```

Используем **набор символов []**:

``` Python3
import re

text = "Еда, беду, победа"

match = re.findall(r"[еЕ]д[ау]", text)
print(match)  # ['Еда', 'еду', 'еда']
```

**Диапазон символов** (*см. выше*):

``` Python3
import re


text_1 = "Еда, беду, 55 победа"

match_1 = re.findall(r"[0123456789]", text_1)  # [] - каждый раз всего один из ...
print(match_1)  # ['5', '5']


text_2 = "Еда, беду, 5 55 победа"

# [0-9][0-9] - то же, что и: [0123456789][0123456789]
match_2 = re.findall(r"[0-9][0-9]", text_2)  # [][] - ищет 2 символа
print(match_2)  # ['55']


text_3 = "Еда, беду, -5 55 победа"

# [-0-9] - минус или цифра
match_3 = re.findall(r"[-0-9][0-9]", text_3)
print(match_3)  # ['-5', '55']


text_4 = "Еда, -5 55"

# [^0-9] - не цифра
match_4 = re.findall(r"[^0-9]", text_4)
print(match_4)  # ['Е', 'д', 'а', ',', ' ', '-', ' ']
```

Для **шестнадцатеричных чисел**:

``` Python3
import re

text = "0xf, 0xa, 0x5"

match = re.findall(r"0x[\da-fA-F]", text)
print(match)  # ['0xf', '0xa', '0x5']
```

**Квантификаторы {}**:

``` Python3
import re

text = "Google, Gooogle, Goooooogle"  # 'o' - 2, 3, 6

# мажорный
match1 = re.findall(r"o{2,5}", text)
print(match1)  # ['oo', 'ooo', 'ooooo'] ('o' - 2, 3, 5)


# минорный
match2 = re.findall(r"o{2,5}?", text)
print(match2)  # ['oo', 'oo', 'oo', 'oo', 'oo'] (по одной паре из 1 и 2 слова и 3 пары из 3-го)
```

Простая проверка номера телефона:

``` Python3
import re

phone = "89123456789"

match = re.findall(r"8\d{10}", phone)
print(match)  # ['89123456789']
```

Необязательная буква:

``` Python3
import re

text = "стеклянный, стекляный"

match = re.findall(r"стеклянн?ый", text)  # вторая 'н' - не обязательна
print(match)  # ['стеклянный', 'стекляный']
```

Разобрать по ключам и значениям:

``` Python3
import re

text = "author=Пушкин А.С.; title = Евгений Онегин; price =200; year= 2001"

match_1 = re.findall(r"\w+\s*=\s*[^;]+", text)
print(match_1)  # ['author=Пушкин А.С.', 'title = Евгений Онегин', 'price =200', 'year= 2001']


match_2 = re.findall(r"(\w+)\s*=\s*([^;]+)", text)
print(match_2)  # [('author', 'Пушкин А.С.'), ('title', 'Евгений Онегин'), ('price', '200'), ('year', '2001')]
```

**Минорный квантификатор**:

``` Python3
import re

text = "<p>Картинка <img src='bg.jpg'> в тексте</p>"

# мажорный
match_1 = re.findall(r"<img.*>", text)
print(match_1)  # ["<img src='bg.jpg'> в тексте</p>"]

# минорный
match_2 = re.findall(r"<img.*?>", text)
print(match_2)  # ["<img src='bg.jpg'>"]

# мажорный аналог минорного
match_3 = re.findall(r"<img[^>]*>", text)
print(match_3)  # ["<img src='bg.jpg'>"]
```

Более правильный вариант, который учитывает наличие `src` (*см. выше*):

``` Python3
import re

text_1 = "<p>Картинка <img> в тексте</p>"
text_2 = "<p>Картинка <img src='bg.jpg'> в тексте</p>"
text_3 = "<p>Картинка <img alt='картинка'  src='bg.jpg'> в тексте</p>"

match_1 = re.findall(r"<img\s+[^>]*?src\s*[^>]+>", text_1)
print(match_1)  # []

match_2 = re.findall(r"<img\s+[^>]*?src\s*[^>]+>", text_2)
print(match_2)  # ["<img src='bg.jpg'>"]

match_3 = re.findall(r"<img\s+[^>]*?src\s*[^>]+>", text_3)
print(match_3)  # ["<img alt='картинка'  src='bg.jpg'>"]
```














**Шаблон**:

``` Python3
import re

text = ""

match = re.findall(r"", text)
print(match)  # 
```
