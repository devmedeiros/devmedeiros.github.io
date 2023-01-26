---
title: Estudo de Caso Analytics Engineer
date: 2022-08-23 22:57:00 -0300
lastmod: 2022-08-27 10:54:00 -0300
categories: [Blog, Projects]
tags: [analytics engineer, caso, kpi, análise de dados, data engineering, manipulação de dados, python, SQL, engenharia de dados]
showtoc: true
summary: Projeto explorando tarefas comuns que um Analytics Engineer faz diariamente.
---

# Como é trabalhar como um Analytics Engineer?

Analytics Engineer refere-se a um profissional de ciência de dados focado em transformar dados em informações de fácil acesso ao usuário final. Eles fornecem relatórios estatísticos e dinâmicos que capacitam a equipe de negócios sem que eles precisem pensar na complexidade por trás da análise de dados.

Neste estudo de caso, quero falar sobre quais seriam as tarefas comuns que um Analytics Engineer precisaria executar e como eu as conduziria.

Nesse cenário, o Analytics Engineer trabalha para o Bankio, um banco digital do Brasil. Como a maioria dos bancos digitais no Brasil, o Bankio oferece transferências gratuitas para todas as contas bancárias do país. Também possui muitos produtos como conta de investimento, conta poupança, conta bancária individual, cartão de crédito sem anuidade e muito mais.

## Tarefa 1: Consulta SQL

> Um Analista de Negócios (_Bussiness Analyst_) do Bankio pede sua ajuda para escrever uma consulta SQL para obter todo o saldo mensal da conta entre janeiro de 2020 e dezembro de 2020.

{{< collapse summary="Solução da consulta SQL _(clique para expandir)_" >}}
```SQL
SELECT
  a.*,
  -- Aqui eu calculo a soma cumulativa do total de depósitos de cada cliente ordenando
  -- por mês, se for nulo, altero o valor para 0, então subtraio a soma cumulativa do
  -- total de retiradas para cada cliente ordenando por mês, se for nulo, altero o
  -- valor para 0 e eu salvo isso como account_monthly_balance
  NVL(SUM(total_transfer_in) OVER (PARTITION BY customer_id 
ORDER BY
  action_month), 0) - NVL(SUM(total_transfer_out) OVER (PARTITION BY customer_id 
ORDER BY
  action_month), 0) AS account_monthly_balance 
FROM
  -- subconsulta total de transações de entrada/saída
( 
  SELECT
    * 
  FROM
    -- subconsulta de depósitos totais
( 
    SELECT
      action_month, customer_id, SUM(amount) AS total_transfer_in 
    FROM
      (
        SELECT
          * 
        FROM
          -- subconsulta de depósitos regulares
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
            -- subconsulta de depósitos pix
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
            -- subconsulta de saques totais
( 
            SELECT
              * 
            FROM
              -- subconsulta de retirada regular
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
                -- subconsulta de retirada de pix
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

## Tarefa 2: Indicadores Chaves de Performance (KPI)

{{< figure src="https://images.unsplash.com/photo-1526628953301-3e589a6a8b74" caption="Foto de Stephen Dawson de Unsplash" alt="tela do computador com 8 retângulos preenchidos com indicadores-chave">}}

> Outro colega do Bankio está interessado em analisar o sucesso do produto [PIX](#pix) da empresa em nível comercial e técnico. Então eles pediram que você apresentasse alguns indicadores-chave para medir isso.

{{< collapse summary="Tempo médio de processamento das transações PIX _(clique para expandir)_" >}}

Isso pode ser obtido usando o tempo que um cliente solicita uma transação PIX e quando ela é concluída, calculamos isso para todas as transações PIX e, em seguida, fazemos a média. O PIX deve ser instantâneo, portanto, essa métrica deve ser a menor possível.

{{< /collapse>}}

{{< collapse summary="A proporção de falhas do PIX _(clique para expandir)_" >}}

Este indicador é importante porque é inconveniente para o cliente que sua transação falhe. Podemos calcular isso dividindo a soma das transações PIX com falha pelo total de transações PIX. Esta medida deve ser minimizada.

{{< /collapse>}}

{{< collapse summary="A proporção de transações usando PIX _(clique para expandir)_" >}}

O sucesso do PIX pode ser medido pela proporção de movimentações usando PIX sobre transações normais. Então, basta contar quantas transações foram concluídas usando o PIX e dividir pelo valor total de transações concluídas. Um valor maior reflete o sucesso do PIX sobre transações regulares.

Alternativamente, em vez de apenas contar as transações, podemos avaliar quanto dinheiro cada tipo de transação está movimentando.

{{< /collapse>}}

{{< collapse summary="A proporção de entrada/saída do PIX _(clique para expandir)_" >}}

Essa medida é boa para analisar se os clientes estão usando seu PIX mais para receber dinheiro ou para enviar dinheiro. Seria melhor se mais clientes estivessem recebendo mais dinheiro do que enviando. Como o Bankio já tinha transações gratuitas para qualquer banco, antes do PIX aparecer, outros ainda tinham que pagar taxas para enviar dinheiro para sua conta do Bankio. Por esse motivo, é melhor contar quantas transações estão chegando pelo PIX e dividi-las por todas as transações PIX. Quanto mais alto melhor.

Nesse caso, também podemos somar o saldo de depósitos e saques da conta do Bankio usando o PIX e compará-lo com transações regulares.

{{< /collapse>}}

## Tarefa 3: Retorno Diário do Investimento

> O Bankio tem uma conta bancária em que o cliente pode investir num produto de rendimento fixo. Considere que este produto oferece aos clientes um retorno diário de 0,01% de acordo com o valor do saldo diário investido. Calcule quanto cada cliente tem em sua conta bankio durante o ano de 2020.

Este retorno é calculado diariamente após todos os saques e/ou depósitos feitos em um determinado dia. E todos os dias, mesmo finais de semana, geram algum retorno.

O exemplo a seguir descreve o cliente A que começa a investir nesse produto de renda fixa no dia 16 do primeiro mês. O saldo anterior era zero, pois esse consumidor está fazendo um primeiro depósito no investimento. Seu depósito inicial foi de 1.000 e, no final do dia, produziu a uma taxa de renda diária de 0,01% de seu saldo. O mesmo produto continua sendo consumido por esse cliente em momentos diferentes ao longo do mês. Lembre-se que esta é apenas uma amostra fictícia do log de transações com cálculos diários aplicados. Observe que a receita desse dia deve ser zerada no caso de Movimentos negativos.

| Dia |  Mês  |  ID Conta  | Depósito | Retirada | Renda no final do Dia | Rendimento Diário da Conta |
|-----|-------|------------|----------|----------|-----------------------|----------------------------|
| 16  | 1     | A          | 1000     | 0        | 0,1                   | 1000,10                    |
| 20  | 1     | A          | 500      | 0        | 0,15                  | 1500,55                    |
| 2   | 2     | A          | 0        | 200      | 0,13                  | 1302,48                    |
| 19  | 2     | A          | 1000     | 200      | 0,21                  | 2104,78                    |

_Movimentos = Saldo do dia anterior + Depósito - Retirada_

_Renda no final do dia = Movimentos * Taxa de Renda_

_Saldo Diário da Conta = Movimentos + Renda no Fim do Dia_

{{< gist devmedeiros 2a52cf2c4431a1993a98bf7f36d0f412 "bankio-task-3-pt.ipynb" >}}

---

# Glossário

### Saldo Mensal da Conta

É a quantidade de dinheiro que um cliente tinha em sua conta no final de um determinado mês.

### Informações da conta (agência, número da conta e dígito de verificação)

No Brasil, uma conta bancária pode ser identificada exclusivamente por três números. O código **agência**, que indica em qual agência bancária as contas foram abertas, vem primeiro. O segundo é o **número da conta** que uma agência usa para identificar contas. O **dígito de verificação**, que é usado apenas para detecção de erros, é o último.

### CPF

É a identificação cadastral do contribuinte individual brasileiro.

### PIX

No Brasil, este é o método mais recente de transferência de dinheiro. Não é pago. É imediato, e tudo o que é necessário para concluir uma transação é a chave PIX associada à conta. 

### Transferências não PIX

Estes são os métodos convencionais para transferir dinheiro entre contas bancárias. Esse tipo de transação exige que sejam fornecidos o CPF, o código da agência, o número da conta e o dígito verificador da conta que receberá os recursos. A maioria dos bancos cobra uma taxa nessas transações, e a confirmação da transação normalmente leva várias horas a dias. 