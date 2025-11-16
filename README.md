# Massive-JSON-Processing-33.99-GB-
Efficient processing of massive JSON (33.99 GB, 9.7M records)


This project demonstrates how to handle and process an extremely large JSON file (**33.99 GB** with **9,732,036 records**) using efficient chunk-based loading in Python, advanced cleaning techniques, and preparing the final dataset for **Google BigQuery**.

The source contains official FTTH coverage data for Spain, including complex fields such as:

- `geometry_rings`
- operator
- coverage status
- speed
- competitive / non-competitive area
- cadastral reference

---

## 🎯 Goals

- Process a huge JSON without running out of RAM.
- Extract only relevant fields to reduce memory usage.
- Normalize and clean key attributes.
- Convert Geometry rings into usable point coordinates.
- Export final dataset into BigQuery.
- Optimize performance and pipeline execution.

---

## 🧪 Technologies used

- Python 3  
- pandas  
- json / ujson  
- chunk-based readers  
- Google Cloud BigQuery  
- CSV / Parquet  
- Memory-optimized data pipelines  

---

## 🧱 Project structure

/src
└── 01_load_chunked.py
└── 02_clean_fields.py
└── 03_export_bigquery.py

/data
└── raw/
└── processed/

/docs
└── field_dictionary.md
└── pipeline_architecture.md

arduino
Copy code

---

## 🧰 Chunk-based loading example

```python
import json
import pandas as pd

def load_json_in_chunks(path, chunk_size=20000):
    with open(path, "r", encoding="utf-8") as f:
        buffer = []
        for line in f:
            buffer.append(json.loads(line))
            if len(buffer) >= chunk_size:
                yield pd.DataFrame(buffer)
                buffer = []

        if buffer:
            yield pd.DataFrame(buffer)
⚙️ Extracting coordinates from geometry_rings
python
Copy code
df['lon'] = df['geometry_rings'].apply(lambda x: x[0][0][0])
df['lat'] = df['geometry_rings'].apply(lambda x: x[0][0][1])
📤 Export to BigQuery
python
Copy code
df.to_gbq(
    destination_table="coverage.raw_json_processed",
    project_id="my_project",
    if_exists="append"
)
🗂 Final output
The processed dataset contains:

clean lat/lon fields

standardized operator names

consolidated coverage status

normalized speed

cadastral references ready for spatial joins

optimized format for analytical SQL queries








