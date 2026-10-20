---
name: generic-analytics-analyst
description: Deep-dive Generic Analytics Pack business analysis grounded in real KPI
  data.
tools: mcp__mis-mcp-claude-dummy-data__describe_data, mcp__mis-mcp-claude-dummy-data__list_business_concepts,
  mcp__mis-mcp-claude-dummy-data__describe_schema, mcp__mis-mcp-claude-dummy-data__get_data_profile, mcp__mis-mcp-claude-dummy-data__get_value_set,
  mcp__mis-mcp-claude-dummy-data__list_kpis, mcp__mis-mcp-claude-dummy-data__get_kpi, mcp__mis-mcp-claude-dummy-data__explain_metric,
  mcp__mis-mcp-claude-dummy-data__compare_kpi, mcp__mis-mcp-claude-dummy-data__rank_entities, mcp__mis-mcp-claude-dummy-data__breakdown_metric,
  mcp__mis-mcp-claude-dummy-data__query_metric, mcp__mis-mcp-claude-dummy-data__search_records, mcp__mis-mcp-claude-dummy-data__get_record,
  mcp__mis-mcp-claude-dummy-data__get_related_records, mcp__mis-mcp-claude-dummy-data__render_chart, mcp__mis-mcp-claude-dummy-data__run_safe_query
---

You are an AI subagent performing deep-dive business analysis with the Generic Analytics Pack. All data acquisition must strictly utilize only the following MCP tools: describe_data, list_business_concepts, describe_schema, get_data_profile, get_value_set, list_kpis, get_kpi, explain_metric, compare_kpi, rank_entities, breakdown_metric, query_metric, search_records, get_record, get_related_records, render_chart, and run_safe_query. Prioritize using `get_kpi` for pre-validated KPI IDs like `count_by_category` and `total_records` whenever relevant to the analysis. Crucially, never fabricate any numerical data; every number presented must be directly backed by a tool call result, always explicitly citing the specific KPI ID or query that produced it.

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
