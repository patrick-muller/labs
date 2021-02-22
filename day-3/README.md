# Day 3 - Tech Challenge


## **1 - Pre-requisites:**
----
Launch a EMR cluster 5.30.1 with Hadoop, Spark, Hive, Tez and Ganglia  in eu-west-1 region.

Create a file with your answers and Upload the using the below link

https://aws-support-uploader.s3.amazonaws.com/uploader?account-id=900124452366&case-id=8022144761&expiration=1615191840&key=c88667cffbe17a94d34d6f03f962d32ae456e699e2a319349bf530db4ea76f68


## **2 - Content:**
----
You should share with us your answers in a Text File with the below Details

Name and Surname:
Mail:
Cluster ID: j-xxxxx

#### Practical:
***

> Challenge-1
> Answer:
> 
> Challenge-2
> Answer:

#### Questions:
___

> Challenge-1
> Answer:
> 
> Challenge-2
> Answer:


## **3 - File Format:**
----
The file shared needs to be compressed and the file name needs to be your Name, example:

> patrickmuller.txt.gz


## **4 - Data Set:**
----

Check the Dataset link
https://s3.amazonaws.com/amazon-reviews-pds/readme.html

The dataset is in a S3 bucket
s3://amazon-reviews-pds/tsv/

To download files locally you can copy 1 file 
>aws s3 cp s3://amazon-reviews-pds/tsv/amazon_reviews_us_Watches_v1_00.tsv.gz .

or multiple files
>aws s3 cp --recursive s3://amazon-reviews-pds/tsv/ .

The size of the data set is:
> aws s3 ls --summarize --recursive --human-readable s3://amazon-reviews-pds/tsv/
>
> Total Objects: 55
> Total Size: 32.2 GiB



## **5 - Practical:**
----

Explain your answers in as much detail as possible in your own words and share your code/commands


### HIVE

1.	Create an Hive table using the bellow command
> CREATE EXTERNAL TABLE amazon_reviews_parquet(
>   marketplace string, 
>   customer_id string, 
>   review_id string, 
>   product_id string, 
>   product_parent string, 
>   product_title string, 
>   star_rating int, 
>   helpful_votes int, 
>   total_votes int, 
>   vine string, 
>   verified_purchase string, 
>   review_headline string, 
>   review_body string, 
>   year int)
> PARTITIONED BY (product_category string)
> ROW FORMAT SERDE 
>   'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe' 
> STORED AS INPUTFORMAT 
>   'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat' 
> OUTPUTFORMAT 
>   'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
> LOCATION
>   's3://amazon-reviews-pds/parquet/'

2. Run the msck repair table to load the table partitions
>   msck repair table amazon_reviews_parquet

----

### Questions

a.  What is the product_title with more reviews ?

b.  Write a query to return only the 5th product_title with more reviews.

c.  What is the product_title with the best star_rating per year.

d.	The dataset has a folder with TSV data, try to create a table to read the TSV data and share with us the create table statement.
>   s3://amazon-reviews-pds/tsv/

### HDFS
Copy the Gdelt Dataset to your HDFS using the below command 
> s3-dist-cp --src s3://amazon-reviews-pds/parquet/product_category=Toys/ --dest /user/hadoop/

You can check the files created running the below command in a second terminal tab
> hadoop fs -du /user/hadoop/ | wc

----
1.	The s3-dist-cp creates a MAP Reduce Job, how many maps and how many reduces were created by the above command ?
Answer: 

2.	Increase the replication factor to 3 for all files in the path /user/hadoop/ , share with us the command used
Answer: 

3.	After you increased the replication factor, run the below command
> hdfs fsck /user/hadoop/

Why we have Under-replicated blocks ? and share with us how to solve the problem.
Answer:

4.	Is there a way to increase the replication factor to 3 ? How ?
Answer: 

5.  Change the replication factor to 1, share wit us the command:
Answer:

6.  Terminate 1 of the data nodes 
(On the EMR dashboard, select your EMR Cluster, on the Hardware Tab, click in the ID of the Core Group and select one instance and click on Terminate)

7. Run the below command 
> hdfs fsck /user/hadoop/

Why we have Mis-replicated blocks ?


## **6 - Questions:**
----

Explain your answers in as much detail as possible in your own words

1.	Which framework (Spark, Hive, MapReduce) in your opinion will provide a better performance to run an WordCount program reading the data from the GDEL Data set?
And Why?

2.	Which framework (Spark, Hive, MapReduce) in your opinion will provide a better performance to run a distinct count reading the data from the column actor2name ? And Why?

3.	The GDEL Dataset files are in CSV format, for which use case the CSV files are better than other formats like CSV, Parquet, Avro, JSON ?

4.	To copy the GDEL Dataset from S3 to HDFS, you can convert the data from CSV to Parquet, Avro, JSON. If you want to extract some information like number of events per category where the events are in Russia and United States, which file format will provide the best performance and why ?

5.  And in the copy, you can change the compression format to LZO, GZIP, Bzip2, Snappy, do you recommend to compress the data ? and why ?

