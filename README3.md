# FTW DE Bootcamp — Exercise Run Guide

This guide shows how to run each **extract → load (dlt)** and **transform (dbt)** exercise in your Docker setup. Commands assume you’re in the project root.

> **Quick tips**
>
> * Start the **core** stack once (ClickHouse + Metabase + Chinook Postgres) and leave it running.
> * Each exercise is: **run dlt → run dbt (in that exercise folder)**.
> * Local vs Remote: edit envs in `compose.yaml` as noted in each service block.

---

## 0) Start/Stop core services

```bash
# Start core (ClickHouse, Metabase, Chinook Postgres)
docker compose --profile core --compatibility up -d 

# Check health
docker ps
# Metabase: http://localhost:3001 • ClickHouse HTTP: http://localhost:8123

# Stop everything (when done)
docker compose down
```

---

## 01 — Auto MPG

**Extract & Load (dlt)**

```bash
docker compose --profile jobs run --rm \
  dlt python extract-loads/01-dlt-mpg-pipeline.py
```

**Transform (dbt)**

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/01_mpg \
  dbt build --profiles-dir . --target local
```

**Verify**

* In Metabase, add ClickHouse (if not added) and browse the `clean` / `mart` models for MPG.

---

## 02 — Chinook (Postgres → ClickHouse)

> Chinook Postgres is prepared by `chinook_fetch` + `postgres_chinook` in the **core** profile.

**Extract & Load (dlt)**

```bash
docker compose --profile jobs run --rm \
  dlt python extract-loads/02-dlt-chinook-pipeline.py
```

**Transform (dbt)**

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/02_chinook \
  dbt build --profiles-dir . --target local
```

**Verify**

* Explore albums, artists, invoices in Metabase; confirm `stg_` views and `mart_` models appear.

---

## 03 — Weather (Meteo)

There are **three** loaders you can try:

* `03-dlt-meteo-pipeline.py` (base daily history)
* `03-dlt-xtra-meteo-ph-regions.py` (regional add-ons)
* `03-dlt-wmo-codes-pipeline.py` (lookup codes)

**Extract & Load (dlt)**

```bash
# Base
docker compose --profile jobs run --rm \
  dlt python extract-loads/03-dlt-meteo-pipeline.py

# Optional extras
docker compose --profile jobs run --rm \
  dlt python extract-loads/03-dlt-xtra-meteo-ph-regions.py

docker compose --profile jobs run --rm \
  dlt python extract-loads/03-dlt-wmo-codes-pipeline.py
```

**Transform (dbt)**

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/03_meteo \
  dbt build --profiles-dir . --target local
```

**Verify**

* Check cleaned daily temps/precip, join to WMO code lookups, and sample dashboards in Metabase.

---

## 04 — Bitcoin (BTC)

**Extract & Load (dlt)**

```bash
docker compose --profile jobs run --rm \
  dlt python extract-loads/04-dlt-btc-pipeline.py
```

**Transform (dbt)**

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/04_btc \
  dbt build --profiles-dir . --target local
```

**Verify**

* Plot OHLC/close vs date in Metabase. Try simple moving averages in a SQL question.

---

## 05 — Pokémon

**Extract & Load (dlt)**

```bash
docker compose --profile jobs run --rm \
  dlt python extract-loads/05-dlt-poke-pipeline.py
```

**Transform (dbt)**

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/05_poke \
  dbt build --profiles-dir . --target local
```

**Verify**

* Browse Pokémon stats/models in Metabase; create a bar chart by type/avg attack.

---

## 06 — Lazada (sample dataset)

**Extract & Load (dlt)**

```bash
docker compose --profile jobs run --rm \
  dlt python extract-loads/06-dlt-laz-pipeline.py
```

**Transform (dbt)**

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/06_laz \
  dbt build --profiles-dir . --target local
```

**Verify**

* Build a price vs rating scatter, category leaders, top sellers, etc.

---

## 07 — Foodpanda Reviews (Optional)

**Extract & Load (dlt)**

```bash
docker compose --profile jobs run --rm \
  dlt python extract-loads/07-dlt-food-pipeline.py
```

**Transform (dbt)**

> ⚠️ Your snippet pointed dbt to `06_laz`. The correct path is **`07_foodpanda`**.

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/07_foodpanda \
  dbt build --profiles-dir . --target local
```

**Verify**

* Sentiment distributions, top restaurants by average rating and review volume.

---

## 08 — Unstructured (Optional)

**Extract & Load (dlt)**

```bash
docker compose --profile jobs run --rm \
  dlt python extract-loads/08-dlt-unstructured-pipeline.py
```

**Transform (dbt)**
(If you add a folder later under `dbt/transforms/08_unstructured`)

```bash
docker compose --profile jobs run --rm \
  -w /workdir/transforms/08_unstructured \
  dbt build --profiles-dir . --target local
```

---

## Remote Mode (students target a server)

1. On the **server** (teacher):

   ```bash
   docker compose --compatibility up -d --profile core
   # Ensure inbound ports 8123 (ClickHouse HTTP) and 3001 (Metabase) are open.
   ```
2. On **student** laptops:

   * In `compose.yaml`:

     * For **dlt**: set `DESTINATION__CLICKHOUSE__CREDENTIALS__HOST` to server IP/DNS
     * For **dbt**: set `CH_HOST` to server IP/DNS and (optionally) `DBT_SCHEMA=ftw_<alias>`
   * Then run the same **dlt** and **dbt** commands per exercise.

---

## Troubleshooting Cheatsheet

* **ClickHouse not ready**

  * Wait for healthcheck: `docker logs clickhouse`
* **Chinook loader not found**

  * Ensure `chinook_fetch` completed and `postgres_chinook` is healthy: `docker ps`
* **DBT permissions**

  * If using a shared remote server, set a unique `DBT_SCHEMA` like `ftw_<alias>`
* **Metabase can’t see new tables**

  * In Admin → Databases → ClickHouse → **Sync** & **Re-scan field values**
* **Port conflicts**

  * Change host ports in `compose.yaml` (e.g., `65432:5432`) then `docker compose up -d`

---

## Clean Up

```bash
# Stop and remove containers (keep volumes)
docker compose down

# Nuke volumes (⚠️ deletes data)
docker compose down -v
```

That’s it—run dlt then dbt for each folder above, and explore your models in Metabase.
