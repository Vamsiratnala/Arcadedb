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

* Directly converted into JSON format .

```python
import pandas as pd

df = pd.read_csv('data.csv')
data = df.to_dict(orient='records')
```

---

### 2. Automating Cypher Query Generation

* To generate cypher queries provide the schema and the json data to the llm to get cypher queries.
* you can use the function "get_cypher_queries_from_llm(data, llm)" available in the "create_graph.ipynb" file.

```python
query = get_cypher_queries_from_llm(your_data,your_llm)
```

---

### 3. Creating Graph in ArcadeDB

* Used ArcadeDB **HTTP API** to create vertices and edges.
* create a database and push the data into the database using "run_cypher" funtion.

```python
  create_database(db_name)
  run_cypher(query)
```
* Run all the queries to get the complete graph.
---

### 4. Serving Data with FastAPI

* Fetch graph data from ArcadeDB and serve via FastAPI.
* Install all the dependencies from 'myenv' in 'arcade-backend'
* Use the 'app function' in the main.py file from the 'arcade-backend' folder

```python
# Run with: uvicorn main:app --reload
```

---

### 5. Visualizing Graph with Angular

* Create an angular project with all the required dependencies from "arcade-frontend"
* Used **vis-graph** to render the graph in the frontend.
* Inside the Angular Application 
# Run with: ng serve 

---

**End of README**
