# NCBA Data Engineering Casestudy by Joy Phoebe
<img src="images/logo.png" alt="Project Logo" width="120" height="120">

## 📗 Table of Contents

1. [👩🏾‍💻Case Sstudy 1](#1)
  - [🛠️ 1.1](#1.1)
  - [🛠️ 1.2](#1.2)
  - [🛠️ 1.3](#1.3)
  - [🛠️ 1.4](#1.4)
  - [🛠️ 1.5](#1.5)
  - [🛠️ 1.6](#1.6)
  - [🛠️ 1.7](#1.7)
2. [👩🏾‍💻 Case Sstudy 2](#q2)
  - [🛠️ 2.1](#2.1)
  - [🛠️ 2.2](#2.2)
  - [🛠️ 2.3](#2.3)
  - [🛠️ 2.4](#2.4)
  - [🛠️ 2.5](#2.5)

## Case Study 1 <a name="1"></a>

#### The LoopDFS business is experiencing rapid growth across various countries, and you are now part of the MSL (Mobile Savings and Loans) Value Stream. This team, comprising diverse experts, will  collaborate intensively to increase agility and responsiveness. Among your key tasks will be designing and developing a robust database and creating ETL processes for core data points, including Customer, Loans, savings, and Transactions data.

1.1. **Process for Designing and Architecting a Data Warehouse Tailored to MSL’s needs (10 marks)** <a name="1.1"></a>

My design process would be as follows:
- I would begin by checking the **nature/type of the data** since this determines the type of warehouse I would come up with. In this case, the data is highly schematized as per the information provided which shows that we have Customer, Loans, Savings, and Transactions data.
- I would proceed by ensuring that the Data Warehouse I am designing and Architecting for LoopDFS **follows the rules and regulations of the country** in which it is being rolled out. In this case, I will research the financial regulations required for Zambia and stay compliant with them. 
- I would choose a **Relational Database Management System (RDMS)** for the storage and management of the schematized data. 
- I will design a **star schema data warehouse.** Here, transactional data will go to the Fact tables to handle data growing exponentially e.g. loans, savings, and transactional data. 
For data that rarely changes, I would use Dimensions tables e.g. names of people, and bank branches. Dimensions tables and Fact tables would be connected based on ID’s.
- I would enforce **scalability** scalability across multiple regions as MSL expands for multi-tenant architecture


1.2. **Key Steps & Considerations in Building the Data Warehouse (10 marks)** <a name="1.2"></a>

- **Landing Schema:** I will create a landing schema to receive the data from external systems
- **Data Transformation:** I will Transform data into Dimensional tables and Fact Tables. Here, transactional data will go to the Fact tables to handle data growing exponentially e.g. loans, savings, and transactional data. For data that rarely changes, I would use Dimensions tables e.g. names of people, and bank branches.
- **Reporting tables:** I will make aggregate tables that will act as reporting tables and work with Power BI among other similar tools. 
- **Ensure Data Lifecycle Management:** In this step, I will retain data using two methods:
  - *Short term:*  Source tables will be stored for up to 6 months only since they carry heavy data. 
  - *Long Term:*  Fact tables will be stored for up to the number of years that are necessary, as per Zambian legal and business requirements. 
- **Testing & Validation** – Cross-verification of information to ensure data accuracy such as loan disbursement accuracy for Zambian credit scoring models.

1.3. **Approach to ETL Process (10 marks)** <a name="1.3"></a>
My approach would be as follows:
- **ETL Tool:** Pick/specify a tool to use. In my case, I would use Oracle Data Integrator (ODI) due to its high-performance data integration and warehousing where transformations are performed within the target database
- **Extract Process:** If the primary system generates files, my process would be to extract files from the Mobile Savings and Loans(MSL) system to a File Transfer Protocol(FTP) location and then process them to my landing schema.
- **Transfer Process:** The data would then be transferred to fact tables and dimensional tables. Here, transactional data will go to the Fact tables to handle data growing exponentially e.g. loans, savings, and transactional data. For data that rarely changes, I would use Dimensions tables e.g. names of people, and bank branches.
- **Load Process:** This is the final process of the ETL process where I would load data by moving them into aggregates. 
- **Enforcing Rules/Conditions:** In the above process, I would enforce a condition to be followed such that the proceeding process does not start until the predecessor process is complete. For instance, having the Transfer Process finish before the Load process starts. 

```css
[Extract- From MSL system to FTP] ----> [Transfer- to Fact & Dimension tables ] ----> [Load- Move data to aggregates]
```

1.4. **Managing ETL Scheduling for Daily Refreshes (5 marks)** <a name="1.4"></a>
- **Create a workflow:** I ensure my ETL processes are run past midnight. This is to avoid processing during business hours when the Mobile Savings and Loans(MSL) databases are actively used for transactions and customer interactions. I would also have this process done by 4 am to ensure dashboards refresh by morning. 
- **Automatic schedule:** I would ensure the predecessor ETL process runs before the preceding one, for instance,  having the Transfer Process finish before the Load process starts. 
- **ETL Processing Time:** I ensure my ETL processes are run past midnight. This is to avoid processing during business hours when the Mobile Savings and Loans(MSL) databases are actively used for transactions and customer interactions. I would also have this process done by 4 am to ensure dashboards refresh by morning. Snapshots would also be taken a few minutes after midnight when business and customer activities are limited to track and audit data.
- **Incremental Loading:** To avoid redundant processing, incremental loading would update only new transactions.
- **Retry Mechanism:** This would rerun failed tasks using Apache Airflow retry policies.

1.5. **Servicing Analytics Needs Across Teams (5 marks)** <a name="1.5"></a>
- **Create data maps for each business unit:** I would start by mapping out data flows and sources for each business unit. This ensures that every team knows where their data comes from and how it connects with other departments. By documenting this in a data map, I can make it easier for analysts and stakeholders to access the right data without confusion.
- **Solve at aggregate layer:** Instead of having each team work with raw, granular data, I would focus on creating aggregated datasets that provide meaningful insights without unnecessary complexity. This approach improves query performance, reduces redundancy, and ensures consistency across reports and dashboards.
- **Ensure everything has its data mart:** To support self-service analytics, I would ensure that each business unit has a dedicated data mart tailored to its needs. This would allow teams to quickly access relevant, pre-processed data without overloading the main data warehouse. Properly structured data marts would enhance efficiency and enable data-driven decision-making across the organization.
- **Customer Retention: Analyze failed loan applications and SMS outreach response rates.
- **CX Team:** Use NLP sentiment analysis on app reviews to improve service.

1.6. **Monitoring ETL Pipelines (5 marks)** <a name="1.6"></a>
- **Implement Alerts via SMS or Email:** I would set up automated alerts that notify me via SMS or email whenever an ETL job fails or encounters an issue. This would help me respond quickly to problems without constantly monitoring dashboards. Overall, this can avoid downtime and data inconsistencies.
- **Schedule Tasks for Automation:** To avoid manual work, I would schedule tasks using a job scheduler like Apache Airflow or AWS Step Functions. This would help maintain consistency in data processing and ensure that critical tasks run smoothly and on time, even if there are temporary failures.
- **Dashboards to Track ETL Job Failures:** I would set up Prometheus & Grafana dashboards to have real-time visibility into ETL performance, This would help identify any anomalies or job failures. Log Anomalies Using AWS CloudWatch Alerts (e.g., Missing Transactions)
- **Implement Data Drift Detection for Unexpected Changes:** Data quality can degrade over time due to shifting patterns, schema changes, or unexpected trends. For example, if I notice that loan approval rates drop significantly in a short period, I would flag the issue for investigation to determine whether it's due to business changes or a potential data pipeline issue. Implementing data drift detection ensures that insights remain reliable and helps catch problems before they affect decision-making.

1.7.**Controls for Data Integrity (5 marks)** <a name="1.7"></a>
- **Table Partitioning (e.g., Fact Table by Dates) for Faster Searches and Aggregation:** I would partition large tables by date or key attributes to improve query performance and scalability. This ensures that searches, aggregations, and reporting run efficiently without scanning unnecessary data.
- **Delete Corrupt Tables and Reload from a Trusted Source:** If a table contains corrupt or incomplete data, I would delete it and reload it from a validated source rather than trying to patch errors manually. This prevents data inconsistencies from spreading and ensures reports are built on reliable data.
- **Count the Number of Records to Validate Completeness:** Before considering a data load successful, I would compare the expected vs. actual record count to detect missing data. If the count is lower than expected, I will investigate and restart the job to ensure completeness.
Filter Duplicate Data to Avoid Over-Reporting and Maintain Accuracy
- **I would set up deduplication rules and unique constraints** to prevent duplicate records from affecting reports and insights. This ensures that metrics remain accurate and trustworthy for decision-making.
Ensure ETL Jobs Run on Time to Maintain Data Freshness: Scheduled jobs must execute on time to prevent delays in reporting and analytics. - I would use monitoring tools and automated alerts to detect failures early and ensure data is always up to date.


## Case Study 2 <a name="q2"></a> 

#### The data within LoopDFS is expanding rapidly, and the business demand for analytics and reporting is on the rise. Each team’s main responsibility is to make data-driven decisions that enhance customer engagement, segmentation, acquisition, retention, and customer experience. The Data Engineering Team’s primary objective is to advocate for a Big Data Platform capable of accommodating these large data volumes and extracting actionable insights.

2.1. **Key Factors for Evaluating a Suitable Platform (10 marks)** <a name="2.1"></a>
- **Data Sources and Data Types:** I would first identify what types of data the platform needs to handle, such as structured (databases), semi-structured (JSON, XML), or unstructured (videos, logs, social media posts). A good platform should support multiple data formats to ensure smooth integration across different business units. If a platform is limited in the types of data it can process, it could create compatibility issues in the long run. Choosing a system that can ingest, transform, and store diverse data types ensures flexibility and scalability.
- **Determine the Platform Based on Data Sources (Log Files, Social Media, Cloud, etc.):** Different platforms specialize in handling different data sources, so I would ensure the one I choose aligns with my needs. If I am working with log files and machine-generated data, I might look at platforms like Splunk or ELK Stack; if dealing with social media and cloud sources, something like Google BigQuery or Snowflake would be a better fit. The platform should be able to pull data from multiple sources without complex workarounds. This helps streamline data ingestion and makes analysis more efficient.
- **Cost Considerations:** I would evaluate both upfront and ongoing costs, including licensing, infrastructure, and maintenance expenses. Some platforms have a pay-as-you-go model, which is great for startups, while others have fixed enterprise pricing, which may be more cost-effective in the long run. I’d also consider the cost of storage and processing power, ensuring that the platform scales efficiently without unnecessary expenses. A well-balanced cost model ensures that the platform remains affordable while meeting performance requirements.
- **Expandability and Scalability:** The platform should be able to grow with business needs, handling increasing data volumes without performance bottlenecks. I would check whether it supports horizontal scaling (adding more servers) or vertical scaling (adding more power to existing servers). A scalable platform prevents frequent migrations to new systems as data grows, reducing operational disruptions. Future-proofing the platform ensures it remains efficient and cost-effective over time.
- **Ease of Use:** A good platform should have an intuitive interface that allows both technical and non-technical users to navigate and extract insights easily. I would prioritize platforms with drag-and-drop tools, user-friendly dashboards, and automation features to simplify workflows. If a platform is too complex, it could slow down adoption and require extensive training. Ensuring a balance between powerful features and usability makes the platform more effective in the long term.
- **Support for Open Source and Customer Support:** I would check if the platform supports open-source tools like Apache Kafka, Spark, or Kubernetes, which can offer more customization and cost savings. Additionally, having strong vendor support or a community of developers is crucial in case of technical issues. A platform with active documentation, forums, and 24/7 customer support ensures faster troubleshooting and smoother operations. The combination of open-source flexibility and solid customer support makes the platform more reliable and adaptable.

2.2. **Approach to Integrating Data from Multiple Sources (10 marks)** <a name="2.2"></a> 
- **Data Lake:** I would use a data lake to store and process raw data from multiple sources, including social media files, logs, and IoT data. This allows for scalability and flexibility, ensuring that both structured and unstructured data can be analyzed efficiently.
- **Spark:**  For real-time and batch processing, I would leverage Apache Spark, which provides a powerful distributed computing framework. It ensures high-speed data transformations and analytics, making it ideal for handling large-scale datasets across multiple nodes.
- **YARN Apache:** Since managing cluster resources is crucial for scalability, I would use YARN (Yet Another Resource Negotiator) to allocate computing power efficiently. This helps in optimizing performance by dynamically distributing workloads across the cluster.
- **Open Source with Robust Community Support**: I would prefer open-source solutions because they offer greater flexibility, cost savings, and continuous improvements. A strong community-driven ecosystem ensures access to plugins, troubleshooting support, and frequent updates.
- **Centralized Lakehouse (Delta Lake)**: I would integrate all data into a centralized lakehouse like Delta Lake (Databricks) to combine the benefits of data lakes and warehouses. This approach maintains data consistency, supports versioning, and ensures analytics-ready datasets for real-time insights.

2.3. **Roadmap for Migrating Data from Legacy Systems (10 marks)** <a name="2.1"></a> 
- **Phase 1: Assessment**
Before starting the migration, I would map legacy schemas from on-prem PostgreSQL to cloud-based Redshift, ensuring that all data structures are well understood. At this stage, I would also classify data into hot (3 months), warm (2 years), and cold (up to 7 years) to determine the best storage strategy for performance and cost efficiency.
- **Phase 2: ETL Refactoring**
To modernize data processing, I would convert outdated cron jobs to Apache Airflow DAGs, which provide better scheduling, monitoring, and error handling. This step also includes optimizing system performance by ensuring that heavy ETL jobs run at night, reports are generated at the aggregate layer, and computationally expensive calculations are performed only once to avoid redundant processing.
- **Phase 3: Parallel Run & Validation**
To ensure a smooth transition, I would run the new cloud-based platform in parallel with the legacy system for 3 months, validating data accuracy and comparing performance. During this phase, I would also start leveraging the new platform for data-driven decisions by setting up real-time processing, BI tools, and risk-scoring models to predict loan defaults, detect fraud, and personalize marketing strategies using machine learning.
- **Phase 4: Full Cutover**
Once the new system proves stable, I would freeze the legacy system, migrate the final delta, and switch all queries to the new infrastructure. This transition would be backed by auto-scaling with Kubernetes to handle high-traffic periods, a disaster recovery plan using AWS multi-region failover, and query optimizations with materialized views for faster report generation.
- **Phase 5: Evaluating the Success of the New Platform**
To measure the success of the migration, I would track key metrics such as query speed (aiming for a 10x improvement), cost reduction (at least 30% lower storage and compute costs), and user adoption metrics (e.g., credit risk reports generating in seconds instead of hours). Additionally, I would monitor system uptime, failure rates, and running costs to ensure the new platform is more efficient than the legacy system while supporting diverse data types across hot, warm, and cold storage tiers.

2.4. **Strategy for optimizing system performance and ensuring business continuity on the new platform.(5 marks)** <a name="2.4"></a> 
- **Data maps:** I would create detailed data maps to ensure that every business unit knows where their data is stored and how it flows across systems. This improves transparency, making it easier to track, manage, and troubleshoot data issues.

- **Real-Time Processing:** By enabling real-time data processing, I would ensure that critical business decisions are based on the most up-to-date information. This allows for instant insights, such as detecting fraudulent transactions or responding to market trends quickly.

- **BI Tools:** I would integrate Business Intelligence (BI) tools like Tableau or Power BI to make data easily accessible and visualized for business users. With intuitive dashboards, teams can analyze trends and make informed decisions without relying on engineers for every report.

- **Data Quality Monitoring:** I would implement automated data quality checks to detect anomalies, inconsistencies, or missing records before they impact decision-making. This ensures that reports and analytics are based on clean, reliable data.

- **Data Governance & Security:** To protect sensitive data, I would enforce role-based access controls and encryption to ensure compliance with industry regulations. Proper governance would help prevent unauthorized access while maintaining data integrity and auditability.

2.5. **Optimizing System Performance & Ensuring Business Continuity (5 marks)** <a name="2.5"></a>  
- **Every heavy job runs at night:** I would schedule resource-intensive jobs to run during off-peak hours to prevent system slowdowns and ensure a smooth user experience during the day. This helps distribute computing loads efficiently and reduces operational costs by taking advantage of lower-demand periods.
- **Reporting at the aggregate layer:** Instead of querying raw data directly, I would use pre-aggregated datasets to generate reports faster and reduce database strain. This improves performance significantly while ensuring that dashboards and analytics tools can fetch insights in real-time.
- **Calculations are done once:** To avoid redundant processing, I would compute complex metrics once and store the results instead of recalculating them for every query. This minimizes processing time, reduces computational costs, and ensures data consistency across reports.
- **Disaster Recovery (AWS S3 Cross-Replication):** I would set up multi-region failover using AWS S3 cross-replication to ensure data availability in case of system failures or outages. This guarantees that business operations continue uninterrupted, even if a primary data center goes down.
- **Query Optimization (Materialized Views):** To speed up frequently accessed reports, I would create materialized views that store precomputed query results, reducing the need for expensive joins and calculations. This significantly enhances performance and improves response times for users who need quick access to insights
2.6. **Evaluating Success of New Platform vs. Legacy (5 marks)** <a name="2.6"></a>  
 
- **Reports of time:** I would measure how long reports take to generate, ensuring that the new platform delivers insights faster and more efficiently than before. If report generation time is significantly reduced, it indicates better system performance and responsiveness.
- **Few failures:**  A key indicator of success is system stability, so I would track failure rates and compare them to the old system. Fewer job failures mean improved reliability, ensuring that data processing runs smoothly with minimal disruptions.
- **Running cost less than the old one:**  The new platform should reduce overall costs, including storage, computing, and maintenance expenses. I would compare monthly operational costs to confirm that we are achieving at least a 30% reduction in expenses.
- **Various data types:** The ability to handle structured, semi-structured, and unstructured data efficiently is crucial for scalability. I would test different formats like JSON, CSV, and Parquet to ensure seamless integration and analysis.
- **Query Speed Comparison:** I would benchmark query execution times, aiming for at least a 10x speed improvement over the legacy system. Faster queries mean quicker decision-making and a better user experience for analysts and business teams

