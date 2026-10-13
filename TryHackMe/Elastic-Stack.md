# Elastic Stack (ELK) for SOC Analysts: Summary

Study notes on the Elastic Stack and how a SOC analyst uses it for log analysis and investigation.

## Overview

The Elastic Stack (ELK) is a set of open-source components that collect data from many sources, store and search it, and visualize it in near real time. It was first built for application monitoring and large-scale search. It has since become popular in security operations, and many SOC teams use it almost like a SIEM.

> As a SOC analyst, you mainly use ELK for log analysis and investigations. You don't need to master the internals of each component, but you should understand the basics.

## Core Components

| Component | Role |
|---|---|
| **Elasticsearch** | Full-text search and analytics engine for JSON documents. It stores, analyzes and correlates data, and exposes a RESTful API. |
| **Logstash** | Data processing pipeline that ingests data from multiple sources, filters/normalizes it, and sends it to a destination (Elasticsearch, Kibana, a port or a file). |
| **Beats** | Lightweight, single-purpose agents ("data shippers") installed on endpoints that send specific data to Elasticsearch or Logstash. |
| **Kibana** | Web interface for searching, investigating and visualizing data stored in Elasticsearch. |

### Logstash configuration

A Logstash config has three sections:

1. **Input**: where the data comes from.
2. **Filter**: how the data is parsed and normalized into field/value pairs.
3. **Output**: where the processed data goes.

### Data flow

```
Endpoints -> Beats -> Logstash -> Elasticsearch -> Kibana
(Winlogbeat: Windows event logs, Packetbeat: network flows)
```

## Kibana: Discover Tab

Discover is where analysts spend most of their investigation time. Its main elements:

- **Logs**: each row is one event. Expanding it shows all parsed fields and values.
- **Fields pane**: lists normalized fields. Clicking a field shows its top 5 values and their percentages. `+` filters *for* a value and `-` filters it *out*.
- **Index pattern**: tells Kibana which Elasticsearch data to explore. Each log source (e.g. VPN logs) has its own pattern, and one pattern can cover several indices.
- **Search bar**: for KQL queries and filters.
- **Time filter**: restricts results to a time range (relative or custom).
- **Timeline**: event counts over time. It is useful for spotting spikes, and clicking a bar narrows results to that period.
- **Add filter**: builds structured filters without typing a query.
- **Top bar**: save, open and share searches.
- **Create table**: choose specific fields to display. This reduces noise, and the layout can be saved and reused.

## Kibana Query Language (KQL)

KQL searches logs in two ways.

### Free-text search

- Searching a term returns every document containing it, in any field.
- KQL matches **whole words**, so `United` will not match `United States`.
- Use the wildcard `*` for partial matches, e.g. `United*`.

### Logical operators

```
"United States" AND "Virginia"
"United States" OR "England"
"United States" AND NOT ("Florida")
```

### Field-based search

Syntax: `field : value`

```
Source_ip : 238.163.231.224 AND UserName : Suleman
```

Kibana suggests available fields as you type.

### Time-based filtering

```
UserName : "Johny Brown" AND @timestamp > "2022-01-01T00:00:00.000Z"
```

## Kibana: Visualizations

- Create charts (tables, pie charts, bar charts, etc.) from the Visualization tab, or by clicking a field in Discover.
- Drag fields into the middle of the canvas to build **correlations** between fields, e.g. `Source_IP` against `Source_Country`.
- **Save** each visualization with a descriptive name so it can be reused in dashboards.

## Key Takeaways

- Beats collect, Logstash normalizes, Elasticsearch stores, and Kibana visualizes.
- Normalized fields make cross-source searching and filtering practical.
- The timeline is a quick way to spot anomalies such as sudden spikes.
- KQL (free text, operators, `field : value`) is the core skill for investigations.
- Saved tables and visualizations speed up repeated analysis.

## References

- Based on the TryHackMe *Elastic Stack: The Basics* room.
