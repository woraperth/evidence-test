## Hello Evidence

### Revenue Highlights

```revenue_summary
select sum(total_revenue) as total_revenue
from test_csv.test_csv
```

```revenue_by_month
with monthly as (
  select
    full_month_name,
    total_revenue,
    case full_month_name
      when 'July' then 7
      when 'August' then 8
      when 'September' then 9
      when 'October' then 10
      when 'November' then 11
      when 'December' then 12
    end as month_num
  from test_csv.test_csv
)
select
  make_date(2024, month_num, 1) as month,
  total_revenue,
  lag(total_revenue) over (order by month_num) as prev_revenue,
  (total_revenue - lag(total_revenue) over (order by month_num))
    / lag(total_revenue) over (order by month_num) as revenue_growth
from monthly
order by month_num desc
```

<BigValue
  data={revenue_summary}
  value=total_revenue
  title="Total Revenue"
  fmt=usd0
/>
<BigValue
  data={revenue_by_month}
  value=total_revenue
  title="Monthly Revenue"
  fmt=usd0
  comparison=revenue_growth
  comparisonFmt=pct1
  comparisonTitle="MoM"
  sparkline=month
/>

### Orders by Month

```orders_by_month
select order_month, count(*) as orders from needful_things.my_query
group by order_month order by order_month desc
limit 12
```
<BarChart
    data={orders_by_month}
    x=order_month
    y=orders
	xFmt="mmm yyyy"
	xAxisTitle="Month"
	yAxisTitle="Orders"
/>

### Orders Table

```my_query_summary_top100
select
   order_datetime,
   first_name,
   last_name,
   email
from needful_things.my_query
order by order_datetime desc
limit 100
```

<DataTable data={my_query_summary_top100}>
   <Column id=order_datetime title="Order Date"/>
   <Column id=first_name />
   <Column id=email />
</DataTable>