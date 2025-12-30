---
date:
  created: 2025-12-29
  updated: 2025-12-29

draft: False

tags:
  - books
  - goodreads
  - duckdb
  - docker

links:
  - Dataset: https://huggingface.co/datasets/BrightData/Goodreads-Books/tree/main
  - DuckDB: https://duckdb.org/

---

# Looking for new books?

Using the [Goodreads dataset](https://huggingface.co/datasets/BrightData/Goodreads-Books/tree/main) we can easily look for books in your domains (like `Fantasy` and `Science Fiction`) to find the highest rated books.

<!-- more -->

The dataset seems recent as the last update is from `June, 23 2024`. The total number of books is 6,389,859. Many more then I will ever read...

## Download the data

First let's kick off a docker container running [DuckDB](https://duckdb.org/). The following command will download the latest version and provide us with the duckdb cli interface. Note that you will need to change the `$PWD/goodreads` according to your system. Your provided folder will then have a file called `goodreads.duckdb` which is 
the database.

```sh
docker run --rm -it -v "$PWD/goodreads:/data" duckdb/duckdb:latest duckdb /data/goodreads.duckdb
```

Once inside the cli run the following commands. I'm using the [httpfs extension](https://duckdb.org/docs/stable/core_extensions/httpfs/overview)

```sql
INSTALL httpfs;
LOAD httpfs;
SET enable_progress_bar = true; 

CREATE TABLE goodreads AS
SELECT * FROM 'hf://datasets/BrightData/Goodreads-Books/Goodreads-Books.csv';
```

The `create` table command will run for awhile but after a few seconds you should at least see a progress bar. In total it took around 3 mins on my laptop.

Note that we don't need to create any indices. DuckDB is a columnar, vectorized engine that performs many query optimizations automatically. And in fact all queries I have run so far are instant.

## Data Structure

Once downloaded we can get the schema via `describe goodreads;`

| column_name         | column_type | null | key  | default | extra |
|---------------------|-------------|------|------|---------|-------|
| url                 | VARCHAR     | YES  | NULL | NULL    | NULL  |
| id                  | VARCHAR     | YES  | NULL | NULL    | NULL  |
| name                | VARCHAR     | YES  | NULL | NULL    | NULL  |
| author              | VARCHAR     | YES  | NULL | NULL    | NULL  |
| star_rating         | DOUBLE      | YES  | NULL | NULL    | NULL  |
| num_ratings         | BIGINT      | YES  | NULL | NULL    | NULL  |
| num_reviews         | BIGINT      | YES  | NULL | NULL    | NULL  |
| summary             | VARCHAR     | YES  | NULL | NULL    | NULL  |
| genres              | VARCHAR     | YES  | NULL | NULL    | NULL  |
| first_published     | VARCHAR     | YES  | NULL | NULL    | NULL  |
| about_author        | VARCHAR     | YES  | NULL | NULL    | NULL  |
| community_reviews   | VARCHAR     | YES  | NULL | NULL    | NULL  |
| kindle_price        | VARCHAR     | YES  | NULL | NULL    | NULL  |

### George R. R. Martin Books

Let's quickly check out all books from the creator of Game Of Thrones:

```sql
COPY (
  select url, name, author, star_rating, num_ratings, summary, genres, first_published
  from goodreads where author ilike '%george%r%martin%'
) TO '/data/george_r_r_martin.csv'
WITH (HEADER, DELIMITER ',', QUOTE '"', ESCAPE '"')
;
```

You can then load the data into vscode or Google Sheets OR we just quickly create an html table and load it into the browser. Just copy and paste the following into a command line.

```sh
python - <<'EOF'
import pandas as pd
import html

df = pd.read_csv("george_r_r_martin.csv")

url_col = df.columns[0]  # first column

def linkify(u):
    if pd.isna(u) or str(u).strip() == "":
        return ""
    u = str(u).strip()
    safe_href = html.escape(u, quote=True)
    safe_text = html.escape(u)
    return f'<a href="{safe_href}" target="_blank" rel="noopener noreferrer">{safe_text}</a>'

df[url_col] = df[url_col].map(linkify)

table = df.to_html(
    index=False,
    classes="styled-table",
    border=0,
    escape=False,      # allow our <a> tags to render
    justify="left"
)

page = f"""<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>George R. R. Martin – Book Data</title>
<style>
body {{
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial;
  background: #f5f7fa;
  padding: 2rem;
}}
h1 {{ margin: 0 0 1rem 0; }}

.styled-table {{
  border-collapse: collapse;
  width: 100%;
  background: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}}
.styled-table thead tr {{
  background-color: #2f3e46;
  color: #fff;
  text-align: left;
}}
.styled-table th, .styled-table td {{
  padding: 10px 12px;
  border-bottom: 1px solid #e0e0e0;
  vertical-align: top;
}}
.styled-table tbody tr:nth-child(even) {{ background-color: #f8f9fa; }}
.styled-table tbody tr:hover {{ background-color: #eef2f7; }}

.styled-table th {{ position: sticky; top: 0; }}

.styled-table a {{
  color: #0b5ed7;
  text-decoration: none;
}}
.styled-table a:hover {{
  text-decoration: underline;
}}
</style>
</head>
<body>
<h1>George R. R. Martin – Book Data</h1>
{table}
</body>
</html>
"""

with open("george_r_r_martin.html", "w", encoding="utf-8") as f:
    f.write(page)
EOF
```

## The "Best Recent" books by genre

I define the best recent books as:

- published after the year 2000
- must have 4 or more star rating
- must have more than 10,000 ratings
- it's number of 5 star ratings must be higher than the number of 4 star ratings

There are a few json strings in the data but the following query works nicely for the genre `Space Opera`:

```sql
SELECT
    url
    , name
    , author
    , extract(year FROM strptime(first_published, '%m/%d/%Y')) AS year
FROM goodreads
WHERE 
    json_contains(genres, '"Space Opera"')
    and star_rating >= 4.0 
    and num_ratings > 10000
    and extract(year FROM strptime(first_published, '%m/%d/%Y')) > 2000
    and json_extract(community_reviews, '$.5_stars.reviews_num')::INT > json_extract(community_reviews, '$.4_stars.reviews_num')::INT
order by 
    star_rating desc
;
```

There are many more things do be done. A `streamlit` app would be an obvious next step but who has the time? Go, read books! ;-)