# sql-80-exercises
-- 1
-- select
--     transaction_id,
--     amount,
--     transaction_ts
-- from core.fact_transaction
-- where amount_usd > 3000
-- order by amount_usd desc


-- 2
-- select
--     merchant_name,
--     category
-- from core.dim_merchant
-- where category = 'restaurants'
-- order by merchant_name asc


-- 3
-- select
--     customer_id,
--     full_name,
--     city
-- from core.dim_customer
-- where city = 'Bishkek'
-- order by customer_id


-- 4
-- select
--     transaction_id,
--     transaction_ts,
--     amount
-- from core.fact_transaction
-- where transaction_type = 'refund'
-- and status = 'approved'


-- 5
-- select
--     account_id,
--     account_type,
--     currency
-- from core.dim_account
-- where status = 'frozen'
-- order by account_id


-- 6
-- select
--     card_id,
--     card_brand,
--     card_type
-- from core.dim_card
-- where status = 'blocked'
-- order by card_id desc


-- 7
-- select
--     full_name,
--     risk_score
-- from core.dim_customer
-- where risk_score between 50 and 80


-- 8
-- select
--     transaction_id,
--     transaction_type,
--     channel_sk
-- from core.fact_transaction
-- where card_sk is null


-- 9
-- select
--     merchant_name,
--     category,
--     city
-- from core.dim_merchant
-- where city in ('Moscow','Dubai')
-- and category in ('electronics','fuel')


-- 10
-- select
--     card_id,
--     card_brand,
--     status
-- from core.dim_card
-- where card_brand like 'vi%'


-- 11
-- select
--     transaction_id,
--     amount,
--     transaction_ts
-- from core.fact_transaction
-- where amount between 100 and 500
-- and transaction_type = 'purchase'


-- 12
-- select
--     full_name,
--     city,
--     segment
-- from core.dim_customer
-- where is_current = 'true'
-- and city is not null
-- and segment != 'retail'


-- 13
-- select
--     count(*) as total_transactions,
--     sum(amount_usd) as total_amount
-- from core.fact_transaction
-- where status = 'approved'


-- 14
-- select
--     avg(risk_score) as avg_risk_score,
--     min(risk_score) as min_risk_score,
--     max(risk_score) as max_risk_score
-- from core.dim_customer
-- where is_current = 'true'


-- 15
-- select count(distinct currency)
-- from core.dim_account


-- 16
-- select
--     round(sum(fee),2) as sum_fee
--     from core.fact_transaction
-- where transaction_type = 'purchase'


-- 17
-- select
--     count(*) as active_cards
-- from core.dim_card
-- where status = 'active'
--
-- select
--     count(*) as blocked_cards
-- from core.dim_card
-- where status = 'blocked'


-- 18


-- 19
-- select
--     transaction_type,
-- count(*) as total_transactions,
-- avg(amount) as avg_amount
-- from core.fact_transaction
-- group by transaction_type
-- order by avg_amount desc


-- 20
-- select
--     channel_sk,
--     sum(amount_usd) as total_oborot
-- from core.fact_transaction
-- group by channel_sk
-- having sum(amount_usd) > 5000000


-- 21
-- select
--     account_type,
--     count(*) as total_account
-- from core.dim_account
-- group by account_type
-- having count(*) > 900


-- 22
-- select
--     country,
--     count(distinct category)
-- from core.dim_merchant
-- group by country
-- having count(distinct category) > 5


-- 23
-- select
--     segment,
--     count(*) as count_risk_score,
--     avg(risk_score) as avg_risk_score
-- from core.dim_customer
-- where is_current = 'true'
-- group by segment
-- having avg(risk_score) > 45


-- 24
-- select
--     count(*) as count_cus_id
-- from core.dim_customer
-- group by customer_id
-- having count(*) > 1


-- 25
-- select
--     ac.currency,
--     count(ac.account_id) as total_accounts,
--     sum(fa.balance_eod) as total_balance
-- from core.dim_account ac
-- join core.fact_account_balance_daily fa
--     on ac.account_sk = fa.account_sk
-- group by ac.currency
-- having count(ac.account_id) > 50


-- 26
-- select
--     ch.channel_name,
--     tr.transaction_id,
--     tr.amount
-- from core.dim_channel ch
-- inner join core.fact_transaction tr on ch.channel_sk = tr.channel_sk


-- 27
-- select
--     ac.account_id,
--     ac.account_type,
--     cu.full_name
-- from core.dim_account ac
-- inner join core.dim_customer cu
--     on ac.customer_id = cu.customer_id
-- where cu.is_current = true


-- 28
-- select
--     cu.full_name,
--     ac.account_sk
-- from core.dim_customer cu
-- left join core.dim_account ac
--     on cu.customer_id = ac.customer_id
-- where cu.is_current = true
--     and ac.account_sk is null


-- 29
-- select
--     tr.transaction_id,
--     tr.amount,
--     me.merchant_name
-- from core.fact_transaction tr
-- left join core.dim_merchant me
-- on tr.merchant_sk = me.merchant_sk


-- 30
-- select
--     ca.card_id,
--     ca.card_type,
--     ac.account_type
-- from core.dim_card ca
-- inner join core.dim_account ac
-- on ca.account_id = ac.account_id
-- where ac.status = 'active'


-- 31
-- select
--     ac.account_id,
--     ac.account_type
-- from core.dim_account ac
-- left join core.fact_transaction ft on ac.account_sk = ft.account_sk
-- where ft.transaction_id is null


-- 32
-- select
--     ft.transaction_id,
--     dc.channel_name,
--     dm.category
-- from core.fact_transaction ft
-- join core.dim_merchant dm on ft.merchant_sk = dm.merchant_sk
-- join core.dim_channel dc on ft.channel_sk = dc.channel_sk


-- 33
-- select
--     ft.transaction_id,
--     dc.full_name,
--     dm.merchant_name
-- from core.fact_transaction ft
-- join core.dim_account da
--     on ft.account_sk = da.account_sk
-- join core.dim_customer dc
--     on da.customer_id = dc.customer_id
-- join core.dim_merchant dm
--     on ft.merchant_sk = dm.merchant_sk


-- 34
-- select
--     dc.channel_name,
--     dm.merchant_name,
--     ft.is_fraud
-- from core.fact_transaction ft
-- join core.dim_channel dc
--     on ft.channel_sk = dc.channel_sk
-- left join core.dim_merchant dm
--     on ft.merchant_sk = dm.merchant_sk
-- where ft.is_fraud = true


-- 35
-- select
--     da.account_id,
--     dc.full_name,
--     count(cd.card_id) as total_cards
-- from core.dim_account da
-- join core.dim_customer dc
--     on da.customer_id = dc.customer_id
-- left join core.dim_card cd
--     on da.account_id = cd.account_id
-- group by da.account_id, dc.full_name


-- 36
-- select
--     dc.full_name,
--     count(distinct da.account_id) as total_accounts,
--     count(ft.transaction_id) as total_transactions
-- from core.dim_customer dc
-- join core.dim_account da
-- on dc.customer_id = da.customer_id
-- join core.fact_transaction ft
-- on da.account_sk = ft.account_sk
-- where dc.is_current = true
-- group by dc.full_name


-- 37
-- select
--     dm.merchant_name,
--     dm.category,
--     avg(ft.amount) as avg_amount
-- from core.fact_transaction ft
-- join core.dim_merchant dm
-- on ft.merchant_sk = dm.merchant_sk
-- group by dm.merchant_name, dm.category
-- having avg(ft.amount) > 2500


-- 38
-- select
--     dc.channel_name,
--     count(ft.transaction_id) as total_transactions,
--     sum(ft.amount_usd) as sum_amount_usd
-- from core.fact_transaction ft
-- join core.dim_channel dc
-- on ft.channel_sk = dc.channel_sk
-- group by dc.channel_name


-- 39
-- select
--     dc.segment,
--     sum(ft.amount_usd) as total_turnover
-- from core.fact_transaction ft
-- join core.dim_account da on ft.account_sk = da.account_sk
-- join core.dim_customer dc on da.customer_id = dc.customer_id
-- group by dc.segment
-- order by total_turnover desc
-- limit 1


-- 40
-- select
--     dm.category,
--     avg(ft.amount) as avg_amount,
--     count(ft.amount) as total_count
-- from core.fact_transaction ft
-- join core.dim_merchant dm on ft.merchant_sk = dm.merchant_sk
-- group by dm.category
-- order by avg_amount desc


-- 41
-- select
--     dc.full_name,
--     dc.segment,
--     count(ft.transaction_id) as total_transactions
-- from core.dim_customer dc
-- join core.dim_account da
--     on dc.customer_id = da.customer_id
-- join core.fact_transaction ft
--     on da.account_sk = ft.account_sk
-- where dc.is_current = true
-- group by dc.full_name, dc.segment
-- having count(ft.transaction_id) > 60


-- 42
-- select
--     dca.card_brand,
--     da.account_type,
--     count(dca.card_id) as total_cards
-- from core.dim_card dca
-- join core.dim_account da
-- on da.account_id = dca.account_id
-- group by dca.card_brand, da.account_type
-- order by total_cards desc


-- 43
-- select
--     city
-- from core.dim_customer
-- where is_current = true
-- union
-- select
--     city
-- from core.dim_merchant
-- order by city asc


-- 44
-- select
--     transaction_id,
--     amount,
--     case
--         when amount > 3000 then 'high'
--         when amount between 1000 and 3000 then 'medium'
--     else 'low'
-- end as risk_level
-- from core.fact_transaction


-- 45
-- select
--     full_name,
--     segment,
--     case
--         when segment = 'vip' then 'VIP'
--         when segment = 'premium' then 'Премиум'
--     else 'Стандарт'
-- end as level
-- from core.dim_customer
-- where is_current = true


-- 46
-- select
--     transaction_type,
--     'IN' as direction
-- from core.fact_transaction
-- where transaction_type = 'deposit'
-- union all
-- select
--     transaction_type,
--     'OUT' as direction
-- from core.fact_transaction
-- where transaction_type = 'withdrawal'


-- 47
-- select
--     case
--         when amount > 4000 then 'крупная'
--         when amount between 1000 and 4000 then 'средняя'
--         when amount < 1000 then 'мелкая'
--     else '-'
-- end as transaction_report,
-- count(*) as total_transactions
-- from core.fact_transaction
-- group by transaction_report


-- 48
-- select
--     transaction_id,
--     amount
-- from core.fact_transaction
-- where amount > (
--     select
--         avg(amount)
--     from core.fact_transaction
-- )


-- 49
-- select
--     transaction_id,
--     account_sk,
--     amount
-- from core.fact_transaction
-- where account_sk in (
--     select
--         account_sk
--     from core.dim_account
--     where account_type = 'credit'
--     )


-- 50
-- select
--     m.merchant_name,
--     (
--         select
--             count(*)
--         from core.fact_transaction ft
--         where ft.merchant_sk = m.merchant_sk
--         ) as total_transactions
-- from core.dim_merchant m


-- 51
-- select
--     c.full_name,
--     c.segment,
--     c.risk_score
-- from core.dim_customer c
-- where c.is_current = true
-- and risk_score  > (
--     select
--         avg(risk_score)
--     from core.dim_customer
--     where segment = c.segment
--     )


-- 52
-- select
--     dc.channel_sk
-- from core.dim_channel dc
-- where dc.channel_sk in (
--     select
--         ft.channel_sk
--     from core.fact_transaction ft
--     where ft.is_fraud = true
--     )


-- 53
-- with top_merchants as (
--     select
--         dm.merchant_name,
--         sum(ft.amount) as sum_turnover
--     from core.dim_merchant dm
--     join core.fact_transaction ft
--     on dm.merchant_sk = ft.merchant_sk
--     group by dm.merchant_name
--     order by sum_turnover desc
--     limit 10
-- )
-- select
--     merchant_name,
--     sum_turnover
-- from top_merchants


-- 54
-- with approved_tx as (
--     select
--         'approved' as tx_group,
--         amount
--     from core.fact_transaction
--     where status = 'approved'
-- ),
-- fraud_tx as (
--     select
--         'fraud' as tx_group,
--         amount
--     from core.fact_transaction
--     where is_fraud = true
-- )
-- select
--     tx_group,
--     count(*) as total_transactions,
--     sum(amount) as total_amount
-- from (
--     select * from approved_tx
--     union all
--     select * from fraud_tx
--      ) t
-- group by tx_group


-- 55
-- with tx_stats as (
--     select
--         transaction_type,
--         count(*) as cnt
--     from core.fact_transaction
--     group by transaction_type
-- )
-- select
--     transaction_type,
--     cnt
-- from tx_stats
-- where cnt > (
--     select
--         avg(cnt)
--     from tx_stats
--     )


-- 56
-- with customer_stats as (
--     select
--         dc.customer_id,
--         dc.full_name,
--         count(ft.transaction_id) as total_transactions,
--         sum(ft.amount_usd) as total_turnover
--     from core.dim_customer dc
--     join core.dim_account da
--     on dc.customer_id = da.customer_id
--     join core.fact_transaction ft
--     on da.account_sk = ft.account_sk
--     group by dc.customer_id, dc.full_name
-- )
-- select
--     customer_id,
--     full_name,
--     total_transactions,
--     total_turnover
-- from customer_stats
-- where total_transactions > 50


-- 57
-- insert into core.dim_merchant (
--     merchant_id, merchant_name, category, country, city
-- )
-- values (
--         501,
--         'Test Shop',
--         'online',
--         'Kyrgyzstan',
--         'Bishkek'
--        )


-- 58
-- insert into core.dim_merchant (merchant_id, merchant_name, category, country, city)
-- values  ('777', 'Bunker','games','Canada','Montreal'),
--         ('999', 'Tech Market','electronics','USA','San-Jose'),
--         ('888','Coffeeina','coffee','Finland','Narva')


-- 59
-- update core.dim_card
-- set status = 'blocked'
-- where card_brand = 'amex'
-- and status = 'active'


-- 60
-- delete from core.dim_merchant
-- where city = 'Berlin'
-- and merchant_id not in (
--     select distinct merchant_sk
--     from core.fact_transaction
--     where dim_merchant.merchant_sk is not null
--     )


-- 61
-- update core.dim_customer
-- set risk_score = risk_score - 5
-- where is_current = true
--     and segment = 'retail'
--     and risk_score > 60
-- returning *


-- 62
-- update core.dim_customer
-- set segment = 'premium'
-- where customer_id in (
--     select
--         customer_id
--     from core.dim_account
--     group by customer_id
--     having count(account_id) > 3
-- )
-- and is_current = true
-- and segment = 'retail'


-- 63
-- select
--     transaction_id,
--     channel_sk,
--     amount_usd,
--     sum(amount_usd) over (
--         partition by channel_sk
--         order by transaction_ts
--     ) as running_sum
-- from core.fact_transaction


-- 64
-- select
--     transaction_id,
--     transaction_type,
--     amount,
--     avg(amount) over (
--         partition by transaction_type
--     ) as avg_type
-- from core.fact_transaction


-- 65
-- select
--     transaction_id,
--     account_sk,
--     row_number() over (
--         partition by account_sk
--         order by transaction_ts
--     ) as rn
-- from core.fact_transaction


-- 66
-- select
--     transaction_id,
--     channel_sk,
--     avg(amount) over (
--         partition by channel_sk
--         order by transaction_ts
--         rows between 2 preceding and current row
--     ) as moving_avg
-- from core.fact_transaction


-- 67
-- select
--     transaction_id,
--     account_sk,
--     amount,
--     max(amount) over (
--         partition by account_sk
--     ) as max_amount
-- from core.fact_transaction


-- 68
-- select
--     transaction_id,
--     account_sk,
--     amount,
--     row_number() over (
--         partition by account_sk
--         order by amount desc
--     ) as rn
-- from core.fact_transaction


-- 69
-- select *
-- from (
--     select
--         transaction_id,
--         amount_usd,
--         rank() over (
--             order by amount_usd desc
--         ) as rnk
--     from core.fact_transaction
-- ) t
-- where rnk = 1


-- 70
-- select
--     ntile,
--     count(*) as cnt,
--     avg(risk_score) as avg_risk
-- from (
--     select
--         risk_score,
--         ntile(4) over (order by risk_score) as ntile
--     from core.dim_customer
-- ) t
-- group by ntile


-- 71
-- select *
-- from (
--     select
--         merchant_sk,
--         sum(amount) as total,
--         dense_rank() over (
--             order by sum(amount) desc
--         ) as rnk
--     from core.fact_transaction
--     group by merchant_sk
-- ) t
-- where rnk <= 5


-- 72
-- select *
-- from (
--     select
--         transaction_id,
--         account_sk,
--         amount,
--         rank() over (
--             partition by account_sk
--             order by amount asc
--         ) as rnk
--     from core.fact_transaction
-- ) t
-- where rnk = 1


-- 73
-- select
--     transaction_id,
--     account_sk,
--     amount,
--     lag(amount, 1) over (
--         partition by account_sk
--         order by transaction_ts
--     ) as prev_amount
-- from core.fact_transaction


-- 74
-- select
--     transaction_id,
--     channel_sk,
--     amount,
--     lead(amount) over (
--         partition by channel_sk
--         order by transaction_ts
--     ) - amount as diff
-- from core.fact_transaction


-- 75
-- select
--     transaction_id,
--     transaction_type,
--     amount,
--     amount - lag(amount) over (
--         partition by transaction_type
--         order by transaction_ts
--     ) as diff
-- from core.fact_transaction


-- 76
-- select
--     transaction_id,
--     account_sk,
--     max(amount) over (partition by account_sk)
--     - min(amount) over (partition by account_sk) as diff
-- from core.fact_transaction


-- 77
-- select
--     transaction_id,
--     account_sk,
--     first_value(amount) over (
--         partition by account_sk
--         order by transaction_ts
--     ) as first_amount,
--     last_value(amount) over (
--         partition by account_sk
--         order by transaction_ts
--         rows between unbounded preceding and unbounded following
--     ) as last_amount
-- from core.fact_transaction


-- 78
-- select *
-- from (
--     select
--         transaction_id,
--         channel_sk,
--         amount,
--         lag(amount) over (
--             partition by channel_sk
--             order by transaction_ts
--         ) as prev_amount
--     from core.fact_transaction
-- ) t
-- where amount > prev_amount


-- 79
-- select
--     segment,
--     count(*) as cnt,
--     avg(amount) as avg_check,
--     sum(case when is_fraud = 1 then 1 else 0 end) as fraud_cnt
-- from core.fact_transaction ft
-- join core.dim_account da on ft.account_sk = da.account_sk
-- join core.dim_customer dc on da.customer_sk = dc.customer_sk
-- group by segment


-- 80
-- with last_tx as (
--     select
--         customer_sk,
--         transaction_id,
--         amount,
--         row_number() over (
--             partition by customer_sk
--             order by transaction_ts desc
--         ) as rn
--     from core.fact_transaction
-- )
-- select
--     customer_sk,
--     transaction_id,
--     amount,
--     rank() over (order by amount desc) as rnk
-- from last_tx
-- where rn = 1
