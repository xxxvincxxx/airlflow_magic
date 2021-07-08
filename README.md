# Airflow Magic!

* DAGS -> a set of tasks with different dependencies

A DAG stands for a Directed Acyclic Graph. In Airflow, this represents the set of tasks that make up your workflow. It consists of the tasks and the dependencies between tasks. DAGs are created with various details about the DAG, including the name, start date, owner, email alerting options, etc.

* How to run a task via:

``` bash
airflow run <dag_id> <task_id> <start_date>
```

## Building blocks

Directed Acyclic Graph, has the following attributes: It is Directed, meaning there is an inherent flow representing the dependencies or order between execution of components. These dependencies (even implicit ones) provide context to the tools on how to order the running of components. A DAG is also Acyclic - it does not loop or repeat. This does not imply that the entire DAG cannot be rerun, only that the individual components are executed once per run. In this case, a Graph represents the components and the relationships (or dependencies) between them. The term DAG is found often in data engineering, not just in Airflow but also Apache Spark, Luigi, and others.


``` bash
# Import the DAG object
from airflow.models import DAG

# Define the default_args dictionary
default_args = {
  'owner': 'dsmith',
  'start_date': datetime(2020, 1, 14),
  'retries': 2
}

# Instantiate the DAG object
etl_dag = DAG('example_etl', default_args=default_args)

```

## Check how many DAGs tasks there are

``` bash
# List all possible commands
airflow -h

# List the possible dags
airflow list_dags
```

## Airflow Operators
 
* they represent tasks.
* they run independently.

## Bash operators
* it executes a given Bash command or script

``` bash
from airflow.operators.bash_operator import BashOperator
exampleBH = BashOperator(task_id = "aaa",?
                         bash_command = "echo 1",
                         dag = dag)
```

## Gotchas for Operators
Pay attention to:
1. Environmental variables
2. Environmental credentials (i.e. DB access)
3. User priviledges

## Tasks Dependency

* either `>>` or `<<` as dependency
* `>>` **before**
* `<<` **after**

``` bash
task1 >> task2 << task3
```
aka:
``` bash
task1 >> task2
task3 >> task2
```

## Using the PythonOperator
``` python
# Define the method
def pull_file(URL, savepath):
    r = requests.get(URL)
    with open(savepath, 'wb') as f:
        f.write(r.content)    
    # Use the print method for logging
    print(f"File pulled from {URL} and saved to {savepath}")
    
    
def pull_file(URL, savepath):
    r = requests.get(URL)
    with open(savepath, 'wb') as f:
        f.write(r.content)   
    # Use the print method for logging
    print(f"File pulled from {URL} and saved to {savepath}")

from airflow.operators.python_operator import PythonOperator

# Create the task
pull_file_task = PythonOperator(
    task_id='pull_file',
    # Add the callable
    python_callable=pull_file,
    # Define the arguments
    op_kwargs={'URL':'http://dataserver/sales.json', 'savepath':'latestsales.json'},
    dag=process_sales_dag)
```








