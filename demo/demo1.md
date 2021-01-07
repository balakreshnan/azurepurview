# Azure Purview End to End tutorial

## Data Governance lab using Azure Purview

## Prerequisite

- Azure Account
- Azure blob storage
- Load sample data sets
- Azure SQL DB
- Azure Synapse Analytics  - dedicated pool (formely SQL DW)
- Azure data factory
- Azure Purview account

## Steps

- First create Azure Purview account in one of the available region
- Make sure have some open data sets available
- In my case I used data.gov population dataset
- Or save the data set from synapse workspace, which are available as samples
- Use notebook to save the dataset in parquet format
- there is a tutorial to create facts and dimension using Azure data factory
- here is sample how to create a data set
- https://github.com/balakreshnan/Accenture/blob/master/cap/populationdataset.md
- give proper permission for managed identity to the storage to scan the files in blob storage
- Give purview managed identity to Azure SQL database and Syanpse (formerly sql dw) permission to access the schema

## Scan the sources

- Here are the list of data sources

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/purview1.jpg "Purview")

- Configure the data sources
- Configure blob Storage using managed identity for Purview
- Blob storage here is ADLS gen2 without HNS
- Configure a scan to scan all the files in the blob storage

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/purviewblob.jpg "Purview")

- Configure Azure SQL database as data source
- Use managed identity
- use the name of managed identity
- Log in with SQL management studio or azure portal and go to query explorer
- select the database and then open query (pls do not run in master database)

```
CREATE USER [managedidenntityname] FROM EXTERNAL PROVIDER;

GRANT CONTROL ON DATABASE::databasename TO managedidenntityname;

GRANT CONNECT TO managedidenntityname;
GRANT SELECT TO managedidenntityname;
GRANT INSERT TO managedidenntityname;
GRANT ADMINISTER DATABASE BULK OPERATIONS TO managedidenntityname;
```

- Add the purview user as External provider user in Azure SQL
- Configure a scan to run 

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/purviewsql.jpg "Purview")

- Configure Azure Synapse dedicated pool as data source
- Use managed identity
- Add the purview user as External provider user in Azure SQL
- Configure a scan to run 

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/purviewsynapse.jpg "Purview")

- from the above pictures you can see system scanned and displays the assets
- Sometimes it takes some time to scan the sources so please be patient

### Scanning Power BI

- Azure Purview allows you to scan your Power BI tenant
- You can use managed identity authentication to set up a Power BI scan (*Note: You must be a Power BI Administrator to set up the scan*)
- Setting up managed identity for a Power BI scan is a two-step process:

1. Create a security group and add the catalog's managed identity to it
2. In the Power BI Admin portal, in the Developer setting for **Allow service principals to use read-only Power BI admin APIs (Preview)**, select **Specific Security Groups** and add your security group with the catalog's managed identity

- After these steps are complete, register your Power BI tenant and set up a scan as you would with other sources
- More details on scanning Power BI can be found [here](https://docs.microsoft.com/en-us/azure/purview/register-scan-power-bi-tenant)

## Link Azure Data Factory

- Go to Management Center section
- Click Data Factory
- Click New and add which data factory to use
- Select the ADF

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/purviewadf.jpg "Purview")

## Browse Assets

- Now go to home page
- Click Browse Assets

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets1.jpg "Purview")

## Azure ADLS gen2 storage

- Select Azure Blob storage
- Select the storage account
- Select the uspopulationinput container

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets2.jpg "Purview")

- Click though Fact folder and then files
- Should take you to the asset details

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets3.jpg "Purview")

- Click Schema

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets4.jpg "Purview")

## Azure Data Factory

- Now lets look into Azure Data Factory and it's lineage
- To get lineage populated make sure run the Azure data factory
- Only when data factory pipeline connected is run the lineage get's populated
- To create a sample ADF you can use this link
- https://github.com/balakreshnan/Accenture/blob/master/cap/adfmultijoin.md
- the above ADF once created and run, below details will get populated

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets5.jpg "Purview")

- Data flow lineage

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets10.jpg "Purview")

## Azure SQL Database

- now let's browse Azure sql and see few schema
- Go to browse again and click Azure SQL
- Click the schema and view the tables
- Select coviddata table
- View the schema and lineage

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets6.jpg "Purview")

- Lineage

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets7.jpg "Purview")

## Azure Synapse Analytics - Formerly SQL DW

- now let's browse Azure Synapse Analytics (formerly SQL DW) and see few schema
- Go to browse again and click Azure SQL
- Click the schema and view the tables
- Select coviddata table
- View the schema and lineage

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets8.jpg "Purview")

- Classification

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/browseassets9.jpg "Purview")

## Power BI
- Purview allows you to view the Power BI Reports, Datasets, and Dashboards in your tenant, as well as the workspaces in which they reside
- Notice how when we browse, we can use the facets to filter our results by Power BI Asset types or particular workspaces

![Power BI](https://github.com/katthoma/Images/blob/master/PurviewPBI1.PNG?raw=true)

- You can also view the Lineage of the Power BI asset from the original data sources used to create the Power BI dataset and any additional reports or dashboards that may be leveraging it today

![Power BI2](https://github.com/katthoma/Images/blob/master/purviewPBI2.PNG)

- For many data sources, you can also choose to **Open in Power BI** which downloads a Power BI Desktop Source (PBDS) file that allows you to open the data source directly in Power BI desktop (given that you have permission to do so)

## Insights Section

- Asset insights

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/insightsassets1.jpg "Purview")

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/insightsassets2.jpg "Purview")

- Classifiation section

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/insightsclassification.jpg "Purview")

More to come
