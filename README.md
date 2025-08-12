# 🚀 Graph Data Extraction, Query Automation & Visualization Pipeline

A complete end-to-end pipeline for:
- Extracting **nodes** and **relationships** from various data sources
- Generating **Cypher queries** automatically using LLMs
- Building and storing graphs in **ArcadeDB**
- Serving the graph via a **FastAPI backend**
- Visualizing the graph in an **Angular frontend** using `vis-graph`

---

## 📑 Table of Contents
1. [Overview](#overview)
2. [1️⃣ Extracting Nodes & Relationships](#1️⃣-extracting-nodes--relationships)
    - [From Text Data](#from-text-data)
    - [From CSV Data](#from-csv-data)
3. [2️⃣ Automating Cypher Query Generation](#2️⃣-automating-cypher-query-generation)
4. [3️⃣ Creating Graph in ArcadeDB](#3️⃣-creating-graph-in-arcadedb-via-http-apis)
5. [4️⃣ Serving Data with FastAPI Backend](#4️⃣-serving-data-with-fastapi-backend)
6. [5️⃣ Angular Frontend Visualization](#5️⃣-angular-frontend-visualization)
7. [License](#-license)

---

## Overview
This project takes raw data (Text or CSV), extracts meaningful entities and relationships, converts them into a graph schema, stores them in **ArcadeDB**, and visualizes the resulting graph in an **Angular frontend**.

---

## **1️⃣ Extracting Nodes & Relationships**

### From Text Data
- **Step 1:** Performed **semantic chunking** on **propositions extracted from text**.
- **Step 2:** Extracted **entities** and **relations** using an LLM, guided by a predefined schema.

chunks = semantic_chunking(text)
llm.generate(f"Extract entities & relations from: {chunks}, using schema: {schema}")
### From CSV Data
Directly mapped columns to schema and converted to JSON.

df = pd.read_csv(file_path)
graph_json = df.to_dict(orient="records")
## **2️⃣ Automating Cypher Query Generation**

Used an LLM prompt to generate Cypher queries from extracted data.

cypher_queries = llm.generate(prompt)

## **3️⃣ Creating Graph in ArcadeDB via HTTP APIs**
Created vertices and edges using ArcadeDB's HTTP API.

requests.post(f"{ARCADEDB_URL}/api/v1/command/graphdb/sql", auth=AUTH, json={"command": query})
4️⃣ Serving Data with FastAPI Backend
Used FastAPI to retrieve graph data from ArcadeDB and serve it to the frontend.

python
Copy
Edit
@app.get("/graph")
def get_graph():
    query = "MATCH (n)-[r]->(m) RETURN n, r, m"
    return requests.post(
        f"{ARCADEDB_URL}/api/v1/command/graphdb/cypher",
        auth=AUTH,
        json={"command": query}
    ).json()
5️⃣ Angular Frontend Visualization
Fetched backend data and displayed it using vis-network.

typescript
Copy
Edit
const nodes = new DataSet(data.nodes);
const edges = new DataSet(data.edges);
new Network(container!, { nodes, edges }, {});
