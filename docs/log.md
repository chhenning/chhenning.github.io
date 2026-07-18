## 2026-07-18

tags: sql

### Prequel - PRQL

PRQL is a modern language for transforming data.

[PRQL](https://prql-lang.org/)



## 2026-04-11

tags: postgres, job queue, IaC, knowledge graph

### OpenTofu

OpenTofu is a reliable, flexible, community-driven infrastructure as code tool under the Linux Foundation's stewardship. It serves as a drop-in replacement for Terraform, preserving your existing workflows and configurations.

[OpenTofu](https://opentofu.org/)
[OpenTofu Github](https://github.com/opentofu/opentofu)

### Graphile Worker

A high performance job queue for PostgreSQL, written in Node.js

[Graphile Worker](https://worker.graphile.org/)
[github repo](https://github.com/graphile/worker)

### atomicapp

Self-hosted, semantically-connected personal knowledge base

[atomicapp](https://atomicapp.ai/)
[atomicapp github](https://github.com/kenforthewin/atomic)


## 2026-04-07

tags: cli, mcp, bm25, vector search, git, regex, knowledge graph

### qmd

[github repo](https://github.com/tobi/qmd)

An on-device search engine for everything you need to remember. Index your markdown notes, meeting transcripts, documentation, and knowledge bases. Search with keywords or natural language. Ideal for your agentic flows.

QMD combines BM25 full-text search, vector semantic search, and LLM re-ranking—all running locally via node-llama-cpp with GGUF models.

### LLM Wiki

A pattern for building personal knowledge bases using LLMs.

This is an idea file, it is designed to be copy pasted to your own LLM Agent (e.g. OpenAI Codex, Claude Code, OpenCode / Pi, or etc.). Its goal is to communicate the high level idea, but your agent will build out the specifics in collaboration with you.

see: 

- [github gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [x](https://x.com/karpathy/status/2039805659525644595)

### git worktrees

```sh
# !!! One branch per worktree
# !!! Don’t delete folders manually

# inside the repo
git worktree add -b feature-branch ../my-project-feature

# list worktrees
git worktree list


git worktree remove ../my-project-feature
# if git complains
git worktree prune
```

### negative keyword pattern

used in Claude Code:

[github](https://github.com/chatgptprojects/clear-code/blob/642c7f944bbe5f7e57c05d756ab7fa7c9c5035cc/src/utils/userPromptKeywords.ts#L8)

## 2026-03-24

tags: cli

I'm very paranoid deleting folders with `rm -rf`. How about safeguarding it?

Add this to `~/.zshrc`:

```sh
rm() {
    echo "Will delete:"
    for arg in "$@"; do
        [[ "$arg" != -* ]] && echo "  $(realpath "$arg" 2>/dev/null || echo "$arg")"
    done
    echo ""
    read -q "REPLY?Proceed? (y/n) " || { echo "\nAborted."; return 1; }
    echo ""
    command rm "$@"
}
compdef rm=rm
```

The last part `compdef rm=rm` makes sure the tab completion still works.

Now every time you try to `rm -rf FOLDER` the shell will ask if you want to delete the folder using the full path.

## 2026-04-06

tags: spacex, ipo, gemma, llm

### Spacex IPO

very interesting insight into the IPO and why it might not be a good idea to invest...

[hackernews](https://news.ycombinator.com/item?id=47613231)

### Gemma 4 - How to Run Locally

Run Google’s new Gemma 4 models locally, including E2B, E4B, 26B A4B, and 31B.

[link](https://unsloth.ai/docs/models/gemma-4)


## 2026-02-28

### opencode

tags: qwen3, llama.cpp, opencode, hackernews

Here is nice little tutorial on how to setup opencode with a local model.

[OpenCode + Llama.cpp Setup Guide](https://gist.github.com/alexpotato/5b76989c24593962898294038b5b835b)

I found this guide on the `hackernews` article:

[Qwen3.5 122B and 35B models offer Sonnet 4.5 performance on local computers](https://news.ycombinator.com/item?id=47199781)

## 2026-02-16

### Blocking Youtube shorts

tags: youtube, ublock, chrome

I'm using Chrome on macos with [ublock origin lite](https://chromewebstore.google.com/detail/ublock-origin-lite/ddkjiahejlhfcafbddmgiahcphecmpfh?hl=en) extension.

Copy and paste the [rules](https://raw.githubusercontent.com/i5heu/ublock-hide-yt-shorts/master/list.txt) into `Custom Filters` text box. Now reload youtube and
the shorts should disappear.

See:

[uBlock filter list to hide all YouTube Shorts](https://github.com/i5heu/ublock-hide-yt-shorts/)

## 2026-02-14

### physarum-step-by-step

Finished my first project with Claude Code. Amazing tool to be honest. I learned so much!

[physarum-step-by-step](https://github.com/chhenning/physarum-step-by-step)

## 2026-02-13

## physarum

tags: go, slime mold, visualization

Running a `go` tool.

```sh
brew install go
```

```sh
git clone git@github.com:fogleman/physarum.git
cd physarum

go run cmd/physarum/main.go
```

Error: `cmd/physarum/main.go:8:2: no required module provides package github.com/fogleman/physarum/pkg/physarum: go.mod file not found in current directory or any parent directory; see 'go help modules'

```
go mod init physarum
go mod tidy
go run ./cmd/physarum
```

Now it works.

Turns out this tool does not do a realtime simulation but creates beautiful images.

## 2026-02-12

### Simulating Physarum polycephalum slime mold

tags: generative algorithms, visualization, slime mold

Simulating Physarum polycephalum slime mold generates very cool pictures. See below.

[Physarum Simulation](https://www.michaelfogleman.com/projects/physarum/)
[Physarum transport model](https://cargocollective.com/sagejenson/physarum)
[Coding Adventure: Ant and Slime Simulations](https://www.youtube.com/watch?v=X-iSQQgOd1A)

## 2026-02-08

### 72M Points of Interest

tags: venues, poi, duckdb

Interesting blog post about a dataset with 72M Points of Interest (POI) including their website.

Not only does Mark present the data but he also shows how to examine it using `duckdb`.

[72M Points of Interest](https://tech.marksblogg.com/overture-places-pois.html)

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
