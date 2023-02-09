# Run trevas lab on docker

Trevas lab contains two parts:
- frontend: inseefrlab/trevas-lab-ui:0.3.4
- backend: inseefrlab/trevas-lab:master

MODULES : in-memory,spark/local
API_BASE_URL : api url

##  Run the trevas lab backend in docker

With below command you can run the backend in docker
```shell
# pull image
docker pull inseefrlab/trevas-lab:master

# run the container with a env var SPARK_CLUSTER_MASTER_LOCAL=local, and a volume mapping with the host
docker run --env=SPARK_CLUSTER_MASTER_LOCAL=local  --volume=C:\trevas_data:/data -p 8080:8080 --runtime=runc -d inseefrlab/trevas-lab:master

```

> The env var `SPARK_CLUSTER_MASTER_LOCAL : local` specifies the trevas backend uses a spark cluster en mode local
 
## Run the trevas lab frontend in docker

By default the `trevas lab frontend(ui)` connect to localhost:8080 (default backend url), if you run the backend with another url, you need to modify the env var **API_BASE_URL** to map the correct backend url

```shell
# run the container with a env var MODULES=in-memory,spark/local and API_BASE_URL=10.50.5.88:8080
docker run --env=MODULES=in-memory,spark/local --env=API_BASE_URL=10.50.5.88:8080 -p 80:80 --runtime=runc -d inseefrlab/trevas-lab-ui:0.3.4
```

> The MODULES=in-memory,spark/local indicates the frontend supports two types of trevas `in-memory` and `spark/local`. API_BASE_URL=10.50.5.88:8080 indicates the backend api url



## test vtl lab via web ui

### step1: Create data bindings
click on `connect`, a form will appear, 
- `binding name` will be the name of the table after VTL read the data source
- `file type` is  csv or parquet
- `connection url` is the path of the data source. For example s3a://bucket_name/path_to_file or file:///path_to_file

Click on `save` after you are done, you should see a new binding in the binding onglet

### Step 2: Write a VTL script which transform the data
Click on `VTL Script`, you will see a terminal. Write below code in it and click on `Execute`

```text
# Suppose the data binding which we have created in step1 is called my_data_binding
# here we just do a value affectation, we create a dataframe(table) from my_data_binding
df1:=my_data_binding

```

### Step 3: Save results

Click on `save results`. You should see a form which ask you to select the output format. Choose s3 for example, you will see a new form with
- "Binding name": Choose a dataframe name which you want to export, Here we can choose `df1`.
- "file type": is the output file format
- "connection url": is the path of the exported data For example s3a://bucket_name/path_to_file or file:///path_to_file

### Step 4: Preview results

Click on `Preview results`. You should see a list of the exported binding(dataframe). Clicked on the bindings, you should see the content of the dataframe in a tabular way.
