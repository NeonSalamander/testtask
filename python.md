## Определите класс car с двумя атрибутами: color и speed. Затем создайте экземпляр и верните speed

```
class Car:
    def __init__(self, color, speed):
        self.color = color
        self.speed = speed

    def get_speed(self):
        return self.speed


car = Car('Red', 120)
speed = car.get_speed()
print(speed)
```

## Что выведет следующая команда:
Оператор целочисленного деления, результат выполнения 2

## Добавить новое значение (6) в объект списка:  list_example = [1,2,3,4,5] 

```
list_example.append(6)
```

## Где вы будете использовать while вместо for?
for там где границы потенциальной итерации известны
while там где границы не изветстны либо до возникнования какого либо события

## Напишите программу на Python для проверки, является ли число простым (ввод числа пользователем). 

```
def is_nature_number(number):
    count = 0
    for i in range(1, number + 1):
        if number % i == 0:
            count += 1
    return True if count == 2 else False

n = int(input('Введите число: '))
number = is_nature_number(n)
print(number)
```

## Напишите программу на Python, чтобы создать треугольник из звезд (вывести треугольник)

```
n = 19
for i in range(0, n):
    print(' ' * i + '*' * (n - i * 2) + ' ' * i)
```

	
## Как можно загрузить данные с сайта (таблица), если нет api?
Если нет api то парсить вывод, в зависимости от формата(html, csv, xml и т.д.) применять необходимые средства для обработки данных	
Таблицу без заголовков обычно беру так (при использовании beautifulsoup)

## нахожу таблицу по классу/id/номеру

```
table = soup.find('table', class_='table class')
# в цикле беру только строки таблицы
for row in table.tbody.find_all('tr'): 
```

## А что делать, если на сайте требуется авторизация?
Что делать если на сайте требуется аутентификация, смотря какая, для начала нужно определить
что ожидают на сайте и исходя из этого делать аутентификацию, передавая в заголовках запроса данные аутентификации токены, 
заполняя формы логина и другие варианты


## Напишите программу, которая будет обращаться к файлу Excel в локальной папке, извлекать новые данные с датой добавления на вчера (поле upd_date) и отправлять их на почту.
(пример абстрактный но подход ясен)

```
from datetime import datetime, timedelta
import pandas as pd
import smtplib
from email.mime.text import MIMEText

yesterday = datetime.now() + timedelta(days=-1)
yesterday_true_format = yesterday.strftime('%d.%m.%Y')
df = pd.read_excel('file.xlsx', sheet_name=0)
df = df[(df['upd_date']==yesterday_true_format) ]

sender = 'admin@example.com'
receivers = ['info@example.com']

port = 1025
msg = MIMEText(df)

msg['Subject'] = 'Result'
msg['From'] = sender
msg['To'] = receivers

with smtplib.SMTP('адрес почтового сервера', port) as server:
    server.sendmail(sender, receivers, msg.as_string())
    print("Успешная отправка")
```	
	

## Что выведет следующий код? 	
Выведет 23, значение переменной а


## Получить список имён файлов в локальной папке S:/Files, которые содержат в названии 'УД'.
```
from pathlib import Path
files = sorted(Path('S:/Files').glob('*УД*.*'))
print(files)
```

## Вам дан список [1,2,3,4,5,6] . Верните аналогичный список, где каждый элемент возведён во вторую степень.
```
l = [1, 2, 3, 4, 5, 6]
res = list(map(lambda x: x ** 2, l))
print(res)
```

## Вам необходимо подключиться к бд Oracle и вывести пять первых строчек из таблицы clients схемы risk_management
обычно и как правило использую ORM но в случаях когда нужны сложные запросы на которых ORM может вести себя не так как надо
можно использовать нативные sql запросы
```
import cx_Oracle

dsn = cx_Oracle.makedsn('10.95.23.432', 777, service_name='prod')
conn = cx_Oracle.connect(user=r'akr', password='123', dsn=dsn)

c = conn.cursor()
c.execute('select * from risk_management.clients FETCH NEXT 5 ROWS ONLY;')
for row in c:
    print (row)
conn.close()
```
