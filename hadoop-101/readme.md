# WELCOME TO OVH WORKSHOP FOR ANALYTICS DATA PLATFORM !




## DISCOVER OVH ANALYTICS

We will size and deploy a big data Apache Hadoop cluster, then perform a use case allowing us to discover basic features : user and rights, dashboard, ingest data, queries on data.
We will play with French DVF datas, the “Demandes de Valeurs Foncières”.
It's the amount of money for each real estate transaction in France since 5 years.


### Big data cluster links

You need to deploy a cluster with OVH : https://www.ovh.com/fr/public-cloud/big-data-hadoop/

It's a big data cluster, deployed and secured in 1 hour on top of OVH Public Cloud.

Then you will have URL for services.

Also : if you just want an easy solution, you can deploy locally Hortonworks Sandbox : 
https://www.cloudera.com/downloads/hortonworks-sandbox.html
It's a mono-VM hadoop, for sandbox usages. It's not secured and not scalable but still, you can use Apache Hadoop software



## USE CASE

### STEP 1

Download Open Data DVF 2018 : https://www.data.gouv.fr/fr/datasets/demandes-de-valeurs-foncieres/ 
Direct link for data if needed : https://www.data.gouv.fr/fr/datasets/r/1be77ca5-dc1b-4e50-af2b-0240147e0346


### STEP 2

Code to create SQL dvf table : see file "create_dvf_table.txt" or copy https://pastebin.com/y2fNc7Jy


### STEP 3

```
SELECT count(*) FROM dvf_<your_login>
SELECT * FROM dvf_<your_login> LIMIT 20
SELECT * from dvf_<your_login> WHERE code_postal = "75017" AND  type_local = "Appartement" LIMIT 100
SELECT * from dvf_<your_login> WHERE code_postal = "75004" AND  type_local = "Appartement" AND voie = "BEAUTREILLIS" AND no_voie = "4" LIMIT 10
```

### STEP 4

Here is the "commas" script for Apache Pig :

```
A = LOAD '/user/<your_login>/ovh/valeursfoncieres-2018.txt' as line;  
B = FOREACH A GENERATE REPLACE(line,'([,]+)','.');  
STORE B INTO '/user/<your_login>/cleaned/cleaned_valeursfoncieres-2018.txt';
```


### STEP 6

Code to create SQL cleaned_data table : see file "create_cleaned_data_table.txt" or copy https://pastebin.com/E801sYtA

Query : 

```
SELECT * from cleaned_data_<your_login> WHERE code_postal = "75004" AND  type_local = "Appartement" AND voie = "BEAUTREILLIS" AND no_voie = "4" LIMIT 10
SELECT AVG(valeur_fonciere), AVG(Surface_reelle_bati) FROM cleaned_data_<your_login> WHERE code_postal = "75017" AND type_local = "Appartement"
```

DONE
Thank you and have a good day !

OVH team