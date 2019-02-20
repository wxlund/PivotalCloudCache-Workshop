#Cloud Environment Setup
cf create-service-key workshop-pcc devkey## Environment Setup

### Create Mysql Service Instance

Pizza store app will make service calls to this mysql instance

```
cf create-service p.mysql db-small workshop-db
```

### Create PCC Instance
Services can be created through Apps Manager Marketplace or by executing cf cli commands

###### Display available PCC plans

```
cf marketplace -s p-cloudcache
```

###### Step 1: create a PCC OnDemand service in your org & space

```
cf create-service p-cloudcache dev-plan workshop-pcc

```

###### Step 2: Create service key for retrieving connection information for GFSH cli

```
cf create-service-key workshop-pcc devkey
```

###### Step 3: Retrieve url for PCC cli (GFSH) and corresponding credentials 

```
cf service-key workshop-pcc devkey
```

###### Step 4: Login into to PCC cli (GFSH)

```
connect --use-http=true --url=http://gemfire-xxxx-xxx-xx-xxxx.system.excelsiorcloud.com/gemfire/v1 --user=cluster_operator --password=*******
```
or if you are in a development environment where security is relaxed you may use the following:
```
connect --use-http=true --use-ssl --skip-ssl-validation=true --url=http://gemfire-xxxx-xxx-xx-xxxx.system.excelsiorcloud.com/gemfire/v1 --user=cluster_operator --password=*******
```

#Local Environment Setup

(Note: There is not need for mysql.  It is an optional step to install a docker image with mysql.  The local project uses the embedded 
h2 database). 
### Step 1: Install gemfire in your local environment
Download the version that matches your cloud foundry release by visiting [pivotal-gemfire](https://network.pivotal.io/products/pivotal-gemfire/#/releases/). Follow the instructions in the [Pivotal Documentation](http://gemfire.docs.pivotal.io/96/gemfire/getting_started/installation/install_standalone.html) for setting up your installation. 

###### Step 2: Login into to gfsh 

```
gfsh
gfsh>start locator
gfsh>start server
```

You now have a local environment for jumping to step #5 for both cloud and local development. 

#Both Environments
## Creating Regions is the same for both

###### Step 5: create PCC regions

Note: Region name created on PCC server and client should match

```
create region --name=customer --type=PARTITION_REDUNDANT_PERSISTENT
create region --name=pizza_orders --type=PARTITION_REDUNDANT_PERSISTENT
```

To verify you have the regions you can type:

```
list regions
```
and see the following as your output:

```
List of regions
---------------
customer
pizza_orders
```

