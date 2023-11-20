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


