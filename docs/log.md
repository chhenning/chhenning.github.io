# 2026-01-23

## postgres_fdw

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

# 2026-01-22

## Dynamic Programming Tutorial

tags: dynamic programming, recursion

It's always fun to revisit dynamic programming. This [tutorial](https://www.youtube.com/watch?v=66hDgWottdA) is very well made.

## How to build your own local AI stack on Linux with llama.cpp, llama-swap, LibreChat and more

tags: llama.cpp, llm, qwen, nvidia, cuda, article, huggingface, librechat, llama-swap

[link](https://imadsaddik.com/blogs/local-ai-stack-on-linux)


https://huggingface.co/Qwen/Qwen3-30B-A3B-Instruct-2507/tree/main


llama.cpp does not work with the `safetensors` format, it works with the `GGUF` format. This format is optimized for quick loading and saving of models, and running models efficiently on consumer hardware.

[convert safetensor to gguf](https://github.com/ggml-org/llama.cpp/blob/master/convert_hf_to_gguf.py)

Projects that convert to gguf:

[Unsloth](https://huggingface.co/unsloth/models)
[Bartowski](https://huggingface.co/bartowski)
[TheBloke](https://huggingface.co/TheBloke)

Avoid downloading models in FP32 or FP16 precision, as these unquantized formats require a lot of memory, especially for very large models.

Instead, download quantized versions of the model in the GGUF format, because they use less memory. A great starting point is the Q8_K quantization level.

## Quantization Types

tags: huggingface, llm

https://huggingface.co/docs/hub/gguf#quantization-types

## OCR with layout

tags: ocr, layout, docling, doctags

`Grounded Text` refers to text directly linked or anchored to specific visual regions in an image (like a bounding box), crucial for vision-language models (VLM) to understand where text is, while `DocTags` (Document Tags) are high-level semantic labels/metadata applied to entire documents or sections, offering what the content is about, with Grounding focusing on spatial, visual-textual alignment (e.g., a price tag on a product image) and DocTags on semantic classification (e.g., "invoice," "receipt," "contract") for better organization and retrieval in document understanding tasks.

[docling document](https://docling-project.github.io/docling/concepts/docling_document/)

# 2026-01-21

## Unconventional PostgreSQL Optimizations  

tags: postgres, sql

[Unconventional PostgreSQL Optimizations](https://news.ycombinator.com/item?id=46692116)

## Magick

```sh
brew install imagemagick

magick ~/Downloads/image.jpeg -quality 50 ~/Downloads/image_small.jpeg

```

# 2026-01-20

## ESPN Unofficial Public API

tags: sports, api, espn

`Disclaimer`: This is documentation for ESPN's undocumented public API. I am not affiliated with ESPN. Use responsibly and follow ESPN's terms of service.

[public ESPN API](https://github.com/pseudo-r/Public-ESPN-API)

## Backtesting.py

tags: python, finance, trading, simulation, pandas

Backtest trading strategies in Python. See [backtesting.py](https://kernc.github.io/backtesting.py/)

Also, [pandas_market_calendars](https://github.com/rsheftel/pandas_market_calendars)


# 2026-01-17

## Docker Cheat Sheet

tags: docker, cheat sheet

[docker cheat sheet](https://docker.how/)


# 2026-01-15

## Why DuckDB is my first choice for data processing

tags: duckdb, hackernews

[Why DuckDB is my first choice for data processing ](https://news.ycombinator.com/item?id=46645176)

## Ask HN: How are you doing RAG locally?

tags: RAG, hackernews, embedding

[Ask HN: How are you doing RAG locally?](https://news.ycombinator.com/item?id=46616529)

