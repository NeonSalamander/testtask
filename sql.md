## Задача 1
```
with es as (
select pay.*,
       row_number() over (PARTITION BY email, sub_id order by pay.sub_id) as every_second,
       pay.date - (cast( extract(isodow from pay.date) as integer) - 1) as week_begin
from public.payments as pay)
select * from es  where es.every_second = 2
```

## Задача 2
```
with activity_clients AS (
select ac.*, date_part('month', ac.report_month) as month_num, rank() OVER (PARTITION BY client_id ORDER BY report_month)
from active_clients as ac)
, result_activity as (select ac.*,
    (select count(ac1.client_id)
        from active_clients ac1
            where ac1.report_month = (ac.report_month + interval '1 month')
              or ac1.report_month = (ac.report_month + interval '2 month')
              or ac1.report_month = (ac.report_month + interval '3 month')
            and   (ac1.client_id = ac.client_id)
            ) as nextmonth
from activity_clients as ac)
select month_number.*,
       count(*) filter ( where ra.nextmonth = 0 ) as refused,
       count(*) filter ( where ra.nextmonth <> 0 ) as active
from generate_series(1, 12) month_number
    left join result_activity as ra on ra.month_num = month_number.month_number
group by month_number
```


## Как ускорить выполнение скрипта SQL?
Писать без лишнего усложнения, стараться избегать подзапросов, использовать индексацию, использовать максимум условий

## Как восстановить удаленные объекты в БД?
Зависит от конкретной реализации СУБД и модели восстановления

## Почему не выполнится этот запрос? SELECT user_name, YEAR(user_birth_date) AS year_of_birth FROM users WHERE year_of_birth = 2000
Потому что в секции WHERE надо использовать имена существующих полей

## В чем отличие между запросам? Какой может вернуть больше строк?
Первый вариант вернет больше строк так как условие накладывается уже после выполненного соединения таблиц

## Есть таблица 1 и 2. Как получить таблицу 3?
Использовать UNION без ALL что бы удалить дубли
