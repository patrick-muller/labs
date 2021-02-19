# Spark Lab 2 - Partitions 

1. Launch an EMR cluster with hive and spark applications ( skip this step if emr is already running )

2. Connect to master node of EMR cluster using ssh ( skip this step if already logged in)

3. Check if you have an access to below s3 location
    ```
    aws s3 ls s3://amazon-reviews-pds/parquet/
    ```
    ![ls_s3.png](resources/ls_s3.png)

4. Launch pyspark shell - by typing command pyspark on master node command line
    ![pyspark.png](resources/pyspark.png)

5. Create dataframes using spark structured API
    ```
    camera_df=spark.read.parquet("s3://amazon-reviews-pds/parquet/product_category=Camera")
    apparel_df=spark.read.parquet("s3://amazon-reviews-pds/parquet/product_category=Apparel")
    ```
    ![dataframe](resources/dataframe.png)

6. Check the number of partition
    ```
    apparel_df.rdd.getNumPartitions()
    apparel_df=camera_df.coalesce(1)
    apparel_df.rdd.getNumPartitions()
    ```
    ![partition1](resources/partition1.png)

    ```
    camera_df.rdd.getNumPartitions()
    camera_df=camera_df.coalesce(1)
    camera_df.rdd.getNumPartitions()
    ```

7. Join
    ```
    joinedDF = camera_df.join(apparel_df, 'customer_id', 'fullouter')
    ```

8. Check the query plan, try to read it from bottom up
    ```
    joinedDF.explain()
    ```
    ![explain](resources/explain.png)

9. Check the extended query plan, try to read different steps
    ```
    joinedDF.explain(extended='true')
    ```
    ![explain_extended](resources/explain_extended.png)

10. Trigger the DAG
      ```
      joinedDF.count()
      ```

11. Executor OOM
      ```
      cross_joinedDF = camera_df.crossJoin(camera_df)
      cross_joinedDF.explain()
      cross_joinedDF.count()
      ```

12. Driver OOM error
      ```
      camera_df.collect()
      ```

13. Spark UI

  * View all applications
    ![applications](resources/sparkui/applications.png)

  * View jobs
    ![jobs](resources/sparkui/jobs.png)

  * View stages
    ![stages](resources/sparkui/stages.png)

  * View tasks
    ![applications](resources/sparkui/applications.png)

  * View executors
    ![executors](resources/sparkui/executors.png)

  * View SQL queries on SQL interface
    > ![sql_queries1](resources/sparkui/spark-sql1.png)
    > ![sql_queries2](resources/sparkui/spark-sql2.png)

  * View DAG
    ![dag](resources/sparkui/dag.png)

  * View query plan
    ![sql_queries3](resources/sparkui/spark-sql3.png)
