# Graph Data Extraction and Visualization Pipeline

## Table of Contents

1. [Overview](#overview)
2. [Steps](#steps)

   * [1. Extracting Nodes and Relationships](#1-extracting-nodes-and-relationships)
   * [2. Automating Cypher Query Generation](#2-automating-cypher-query-generation)
   * [3. Creating Graph in ArcadeDB](#3-creating-graph-in-arcadedb)
   * [4. Serving Data with FastAPI](#4-serving-data-with-fastapi)
   * [5. Visualizing Graph with Angular](#5-visualizing-graph-with-angular)

---

## Overview

This project demonstrates a complete pipeline for:

* Extracting entities and relationships from text/CSV data.
* Automating Cypher query generation with LLM.
* Creating and storing graphs in ArcadeDB.
* Serving the data via FastAPI.
* Visualizing the graph in an Angular application using `vis-graph`.

---

## Steps

### 1. Extracting Nodes and Relationships

**For text data:**

* Used **semantic chunking** on propositions extracted from text.
* Entities and relationships are extracted and provided to the LLM with a schema.

```python
# Example: Semantic chunking and entity extraction
chunks = semantic_chunking(propositions)
entities, relations = extract_entities_relations(chunks)

schema = {
    "nodes": ["Person", "Item", "InstagramAccount", "Location"],
    "relationships": ["PAST_PURCHASED", "FOLLOWS", "LOCATED_IN"]
}
```

**For CSV data:**

* Directly converted into JSON format by defining schema.

```python
import pandas as pd

df = pd.read_csv('data.csv')
json_data = df.to_dict(orient='records')
```

---

### 2. Automating Cypher Query Generation

* LLM is prompted with schema and relationships to generate Cypher queries.

```python
prompt = f"Generate Cypher queries for the following schema: {schema}"
queries = llm.generate(prompt)
```

---

### 3. Creating Graph in ArcadeDB

* Used ArcadeDB **HTTP API** to create vertices and edges.

```python
import requests

url = "http://localhost:2480/command/graphdb/sql"
headers = {"Authorization": "Basic ..."}

data = {
    "command": "CREATE VERTEX Person SET Name='John Doe'"
}

response = requests.post(url, headers=headers, json=data)
```

---

### 4. Serving Data with FastAPI

* Fetch graph data from ArcadeDB and serve via FastAPI.

```python
from fastapi import FastAPI
import requests

app = FastAPI()

@app.get("/graph")
def get_graph():
    response = requests.post(url, headers=headers, json={"command": "MATCH (n)-[r]->(m) RETURN n,r,m"})
    return response.json()

# Run with: uvicorn main:app --reload
```

---

### 5. Visualizing Graph with Angular

* Used **vis-graph** to render the graph in the frontend.

```typescript
const nodes = new DataSet(response.nodes.map(node => ({
  id: node['@rid'],
  label: node['Name'] || 'Node'
})));

const edges = new DataSet(response.edges.map(edge => ({
  from: edge.out,
  to: edge.in
})));

const container = document.getElementById('graph');
const data = { nodes, edges };
new Network(container, data, {});
```

---

**End of README**
