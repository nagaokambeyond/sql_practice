# サンプルデータ
https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/

# dockerコマンド
## 起動
> docker compose up -d

## 停止
> docker compose down

## 破棄
> docker compose down --rmi all --volumes --remove-orphans

# SQL練習
- その１
  - payment.payment_dateの年月毎にamountをsummaryする
    | yyyymm | summary |
    | -- | -- |
    | 200702 | 8351.84 |
    | 200703 | 23886.56 |
    | 200704 | 28559.46 |
    | 200705 | 514.18 |

    <details>
    <summary>answer</summary>

    ```sql
    select 
      to_char(payment_date, 'YYYYMM') yyyymm
      ,sum(amount) summary
    from payment
    group by yyyymm
    order by yyyymm
    ;
    ```
    </details>

- その２
  - payment.payment_dateの年月毎の件数が3000以上を取得する
    | yyyymm |
    | -- |
    | 200703 |
    | 200704 |

    <details>
    <summary>answer</summary>

    ```sql
    select 
      to_char(payment_date, 'YYYYMM') yyyymm
    from payment
    group by yyyymm
    having count(*) >= 3000
    order by yyyymm
    ;
    ```
    </details>

- その３
  - payment.staff_id、payment.payment_dateの年月毎にamountをsummaryする
    | staff_id | yyyymm | summary |
    | -- | -- | -- |
    | 1 | 200702 | 4160.84 |
    | 1 | 200703 | 11776.83 |
    | 1 | 200704 | 14080.36 |
    | 1 | 200705 | 234.09 |
    | 1 | null | 30252.12 |
    | 2 | 200702 | 4191.00 |
    | 2 | 200703 | 12109.73 |
    | 2 | 200704 | 14479.10 |
    | 2 | 200705 | 280.09 |
    | 2 | null | 31059.92 |
    | null | null | 61312.04 |

    <details>
    <summary>answer</summary>
    
    ```sql
    select 
      staff_id
      ,to_char(payment_date, 'YYYYMM') yyyymm
      ,sum(amount) summary
    from payment
    group by grouping sets ((staff_id, yyyymm), staff_id, ())
    order by staff_id, yyyymm
    ;
    ```
    </details>
