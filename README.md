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

As a best practice in SQL, you should **separate logic** that **cleans up** your data from logic that **transforms** your data. You have already started doing this in the existing query by using common table expressions (CTEs).

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



## Add tests to your models[​](https://docs.getdbt.com/guides/databricks?step=11#add-tests-to-your-models "Direct link to Add tests to your models")

Adding  [tests](https://docs.getdbt.com/docs/build/tests)  to a project helps validate that your models are working correctly.

To add tests to your project:

1.  Create a new YAML file in the  `models`  directory, named  `models/schema.yml`
    
2.  Add the following contents to the file:
    
    models/schema.yml
   
```yml
version: 2

models:
  - name: customers
    columns:
      - name: customer_id
        tests:
          - unique
          - not_null

  - name: stg_customers
    columns:
      - name: customer_id
        tests:
          - unique
          - not_null

  - name: stg_orders
    columns:
      - name: order_id
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']
      - name: customer_id
        tests:
          - not_null
          - relationships:
              to: ref('stg_customers')
              field: customer_id
```
    
3.  Run  `dbt test`, and confirm that all your **tests passed**.
    

When you run  `dbt test`, dbt **iterates** through your **YAML files, and constructs a query for each test**. Each query will return the number of records that fail the test. If this number is 0, then the test is successful.

## Document your models
Adding documentation to your project allows you to describe your models in rich detail, and share that information with your team. Here, we're going to add some basic documentation to our project.

1. Update your **models/schema.yml** file to include some descriptions, such as those below.

```yml
version: 2

models:
  - name: customers
    description: One record per customer
    columns:
      - name: customer_id
        description: Primary key
        tests:
          - unique
          - not_null
      - name: first_order_date
        description: NULL when a customer has not yet placed an order.

  - name: stg_customers
    description: This model cleans up customer data
    columns:
      - name: customer_id
        description: Primary key
        tests:
          - unique
          - not_null

  - name: stg_orders
    description: This model cleans up order data
    columns:
      - name: order_id
        description: Primary key
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']
      - name: customer_id
        tests:
          - not_null
          - relationships:
              to: ref('stg_customers')
              field: customer_id
```

2. Run dbt docs generate to generate the documentation for your project. dbt introspects your project and your warehouse to generate a JSON file with rich documentation about your project.

3. Click the **book icon** in the Develop interface to launch documentation in a new tab.

## Commit your changes
Now that you've built your **customer model**, you need to commit the changes you made to the project so that the repository has your latest code.

Under Version Control on the left, click Commit and sync and add a message. 
*For example*, "Add customers model, tests, docs."
Click Merge this branch to main to add these changes to the main branch on your repo.



## Deploy dbt[​](https://docs.getdbt.com/guides/databricks?step=14#deploy-dbt "Direct link to Deploy dbt")

Use **dbt Cloud's Scheduler** to deploy your *production jobs* confidently and build *observability *into your processes. You'll learn to create a deployment environment and run a job in the following steps.

### Create a deployment environment[​](https://docs.getdbt.com/guides/databricks?step=14#create-a-deployment-environment "Direct link to Create a deployment environment")

1.  In the upper left, select  **Deploy**, then click  **Environments**.
2.  Click  **Create Environment**.
3.  In the  **Name**  field, write the name of your deployment environment. For example, "Production."
4.  In the  **dbt Version**  field, select the latest version from the dropdown.
5.  Under  **Deployment Credentials**, enter the name of the dataset you want to use as the target, such as "Analytics". This will allow dbt to build and work with that dataset. For some data warehouses, the target dataset may be referred to as a "schema".
6.  Click  **Save**.

### Create and run a job[​](https://docs.getdbt.com/guides/databricks?step=14#create-and-run-a-job "Direct link to Create and run a job")

Jobs are a set of dbt commands that you want to run on a schedule. For example,  `dbt build`.

As the  `jaffle_shop`  business gains more customers, and those customers create more orders, you will see more records added to your source data. Because you materialized the  `customers`  model as a table, you'll need to periodically rebuild your table to ensure that the data stays up-to-date. This update will happen when you run a job.

1.  After creating your deployment environment, you should be directed to the page for a new environment. If not, select  **Deploy**  in the upper left, then click  **Jobs**.
2.  Click  **Create one**  and provide a name, for example, "Production run", and link to the Environment you just created.
3.  Scroll down to the  **Execution Settings**  section.
4.  Under  **Commands**, add this command as part of your job if you don't see it:
    -   `dbt build`
5.  Select the  **Generate docs on run**  checkbox to automatically  [generate updated project docs](https://docs.getdbt.com/docs/collaborate/build-and-view-your-docs)  each time your job runs.
6.  For this exercise, do  _not_  set a schedule for your project to run  —  while your organization's project should run regularly, there's no need to run this example project on a schedule. Scheduling a job is sometimes referred to as  _deploying a project_.
7.  Select  **Save**, then click  **Run now**  to run your job.
8.  Click the run and watch its progress under "Run history."
9.  Once the run is complete, click  **View Documentation**  to see the docs for your project.


## Access Token Issues

[community solutions](https://community.databricks.com/t5/data-engineering/not-able-to-generate-access-token-for-service-principal-using/m-p/7390#:~:text=The%20error%20message%20you%20are%20seeing%20indicates%20that,request%20that%20they%20enable%20it%20for%20your%20workspace.)



## dbt Cloud IDE

The dbt Cloud integrated development environment (IDE) is a single interface for building, testing, running, and version-controlling dbt projects from your browser. With the Cloud IDE, you can compile dbt code into SQL and run it against your database directly

![](https://files.cdn.thinkific.com/file_uploads/342803/images/4b6/dc6/983/ide-basic-layout-blank.jpeg)

Basic layout

The IDE streamlines your workflow, and features a popular user interface layout with files and folders on the left, editor on the right, and command and console information at the bottom.

![](https://files.cdn.thinkific.com/file_uploads/342803/images/384/7a7/cae/2-ide-side-menu.jpeg)

  

**1. Git repository link**  — Clicking the Git repository link, located on the upper left of the IDE, takes you to your repository on the same active branch.

**2. Documentation site button**  — Clicking the Documentation site book icon, located next to the Git repository link, leads to the dbt Documentation site. The site is powered by the latest dbt artifacts generated in the IDE using the dbt docs generate command from the Command bar.

**3. Version Control** — The IDE's powerful Version Control section contains all git-related elements, including the Git actions button and the Changes section.

**4. File Explorer** — The File Explorer shows the filetree of your repository. You can:

-   Click on any file in the filetree to open the file in the File Editor.
-   Click and drag files between directories to move files.
-   Right click a file to access the sub-menu options like duplicate file, copy file name, copy as ref, rename, delete.
    -   **Note:**  To perform these actions, the user must not be in read-only mode, which generally happens when the user is viewing the default branch.
-   Use file indicators, located to the right of your files or folder name, to see when changes or actions were made:
    -   Unsaved (•) — The IDE detects unsaved changes to your file/folder
    -   Modification (M) — The IDE detects a modification of existing files/folders
    -   Added (A) — The IDE detects added files
    -   Deleted (D) — The IDE detects deleted files

  

![](https://files.cdn.thinkific.com/file_uploads/342803/images/f71/688/5b8/ide-cmd-status.jpg)

  

**5.Command bar**  — The Command bar, located in the lower left of the IDE, is used to invoke dbt commands. When a command is invoked, the associated logs are shown in the Invocation History Drawer.

**6. IDE Status button**  — The IDE Status button, located on the lower right of the IDE, displays the current IDE status. If there is an error in the status or in the dbt code that stops the project from parsing, the button will turn red and display "Error". If there aren't any errors, the button will display a green "Ready" status. To access the IDE Status modal, simply click on this button.

## Models

-   Models are .sql files that live in the models folder.
-   Models are simply written as select statements - there is no DDL/DML that needs to be written around this. This allows the developer to focus on the logic.
-   In the Cloud IDE, the Preview button will run this select statement against your data warehouse. The results shown here are equivalent to what this model will return once it is materialized.
-   After constructing a model,  `dbt run`  in the command line will actually materialize the models into the data warehouse. The default materialization is a view.
-   The materialization can be configured as a table with the following configuration block at the top of the model file:

```yaml
{{ config(
materialized='table'
) }}

```

-   The same applies for configuring a model as a view:

```yaml
{{ config(
materialized='view'
) }}

```

-   When  `dbt run`  is executing, dbt is wrapping the select statement in the correct DDL/DML to build that model as a table/view. If that model already exists in the data warehouse, dbt will automatically drop that table or view before building the new database object. *Note: If you are on BigQuery, you may need to run  `dbt run --full-refresh`  for this to take effect.
    
-   The DDL/DML that is being run to build each model can be viewed in the logs through the cloud interface or the target folder.
    

![](https://files.cdn.thinkific.com/file_uploads/342803/images/98f/7d5/567/cloud_run_logs.png)

## Modularity

-   We could build each of our final models in a single model as we did with dim_customers, however with dbt we can create our final data products using modularity.
-   **Modularity**  is the degree to which a system's components may be separated and recombined, often with the benefit of flexibility and variety in use.
-   This allows us to build data artifacts in logical steps.
-   For example, we can stage the raw customers and orders data to shape it into what we want it to look like. Then we can build a model that references both of these to build the final dim_customers model.
-   Thinking modularly is how software engineers build applications. Models can be leveraged to apply this modular thinking to analytics engineering.

## ref Macro

-   Models  _can_  be written to reference the underlying tables and views that were building the data warehouse (e.g.  `analytics.dbt_jsmith.stg_customers`). This hard codes the table names and makes it difficult to share code between developers.
-   The  `ref`  function allows us to build dependencies between models in a flexible way that can be shared in a common code base. The  `ref`  function compiles to the name of the database object as it has been created on the most recent execution of  `dbt run`  _in the particular development environment._  This is determined by the environment configuration that was set up when the project was created.
-   Example:  `{{ ref('stg_customers') }}`  compiles to  `analytics.dbt_jsmith.stg_customers`.
-   The  `ref`  function also builds a lineage graph like the one shown below. dbt is able to determine dependencies between models and takes those into account to build models in the correct order.

![](https://files.cdn.thinkific.com/file_uploads/342803/images/07a/64f/d1f/DAG.png)

## Modeling History

-   There have been multiple modeling paradigms since the advent of database technology. Many of these are classified as normalized modeling.
-   Normalized modeling techniques were designed when storage was expensive and computational power was not as affordable as it is today.
-   With a modern cloud-based data warehouse, we can approach analytics differently in an  _agile_  or  _ad hoc_  modeling technique. This is often referred to as denormalized modeling.
-   dbt can build your data warehouse into any of these schemas. dbt is a tool for  _how_  to build these rather than enforcing  _what_  to build.

## Naming Conventions

In working on this project, we established some conventions for naming our models.

-   **Sources**  (`src`) refer to the raw table data that have been built in the warehouse through a loading process. (We will cover configuring Sources in the Sources module)
-   **Staging**  (`stg`) refers to models that are built directly on top of sources. These have a one-to-one relationship with sources tables. These are used for very light transformations that shape the data into what you want it to be. These models are used to clean and standardize the data before transforming data downstream. Note: These are typically materialized as views.
-   **Intermediate**  (`int`) refers to any models that exist between final fact and dimension tables. These should be built on staging models rather than directly on sources to leverage the data cleaning that was done in staging.
-   **Fact**  (`fct`) refers to any data that represents something that occurred or is occurring. Examples include sessions, transactions, orders, stories, votes. These are typically skinny, long tables.
-   **Dimension**  (`dim`) refers to data that represents a person, place or thing. Examples include customers, products, candidates, buildings, employees.
-   Note: The Fact and Dimension convention is based on previous normalized modeling techniques.

## Reorganize Project

-   When  `dbt run`  is executed, dbt will automatically run every model in the models directory.
-   The subfolder structure within the models directory can be leveraged for organizing the project as the data team sees fit.
-   This can then be leveraged to select certain folders with  `dbt run`  and the model selector.
-   Example: If  `dbt run -s staging`  will run all models that exist in  `models/staging`. (Note: This can also be applied for  `dbt test`  as well which will be covered later.)
-   The following framework can be a starting part for designing your own model organization:
-   **Marts**  folder: All intermediate, fact, and dimension models can be stored here. Further subfolders can be used to separate data by business function (e.g. marketing, finance)
-   **Staging**  folder: All staging models and source configurations can be stored here. Further subfolders can be used to separate data by data source (e.g. Stripe, Segment, Salesforce). (We will cover configuring Sources in the Sources module)

![](https://files.cdn.thinkific.com/file_uploads/342803/images/37a/954/943/models.png)

# Q & A

**Q1. In the dbt context, what is a model?**
* A. A select statement written in SQL

**Q2. What file type is used for building a model?**
* A. .sql

**Q3. What is the default materialization of a model if you don't proactively configure the materialization for a model?**
* A. View

**Q4. A given model called `events` is configured to be materialized as a view in dbt_project.yml and configured as a table in a config block at the top of the model. When you execute dbt run, what will happen in dbt?**
* A. dbt will build the `events` model as a table

**Q5. How do you build dependencies between models?**
* A. Use the 'ref' function in the from clause of a model

**Q6. Which one of the following is true about staging models as defined in this course?**
* A. Staging models are used to perform light touch transformations to shape the data to how you wish it looked

**Q7. Which of the following is a benefit of using subdirectories in your models directory?**
* A. Subdirectories allow you to configure materializations at the folder level for a collection of models

**Q8. Which command below will attempt to only materialize dim_customers and its downstream models?**
* A. dbt run --select dim_customers+



# Practice

Using the resources in this module, complete the following in your dbt project.

## Configure sources

-   Configure a source for the tables  `raw.jaffle_shop.customers`  and  `raw.jaffle_shop.orders`  in a file called  `src_jaffle_shop.yml`.

**`models/staging/jaffle_shop/src_jaffle_shop.yml`**

```yaml
version: 2

sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop
    tables:
      - name: customers
      - name: orders

```

-   Extra credit: Configure a source for the table  `raw.stripe.payment`  in a file called  `src_stripe.yml`.

## Refactor staging models

-   Refactor  `stg_customers.sql`  using the source function.

**`models/staging/jaffle_shop/stg_customers.sql`**

```sql
select 
    id as customer_id,
    first_name,
    last_name
from {{ source('jaffle_shop', 'customers') }}

```

-   Refactor  `stg_orders.sql`  using the source function.

**`models/staging/jaffle_shop/stg_orders.sql`**

```sql
select
    id as order_id,
    user_id as customer_id,
    order_date,
    status
from {{ source('jaffle_shop', 'orders') }}

```

-   Extra credit: Refactor  `stg_payments.sql`  using the source function.

## Extra credit

-   Configure your Stripe payments data to check for source freshness.
-   Run  `dbt source freshness`.

  

You can configure your  `sources.yml`  file as below:
```yaml
version: 2

sources:
  - name: my_table_name
    database: my_database_name
    schema: my_schema_name
    tables:
      - name: my_table_name
      - ...
        loaded_at_field: _etl_loaded_at
        freshness:
          warn_after:
	          count: 12
	          period: hour
          error_after:
		      count: 24
	          period: hour
```

