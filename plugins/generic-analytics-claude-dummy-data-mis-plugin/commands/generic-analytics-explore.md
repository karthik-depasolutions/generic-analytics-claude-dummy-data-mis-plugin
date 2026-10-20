---
description: 'Generic Analytics Pack Database Explorer: analyze this database with
  verified tools and queries.'
allowed-tools: mcp__mis-mcp-claude-dummy-data__describe_data, mcp__mis-mcp-claude-dummy-data__list_business_concepts,
  mcp__mis-mcp-claude-dummy-data__describe_schema, mcp__mis-mcp-claude-dummy-data__get_data_profile, mcp__mis-mcp-claude-dummy-data__get_value_set,
  mcp__mis-mcp-claude-dummy-data__list_kpis, mcp__mis-mcp-claude-dummy-data__get_kpi, mcp__mis-mcp-claude-dummy-data__explain_metric,
  mcp__mis-mcp-claude-dummy-data__compare_kpi, mcp__mis-mcp-claude-dummy-data__rank_entities, mcp__mis-mcp-claude-dummy-data__breakdown_metric,
  mcp__mis-mcp-claude-dummy-data__query_metric, mcp__mis-mcp-claude-dummy-data__search_records, mcp__mis-mcp-claude-dummy-data__get_record,
  mcp__mis-mcp-claude-dummy-data__get_related_records, mcp__mis-mcp-claude-dummy-data__render_chart, mcp__mis-mcp-claude-dummy-data__run_safe_query
---

Run the Generic Analytics Pack Database Explorer command to explore Generic Analytics Pack business data.

1. Read `schema://model`, `schema://relationships`, and `schema://cookbook`.
2. Choose a relevant verified example for this database: How many total checkpoint blobs are stored?; List all unique communication channels present in checkpoint blobs.; Count the number of checkpoint blobs per channel, ordered by the most frequent.; Retrieve an example of a blob content for the 'messages' channel.; What is the latest database migration version for the checkpointing system?.
3. Retrieve its SQL from `schema://cookbook`, adapt only verified table and column names, then run it with `run_safe_query`.
4. Explain the result with its grain, caveats, and the query used.
