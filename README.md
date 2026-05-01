# PySpark-ETL-Pipeline
PySpark ETL pipeline — food delivery data cleaning, transformation &amp; Delta Lake loading
# ⚡ PySpark ETL Pipeline — Food Delivery Data

![PySpark](https://img.shields.io/badge/PySpark-3.x-orange?logo=apachespark&logoColor=white)
![Delta Lake](https://img.shields.io/badge/Delta_Lake-Enabled-blue)
![Status](https://img.shields.io/badge/Pipeline-Completed-brightgreen)
![Platform](https://img.shields.io/badge/Platform-Palantir_Foundry-black)

> A real-world ETL pipeline built with PySpark — Extract raw food delivery data, Transform with business logic, Load to Delta Lake (Gold layer).

---

## 📌 What This Pipeline Does

| Stage | What Happens |
|---|---|
| **Extract** | Load raw order data with intentional issues — nulls, inconsistent casing, cancelled orders |
| **Transform** | Clean nulls, standardise names, classify orders, filter by status |
| **Load** | Write clean Gold-layer data to Delta Lake with overwrite mode |

---

## 🛠️ Tech Stack

| Tool | Usage |
|---|---|
| PySpark (DataFrame API) | Data transformation, filtering, aggregation |
| PySpark SQL Functions | `F.when`, `F.col`, `F.initcap`, `F.isNotNull` |
| Delta Lake | Gold layer storage with ACID guarantees |
| Palantir Foundry | Execution environment |

---

## 🔍 Data Quality Issues Handled

```
Raw Data Problems          →   How Fixed
──────────────────────────────────────────────────
NULL amount values         →   Filtered out before processing
ALL-CAPS customer names    →   F.initcap() normalised to Title Case
Cancelled orders in data   →   Filtered: status == "Delivered" only
```

---

## 💻 Core Pipeline Code

```python
from pyspark.sql import functions as F

# EXTRACT
raw_df = spark.createDataFrame(raw_data, schema)

# TRANSFORM
cleaned_df = raw_df.filter(F.col("amount").isNotNull())
cleaned_df = cleaned_df.withColumn("cust_name", F.initcap(F.col("cust_name")))

transformed_df = cleaned_df.withColumn(
    "order_category",
    F.when(F.col("amount") >= 500, "Premium").otherwise("Regular")
)

final_df = transformed_df.filter(F.col("status") == "Delivered")

# LOAD
final_df.write.format("delta").mode("overwrite").save(gold_path)
```

---

## 📊 Pipeline Output

**Raw data (5 records):**
```
101 | vijay ahirwar | Pizza Hut  | 450  | Delivered
102 | amit          | Burger King| 1200 | Cancelled   ← removed
103 | neha          | Pizza Hut  | NULL | Delivered   ← removed
104 | rahul         | Subway     | 300  | Delivered
105 | VIJAY AHIRWAR | Dominos    | 850  | Delivered
```

**Gold layer (3 clean records):**
```
101 | Vijay Ahirwar | Pizza Hut | 450 | Delivered | Regular
104 | Rahul         | Subway    | 300 | Delivered | Regular
105 | Vijay Ahirwar | Dominos   | 850 | Delivered | Premium
```

---

## 📚 Learning Path

This project is part of a structured **26-day PySpark curriculum** completed as part of the Analyticorex Palantir Foundry Engineering Program:

- ✅ Spark Architecture & RDD Basics
- ✅ DataFrame Operations (Select, Filter, GroupBy, Joins)
- ✅ Performance Optimization (Caching, Broadcast, AQE)
- ✅ Window Functions & UDFs
- ✅ Delta Lake (Time Travel, Z-ordering)
- ✅ Real-World ETL Pipeline (this project)

---

## 👤 Author

**Vijay Ahirwar**  
B.S. Data Science & Applications — IIT Madras  
[LinkedIn](https://linkedin.com/in/vijay-ahirwar-ds45) | [GitHub](https://github.com/Vijay4532)
