---
title: "Apache Airflow - Basic Key Components and Initialization"
date: 2024-04-30
categories: Others
---


### Key Idea: Apache Airflow is used for the automation of scheduling.

### Key Concepts:
 - **DAGs:** 
     - Directed Acyclic Graphs.
     - An individual run is created when a DAGs is run.
     - Same DAGs can be executed many times in parallel.
     ```python
        with DAG(
        "mlops", #name
        default_args={
            "retries": 2, # re-run twice after possible failures
        },
        schedule=timedelta(days=1),
        start_date=datetime(2024, 4, 30) # start on
    ) as dag:
        #dag code

     ```

 - **Tasks:**
     - In simple term, it is an individual piece of code.
     - Each task may have upstream or downstream (i.e. dependencies).
     - With each DAG run, each Task instance is created.

 - **Operators:**
     - Viewed as templates for predefined tasks.
     - Standard templates are: bashOperator, PythonOperator, MySqlOperator, S3FileTransformOperator
     - These operators are defined inside the DAG context manager.
     ```python
     with DAG(
        "mlops"
    ) as dag:

        task1 = BashOperator(
            task_id="print_dagname",
            bash_command="dagname",
        )

    task2 = MySqlOperator(
            task_id="load_config",
            mysql_conn_id="mysql_admin",
            sql="SELECT * FROM load_config;"
        )
     ```

 - **Task dependencies:**
     - ">> or <<" symbol: task1 >> task2 ==> first perfom task1 and then task2 like wise for "<<".
     - "set_downstream, set_upstream" : t1.set_downstream([t2]) ==> first perfom task1 and then task2 like wise "set_upstream".

 - **XComs:**
     - Cross communications.
     - Allows to push or pull data between tasks.
     - 

 - **Hooks:**
    - Hooks are interfaces to interact with specific types of external systems or services.
    ```python
        #after intializing the tasks from above
        hook = MySqlHook(mysql_conn_id="mysql_admin")# Retrieve connection using hook
        result = hook.get_records("SELECT COUNT(*) FROM load_config")# Execute a query using hook
        
    ```
### Key Components: 
 - **Scheduling/Scheduler:**  
     - How to execute the task.
     - When to execute the task based on intervals.
     - Determine the execution details for the executed task.
     - Also perform, Re-running pipelines, Backfilling data, ensuring tasks completion,

 - **Webserver:** 
     - Airflow’s user interface (UI).
     - Allow interaction with the airflow components without using api or cli.
     - Allows:
         - Execute, and monitor pipelines.
         - Create connections with external systems.
         - Inspect datasets.
         - Etc. 

 - **Executor:** 
     - These are the mechanism by which pipelines run.
     - Example: LocalExecutor, SequentialExecutor, CeleryExecutor and KubernetesExecutor.

 - **PostgreSQL:** 
     - All pipeline metadata is stored.
     - SQL databases are supported too.



### Setup: I did in Visual Studion in MacOS

```python
  #Create venv 
  python -m venv airflow_venv
  source airflow_venv/bin/activate  # On Unix/macOS
  AIRFLOW_VERSION=2.3.0
```
```python
  # Install dependencies
  PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
  CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
  pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

```python
  # Initialize the Airflow database
  airflow db init
```

```python
  # Start the web server in one terminal
  airflow webserver --port 8080
```

```python
  # Start the scheduler in another terminal:
  airflow scheduler
```

```python
  # Go to the Airflow DAGs folder, typically found in ~/airflow/dags/
  # Create a new Python file, dummy.py
  #Below is a sample code, may need some changes to run without errors

  from datetime import datetime
  from airflow import DAG
  from airflow.operators.dummy_operator import DummyOperator

  default_args = {
    'owner': 'airflow',
    'retries': 1,
    'retry_delay': datetime.timedelta(minutes=5),
  }

  with DAG('sample_dag',
         default_args=default_args,
         description='A dummy DAG',
         schedule_interval=datetime.timedelta(days=1),
         start_date=datetime(2024, 4, 30),
         catchup=False) as dag:

     task1 = BashOperator(
            task_id="print_dagname",
            bash_command="dagname",
        )

     task2 = MySqlOperator(
            task_id="load_config",
            mysql_conn_id="mysql_admin",
            sql="SELECT * FROM load_config;"
        )

     task1 >> task2

    # Open the web interface at http://localhost:8080
    # Enable your DAG and trigger a run to see it in action.

```
