# iac-2-tier-example
An example of how to implement a 2-tier Infrastructure-as-Code.

## Design decisions

### Database

#### Why RDS as database type

I chose Amazon RDS given we want a managed database service for which we can use SQL to query with.

In a real-world scenario, I would opt to use MySQL on Amazon Aurora instead of MySQL on RDS. Aurora is also a managed database service like RDS, except that it is AWS cloud-optimised - AWS claims that MySQL on Aurora has 5x performance improvement over MySQL on RDS. It costs about 20% more than RDS for a given setup, but given that it is way more efficient, it should be more cost-effective in terms of overall spending.

I chose Amazon RDS instead of Aurora because only the former is in the free tier.

#### Why read replica

To avoid impacting the performance of the web application by the heavy database operations incurred by the daily report generation process, I added a read replica which will be solely used for report generation. The report generation will not access the master database instance which be solely used for the web application. This way, the heavy database operations incurred by the report generation will have minimal impact on the master database instance used by the web application, and hence the performance of the web application.

I made this architectural decision based on the assumption that the report generation process only requires read operations.