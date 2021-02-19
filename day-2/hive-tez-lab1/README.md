# [Hive using Tez vs MR](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/tez-using.html)
The following examples show you how to use Tez for the data and scripts used in the tutorial

Compare the Hive runtimes of MapReduce vs. Tez
1. Create a S3 bucket
  * Go to S3 Console
  ![go_to_s3](resources/go_to_s3.png)
  ![no_bucket](resources/no_bucket.png)
  ![create_bucket](resources/create_bucket.png)
  ![empty_s3](resources/empty_s3.png)
  ![bucket_created](resources/bucket_created.png)

2. Create a cluster as shown in the procedure called To create a cluster with Tez installed using the console. Choose Hive as an application in addition to Tez.
![emr-cluster](resources/emr-cluster.png)

3. Connect to the cluster master node 

4. Run the `Hive_CloudFront.q` script using MapReduce with the following command, where region is the region in which your cluster is located:
  > <pre>hive -f s3://eu-central-1.elasticmapreduce.samples/cloudfront/code/Hive_CloudFront.q \<br />-d INPUT=s3://eu-central-1.elasticmapreduce.samples \<br />-d OUTPUT=s3://<code style="color:red;"/>umesh-hive</code>/mr-test/ <i>-hiveconf hive.execution.engine=mr</i></pre>
  > <small> * change the highlighted to your name and append "-hive" e.g. umesh-hive</small>

  > The output should look something like the following:  
![MapReduce output](resources/hive-mr0.png)
<small>* Take note of the time taken to run the query 43.705 seconds when using the deprecated MapReduce engine </small>

5. Now run the job with the Tez execution engine using the following command:
  > <pre>hive -f s3://eu-central-1.elasticmapreduce.samples/cloudfront/code/Hive_CloudFront.q \<br />-d INPUT=s3://eu-central-1.elasticmapreduce.samples \<br />-d OUTPUT=s3://<code style="color:red;"/>umesh-hive</code>/tez-test/ <i>-hiveconf hive.execution.engine=tez</i></pre>
  > <small> * change the highlighted to your name and append "-hive" e.g. umesh-hive</small>

  > The output should look something like the following:
![Tez output](resources/hive-tez.png)
<small> * Take note of the time taken to run the query 31.273 seconds when using the recommended Tez engine, ~30% faster than MR </small>

# [Tez Web UI](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/tez-web-ui.html)

Tez has its own web user interface. To view the web UI, see:

 ![fig17](./resources/tez.png)


* Here's an output to see all Tez queries
![Tez query detailed output](resources/hive-tez1.png)

* Here's output to view details of a single query
![Tez query detailed output](resources/hive-tez2.png)

# [YARN Timeline Server](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/tez-timeline-server.html)

The YARN Timeline Service is configured to run when Tez is installed. To view jobs submitted through Tez or MapReduce execution engines using the timeline service, view the web UI using clicking in the "YARN timeline server"


 ![fig17](./resources/Yarn.png)


  * Screenshot of a view of all YARN applications.
  ![YARN all applications](resources/hive-mr1.png)  
    * Here we can see both the MapReduce and Tez queries we ran earlier


  * Screenshot of YARN details of a single application
    <p float="left" width="100%"><img src="resources/hive-mr2.png" width="45%" /><img src="resources/hive-mr3.png" width="45%" /></p>
     <ul><li>Here we can see the node in the cluster which the job ran on. </li><li>We can also get access to logs of the application.<sup> * Which are very important when troubleshooting issues regarding a job</sup> </li></ul>
