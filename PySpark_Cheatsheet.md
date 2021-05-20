Boilerplate starter code:

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

Reading from text files with headers:
```python
df = spark.read.option("sep", "\t").option("header", "true").option("inferSchema", "true")\
    .csv("file:///dir/filename.csv"
```


Reading from text files with no headers:
```python
schema = StructType([ \
                     StructField("stationID", StringType(), True), \
                     StructField("date", IntegerType(), True), \
                     StructField("measure_type", StringType(), True), \
                     StructField("temperature", FloatType(), True)])

# // Read the file as dataframe
df = spark.read.schema(schema).csv("file:///dir/filename.csv")
```

Common df methods:
```python
df.printSchema()

df.select("col1", "col2")

df.filter(df.colname != '')

df.groupby("age").agg(func.round(func.avg("income"), 2)
    .alias("avg_income")).sort("age").show()

df.orderBy(func.desc("col"))

df.withColumn("newColumn", func.round(func.col("min(oldColume)") * 3.14, 2)
    
df.show() # only shows top 20 rows
results_df.show(results_df.count())

```

Using UDFs:
```python
def exampleUDF(x):
    return x * 3.14

mySparkUDF = func.udf(exampleUDF)

df = df.withColumn("newColumn", mySparkUDF(func.col("someCol")))

```

Broadcast Variables:
```python
import codecs

def loadData():
    myDataDict = {}
    with codecs.open(filePath, "r", encoding='ISO-8859-1', errors='ignore') as f:
        for line in f:
            fields = line.split('|')
            myDataDict[int(fields[0])] = fields[1]
    return myDataDict

myDataDict = spark.sparkContext.broadcast(loadData()) # Broadcast var object

```


### Notes
Difference between `orderBy` and `sort`
> They are NOT the SAME.  
The SORT BY clause is used to return the result rows sorted within each partition in the user specified order. When there is more than one partition SORT BY may return result that is partially ordered.  
Reference :https://spark.apache.org/docs/latest/sql-ref-syntax-qry-select-sortby.html  
The ORDER BY clause is used to return the result rows in a sorted manner in the user specified order. Unlike the SORT BY clause, this clause guarantees a total order in the output.  
Reference : https://spark.apache.org/docs/latest/sql-ref-syntax-qry-select-orderby.html  
Source: https://stackoverflow.com/questions/40603202/what-is-the-difference-between-sort-and-orderby-functions-in-spark
