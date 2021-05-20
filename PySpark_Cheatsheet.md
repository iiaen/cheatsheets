Boilerplate

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as func
from pyspark.sql.types import StructType, StructField, StringType, IntegerType, FloatType

spark = SparkSession.builder.appName("SomeName").getOrCreate()

.
.
.

spark.stop()
```

Reading from csv with headers
```python
df = spark.read.option("header", "true").option("inferSchema", "true")\
    .csv("file:///dir/filename.csv"
```


Reading from csv with no headers
```python
schema = StructType([ \
                     StructField("stationID", StringType(), True), \
                     StructField("date", IntegerType(), True), \
                     StructField("measure_type", StringType(), True), \
                     StructField("temperature", FloatType(), True)])

# // Read the file as dataframe
df = spark.read.schema(schema).csv("file:///dir/filename.csv")
```

Common df methods
```python
df.printSchema()

df.select("col1", "col2")

df.filter(df.colname != '')

df.groupby("age").agg(func.round(func.avg("income"), 2)
    .alias("avg_income")).sort("age").show()
    
df.withColumn("newColumn", func.round(func.col("min(oldColume)") * 3.14, 2)
    
df.show() # only shows top 20 rows
results_df.show(results_df.count())

```
