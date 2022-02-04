# Security Force Monitor: Senior Developer Code Challenge

Hello and welcome to Security Force Monitor's (SFM) code challenge for Senior Developers!

We're asking you to do a data wrangling task. You'll be creating a script that transforms SFM's tabular data about security force organizational structures into a graph that can be analysed and visualised.

## The challenge

The data provided in `data/202106251341-HEAD-Mali-Backup-slim.tsv` describes the security and defence forces of Mali. The data model is documented below.

Your task is as follows:

- Write a script that constructs a directed graph from this data, shows the full parentage and children of the unit called `32 Régiment d’Infanterie Motorisée` and outputs in GraphML format.

Your response to this challenge should:

- Be written in Python;
- Make use of the [NetworkX](https://networkx.org/) library; and,
- Include instructions for running your code and validating the output.

The attribute to use as a node label is:

 - `unit:name`

The attributes to use in the edge label are a composite of:

 - `unit:related_unit_first_cited_date`
 - `unit:related_unit_first_cited_date_start`
 - `unit:related_unit_last_cited_date`
 - `unit:related_unit_open`
 
The expected output of your script, in GraphML format, is provided in `output/324_cd_command_chain_raw.graphml`. 

To assist you, we have also laid out and formatted a version of the output in `examples/324_cdm_command_chain_formatted.graphml` and `examples/324_cdm_command_chain_formatted.pdf`. We laid out the graph using [yEd Graph Editor](https://www.yworks.com/products/yed).

## How to submit your response to us

To submit your work to us, please do the following:

 - [Mirror this repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/duplicating-a-repository#mirroring-a-repository) to a private repository on Github.
 - Adapt your private mirror of this respository to contain your response to the task.
 - When you have completed your response, tell your contact at SFM that you are ready to submit. They will let you know which Github accounts will need read access to the private repository.

Your response should be complete before granting us access. We will not accept commits made after this point.

## Data model

Security Force Monitor data is stored in a flat representation of a directed acyclic graph. It describes a set of hierarchical relationships between "units" - such as companies, battalions, brigades, divisions. 

### Fields

Position|Name|Description
---|---|---
1|`unit:id:admin`|Unit's unique identifier (UUID v4 format)|
2|`unit:name`|Name of the unit|
3|`unit:classification`|Security force branch(es) that unit is part of (semi-colon separated entries)|
4|`unit:relation_type`|Type of relationship between units ("child", "member")|
5|`unit:related_unit_id:admin`|Unique id of related unit (UUID v4 format)|
6|`unit:related_unit`|Name of the related unit|
7|`unit:related_unit_class`|Nature of authority in relationship ("command", "informal","administrative")|
8|`unit:related_unit_first_cited_date`|Full or partial date when the relationship was first observed, or strarted|
9|`unit:related_unit_first_cited_date_start`|Whether the date on which relationsip was first observed is an actual start date ("Y"), or just the earliest observation ("N")|
10|`unit:related_unit_last_cited_date`|Full or partial date when the relationship was most recently observed, or ended|
11|`unit:related_unit_open`|Whether the date the relationship between units is open-ended ("Y") or not ("N","E")|

We have published extended documentation on the [unit entity and fieldset](https://help.securityforcemonitor.org/en/latest/units.html) in our Research Handbook.

### Graph construction rules for SFM data

To transform the tabular data into a graph, consider the following rules:

- The complete universe of nodes is described the unique list of values in `unit:id:admin`.
- Each row describes an edge.
- Nodes may be linked with multiple edges simultaneously or over time.
- The source or parent node of a node is described in `unit:related_unit_id:admin`, and the target or child node is described in `unit:id:admin`.
- The duration over which the edge is valid is described with an earlier date (`unit:related_unit_first_cited_date`) and a later date (`unit:related_unit_last_cited_date`).

## Contents of this repository

```
.
├── README.md
├── data
│   └── 202106251341-HEAD-Mali-Backup-slim.tsv
├── examples
│   ├── 324_cdm_command_chain_formatted.graphml
│   └── 324_cdm_command_chain_formatted.pdf
└── output
    └── 324_cdm_command_chain_raw.graphml
```
