
# Project Title:
# Analysis of New Zealand's 2024 Export Data using BigQuery and SQL

## Summary

This project involves analyzing New Zealand's monthly export data for the year 2024. The data was downloaded, loaded into BigQuery, and then analyzed using SQL. 
The primary purpose of this analysis is to identify New Zealand's main export destination countries and key export commodities.

## Data Used

- Data Source: Stats NZ "Overseas merchandise trade datasets"
- Period: January 2024 - December 2024 (or 01/2024 - 12/2024)
- Key Columns: country (destination country), hs_desc (commodity description), total_export_FOB (total export Free On Board value), month, etc.

## Tools and Platforms Used

- Google BigQuery
- SQL

## Analyse process

1. 12ヶ月分の月次輸出データCSVをBigQueryの各月テーブルにロードしました。
2. 以下のSQLクエリを使用して、12ヶ月分のデータを統合するビュー `all_exports_2024` を作成しました。

```sql  
CREATE OR REPLACE VIEW `elegant-rock-451218-q4.nz_exports.all_exports_2024` AS
SELECT *, '2024-01' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_01`
UNION ALL
SELECT *, '2024-02' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_02`
UNION ALL
SELECT *, '2024-03' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_03`
UNION ALL
SELECT *, '2024-04' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_04`
UNION ALL
SELECT *, '2024-05' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_05`
UNION ALL
SELECT *, '2024-06' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_06`
UNION ALL
SELECT *, '2024-07' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_07`
UNION ALL
SELECT *, '2024-08' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_08`
UNION ALL
SELECT *, '2024-09' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_09`
UNION ALL
SELECT *, '2024-10' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_10`
UNION ALL
SELECT *, '2024-11' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_11`
UNION ALL
SELECT *, '2024-12' AS file_month FROM `elegant-rock-451218-q4.nz_exports.exports_2024_12`;

## 上位10輸出国を特定するために、以下のクエリを実行しました。

```sql
SELECT
  country,                         
  SUM(total_export_FOB) AS total_fob_for_country 
FROM
  `elegant-rock-451218-q4.nz_exports.all_exports_2024` 
GROUP BY
  country                           
ORDER BY
  total_fob_for_country DESC        
LIMIT 10;

## 上位10輸出品目を特定するために、以下のクエリを実行しました。
```sql
SELECT
  hs_desc,                            -- 品目（HSコードの説明）を表すカラム
  SUM(total_export_FOB) AS total_fob_for_hs_desc -- 品目ごとの総輸出FOB価額
FROM
  `elegant-rock-451218-q4.nz_exports.all_exports_2024` -- create viewー
GROUP BY
  hs_desc                             -- 品目ごとに集計
ORDER BY
  total_fob_for_hs_desc DESC          -- 総輸出FOB価額が多い順に並べ替え
LIMIT 10;                             -- 上位10品目に絞り込み

分析結果
上位10輸出国
順位	国名	総輸出FOB価額 (NZD)
1	[実際の国名]	[実際の金額]
2	[実際の国名]	[実際の金額]
...	...	...
10	[実際の国名]	[実際の金額]
(ここにBigQueryで得られた結果の表をMarkdownの表形式で書くか、後述する画像で貼り付けます)
上位10輸出品目
順位	品目	総輸出FOB価額 (NZD)
1	[実際の品目名]	[実際の金額]
2	[実際の品目名]	[実際の金額]
...	...	...
10	[実際の品目名]	[実際の金額]
(同様に結果を記載)
考察
この分析から、2024年のニュージーランドの輸出において、[国名]が最大の輸出相手国であり、[品目名]が最も輸出額の大きい品目であることが明らかになりました。
[その他、気づいたことや、この分析から考えられることなどを書く]
連絡先
LinkedIn: [あなたのLinkedInプロフィールのURL]
Email: [あなたのメールアドレス (公開しても良ければ)]





                    
