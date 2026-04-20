##

## 1. Обработка данных (SQL)
Этот запрос использовался для очистки данных и создания модели:
```sql
SELECT
  t.*,
  PARSEDATETIME(t."transactiondate", 'dd.MM.yyyy') AS "DT",
  CAST(t."amount" AS DECIMAL(10, 2)) AS "value",
  CASE 
    WHEN t."type" = 'Списание' THEN CAST(t."amount" AS DECIMAL(10, 2)) * -1 
    ELSE CAST(t."amount" AS DECIMAL(10, 2)) 
  END AS "income",
  CAST(REPLACE(t."bonusvalue", '+', '') AS DECIMAL(10, 2)) AS "bonusValue_clean"
FROM "data_20260420043051" AS t
```
## 2. Графики 
### Анализ кэшбека по категориям.
![](https://raw.githubusercontent.com/Zestros/Infographics/main/5.png)

### Распределение суммарных трат по категориям (Pie).
![](https://raw.githubusercontent.com/Zestros/Infographics/main/6.png)

### Распределение суммарных трат по категориям и месяцам (Sankey).
![](https://raw.githubusercontent.com/Zestros/Infographics/main/7.png)

### Индикатор среднемесячных трат.
![](https://raw.githubusercontent.com/Zestros/Infographics/main/8.png)

### Суммарный приход/расход по месяцам.
![](https://raw.githubusercontent.com/Zestros/Infographics/main/9.png)

### Движение средств.
![](https://raw.githubusercontent.com/Zestros/Infographics/main/10.png)

### Запрос для движения средств (Счет баланса по дням)
```
SELECT 
    daily_sums.day_date AS "Дата",
    SUM(daily_sums.day_income) OVER (ORDER BY daily_sums.day_date) AS "Баланс"
FROM (
    SELECT 
        PARSEDATETIME("transactiondate", 'dd.MM.yyyy') AS day_date,
        SUM(CASE 
            WHEN "type" = 'Списание' THEN CAST("amount" AS DECIMAL(10, 2)) * -1 
            ELSE CAST("amount" AS DECIMAL(10, 2)) 
        END) AS day_income
    FROM "data_20260420043051"
    GROUP BY PARSEDATETIME("transactiondate", 'dd.MM.yyyy') 
) AS daily_sums
ORDER BY 1

```
## 3. Работа Фильтров
### Кросс фильтр и фильтр по дате
![Кросс фильтр и фильтр по дате](https://raw.githubusercontent.com/Zestros/Infographics/main/1.png)

![Кросс фильтр и фильтр по дате](https://raw.githubusercontent.com/Zestros/Infographics/main/2.png)
### Кросс фильтр и фильтр по категории
![Кросс фильтр и фильтр по категории](https://raw.githubusercontent.com/Zestros/Infographics/main/3.png)

![Кросс фильтр и фильтр по категории](https://raw.githubusercontent.com/Zestros/Infographics/main/4.png)
