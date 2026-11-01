# econ3916-lab11-sql
# SQL for Economists — Joins, Windows and the Cardinality Trap

## Objective
I translated pandas operations into SQL with DuckDB to see where the two give the same answer and where they quietly differ.

## Methodology
- I translated pandas select, filter, sort, groupby and merge operations into SQL with DuckDB and checked that both gave the same rows.
- I ran a one-to-many LEFT JOIN and counted the rows before and after to show the cardinality trap.
- I ran the same Welch t-test on the same revenue figures, once on the original customer rows and once on the joined rows.
- I used window functions (AVG OVER PARTITION BY and RANK) to compute group averages and ranks without collapsing rows.
- I ran the filter WHERE income != 50000 on a 5-row survey table and compared the result with the same filter in pandas.
- I queried a public Parquet file of about 3 million NYC taxi trips over HTTP without downloading it. DuckDB read only the columns the query needed.
- I asked an AI to translate three of my pandas steps into SQL, revised my prompt once, and compared the row counts of its queries with my pandas results.

## Key Findings
- Cardinality trap: The one-to-many LEFT JOIN turned 600 customer rows into 3,612 rows[cite: 1]. The Welch t-test went from t = 0.07 (p = 0.9413) to t = 0.71 (p = 0.4749)[cite: 1]. The repeated rows shrank the standard error and moved the difference, even though the revenue figures were the same.
- Window functions: AVG OVER PARTITION BY and RANK gave me group averages and ranks while keeping every row, which GROUP BY cannot do.
- Three-valued logic: WHERE income != 50000 kept 3 of 5 survey rows, because SQL treats comparisons with NULL as unknown and drops missing values[cite: 1]. The same filter in pandas kept 5[cite: 1].
- Parquet over HTTP: DuckDB queried the taxi file of about 3 million trips without a download and read only the columns the query used.
- AI translation: The AI’s first translation depended on guesses my prompt left open, such as how to treat missing values. After I revised the prompt, I compared its row counts with my pandas results.
