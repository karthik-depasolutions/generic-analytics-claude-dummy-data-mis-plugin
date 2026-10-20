---
name: generic-analytics-analyst
description: Use this skill when analyzing Generic Analytics Pack data - revenue,
  volumes, trends, and pre-built KPIs (count_by_category, total_records). Guides querying
  via the bundled MCP tools instead of ad-hoc SQL.
metadata:
  pack_slug: generic-analytics
  pack_version: 1.0.0
---

# Generic Analytics Pack Analyst

The Generic Analytics Pack skill should be utilized when analyzing tabular business data that does not clearly align with any specialized industry analysis packs. This skill serves as an industry-agnostic fallback, providing a foundational baseline analysis when a more specific match isn't identified.

## This database

This database primarily logs the execution states and events of AI agent runs. The core entities are 'checkpoints', which represent specific states of an agent run, and 'checkpoint_writes' and 'checkpoint_blobs', which detail the data written to various channels during these runs. While no explicit relationships were detected, tables share common identifiers like 'thread_id' and 'checkpoint_id', suggesting implicit connections for tracking an agent's progression. 'checkpoint_migrations' tracks schema versions for the checkpointing system itself.

The knowledge pack documents 4 table(s). The tables below are a starting point; use `schema://model` for the full inventory.

### Business entities and grain
- `checkpoint_blobs` (log): One row represents a specific checkpoint blob, storing the state of an AI agent run at a particular version and channel.. Grain: one row per instance of a checkpoint blob for a specific AI agent run, version, and channel (no unique key found across .
  - `thread_id`: A unique identifier for a specific AI agent run or thread.
  - `channel`: The specific communication channel or component within the AI agent run that this checkpoint relates to.
  - `version`: A version or sequence number indicating the progression of the checkpoint within the AI agent run.
- `checkpoint_migrations` (log): This table tracks the version history of database schema migrations specifically for the system managing AI agent run checkpoints.. Grain: one row per distinct database migration version for the checkpointing system.
  - `v`: The sequential version number or step in the database migration process for the checkpointing system.
- `checkpoint_writes` (log): Records specific states or events (checkpoints) during the execution of AI agent runs, detailing the data written to various channels.. Grain: one row per unique (checkpoint_id, channel).
  - `thread_id`: Unique identifier for a specific AI agent run or conversation thread.
  - `checkpoint_ns`: Namespace for the checkpoint, currently unused or defaulting to an empty value.
  - `checkpoint_id`: Unique identifier for a specific checkpoint within an AI agent run.
- `checkpoints` (log): One row represents a specific state or 'checkpoint' reached during an AI agent's execution or conversation thread.. Grain: one row per unique (checkpoint_id, thread_id).
  - `thread_id`: A unique identifier for a specific AI agent run or conversation thread.
  - `checkpoint_ns`: The namespace associated with the checkpoint.
  - `checkpoint_id`: A unique identifier for this specific checkpoint within its thread.

### Interpretation caveats
- The `type` column in `srcdb."public"."checkpoints"` is always null and provides no information.
- The `checkpoint_ns` column in `srcdb."public"."checkpoint_blobs"`, `srcdb."public"."checkpoint_writes"`, and `srcdb."public"."checkpoints"` is always an empty string and can be ignored.
- The `type` column in `srcdb."public"."checkpoint_blobs"` is always 'msgpack'.
- The `type` column in `srcdb."public"."checkpoint_writes"` is predominantly 'msgpack' (73.4%), but can also be 'null'.
- The `idx` column in `srcdb."public"."checkpoint_writes"` acts as a binary flag, with values '0' and '1'.

### Questions with verified query examples
- How many total checkpoint blobs are stored?
- List all unique communication channels present in checkpoint blobs.
- Count the number of checkpoint blobs per channel, ordered by the most frequent.
- Retrieve an example of a blob content for the 'messages' channel.
- What is the latest database migration version for the checkpointing system?
Retrieve the corresponding SQL from `schema://cookbook` before using it.

## Analytical Methodology

When analyzing business performance, follow this structured diagnostic protocol:
1. **Frame the Question**: Restate ambiguous questions with specific metrics, time horizons, and baseline periods.
2. **Decompose Metrics First**: Break composite metrics into constituent drivers (e.g. `Revenue = Volume × Unit Price × Mix`).
3. **Query via Grounded Tools**: Fetch data using the bundled MCP tools (`get_kpi`, `run_safe_query`) without guessing columns.
4. **Drill Down by Dimensions**: Compare period-over-period performance across available segments, ordering by absolute variance contribution.
5. **Synthesize with BLUF**: Lead with the Bottom Line Up Front, follow with quantified evidence, and conclude with actionable recommendations.

## Available KPIs

- `count_by_category` - **Count by Category**: Record count grouped by the primary categorical dimension. (unit: count)
- `total_records` - **Total Records**: Total number of rows in the primary table. (unit: count)

Not available for this data source (required data was missing): `average_measure`, `sum_measure`, `trend_by_month`

## How to Use the MCP Analytics Tools

Before writing any SQL, read the `schema://model` resource (per-table meaning, column definitions, enum decodes) and `schema://cookbook` (verified example queries for this exact database). Use `schema://relationships` for join keys. Then follow this hierarchy:
1. **Discovery**: Call `mcp__mis-mcp-claude-dummy-data__describe_data`, `mcp__mis-mcp-claude-dummy-data__list_business_concepts`, or `mcp__mis-mcp-claude-dummy-data__list_kpis` first to discover available business entities and metrics.
2. **KPIs & Metrics**: Prefer `mcp__mis-mcp-claude-dummy-data__get_kpi` for standard verified metrics, `mcp__mis-mcp-claude-dummy-data__compare_kpi` for period comparisons, `mcp__mis-mcp-claude-dummy-data__rank_entities` for top/bottom rankings, and `mcp__mis-mcp-claude-dummy-data__breakdown_metric` for dimensional slices.
3. **Categorical Slices**: Use `mcp__mis-mcp-claude-dummy-data__get_value_set` to discover distinct category values safely without exploratory SQL.
4. **Entity Records**: Use `mcp__mis-mcp-claude-dummy-data__get_record`, `mcp__mis-mcp-claude-dummy-data__search_records`, or `mcp__mis-mcp-claude-dummy-data__get_related_records` for entity lookups.
5. **Escape Hatch**: Use `mcp__mis-mcp-claude-dummy-data__run_safe_query` ONLY as a last resort when no semantic or metric tool can answer the question.

## Guardrails & Privacy

- No industry-specific vocabulary is assumed — treat all categorical values literally
- All data access must be read-only
