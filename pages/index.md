---
title: Welcome to Evidence
---

<Details title='How to edit this page'>

  This page can be found in your project at `/pages/index.md`. Make a change to the markdown file and save it to see the change take effect in your browser.
</Details>

### BigQuery Orders Dashboard

```revenue_by_category
select
  category,
  sum(revenue) as total_revenue,
  count(*) as order_count
from bg_test.orders
group by category
order by total_revenue desc
```

```daily_revenue
select
  order_date,
  sum(revenue) as total_revenue,
  count(*) as order_count
from bg_test.orders
group by order_date
order by order_date
```

```summary
select
  count(*) as total_orders,
  sum(revenue) as total_revenue,
  avg(revenue) as avg_revenue
from bg_test.orders
```

<BigValue
  data={summary}
  value=total_orders
  title="Total Orders"
/>
<BigValue
  data={summary}
  value=total_revenue
  title="Total Revenue"
  fmt=usd2
/>
<BigValue
  data={summary}
  value=avg_revenue
  title="Avg Order Value"
  fmt=usd2
/>

<Grid cols=2>
  <Group>

  #### Revenue by Category
  <BarChart
      data={revenue_by_category}
      x=category
      y=total_revenue
      yFmt=usd2
      xAxisTitle="Category"
      yAxisTitle="Revenue"
  />

  </Group>
  <Group>

  #### Orders by Category
  <ECharts config={
    {
      tooltip: { trigger: 'item' },
      series: [
        {
          type: 'pie',
          data: revenue_by_category.map(row => ({
            name: row.category,
            value: row.order_count
          }))
        }
      ]
    }
  }/>

  </Group>
  <Group>

  #### Daily Revenue
  <LineChart
      data={daily_revenue}
      x=order_date
      y=total_revenue
      yFmt=usd2
      xAxisTitle="Date"
      yAxisTitle="Revenue"
  />

  </Group>
  <Group>

  #### Daily Order Count
  <AreaChart
      data={daily_revenue}
      x=order_date
      y=order_count
      xAxisTitle="Date"
      yAxisTitle="Orders"
  />

  </Group>
</Grid>
