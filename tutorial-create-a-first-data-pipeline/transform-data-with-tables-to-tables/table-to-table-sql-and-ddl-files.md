---
description: >-
  Learn how to create the SQL file corresponding to the workflow tasks of a
  Table to Table data operation.
---

# SQL script file

### :map: Overview

A SQL workflow is a sequence of tasks that feed tables in parallel or sequentially. It gives instructions to load, merge and reorganize data.

### :oil: SQL tasks

SQL tasks are steps from the workflow. Each SQL task is defined with a .sql file that contains the query. You can write the queries directly in the query editor of [BigQuery](https://console.cloud.google.com/bigquery) and then save them to .sql files.

{% hint style="info" %}
There can be only one SQL query for each task. If a table need several SQL queries to be well loaded, you need to execute a task for each of them.
{% endhint %}

{% hint style="info" %}
The name of the SQL file should be the same as the SQL task.
{% endhint %}

### :video\_camera: SQL script video

From the sales, we will retrieve the weekly sales for each store for each category of alcohol sold and calculate the sales ratio as Pwisky + Prum + ... + Pspecial = Ptotal = 1.

```
#standardSQL
with week_categories as (
    select
        store_number,
        store_name,
        sale__dollars_ as sale_dollars,
        volume_sold__liters_ as volume_sold_liters,
        DATE_TRUNC(date, WEEK(monday)) as isoweek_monday,
        EXTRACT(isoweek from date) as isoweek_number,
        case when(LOWER(category_name) like '%vodka%') then 'vodka'
            when(LOWER(category_name) like '%whiskies%') then 'whisky'
            when(LOWER(category_name) like '%scotch%') then 'whisky'
            when(LOWER(category_name) like '%bourbon%') then 'whisky'
            when(LOWER(category_name) like '%rum%') then 'rum'
            when(LOWER(category_name) like '%liqueur%') then 'liqueur'
            when(LOWER(category_name) like '%triple sec%') then 'liqueur'
            when(LOWER(category_name) like '%tequila%') then 'tequila'
            when(LOWER(category_name) like '%schnapps%') then 'schnapps'
            when(LOWER(category_name) like '%gin%') then 'gin'
            when(LOWER(category_name) like '%cocktails%') then 'cocktail'
            when(LOWER(category_name) like '%brandies%') then 'brandy'
            when(LOWER(category_name) like '%mezcal%') then 'brandy'
            when(LOWER(category_name) like '%spirit%') then 'spirit'
            when(LOWER(category_name) like '%special%') then 'special_packages'
            else 'others' end as cat_alcool
    from `fd-io-jarvis-demo-dlk.dlk_demo_iowa_liquor_psa.sales_details_2019`
    where date >= '2019-01-01' and date < '2020-01-01'
)

select
    isoweek_monday,
    isoweek_number,
    store_name,
    CAST(store_number as string) as store_number,
    ROUND(SUM(sale_dollars), 2) as sale_dollars,
    ROUND(SUM(volume_sold_liters), 2) as volume_sold_liters,
    ROUND(
        SUM(
            case when(cat_alcool = 'vodka') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_vodka,
    ROUND(
        SUM(
            case when(cat_alcool = 'whisky') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_whisky,
    ROUND(
        SUM(
            case when(cat_alcool = 'rum') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_rum,
    ROUND(
        SUM(
            case when(cat_alcool = 'liqueur') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_liqueur,
    ROUND(
        SUM(
            case when(cat_alcool = 'tequila') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_tequila,
    ROUND(
        SUM(
            case when(cat_alcool = 'schnapps') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_schnapps,
    ROUND(
        SUM(
            case when(cat_alcool = 'gin') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_gin,
    ROUND(
        SUM(
            case when(cat_alcool = 'cocktail') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_cocktail,
    ROUND(
        SUM(
            case when(cat_alcool = 'brandy') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_brandy,
    ROUND(
        SUM(
            case when(cat_alcool = 'spirit') then sale_dollars else 0 end
        ) / SUM(sale_dollars),
        2
    ) as p_spirit,
    ROUND(
        SUM(
            case
                when(cat_alcool = 'special_packages') then sale_dollars else 0
            end
        ) / SUM(sale_dollars),
        2
    ) as p_special
from week_categories
group by store_number, store_name, isoweek_number, isoweek_monday

```

