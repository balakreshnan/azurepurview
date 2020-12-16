# Azure Purview End to End tutorial

## Data Governance lab using Azure Purview

## Prerequistie

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
- in my case i used data.gov population dataset
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

## Insights Section

- Asset insights

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/insightsassets1.jpg "Purview")

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/insightsassets2.jpg "Purview")

- Classifiation section

![alt text](https://github.com/balakreshnan/azurepurview/blob/main/images/insightsclassification.jpg "Purview")

More to come