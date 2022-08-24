---
title: Case Study Analytics Engineer
date: 2022-08-23 22:57:00 -0300
categories: [Blog, Projects]
tags: [analytics engineer, case, kpi, data analysis, data engineer, data manipulation, python, SQL]
showtoc: true
summary: Project exploring common tasks an Analytics Engineer needs to perform on a daily day.
---

# What is it like working as an Analytics Engineer?

Analytics Engineer refers to a Data Science professional focused on transforming data into information that is easy to access to the end-user. They provide static and dynamic reports that empower the business team without them needing to think about the complexity behind data analysis.

In this case study, I want to talk about what would be common tasks that an Analytics Engineer would need to perform and how I'd navigate them.

In this scenario, the Analytics Engineer works for Bankio a digital bank from Brazil. Like most digital banks in Brazil, Bankio offers free transfers for every bank account in the country. It also has many products like an investment account, a savings account, an individual bank account, a credit card without an annual fee, and many more.

## Task 1: SQL Query

> A Bussiness Analyst from Bankio asks for your assistance writing a SQL query to get all the account's monthly balance between January 2020 to December 2020.

{{< collapse summary="SQL Query Solution _(click to expand)_" >}}
```SQL
SELECT
  a.*,
  -- Here I calculate the cumulative sum of total deposits for each customer ordering it 
  -- by month, if its null I change the value to 0 then I subtract the cumulative sum of
  -- total withdrawals for each customer ordering it by month, if its null I change the 
  -- value to 0 and I save this as ours account_monthly_balance
  NVL(SUM(total_transfer_in) OVER (PARTITION BY customer_id 
ORDER BY
  action_month), 0) - NVL(SUM(total_transfer_out) OVER (PARTITION BY customer_id 
ORDER BY
  action_month), 0) AS account_monthly_balance 
FROM
  -- total transactions in/out subquery
( 
  SELECT
    * 
  FROM
    -- total deposits subquery
( 
    SELECT
      action_month, customer_id, SUM(amount) AS total_transfer_in 
    FROM
      (
        SELECT
          * 
        FROM
          -- regular deposits subquery
( 
          SELECT
            d_month.action_month, accounts.customer_id, SUM(transfer_ins.amount) AS amount 
          FROM
            d_time 
            INNER JOIN
              transfer_ins 
              ON transfer_ins.transaction_completed_at = d_time.time_id 
            LEFT JOIN
              d_month USING(month_id) 
            LEFT JOIN
              accounts USING(account_id) 
          WHERE
            transfer_ins.status = 'completed' 
          GROUP BY
            d_month.action_month, accounts.customer_id ) transfer_in 
          UNION ALL
          SELECT
            * 
          FROM
            -- pix deposits subquery
( 
            SELECT
              d_month.action_month, accounts.customer_id, SUM(pix_movements.pix_amount) AS amount 
            FROM
              d_time 
              INNER JOIN
                pix_movements 
                ON pix_movements.pix_completed_at = d_time.time_id 
              LEFT JOIN
                d_month USING(month_id) 
              LEFT JOIN
                accounts USING(account_id) 
            WHERE
              pix_movements.status = 'completed' 
              AND pix_movements.in_or_out = 'pix_in' 
            GROUP BY
              d_month.action_month, accounts.customer_id ) 
      )
    GROUP BY
      action_month, customer_id ) 
      FULL JOIN
        (
          SELECT
            action_month,
            customer_id,
            SUM(amount) AS total_transfer_out 
          FROM
            -- total withdrawals subquery
( 
            SELECT
              * 
            FROM
              -- regular withdrawal subquery
( 
              SELECT
                d_month.action_month, accounts.customer_id, SUM(transfer_outs.amount) AS amount 
              FROM
                d_time 
                INNER JOIN
                  transfer_outs 
                  ON transfer_outs.transaction_completed_at = d_time.time_id 
                LEFT JOIN
                  d_month USING(month_id) 
                LEFT JOIN
                  accounts USING(account_id) 
              WHERE
                transfer_outs.status = 'completed' 
              GROUP BY
                d_month.action_month, accounts.customer_id ) 
              UNION ALL
              SELECT
                * 
              FROM
                -- pix withdrawal subquery
( 
                SELECT
                  d_month.action_month, accounts.customer_id, SUM(pix_movements.pix_amount) AS amount 
                FROM
                  d_time 
                  INNER JOIN
                    pix_movements 
                    ON pix_movements.pix_completed_at = d_time.time_id 
                  LEFT JOIN
                    d_month USING(month_id) 
                  LEFT JOIN
                    accounts USING(account_id) 
                WHERE
                  pix_movements.status = 'completed' 
                  AND pix_movements.in_or_out = 'pix_out' 
                GROUP BY
                  d_month.action_month, accounts.customer_id ) ) 
                GROUP BY
                  action_month,
                  customer_id 
        )
        USING (action_month, customer_id) ) a;
```
{{< /collapse >}}

## Task 2: Key Performance Indicators

{{< figure src="https://images.unsplash.com/photo-1526628953301-3e589a6a8b74" caption="Photo by Stephen Dawson on Unsplash" alt="computer screen with 8 rectangles filled with key indicators">}}

> Another collegue from Bankio is interested in analysing the success of the company product [PIX](#pix) on a business and technical level. So they asked you to come up with some key indicators to measure this.

{{< collapse summary="Mean processing time of PIX transactions _(click to expand)_" >}}

This can be obtained using the time a customer requests a PIX transaction and when it is completed, we calculate this for all the PIX transactions then we take the mean value. PIX is supposed to be instantaneous, so this metric should be as small as possible. 

{{< /collapse>}}

{{< collapse summary="The proportion of PIX failures _(click to expand)_" >}}

This indicator is important because it’s inconvenient for the client to have their transaction fail. We can calculate this by dividing the sum of failed PIX transactions by the total PIX transactions. This measure should be minimized.

{{< /collapse>}}

{{< collapse summary="The proportion of transactions using PIX _(click to expand)_" >}}

PIX success can be measured by the proportion of transaction movements using PIX over normal transactions. So just count how many completed transactions using PIX were made and divide it by the total amount of completed transactions. A bigger measurement reflects PIX success over regular transactions.

Alternatively instead of just counting the transactions we can evaluate how much money each transaction type is moving.

{{< /collapse>}}


{{< collapse summary="The proportion of in/out of PI _(click to expand)_" >}}

This measure is good to analyze if customers are using their PIX to receive more money or to send money. I think it would be better if more customers are receiving more money than they are sending. Because Bankio already had free transactions for any bank, before PIX came around, others still had to pay fees to send money to your Bankio account. For this reason, I think it is better to count how many transactions are coming in through PIX and divide it by all PIX transactions. The higher the better.

In this case we could also sum the balance of deposit and withdrawals from the Bankio account using PIX and compare it with regular transactions.

{{< /collapse>}}

## Task 3: Daily Investment Return

> Bankio has a customer banking account that allows you to invest on a fixed rate income product. Consider that this product provides customers with a daily return of 0.01% according to their daily invested balance amount. Calculate how much each customer has on their bankio account during the year 2020. 

This return is calculated daily after all withdrawals and/or deposits made on a given day. And every day, even weekends, generate some return.

The following example describes customer A who begins investing in this fixed income product on day 16 of the first month. The prior balance was zero since this consumer is making a first-time deposit into the investment. His initial deposit was 1,000, and at the end of the day, it produced a daily income rate of 0.01% of his balance. The same product is still being consumed by this client at different times throughout the month. Remember that this is just a dummy sample of the transaction log with daily calculations applied. Note that the income for that day should be set to zero in the event of negative Movements. 

| Day | Month | Account ID | Deposit | Withdrawal | End of Day Income | Account Daily Balance |
|-----|-------|------------|---------|------------|-------------------|-----------------------|
| 16  | 1     | A          | 1000    | 0          | 0.1               | 1000.10               |
| 20  | 1     | A          | 500     | 0          | 0.15              | 1500.55               |
| 2   | 2     | A          | 0       | 200        | 0.13              | 1302.48               |
| 19  | 2     | A          | 1000    | 200        | 0.21              | 2104.78               |

_Movements = Previous Day Balance + Deposit - Withdrawal_

_End of Day Income = Movements * Income Rate_

_Account Daily Balance = Movements + End of Day Income_

{{< gist devmedeiros 2a52cf2c4431a1993a98bf7f36d0f412 >}}

---

# Glossary

### Account Monthly Balance

It is the amount of money a customer had in their account at the end of a given month.

### Account info (branch, number and check-digit)

In Brazil, a bank account can be uniquely identifiable by three numbers. The **branch** code, which indicates which bank branch the accounts were opened in, comes first. The second is the **account number** that a branch uses to identify accounts. The **check-digit**, which is only used for error detection, is the last. 

### CPF

It is the Brazilian individual taxpayer registry identification.

### PIX

In Brazil, this is the most recent method of money transmission. It's unpaid. It is immediate, and all that is required to complete a transaction is the Pix-Key associated with the account. 

### Non PIX transfers

These are the conventional methods for transferring money between bank accounts. This type of transaction requires the CPF, the branch code, the account number, and the check digit of the account that will receive the funds to be provided. Most banks charge a fee in these transactions, and the confirmation of the transaction typically takes several hours to days. 