# CSC 222: Data Warehousing

Hofstra University · Taught Spring 2025, Fall 2025 and Spring 2026

This course covers traditional and modern data warehousing: the design and implementation of database, data warehousing and analytics database systems. Topics include data modeling and architecture, the ETL process, security, NoSQL databases and complex analytics processing, and students build a complete data warehouse system that includes a business intelligence project. In Spring 2026 the schedule ran from Python, SQL, web scraping and MongoDB through recommender systems and unsupervised learning to retrieval-augmented generation (RAG) with ChromaDB, evaluated with RAGAS.

## What students learn

- ETL on a real vendor file: parsing a padded, comma-separated export with a date banner, cleaning column names that contain `%`, turning sentinel values into NULLs, and loading into SQLite through SQLAlchemy with explicit column types.
- Analytical SQL: aggregates, `GROUP BY` vs. window functions (`OVER (PARTITION BY ...)`), `DISTINCT`, `IN`, `UNION`, `CASE`, `HAVING` vs. `WHERE`, `LIKE`, `EXISTS`, and NULL handling with `IS NULL` and `COALESCE`.
- Organizing complex logic three ways (CTEs, views and subqueries) and comparing their readability and speed.
- Physical design and integrity: primary keys, single and composite indexes, reading `EXPLAIN QUERY PLAN`, and transactions with `ROLLBACK` for what-if stress tests (ACID).
- Joins across time: comparing December 2007 with December 2008 using INNER vs. LEFT joins to expose survivorship bias, and simulating a FULL OUTER JOIN with `UNION`.
- NoSQL pipelines: scraping with requests, BeautifulSoup and Selenium; MongoDB document design with a compound unique index on `(query_date, etf_ticker)`; idempotent upserts that resume where they stopped; and `.explain()` to compare COLLSCAN with IXSCAN.
- Generative AI inside a data pipeline: LLM-written summaries produced by a rate-limit-aware, self-healing loop, checks for hallucination, and ETF similarity three ways (Jaccard, cosine on holdings weights, cosine on sentence embeddings).

## Highlights

| Notebook | What it covers | Open |
|---|---|---|
| [01 SQL on risk-model data](notebooks/01_sql_on_risk_model_data.ipynb) | The Spring 2026 SQL lecture notebook, built on December 2007 and December 2008 Barra USE3 snapshots around the 2008 financial crisis. Moves from extract-transform-load in pandas to SQLite through SQL types, aggregates and window functions, CTEs vs. views vs. subqueries, indexes and `EXPLAIN`, transactions, primary keys, joins and survivorship, NULL handling, `EXISTS`, `CASE`, `HAVING` and `LIKE`. Ends with open research questions and project hints. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jordan-low-mit/hofstra-csc222-data-warehousing/blob/main/notebooks/01_sql_on_risk_model_data.ipynb) |
| [02 Project: SQL warehouse for the 2008 crisis](notebooks/02_project_sql_financial_crisis.ipynb) | The project specification. Students design a multi-table schema (2007 data, 2008 data, an industry-to-sector mapping), choose data types and keys, and compare beta and risk measures. They test which risk factors predicted 2007–2008 changes, run a Bear Stearns/Lehman what-if inside a transaction, and implement the same analysis as a CTE, a view and a subquery. Includes the grading rubric. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jordan-low-mit/hofstra-csc222-data-warehousing/blob/main/notebooks/02_project_sql_financial_crisis.ipynb) |
| [03 Project: ETF recommender with GenAI](notebooks/03_project_etf_recommender_genai.ipynb) | The Spring 2026 project specification. Students scrape the ETF universe and holdings from public sources into MongoDB, track holdings over time with `query_date`, and prove their indexes with `.explain()`. They generate LLM strategy summaries in a self-healing pipeline, then recommend similar ETFs with three similarity measures. Includes the grading guidelines. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jordan-low-mit/hofstra-csc222-data-warehousing/blob/main/notebooks/03_project_etf_recommender_genai.ipynb) |

## How the course is taught

- **Flipped classroom.** Students watch the assigned videos and do the readings before class. Class time goes to questions and problem-solving, and class ends early if there are none.
- **Project-based grading.** Projects are 90% of the grade and class participation (answering questions in class) is 10%. Students work in groups of 3 to 5. Each project is submitted as a 5–10 minute presentation (it can be pre-recorded, and every member speaks for equal time), code notebooks and a Trello board.
- **AI tools within a written policy.** Students must disclose which tools they used, how they used them and how they verified the output. Hallucinations and bugs from generative AI carry a penalty. The ETF project grades the AI disclosure and asks students to catch and handle at least one hallucination.

## Data

- **Notebooks 01 and 02** use **Barra US Equity Model (USE3) risk files for 31 December 2007 and 31 December 2008 (licensed; not redistributed)**. The files are not in this repository, and outputs that showed company-level rows have been cleared. Each file has a date on the first line, then a comma-separated header and one row per security. Text fields are space-padded, and missing values are coded as -999. Columns:
  - Identifiers: `BARRID`, `TICKER`, `CUSIP`, `NAME`.
  - Risk: two beta measures (`HBTA`, `BETA`) and two risk measures (`SRISK%`, `TRISK%`).
  - Style exposures: `VOLTILTY`, `MOMENTUM`, `SIZE`, `SIZENONL`, `TRADEACT`, `GROWTH`, `EARNYLD`, `VALUE`, `EARNVAR`, `LEVERAGE`, `CURRSEN`, `YIELD`.
  - Estimation-universe flags: `NONESTU`, `E3ESTU`.
  - Up to five industry memberships with weights: `INDNAME1`–`INDNAME5`, `IND1`–`IND5`, `WGT1%`–`WGT5%`.
  - Market data: `PRICE`, `CAPITALIZATION`, `YLD%`.
  - Index-membership flags: `SAP500`, `SAPVAL`, `SAPGRO`, `MIDCAP`, `MIDVAL`, `MIDGRO`, `SC600`, `SCVAL`, `SCGRO`.
- **Notebook 03** has students collect their own data from public sources: Finviz and StockAnalysis.com screeners, and the iShares, Invesco and SSGA (SPDR) holdings files. A fallback Bloomberg export and two archived SSGA snapshots went to the class and are not included here.

To follow notebook 01 without Barra data, any cross-section of stocks at two dates with an identifier, name, industry, market cap and a few risk or style measures will do. Rename your columns to match.

## Instructor

[Jordan Low](https://github.com/jordan-low-mit), adjunct professor at Hofstra University.

---

Materials © Jordan Low. Shared for reference; please ask before reusing in a course.
