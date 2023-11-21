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

## Build your first model[​](https://docs.getdbt.com/guides/databricks?step=7#build-your-first-model "Direct link to Build your first model")

1.  Under  **Version Control**  on the left, click  **Create branch**. You can name it  `add-customers-model`. You need to create a new branch since the main branch is set to read-only mode.
2.  Click the  **...**  next to the  `models`  directory, then select  **Create file**.
3.  Name the file  `customers.sql`, then click  **Create**.
4.  Copy the following query into the file and click  **Save**.

```sql
with customers as (

    select
        id as customer_id,
        first_name,
        last_name

    from jaffle_shop_customers

),

orders as (

    select
        id as order_id,
        user_id as customer_id,
        order_date,
        status

    from jaffle_shop_orders

),

customer_orders as (

    select
        customer_id,

        min(order_date) as first_order_date,
        max(order_date) as most_recent_order_date,
        count(order_id) as number_of_orders

    from orders

    group by 1

),

final as (

    select
        customers.customer_id,
        customers.first_name,
        customers.last_name,
        customer_orders.first_order_date,
        customer_orders.most_recent_order_date,
        coalesce(customer_orders.number_of_orders, 0) as number_of_orders

    from customers

    left join customer_orders using (customer_id)

)

select * from final
```

6. Enter  `dbt run`  in the command prompt at the bottom of the screen. You should get a successful run and see the three models.
Later, you can connect your business intelligence (BI) tools to these views and tables so they only read cleaned up data rather than raw data in your BI tool.


## Change the way your model is materialized
One of the most powerful features of dbt is that you can change the way a model is materialized in your warehouse, simply by changing a configuration value. You can change things between tables and views by changing a keyword rather than writing the data definition language (DDL) to do this behind the scenes.
                                                                                                                                       `
By **default**, everything gets created as a **view**. You can **override** that at the directory level so everything in that directory will materialize to a different materialization.

1.  Edit your  `dbt_project.yml`  file.
    - Update your project  `name`  to:    
        dbt_project.yml
        
    ```css
        name: 'jaffle_shop'
    ```
        
   -   Configure  `jaffle_shop` so everything in it will be materialized as a table; and configure  `example`  so everything in it will be materialized as a view. Update your `models`  config block to:
        **dbt_project.yml**
        
    ```css
        models:  jaffle_shop:    +materialized: table    example:      +materialized: view
    ```    
    -   Click  **Save**.
        
2.  Enter the  `dbt run`  command. Your  `customers`  model should now be built as a table!
    **INFO**
    
    To do this, dbt had to first run a  `drop view`  statement (or API call on BigQuery), then a  `create table as`  statement.
    
3.  Edit  `models/customers.sql`  to override the  `dbt_project.yml`  for the  `customers`  model only by adding the following snippet to the top, and click  **Save**:
    **models/customers.sql**
    
    ```sql
	   {{ config( materialized='view' )}}with customers as ( select id as customer_id...)
    ```
4.  Enter the  `dbt run`  command. Your model,  `customers`  should now build as a view.
5.  Enter the  `dbt run --full-refresh`  command for this to take effect in your warehouse.

## Summary(Common commands)
`dbt run`
`dbt run --full-refresh`

* In dbt, **tables** store the data itself as opposed to views which store the query logic. 
Currently, dbt supports four types of materializations: table, view, incremental, and ephemeral. 

* The table and incremental materializations persist a table, the view materialization creates a view, and the ephemeral materialization returns results directly using a **common table expression** (CTE). 

* If you use dbt, there's little need for **materialized views** since a **materialized view is a table based on a query**, and **you can materialize a dbt model** as a table to get the same result.

**Note** - Optionally set a resource to always or never full-refresh. 1. If specified as true or false, thefull_refresh config will take precedence over the presence or absence of the `--full-refreshflag`. 2. If the full_refresh config is none or omitted, the resource will use the value of the `--full-refreshflag`.

`dbt run full refresh` is a command that treats **incremental** models as **table models**. 
This means that dbt will **drop** and **recreate** the models **instead of appending new data to them**. 

* This is useful when the **schema or the logic of the incremental models changes** and you need to **reprocess the entire data**. The `--full-refresh` flag affects the `is_incremental()` macro, which will return `false` for **all models when the flag is specified**.


## Delete the example models

You can now delete the files that dbt created when you initialized the project:

1.  Delete the  `models/example/`  directory.
    
2.  Delete the  `example:`  key from your  `dbt_project.yml`  file, and any configurations that are listed under it.
    
    dbt_project.yml
    
    ```css
    # beforemodels:  
        jaffle_shop:    
            +materialized: table    
        example:      
            +materialized: view
    ```
    
    dbt_project.yml
    
    ```css
    # aftermodels:  
        jaffle_shop:    
            +materialized: table
    ```
    
3.  Save your changes.



## Build models on top of other models

As a best practice in SQL, you should **separate logic** that cleans up your data from logic that transforms your data. You have already started doing this in the existing query by using common table expressions (CTEs).

Now you can experiment by separating the logic out into separate models and using the  [ref](https://docs.getdbt.com/reference/dbt-jinja-functions/ref)  function to build models on top of other models:

[![The DAG we want for our dbt project](https://docs.getdbt.com/img/dbt-dag.png?v=2 "The DAG we want for our dbt project")](https://docs.getdbt.com/guides/databricks?step=10#)The DAG we want for our dbt project

1.  Create a new SQL file,  `models/stg_customers.sql`, with the SQL from the  `customers`  CTE in our original query.
    
2.  Create a second new SQL file,  `models/stg_orders.sql`, with the SQL from the  `orders`  CTE in our original query.
    
    **models/stg_customers.sql**
    
    ```sql
    select    id as customer_id,    first_name,    last_name
    from jaffle_shop_customers
    ```
    
    **models/stg_orders.sql**
    
    ```sql
    select    id as order_id,    user_id as customer_id,    order_date,    			    
    status
    from jaffle_shop_orders
    ```
    
**3.  Edit the SQL in your  `models/customers.sql`  file as follows:**
    
   **models/customers.sql**
    
 ```sql
   with customers as (
  select 
    * 
  from 
    {{ ref('stg_customers') }}
), 
orders as (
  select 
    * 
  from 
    {{ ref('stg_orders') }}
), 
customer_orders as (
  select 
    customer_id, 
    min(order_date) as first_order_date, 
    max(order_date) as most_recent_order_date, 
    count(order_id) as number_of_orders 
  from 
    orders 
  group by 
    1
), 
final as (
  select 
    customers.customer_id, 
    customers.first_name, 
    customers.last_name, 
    customer_orders.first_order_date, 
    customer_orders.most_recent_order_date, 
    coalesce(
      customer_orders.number_of_orders, 
      0
    ) as number_of_orders 
  from 
    customers 
    left join customer_orders using (customer_id)
) 
select 
  * 
from 
  final
  ```

    
**4.  Execute  dbt run.**
    
   This time, when you performed a  `dbt run`, **separate views/tables** were created for  `stg_customers`,  `stg_orders`  and  `customers`. dbt inferred the order to run these models. Because  `customers`  depends on  `stg_customers`  and  `stg_orders`, dbt builds  `customers`  last. You **do not need** to explicitly define these dependencies.