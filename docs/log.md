
## 2026-02-07

### Debugging Python in vscode

Hopefully for the last time in my live this is the correct `launch.json` for debugging a Python script inside vscode.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Current File",
      "type": "debugpy",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "env": {
        "PYTHONPATH": "${workspaceFolder}"
      }
    }
  ]
}
```


## 2026-02-05

### docling

tags: todo, ocr, ibm, docling, pdf

Docling converts messy documents into structured data and simplifies downstream document and AI processing by detecting tables, formulas, reading order, OCR, and much more.

It's github repo has over 52K stars!

[How to use cli and python](https://simonwillison.net/2024/Nov/3/docling/)


## 2026-01-27

### .sqliterc

tags: sqlite, dot file

The default config for sqlite's cli tool are not very good when displaying data in the terminal. But sqlite does support a config file
called `.sqliterc` in your home folder.

Here is mine:

```
.headers on
.mode column
.nullvalue NULL
.prompt "sqlite> "
.timer on
```

## 2026-01-23

### postgres_fdw

tags: postgres, aws, rds

I have been using Postgres' [dblink_connect](https://www.postgresql.org/docs/current/contrib-dblink-connect.html) for the longest time.

Turns out there is possibly an even better way. Enter [postgres_fdw](https://www.postgresql.org/docs/current/postgres-fdw.html). It is an supported
extension even by AWS' RDS and it let's you query remote tables in normal sql.

An example:

```sql
CREATE EXTENSION postgres_fdw;

CREATE SERVER remote_pg
FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (
    host 'other-db.abcdefg.us-east-1.rds.amazonaws.com',
    port '5432',
    dbname 'orders'
);

CREATE USER MAPPING FOR my_user
SERVER remote_pg
OPTIONS (
    user 'remote_user',
    password 'secret'
);

CREATE FOREIGN TABLE foreign_orders (
    id bigint,
    total numeric
)
SERVER remote_pg
OPTIONS (schema_name 'public', table_name 'orders');
```

### A mental model for binary classifier confusion matrix

```
                    ACTUAL
               Positive    Negative
PREDICTED
Positive        TP           FP
Negative        FN           TN
```

## 2026-01-22

### Dynamic Programming Tutorial

tags: dynamic programming, recursion

It's always fun to revisit dynamic programming. This [tutorial](https://www.youtube.com/watch?v=66hDgWottdA) is very well made.

### How to build your own local AI stack on Linux with llama.cpp, llama-swap, LibreChat and more

tags: llama.cpp, llm, qwen, nvidia, cuda, article, huggingface, librechat, llama-swap

[link](https://imadsaddik.com/blogs/local-ai-stack-on-linux)

https://huggingface.co/Qwen/Qwen3-30B-A3B-Instruct-2507/tree/main

llama.cpp does not work with the `safetensors` format, it works with the `GGUF` format. This format is optimized for quick loading and saving of models, and running models efficiently on consumer hardware.

[convert safetensor to gguf](https://github.com/ggml-org/llama.cpp/blob/master/convert_hf_to_gguf.py)

Projects that convert to gguf:

- [Unsloth](https://huggingface.co/unsloth/models)
- [Bartowski](https://huggingface.co/bartowski)
- [TheBloke](https://huggingface.co/TheBloke)

Avoid downloading models in FP32 or FP16 precision, as these unquantized formats require a lot of memory, especially for very large models.

Instead, download quantized versions of the model in the GGUF format, because they use less memory. A great starting point is the Q8_K quantization level.

### Quantization Types

tags: huggingface, llm

[huggingface's quantization-types](https://huggingface.co/docs/hub/gguf#quantization-types)

### OCR with layout

tags: ocr, layout, docling, doctags

`Grounded Text` refers to text directly linked or anchored to specific visual regions in an image (like a bounding box), crucial for vision-language models (VLM) to understand where text is, while `DocTags` (Document Tags) are high-level semantic labels/metadata applied to entire documents or sections, offering what the content is about, with Grounding focusing on spatial, visual-textual alignment (e.g., a price tag on a product image) and DocTags on semantic classification (e.g., "invoice," "receipt," "contract") for better organization and retrieval in document understanding tasks.

[docling document](https://docling-project.github.io/docling/concepts/docling_document/)

## 2026-01-21

### Unconventional PostgreSQL Optimizations

tags: postgres, sql

[Unconventional PostgreSQL Optimizations](https://news.ycombinator.com/item?id=46692116)

### Magick

```sh
brew install imagemagick

magick ~/Downloads/image.jpeg -quality 50 ~/Downloads/image_small.jpeg

```

## 2026-01-20

### ESPN Unofficial Public API

tags: sports, api, espn

`Disclaimer`: This is documentation for ESPN's undocumented public API. I am not affiliated with ESPN. Use responsibly and follow ESPN's terms of service.

[public ESPN API](https://github.com/pseudo-r/Public-ESPN-API)

### Backtesting.py

tags: python, finance, trading, simulation, pandas

Backtest trading strategies in Python. See [backtesting.py](https://kernc.github.io/backtesting.py/)

Also, [pandas_market_calendars](https://github.com/rsheftel/pandas_market_calendars)

## 2026-01-17

### Docker Cheat Sheet

tags: docker, cheat sheet

[docker cheat sheet](https://docker.how/)

## 2026-01-15

### Why DuckDB is my first choice for data processing

tags: duckdb, hackernews

[Why DuckDB is my first choice for data processing ](https://news.ycombinator.com/item?id=46645176)

### Ask HN: How are you doing RAG locally?

tags: RAG, hackernews, embedding

[Ask HN: How are you doing RAG locally?](https://news.ycombinator.com/item?id=46616529)

## 2026-01-14

### turn off type checking

tags: vscode, python, pylance

During development the constant type checking will result in red squiggles inside the code. I find that really annoying and distraction.

In your `.vscode/settings.json` just add the next line.

```json
"python.analysis.typeCheckingMode": "off"
```

## 2026-01-10

### OpenCode

tags: LLM, agent, vibe coding, todo

The open source AI coding agent.

[OpenCode](https://opencode.ai/)
[github](https://github.com/anomalyco/opencode)

### Machine Learning, Statistical Inference and Induction

tags: ml, article, todo

[Machine Learning, Statistical Inference and Induction](https://web.archive.org/web/20250815161703/http://www.bactra.org/notebooks/learning-inference-induction.html)

### The Q, K, V Matrices

tags: todo, transformer, attention

[The Q, K, V Matrices](https://news.ycombinator.com/item?id=46523887)

### Use Claude Code to Query 600 GB Indexes over Hacker News, ArXiv, etc.

tags: llm, search, todo, sql, fts

[Show HN: Use Claude Code to Query 600 GB Indexes over Hacker News, ArXiv, etc.](https://news.ycombinator.com/item?id=46442245)

### Raymond Hettinger - Modern solvers: Problems well-defined are problems solved - PyCon 2019

tags: youtube, solvers, search, rl, python, tutorial

[video](https://www.youtube.com/watch?v=_GP9OpZPUYc)
[doc](https://rhettinger.github.io/)

## 2026-01-05

### Struddel

tags: music, melody, struddel

Twinkle Twinkle Little Star

```
note(`
<c c g g a a g@2
 f f e e d d c@2
 g g f f e e d@2
 g g f f e e d@2
 c c g g a a g@2
  f f e e d d c@2>*4
`).sound('piano')
```

### OpenML

tags: dataset, ml, sklearn

[openml](https://www.openml.org/)

[fetch_openml](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_openml.html)

### Vehicle Dataset

tags: dataset, kaggle, ml

[dataset](https://www.kaggle.com/datasets/nehalbirla/vehicle-dataset-from-cardekho/data)

## 20260101

### Comtrade

tags: python, comtrade

[Python Comtrade](https://github.com/dparrini/python-comtrade)

[pyComtrade](https://github.com/miguelmoreto/pycomtrade)

## 20251231

### postgres extensions

tags: postgres, vector db, pgvector, FTS

[pgvectorscale](https://github.com/timescale/pgvectorscale)
[pg_textsearch](https://github.com/timescale/pg_textsearch)

## 2025-12-29

### create a password

tags: cli, sh

```sh
openssl rand -base64 24
```

### Add markdown to Google Document

tags: markdown, Google Doc

Inside a Google document click on Tool->Preferences. There `Enable Markdown`. Now you can `Paste From Markdown`.

## 2025-12-23

### blog links

tags: blog, markdown

I have been searching for a good blog solution for awhile and finally I have found it.

[Material for mkdocs](https://squidfunk.github.io/mkdocs-material/reference/)
[github workflow](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)
[pymdown extension](https://facelessuser.github.io/pymdown-extensions/extensions/blocks/plugins/caption/)

### Instant database clones with PostgreSQL 18

tags: postgresql, sql

[blog](https://boringsql.com/posts/instant-database-clones/)
[hackernews](https://news.ycombinator.com/item?id=46363360)

## 2025-12-22

### color-science

tags: python, color

[repo](https://github.com/colour-science/colour)

## 2025-12-21

### Hands-On ML with Scikit-Learn and PyTorch

tags: python, ml, sklearn, torch, pandas, matplotlib, book

There is a new edition of my favorite ML book. This time with pytorch!

[repo](https://github.com/ageron/handson-mlp/tree/main)

[numpy cheatsheet](https://github.com/ageron/handson-mlp/blob/main/tools_numpy.ipynb)
[pandas cheatsheet](https://github.com/ageron/handson-mlp/blob/main/tools_pandas.ipynb)
[matplotlib cheatsheet](https://github.com/ageron/handson-mlp/blob/main/tools_matplotlib.ipynb)

## Leap 71

tags: company, space, rocket, computational engineering

[video](https://youtu.be/6Xx1GXjRbMk?si=ZAu1NdyTnzdl_TA4)
[company](https://leap71.com/)
[Lin Kayser](https://www.linkedin.com/in/linkayser/)

## ResumeCV

tags: resume, yaml, pdf

Great module to create good looking resumes. The resume data is a yaml file. And that is easy to be fed into a LLM!

[doc](https://docs.rendercv.com/)

## 2025-12-20

### Jekyll

tags: website, blog, github, site generator

(jekyll)[https://jekyllrb.com/]

### Material for MkDocs

tags: site generator, python, blog, website

[repo](https://github.com/squidfunk/mkdocs-material/)

[tutorial](https://jameswillett.dev/getting-started-with-material-for-mkdocs)

[blog example](https://github.com/mkdocs-material/create-blog)
[How To Build and Deploy a Stunning Blog for FREE using Material for MkDocs](https://www.youtube.com/watch?v=pPEUhfTZswc&list=PLw_jGKXm9lIaJCD8YClu6cAz1TcFdJdIf&index=9)


## 2025-12-19

### Self hosting challenges and how to limit scraper bots

tags: scraper, bot, blog, vps, self hosting, hackernews

This article prompted me to do some research of how to deal with excessive scraper bots when self hosting an app or a blog.

[I got hacked, my server started mining Monero this morning.](https://blog.jakesaunders.dev/my-server-started-mining-monero-this-morning/)

[Anubis](https://github.com/TecharoHQ/anubis) is a Web AI Firewall Utility that weighs the soul of your connection using one or more challenges in order to protect upstream resources from scraper bots. Also see [this on hackernews](https://news.ycombinator.com/item?id=46294144).

#### Server-Side Rate Limiting (Web Server)

Asking Gemini for a good solutions to avoid scraper bots -> [gemini chat](https://aistudio.google.com/prompts/1ppxjZccFQnDSDKu9vaDKSpe7LeFUBeV3)

You can configure your web server to strictly limit how fast any single IP can download pages. This makes scraping painfully slow for bots, effectively discouraging them.

If you use `Nginx`:
Add this to your nginx.conf (http block):

```
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
```

```
location / {
    limit_req zone=one burst=10 nodelay;
    # ... rest of config
}
```

This limits every IP to roughly 1 request per second. Real users won't notice, but scrapers trying to fetch 100 pages at once will get rejected (Error 503).

#### "Hard" Limit via Custom Script (Advanced)

If you absolutely must ensure the server shuts down after a certain bandwidth limit (e.g., 1 TB), you have to script it yourself.

**Option A: Bandwidth Speed Limit (tc or wondershaper)**

You can cap your server's uplink speed. For example, if you cap your upload speed to 10 Mbps, the maximum theoretical outbound traffic you can generate in a month is about 3.2 TB, making it physically impossible to exceed the 20 TB limit.

**Option B: Auto-Shutdown Script**

Install a tool like `vnstat` to monitor traffic, and write a simple cron script that checks usage every hour.

Add it to crontab to run hourly.

```sh
#!/bin/bash
# Get current monthly TX (transmit/egress) in GiB
USAGE=$(vnstat --oneline | cut -d';' -f10 | cut -d' ' -f1)

# Set limit to 1000 GiB (1 TB)
LIMIT=1000

# Compare (using integer math)
if (( $(echo "$USAGE > $LIMIT" | bc -l) )); then
    echo "Limit exceeded. Shutting down network interface."
    # Choose one:
    # ip link set eth0 down   # Kills network
    # poweroff                # Shuts down server completely
fi
```

### htmx

tags: htmx, javascript, html, webapp, hackernews

[Please just try HTMX](https://news.ycombinator.com/item?id=46312973)

A quote:

```
Hey, I created htmx and while I appreciate the publicity, I’m not a huge fan of these types of hyperbolic articles. There are lots of different ways to build web apps with their own strengths and weaknesses. I try to assess htmx’s strengths and weaknesses here:
https://htmx.org/essays/when-to-use-hypermedia/

Also, please try unpoly:

It’s another excellent hypermedia oriented library

Edit: the article is actually not nearly as unreasonable as I thought based on the just-f*king-use template. Still prefer a chill vibe for htmx though.
```

See: [unpoly](https://github.com/unpoly/unpoly)

### unidecode

tags: ascii, unicode, string, python

[unidecode](https://github.com/avian2/unidecode) is a great lib for a common problem. How to make a reasonable ascii string out of unicode? 

For example:

- `unidecode('kožušček')` -> `'kozuscek'`
- `unidecode('30 \U0001d5c4\U0001d5c6/\U0001d5c1')` -> `'30 km/h'`


## 2025-12-18

### Pydantic AI

tags: pydantic, Python, AI, Agents

[repo](https://github.com/pydantic/pydantic-ai)

### Langchain course

tags: langchain, python, AI, Agents

[course](https://academy.langchain.com/courses/foundation-introduction-to-langchain-python)

### Postgresql distinct

tags: postgres, sql

Great overview of how to use the `distinct` keyword in PostgreSQL. 

[https://hakibenita.com/the-many-faces-of-distinct-in-postgre-sql](https://hakibenita.com/the-many-faces-of-distinct-in-postgre-sql)

A few code examples:

```sql
CREATE TEMP TABLE tmp_employee (
    id         INT,
    name       TEXT,
    department TEXT,
    salary     INT
)
;

INSERT INTO tmp_employee (id, name, department, salary) VALUES
(30, 'Sara Roberts',     'Accounting',               13845),
(4,  'Benjamin Brown',   'Business Development',      7386),
(3,  'Carolyn Carter',   'Engineering',               8366),
(20, 'Janet Hall',       'Human Resources',            2826),
(14, 'Chris Phillips',   'Legal',                     3706),
(10, 'James Cunningham', 'Legal',                     3706),
(11, 'Richard Bradley',  'Marketing',                11272),
(2,  'Richard Fox',      'Product Management',       13449),
(25, 'Evelyn Rodriguez', 'Research and Development', 10628),
(17, 'Benjamin Carter',  'Sales',                     6197),
(24, 'Jessica Elliott',  'Services',                 14542),
(7,  'Bonnie Robertson', 'Support',                  12674),
(8,  'Jean Bailey',      'Training',                 13230)
;
```

```sql
-- get all unique departments
SELECT DISTINCT department FROM tmp_employee;
```

#### DISTINCT ON

```sql
-- get the highest earners per department
-- use the employee id as the tiebreaker
SELECT DISTINCT ON (department)
    *
FROM
    tmp_employee
ORDER BY
    department,
    salary DESC,
    id ASC;
;
```

#### DISTINCT FROM

`DISTINCT FROM` treats NULL values as real value and so comparing will get a boolean answers.

```sql
WITH old_data AS (
    SELECT 1 AS emp_id, 'Engineer' AS title UNION ALL
    SELECT 2, NULL UNION ALL
    SELECT 3, 'Manager'
),
new_data AS (
    SELECT 1 AS emp_id, 'Engineer' AS title UNION ALL
    SELECT 2, 'Analyst' UNION ALL
    SELECT 3, NULL
)
SELECT
    o.emp_id,
    o.title AS old_title,
    n.title AS new_title,
    o.title = n.title AS equals_operator,       -- this will break when one side is NULL
    o.title IS DISTINCT FROM n.title AS changed -- this works even when one side is NULL
FROM old_data o
JOIN new_data n USING (emp_id);
```

#### ARRAY_AGG

Bonus!

Aggregate all values into a json.

```sql
SELECT
    department,
    ARRAY_AGG(name) AS employees
FROM
    tmp_employee
GROUP BY
    department
;
```
