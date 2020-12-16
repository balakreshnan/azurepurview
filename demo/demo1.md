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

- Configure the data sources
- Configure blob Storage using managed identity for Purview
- Blob storage here is ADLS gen2 without HNS
- Configure a scan to scan all the files in the blob storage

![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")