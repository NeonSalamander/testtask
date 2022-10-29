```
with es as (
select pay.*,
       row_number() over (PARTITION BY email, sub_id order by pay.sub_id) as every_second,
       pay.date - (cast( extract(isodow from pay.date) as integer) - 1) as week_begin
from public.payments as pay)
select * from es  where es.every_second = 2
```

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
