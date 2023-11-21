## DBT Fundamentals

Who is an **Analytics Engineer**?
 1. Traditional Data Teams
 2. ETL vs ELT
 3. Analytics Engineer
 4. Data Build Tool



## 1. Traditional Data Teams

The *typically* have 2 roles.
**Data Analysts** - Work closely with the **business** decision makers in **finance**, **marketing** and other departments. They **query the data** to serve it up using dashboards or different types of reports.


>***Skill set***: Dashboards, Reporting, Excel, SQL

**Data Engineers** - In charge of **building** the infrastructure that the data is hosted on usually database and also for managing ETL process.

>***Skill set***: ETL, Orchestration, SQL, Python, Java etc

What is an **ETL process**?
ETL refers to the process of **Extracting Manipulating/Transforming & Loading** data.

## Data Warehouses
Data Warehouses combine a database and a supercomputer for transforming data.
Data can now be transformed directly in the database. There's no need to extract and load repeatedly.

This resulted in **ELT** processes i.e **Extract Load Transform**.
This means that the Data Team can focus on first **extracting** some data from a **source** and then **loading** it into their warehouse. The data can then be **transformed**.


![](https://github.com/projectfinalaudio/DBT_FUNDAMENTALS/blob/main/images/dbt-and-analytics-engineering.png?raw=true)

## 2. Modern Data Team

 1. Data Engineer
 2. Analytics Engineer
 3. Data Analysts


The emergence of **EL(Extract, Load)** tools has made the process of extracting data from various data sources and then load them into your data platform a lot easier.

## Data Flow(The Modern Data Stack):
* Data Sources -> Loaders -> Data Platforms(*DBT - Manages data transformations, runs tests*) -> Usages

 ![](https://github.com/projectfinalaudio/DBT_FUNDAMENTALS/blob/main/images/dbt_workflow.png?raw=true)

* Data transfers are handled using loaders(**EL** tools) or custom tooling written in different technologies.
* The data is then consumed using tools such as BI tools, for Machine Learning etc.
* DBT WORKS directly with the data platform to manage transformations, test them and also document them.

### Usages:
1. BI Tools
2. ML Model
3. Operational Analytics

* DBT makes it easier to develop **transformation pipelines** since it allows the user to write modular code in models as SQL select statements.

* You don't have to worry about the **DDL - Data Design Language** and **DML - Data Manipulation Language**


After you're satisfied with your **transformations**, you can deploy your DBT project on a schedule via the **DBT Cloud interface**, setup a job that can run *weekly, daily, hourly* and so on.

Then you'll have refreshed datasets on whatever cadence that you need.

## Review
### Traditional Data Teams
**Data engineers** are responsible for maintaining data infrastructure and the ETL process for creating tables and views.
**Data analysts** focus on querying tables and views to drive business insights for stakeholders.

### ETL and ELT
* ETL (extract transform load) is the process of creating new database objects by extracting data from multiple data sources, transforming it on a local or third party machine, and loading the transformed data into a data warehouse.
* ELT (extract load transform) is a more recent process of creating new database objects by first extracting and loading raw data into a data warehouse and then transforming that data directly in the warehouse.
* The new ELT process is made possible by the introduction of cloud-based data warehouse technologies.

### Analytics Engineering
Analytics engineers focus on the transformation of raw data into transformed data that is ready for analysis. This new role on the data team changes the responsibilities of data engineers and data analysts.

**Data engineers** can focus on larger data architecture and the EL in ELT.
**Data analysts** can focus on insight and dashboard work using the transformed data.

**Note**: At a small company, a data team of one may own all three of these roles and responsibilities. As your team grows, the lines between these roles will remain blurry.
dbt

* dbt empowers data teams to leverage software engineering principles for transforming data.


## Q/A Section
**Q.** What technological changes have contributed to the evolution of Extract-Transform-Load (ETL) to Extract-Load-Transform (ELT)?

**A.** 
- Modern cloud-based data warehouses with scalable storage and compute
- Off-the-shelf data pipeline/extraction tools (ex: Stitch, Fivetran)
- Self-service business intelligence tools increased the ability for stakeholders to access and analyze data

**Q.** dbt is part of which step of the Extract-Load-Transform (ELT) process?

**A.**
- Transform

**Q.** Which one of the following is true about dbt in the context of the modern data stack?

**A.**
dbt connects directly to your data platform to model data


**Q.**Which one of the following is true about YAML files in dbt?

YAML files are used for configuring generic tests


## Connecting DBT Cloud to Databricks
There are 2 ways to connect dbt Cloud to Databricks.
1. Patner Connect - Provides a streamlined setup to create your dbt Cloud account from within your Databricks account.
2. Creating a dbt Cloud account seperately and build the Databricks connection yourself(manually).

**NB** - **Patner Connect** is the easiest way to get set up.


## Initialize your dbt project​ and start developing
Now that you have a repository configured, you can **initialize** your project and start development in dbt Cloud:

Click **Start developing in the IDE**. It might take a few minutes for your project to spin up for the first time as it establishes your git connection, clones your repo, and tests the connection to the warehouse.
Above the file tree to the left, click Initialize dbt project. This builds out your folder structure with example models.
Make your initial commit by clicking **Commit and sync**. Use the commit message `initial commit` and click Commit. This creates the first commit to your managed repo and allows you to **open a branch where you can add new dbt code**.
You can now directly query data from your warehouse and execute `dbt run`. You can try this out now:
Click + **Create new file**, add this query to the new file, and click **Save as** to save the new file:

```sql
select * from default.jaffle_shop_customers
```

In the command line bar at the bottom, enter `dbt run` and click Enter. You should see a `dbt run succeeded` message.
