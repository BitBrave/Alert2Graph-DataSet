# Alert2Graph-DataSet

Privacy-preserving demo datasets for Alert2Graph-style IP-alert heterogeneous graph analysis.

This repository provides two anonymized SOC demo datasets. Each dataset is organized as a sequence of time-windowed graph snapshots. The graph schema is intentionally simple and suitable for graph construction, graph statistics, threat propagation analysis, visualization, and security analytics pipelines.

## Repository Structure

```text
.
├── README.md
├── SOC1_Demo
│   ├── anonymization_summary.json
│   └── 4_ip_alert_node_hetero_enhanced_features
│       └── sp_*_enhanced.json
└── SOC2_Demo
    ├── anonymization_summary.json
    └── 4_ip_alert_node_hetero_enhanced_features
        └── sp_*_enhanced.json
```

## Graph Schema

Each snapshot is stored as a JSON file and follows a heterogeneous graph format:

- Node types: `ip`, `alert`
- Edge types:
  - `('ip', 'fires_alert', 'alert')`
  - `('alert', 'targets_ip', 'ip')`
- Main fields:
  - `node_types`: available node categories
  - `edge_types`: available relation categories
  - `nodes`: node IDs grouped by node type
  - `edges`: typed edge lists
  - Additional feature or metadata fields are preserved when present

Example usage:

```python
import json
from pathlib import Path

snapshot = Path(
    "SOC1_Demo/4_ip_alert_node_hetero_enhanced_features/"
    "sp_0_2024-10-09 00:00:00_2024-10-10 00:00:00_enhanced.json"
)

with snapshot.open("r", encoding="utf-8") as f:
    graph = json.load(f)

ip_nodes = graph["nodes"]["ip"]
alert_nodes = graph["nodes"]["alert"]
ip_to_alert_edges = graph["edges"]["('ip', 'fires_alert', 'alert')"]

print(len(ip_nodes), len(alert_nodes), len(ip_to_alert_edges))
```

## Dataset Summary

| Dataset | Snapshot JSON Files | Anonymized IPv4 Count | Private IPv4 Prefix |
| --- | ---: | ---: | --- |
| SOC1_Demo | 120 | 3865 | `10.1.0.0/16` |
| SOC2_Demo | 141 | 39128 | `10.2.0.0/16` |

## Anonymization

All IPv4 addresses in JSON keys and string values have been replaced with private IPv4 addresses.

- SOC1 demo addresses use the `10.1.0.0/16` private range.
- SOC2 demo addresses use the `10.2.0.0/16` private range.
- Address replacement is stable within each demo dataset, so repeated occurrences of the same address remain consistent across snapshots.
- The address mapping table is not included.
- Non-address graph structure, alert labels, edge types, snapshot file names, numeric features, and non-address metadata are preserved as much as possible.

## Intended Use

These demo datasets are intended for:

- Reproducing Alert2Graph-style data loading and graph construction workflows
- Testing heterogeneous graph processing code
- Running graph statistics and visualization examples
- Developing and validating security analytics pipelines on anonymized data

## Notes

- This repository contains demo data only.
- The datasets are anonymized and do not include raw address mappings.
- If you use this dataset in a publication or project, please cite or link to this repository.
