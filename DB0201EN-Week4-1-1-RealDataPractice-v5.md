<center>
    <img src="https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/Logos/organization_logo/organization_logo.png" width="300" alt="cognitiveclass.ai logo"  />
</center>

# Working with a real world data-set using SQL and Python

Estaimted time needed: **30** minutes

## Objectives

After complting this lab you will be able to:

*   Understand the dataset for Chicago Public School level performance
*   Store the dataset in an Db2 database on IBM Cloud instance
*   Retrieve metadata about tables and columns and query data from mixed case columns
*   Solve example problems to practice your SQL skills including using built-in database functions


## Chicago Public Schools - Progress Report Cards (2011-2012)

The city of Chicago released a dataset showing all school level performance data used to create School Report Cards for the 2011-2012 school year. The dataset is available from the Chicago Data Portal: [https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t](https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)

This dataset includes a large number of metrics. Start by familiarizing yourself with the types of metrics in the database: [https://data.cityofchicago.org/api/assets/AAD41A13-BE8A-4E67-B1F5-86E711E09D5F?download=true](https://data.cityofchicago.org/api/assets/AAD41A13-BE8A-4E67-B1F5-86E711E09D5F?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01&download=true&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)

**NOTE**:

Do not download the dataset directly from City of Chicago portal. Instead download a static copy which is a more database friendly version from this <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01">link</a>.

**NOTE**:

For the learners who are encountering issues with loading from .csv in DB2 on Firefox, you can download the .txt files and load the data with those: <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.txt?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01">link</a>.

Now review some of its contents.


### Store the dataset in a Table

In many cases the dataset to be analyzed is available as a .CSV (comma separated values) file, perhaps on the internet. To analyze the data using SQL, it first needs to be stored in the database.

While it is easier to read the dataset into a Pandas dataframe and then PERSIST it into the database as we saw in the previous lab, it results in mapping to default datatypes which may not be optimal for SQL querying. For example a long textual field may map to a CLOB instead of a VARCHAR.

Therefore, **it is highly recommended to manually load the table using the database console LOAD tool, as indicated in Week 2 Lab 1 Part II**. The only difference with that lab is that in Step 5 of the instructions you will need to click on create "(+) New Table" and specify the name of the table you want to create and then click "Next".

##### Now open the Db2 console, open the LOAD tool, Select / Drag the .CSV file for the CHICAGO PUBLIC SCHOOLS dataset and load the dataset into a new table called **SCHOOLS**.

<a href="https://cognitiveclass.ai/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01"><img src = "https://ibm.box.com/shared/static/uc4xjh1uxcc78ks1i18v668simioz4es.jpg"></a>


### Connect to the database

Let us now load the ipython-sql  extension and establish a connection with the database

The following modules are pre-installed in the Skills Network Labs environment. However if you run this notebook commands in a different Jupyter environment (e.g. Watson Studio or Ananconda) you may need to install these libraries by removing the `#` sign before `!pip` in the code cell below.



```python
# These libraries are pre-installed in SN Labs. If running in another environment please uncomment lines below to install them:
# !pip install --force-reinstall ibm_db==3.1.0 ibm_db_sa==0.3.3
# Ensure we don't load_ext with sqlalchemy>=1.4 (incompadible)
# !pip uninstall sqlalchemy==1.4 -y && pip install sqlalchemy==1.3.24
# !pip install ipython-sql
```


```python
%load_ext sql
```


```python
# Enter the connection string for your Db2 on Cloud database instance below
# %sql ibm_db_sa://my-username:my-password@my-hostname:my-port/my-db-name?security=SSL
%sql ibm_db_sa://kmx22828:p1uKv0i9qXYnCbMt@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB?security=SSL
```




    'Connected: kmx22828@BLUDB'



### Query the database system catalog to retrieve table metadata

##### You can verify that the table creation was successful by retrieving the list of all tables in your schema and checking whether the SCHOOLS table was created



```python
# type in your query to retrieve list of all tables in the database for your db2 schema (username)
%sql select *  from syscat.tables where TABSCHEMA = 'KMX22828';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>tabschema</th>
            <th>tabname</th>
            <th>owner</th>
            <th>ownertype</th>
            <th>TYPE</th>
            <th>status</th>
            <th>base_tabschema</th>
            <th>base_tabname</th>
            <th>rowtypeschema</th>
            <th>rowtypename</th>
            <th>create_time</th>
            <th>alter_time</th>
            <th>invalidate_time</th>
            <th>stats_time</th>
            <th>colcount</th>
            <th>tableid</th>
            <th>tbspaceid</th>
            <th>card</th>
            <th>npages</th>
            <th>mpages</th>
            <th>fpages</th>
            <th>npartitions</th>
            <th>nfiles</th>
            <th>tablesize</th>
            <th>overflow</th>
            <th>tbspace</th>
            <th>index_tbspace</th>
            <th>long_tbspace</th>
            <th>parents</th>
            <th>children</th>
            <th>selfrefs</th>
            <th>keycolumns</th>
            <th>keyindexid</th>
            <th>keyunique</th>
            <th>checkcount</th>
            <th>datacapture</th>
            <th>const_checked</th>
            <th>pmap_id</th>
            <th>partition_mode</th>
            <th>log_attribute</th>
            <th>pctfree</th>
            <th>append_mode</th>
            <th>REFRESH</th>
            <th>refresh_time</th>
            <th>LOCKSIZE</th>
            <th>VOLATILE</th>
            <th>row_format</th>
            <th>property</th>
            <th>statistics_profile</th>
            <th>compression</th>
            <th>rowcompmode</th>
            <th>access_mode</th>
            <th>clustered</th>
            <th>active_blocks</th>
            <th>droprule</th>
            <th>maxfreespacesearch</th>
            <th>avgcompressedrowsize</th>
            <th>avgrowcompressionratio</th>
            <th>avgrowsize</th>
            <th>pctrowscompressed</th>
            <th>logindexbuild</th>
            <th>codepage</th>
            <th>collationschema</th>
            <th>collationname</th>
            <th>collationschema_orderby</th>
            <th>collationname_orderby</th>
            <th>encoding_scheme</th>
            <th>pctpagessaved</th>
            <th>last_regen_time</th>
            <th>secpolicyid</th>
            <th>protectiongranularity</th>
            <th>auditpolicyid</th>
            <th>auditpolicyname</th>
            <th>auditexceptionenabled</th>
            <th>definer</th>
            <th>oncommit</th>
            <th>logged</th>
            <th>onrollback</th>
            <th>lastused</th>
            <th>control</th>
            <th>temporaltype</th>
            <th>tableorg</th>
            <th>extended_row_size</th>
            <th>pctextendedrows</th>
            <th>remarks</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>KMX22828</td>
            <td>TEST</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-19 23:14:17.753320</td>
            <td>2022-09-19 23:14:17.753320</td>
            <td>2022-09-19 23:14:17.753320</td>
            <td>2022-09-19 23:59:54.131941</td>
            <td>2</td>
            <td>4</td>
            <td>5447</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>1</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>0</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-19 23:14:17.753320</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>0001-01-01</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>PETSALE</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-19 23:18:51.627787</td>
            <td>2022-09-19 23:44:31.009786</td>
            <td>2022-09-19 23:44:31.009792</td>
            <td>2022-10-12 22:00:02.563535</td>
            <td>5</td>
            <td>5</td>
            <td>5447</td>
            <td>5</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>54</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-19 23:18:51.627787</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-09-19</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>PETRESCUE</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-30 18:20:19.196084</td>
            <td>2022-09-30 18:20:19.358642</td>
            <td>2022-09-30 18:20:19.358648</td>
            <td>2022-09-30 18:24:05.505994</td>
            <td>5</td>
            <td>11</td>
            <td>5447</td>
            <td>9</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>38</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-30 18:20:19.196084</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-09-30</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>EMPLOYEES</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-30 19:11:45.176294</td>
            <td>2022-09-30 19:11:45.285483</td>
            <td>2022-09-30 19:11:45.285489</td>
            <td>2022-09-30 19:19:05.885559</td>
            <td>11</td>
            <td>6</td>
            <td>5447</td>
            <td>10</td>
            <td>1</td>
            <td>0</td>
            <td>2</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>118</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-30 19:11:45.176294</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>0001-01-01</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>JOB_HISTORY</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-30 19:11:45.301309</td>
            <td>2022-09-30 19:11:45.424850</td>
            <td>2022-09-30 19:11:45.424856</td>
            <td>2022-09-30 20:24:56.205363</td>
            <td>4</td>
            <td>7</td>
            <td>5447</td>
            <td>10</td>
            <td>1</td>
            <td>0</td>
            <td>2</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>2</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>43</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-30 19:11:45.301309</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-09-30</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>JOBS</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-30 19:11:45.448807</td>
            <td>2022-09-30 19:11:45.563812</td>
            <td>2022-09-30 19:11:45.563818</td>
            <td>2022-09-30 19:39:13.768927</td>
            <td>4</td>
            <td>8</td>
            <td>5447</td>
            <td>10</td>
            <td>1</td>
            <td>0</td>
            <td>2</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>53</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-30 19:11:45.448807</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-09-30</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>DEPARTMENTS</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-30 19:11:45.591038</td>
            <td>2022-09-30 19:11:45.698733</td>
            <td>2022-09-30 19:11:45.698740</td>
            <td>2022-09-30 20:06:23.800484</td>
            <td>4</td>
            <td>9</td>
            <td>5447</td>
            <td>3</td>
            <td>1</td>
            <td>0</td>
            <td>2</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>57</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-30 19:11:45.591038</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>0001-01-01</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>LOCATIONS</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-09-30 19:11:45.714877</td>
            <td>2022-09-30 19:11:45.823560</td>
            <td>2022-09-30 19:11:45.823567</td>
            <td>2022-09-30 20:24:56.415905</td>
            <td>2</td>
            <td>10</td>
            <td>5447</td>
            <td>3</td>
            <td>1</td>
            <td>0</td>
            <td>2</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>2</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>28</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-09-30 19:11:45.714877</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-09-30</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>INSTRUCTOR</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-10-10 20:19:04.474356</td>
            <td>2022-10-10 20:19:04.614849</td>
            <td>2022-10-10 20:19:04.614855</td>
            <td>2022-10-10 20:34:06.279562</td>
            <td>5</td>
            <td>12</td>
            <td>5447</td>
            <td>3</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>49</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-10-10 20:19:04.474356</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-10-10</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>INTERNATIONAL_STUDENT_TEST_SCORES</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-10-10 20:42:11.875510</td>
            <td>2022-10-10 20:42:11.875510</td>
            <td>2022-10-10 20:42:11.875510</td>
            <td>2022-10-10 20:43:40.059781</td>
            <td>4</td>
            <td>13</td>
            <td>5447</td>
            <td>99</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>49</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-10-10 20:42:11.875510</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-10-10</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>CHICAGO_SOCIOECONOMIC_DATA</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-10-10 21:36:44.834771</td>
            <td>2022-10-10 21:36:44.834771</td>
            <td>2022-10-10 21:36:45.196683</td>
            <td>2022-10-10 21:38:12.702634</td>
            <td>10</td>
            <td>14</td>
            <td>5447</td>
            <td>78</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>111</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-10-10 21:36:44.834771</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-10-10</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
        <tr>
            <td>KMX22828</td>
            <td>CHICAGO_PUBLIC_SCHOOLS</td>
            <td>KMX22828</td>
            <td>U</td>
            <td>T</td>
            <td>N</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>2022-10-10 22:32:20.754359</td>
            <td>2022-10-10 22:32:20.754359</td>
            <td>2022-10-10 22:32:20.754359</td>
            <td>2022-10-10 22:45:34.085913</td>
            <td>78</td>
            <td>15</td>
            <td>5447</td>
            <td>566</td>
            <td>15</td>
            <td>0</td>
            <td>16</td>
            <td>-1</td>
            <td>-1</td>
            <td>-1</td>
            <td>0</td>
            <td>KMX22828SPACE1</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>N</td>
            <td>YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY</td>
            <td>1</td>
            <td> </td>
            <td>0</td>
            <td>-1</td>
            <td>N</td>
            <td> </td>
            <td>None</td>
            <td>R</td>
            <td> </td>
            <td>N</td>
            <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
            <td>None</td>
            <td>N</td>
            <td> </td>
            <td>F</td>
            <td>None</td>
            <td>0</td>
            <td>N</td>
            <td>999</td>
            <td>0</td>
            <td>0.0</td>
            <td>843</td>
            <td>0.0</td>
            <td>None</td>
            <td>1208</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td>SYSIBM</td>
            <td>IDENTITY</td>
            <td> </td>
            <td>0</td>
            <td>2022-10-10 22:32:20.754359</td>
            <td>0</td>
            <td> </td>
            <td>None</td>
            <td>None</td>
            <td>N</td>
            <td>KMX22828</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>2022-10-10</td>
            <td> </td>
            <td>N</td>
            <td>R</td>
            <td>N</td>
            <td>-1.0</td>
            <td>None</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
In Db2 the system catalog table called SYSCAT.TABLES contains the table metadata
-->


Double-click **here** for the solution.

<!-- Solution:

%sql select TABSCHEMA, TABNAME, CREATE_TIME from SYSCAT.TABLES where TABSCHEMA='YOUR-DB2-USERNAME'

or, you can retrieve list of all tables where the schema name is not one of the system created ones:

%sql select TABSCHEMA, TABNAME, CREATE_TIME from SYSCAT.TABLES \
      where TABSCHEMA not in ('SYSIBM', 'SYSCAT', 'SYSSTAT', 'SYSIBMADM', 'SYSTOOLS', 'SYSPUBLIC')
      
or, just query for a specifc table that you want to verify exists in the database
%sql select * from SYSCAT.TABLES where TABNAME = 'SCHOOLS'

-->


### Query the database system catalog to retrieve column metadata

##### The SCHOOLS table contains a large number of columns. How many columns does this table have?



```python
# type in your query to retrieve the number of columns in the SCHOOLS table
%sql select count(*) from syscat.columns where tabname = 'CHICAGO_PUBLIC_SCHOOLS';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>78</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
In Db2 the system catalog table called SYSCAT.COLUMNS contains the column metadata
-->


Double-click **here** for the solution.

<!-- Solution:

%sql select count(*) from SYSCAT.COLUMNS where TABNAME = 'SCHOOLS'

-->


Now retrieve the the list of columns in SCHOOLS table and their column type (datatype) and length.



```python
# type in your query to retrieve all column names in the SCHOOLS table along with their datatypes and length
%sql select colname, colno, typename, length from syscat.columns where tabname = 'CHICAGO_PUBLIC_SCHOOLS';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>colname</th>
            <th>colno</th>
            <th>typename</th>
            <th>length</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>SCHOOL_ID</td>
            <td>0</td>
            <td>INTEGER</td>
            <td>4</td>
        </tr>
        <tr>
            <td>NAME_OF_SCHOOL</td>
            <td>1</td>
            <td>VARCHAR</td>
            <td>64</td>
        </tr>
        <tr>
            <td>Elementary, Middle, or High School</td>
            <td>2</td>
            <td>VARCHAR</td>
            <td>2</td>
        </tr>
        <tr>
            <td>STREET_ADDRESS</td>
            <td>3</td>
            <td>VARCHAR</td>
            <td>29</td>
        </tr>
        <tr>
            <td>CITY</td>
            <td>4</td>
            <td>VARCHAR</td>
            <td>7</td>
        </tr>
        <tr>
            <td>STATE</td>
            <td>5</td>
            <td>VARCHAR</td>
            <td>2</td>
        </tr>
        <tr>
            <td>ZIP_CODE</td>
            <td>6</td>
            <td>INTEGER</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PHONE_NUMBER</td>
            <td>7</td>
            <td>VARCHAR</td>
            <td>14</td>
        </tr>
        <tr>
            <td>LINK</td>
            <td>8</td>
            <td>VARCHAR</td>
            <td>78</td>
        </tr>
        <tr>
            <td>NETWORK_MANAGER</td>
            <td>9</td>
            <td>VARCHAR</td>
            <td>40</td>
        </tr>
        <tr>
            <td>COLLABORATIVE_NAME</td>
            <td>10</td>
            <td>VARCHAR</td>
            <td>34</td>
        </tr>
        <tr>
            <td>ADEQUATE_YEARLY_PROGRESS_MADE_</td>
            <td>11</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>TRACK_SCHEDULE</td>
            <td>12</td>
            <td>VARCHAR</td>
            <td>12</td>
        </tr>
        <tr>
            <td>CPS_PERFORMANCE_POLICY_STATUS</td>
            <td>13</td>
            <td>VARCHAR</td>
            <td>16</td>
        </tr>
        <tr>
            <td>CPS_PERFORMANCE_POLICY_LEVEL</td>
            <td>14</td>
            <td>VARCHAR</td>
            <td>15</td>
        </tr>
        <tr>
            <td>HEALTHY_SCHOOL_CERTIFIED</td>
            <td>15</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>SAFETY_ICON</td>
            <td>16</td>
            <td>VARCHAR</td>
            <td>11</td>
        </tr>
        <tr>
            <td>SAFETY_SCORE</td>
            <td>17</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>FAMILY_INVOLVEMENT_ICON</td>
            <td>18</td>
            <td>VARCHAR</td>
            <td>11</td>
        </tr>
        <tr>
            <td>FAMILY_INVOLVEMENT_SCORE</td>
            <td>19</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>ENVIRONMENT_ICON</td>
            <td>20</td>
            <td>VARCHAR</td>
            <td>11</td>
        </tr>
        <tr>
            <td>ENVIRONMENT_SCORE</td>
            <td>21</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>INSTRUCTION_ICON</td>
            <td>22</td>
            <td>VARCHAR</td>
            <td>11</td>
        </tr>
        <tr>
            <td>INSTRUCTION_SCORE</td>
            <td>23</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>LEADERS_ICON</td>
            <td>24</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>LEADERS_SCORE</td>
            <td>25</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>TEACHERS_ICON</td>
            <td>26</td>
            <td>VARCHAR</td>
            <td>11</td>
        </tr>
        <tr>
            <td>TEACHERS_SCORE</td>
            <td>27</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>PARENT_ENGAGEMENT_ICON</td>
            <td>28</td>
            <td>VARCHAR</td>
            <td>7</td>
        </tr>
        <tr>
            <td>PARENT_ENGAGEMENT_SCORE</td>
            <td>29</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>PARENT_ENVIRONMENT_ICON</td>
            <td>30</td>
            <td>VARCHAR</td>
            <td>7</td>
        </tr>
        <tr>
            <td>PARENT_ENVIRONMENT_SCORE</td>
            <td>31</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>AVERAGE_STUDENT_ATTENDANCE</td>
            <td>32</td>
            <td>VARCHAR</td>
            <td>6</td>
        </tr>
        <tr>
            <td>RATE_OF_MISCONDUCTS__PER_100_STUDENTS_</td>
            <td>33</td>
            <td>DECIMAL</td>
            <td>5</td>
        </tr>
        <tr>
            <td>AVERAGE_TEACHER_ATTENDANCE</td>
            <td>34</td>
            <td>VARCHAR</td>
            <td>6</td>
        </tr>
        <tr>
            <td>INDIVIDUALIZED_EDUCATION_PROGRAM_COMPLIANCE_RATE</td>
            <td>35</td>
            <td>VARCHAR</td>
            <td>7</td>
        </tr>
        <tr>
            <td>PK_2_LITERACY__</td>
            <td>36</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PK_2_MATH__</td>
            <td>37</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR3_5_GRADE_LEVEL_MATH__</td>
            <td>38</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR3_5_GRADE_LEVEL_READ__</td>
            <td>39</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR3_5_KEEP_PACE_READ__</td>
            <td>40</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR3_5_KEEP_PACE_MATH__</td>
            <td>41</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR6_8_GRADE_LEVEL_MATH__</td>
            <td>42</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR6_8_GRADE_LEVEL_READ__</td>
            <td>43</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR6_8_KEEP_PACE_MATH_</td>
            <td>44</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR6_8_KEEP_PACE_READ__</td>
            <td>45</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR_8_EXPLORE_MATH__</td>
            <td>46</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GR_8_EXPLORE_READ__</td>
            <td>47</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>ISAT_EXCEEDING_MATH__</td>
            <td>48</td>
            <td>DECIMAL</td>
            <td>4</td>
        </tr>
        <tr>
            <td>ISAT_EXCEEDING_READING__</td>
            <td>49</td>
            <td>DECIMAL</td>
            <td>4</td>
        </tr>
        <tr>
            <td>ISAT_VALUE_ADD_MATH</td>
            <td>50</td>
            <td>DECIMAL</td>
            <td>3</td>
        </tr>
        <tr>
            <td>ISAT_VALUE_ADD_READ</td>
            <td>51</td>
            <td>DECIMAL</td>
            <td>3</td>
        </tr>
        <tr>
            <td>ISAT_VALUE_ADD_COLOR_MATH</td>
            <td>52</td>
            <td>VARCHAR</td>
            <td>6</td>
        </tr>
        <tr>
            <td>ISAT_VALUE_ADD_COLOR_READ</td>
            <td>53</td>
            <td>VARCHAR</td>
            <td>6</td>
        </tr>
        <tr>
            <td>STUDENTS_TAKING__ALGEBRA__</td>
            <td>54</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>STUDENTS_PASSING__ALGEBRA__</td>
            <td>55</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>9th Grade EXPLORE (2009)</td>
            <td>56</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>9th Grade EXPLORE (2010)</td>
            <td>57</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>10th Grade PLAN (2009)</td>
            <td>58</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>10th Grade PLAN (2010)</td>
            <td>59</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>NET_CHANGE_EXPLORE_AND_PLAN</td>
            <td>60</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>11th Grade Average ACT (2011)</td>
            <td>61</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>NET_CHANGE_PLAN_AND_ACT</td>
            <td>62</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>COLLEGE_ELIGIBILITY__</td>
            <td>63</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>GRADUATION_RATE__</td>
            <td>64</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>COLLEGE_ENROLLMENT_RATE__</td>
            <td>65</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>COLLEGE_ENROLLMENT</td>
            <td>66</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>GENERAL_SERVICES_ROUTE</td>
            <td>67</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>FRESHMAN_ON_TRACK_RATE__</td>
            <td>68</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>X_COORDINATE</td>
            <td>69</td>
            <td>DECIMAL</td>
            <td>13</td>
        </tr>
        <tr>
            <td>Y_COORDINATE</td>
            <td>70</td>
            <td>DECIMAL</td>
            <td>13</td>
        </tr>
        <tr>
            <td>LATITUDE</td>
            <td>71</td>
            <td>DECIMAL</td>
            <td>18</td>
        </tr>
        <tr>
            <td>LONGITUDE</td>
            <td>72</td>
            <td>DECIMAL</td>
            <td>18</td>
        </tr>
        <tr>
            <td>COMMUNITY_AREA_NUMBER</td>
            <td>73</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>COMMUNITY_AREA_NAME</td>
            <td>74</td>
            <td>VARCHAR</td>
            <td>22</td>
        </tr>
        <tr>
            <td>WARD</td>
            <td>75</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>POLICE_DISTRICT</td>
            <td>76</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>LOCATION</td>
            <td>77</td>
            <td>VARCHAR</td>
            <td>27</td>
        </tr>
    </tbody>
</table>



Double-click **here** for the solution.

<!-- Solution:

%sql select COLNAME, TYPENAME, LENGTH from SYSCAT.COLUMNS where TABNAME = 'SCHOOLS'

or

%sql select distinct(NAME), COLTYPE, LENGTH from SYSIBM.SYSCOLUMNS where TBNAME = 'SCHOOLS'

-->


### Questions

1.  Is the column name for the "SCHOOL ID" attribute in upper or mixed case?
2.  What is the name of "Community Area Name" column in your table? Does it have spaces?
3.  Are there any columns in whose names the spaces and paranthesis (round brackets) have been replaced by the underscore character "\_"?


## Problems

### Problem 1

##### How many Elementary Schools are in the dataset?



```python
%sql select count(*) from CHICAGO_PUBLIC_SCHOOLS where "Elementary, Middle, or High School" = 'ES';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>462</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
Which column specifies the school type e.g. 'ES', 'MS', 'HS'? ("Elementary School, Middle School, High School")
-->


Double-click **here** for another hint

<!--
Does the column name have mixed case, spaces or other special characters?
If so, ensure you use double quotes around the "Name of the Column"
-->


Double-click **here** for the solution.

<!-- Solution:

%sql select count(*) from SCHOOLS where "Elementary, Middle, or High School" = 'ES'

Correct answer: 462

-->


### Problem 2

##### What is the highest Safety Score?



```python
%sql select MAX(SAFETY_SCORE) as MAX_SAFETY_SCORE from CHICAGO_PUBLIC_SCHOOLS;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>max_safety_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>99</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
Use the MAX() function
-->


Double-click **here** for the solution.

<!-- Hint:

%sql select MAX(Safety_Score) AS MAX_SAFETY_SCORE from SCHOOLS

Correct answer: 99
-->


### Problem 3

##### Which schools have highest Safety Score?



```sql
%%sql 
select NAME_OF_SCHOOL, SAFETY_SCORE from CHICAGO_PUBLIC_SCHOOLS 
where SAFETY_SCORE = (select MAX(SAFETY_SCORE) from CHICAGO_PUBLIC_SCHOOLS) 

```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>name_of_school</th>
            <th>safety_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Abraham Lincoln Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Alexander Graham Bell Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Annie Keller Elementary Gifted Magnet School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Augustus H Burley Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Edgar Allan Poe Elementary Classical School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Edgebrook Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Ellen Mitchell Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>James E McDade Elementary Classical School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>James G Blaine Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>LaSalle Elementary Language Academy</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Mary E Courtenay Elementary Language Arts Center</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Northside College Preparatory High School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Northside Learning Center High School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Norwood Park Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Oriole Park Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Sauganash Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Stephen Decatur Classical Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Talman Elementary School</td>
            <td>99</td>
        </tr>
        <tr>
            <td>Wildwood Elementary School</td>
            <td>99</td>
        </tr>
    </tbody>
</table>



Double-click **here** for the solution.

<!-- Solution:
In the previous problem we found out that the highest Safety Score is 99, so we can use that as an input in the where clause:

%sql select Name_of_School, Safety_Score from SCHOOLS where Safety_Score = 99

or, a better way:

%sql select Name_of_School, Safety_Score from SCHOOLS where \
  Safety_Score= (select MAX(Safety_Score) from SCHOOLS)


Correct answer: several schools with with Safety Score of 99.
-->


### Problem 4

##### What are the top 10 schools with the highest "Average Student Attendance"?



```sql
%%sql 
select NAME_OF_SCHOOL, AVERAGE_STUDENT_ATTENDANCE from CHICAGO_PUBLIC_SCHOOLS
order by AVERAGE_STUDENT_ATTENDANCE desc nulls last limit 10;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>name_of_school</th>
            <th>average_student_attendance</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>John Charles Haines Elementary School</td>
            <td>98.40%</td>
        </tr>
        <tr>
            <td>James Ward Elementary School</td>
            <td>97.80%</td>
        </tr>
        <tr>
            <td>Edgar Allan Poe Elementary Classical School</td>
            <td>97.60%</td>
        </tr>
        <tr>
            <td>Orozco Fine Arts &amp; Sciences Elementary School</td>
            <td>97.60%</td>
        </tr>
        <tr>
            <td>Rachel Carson Elementary School</td>
            <td>97.60%</td>
        </tr>
        <tr>
            <td>Annie Keller Elementary Gifted Magnet School</td>
            <td>97.50%</td>
        </tr>
        <tr>
            <td>Andrew Jackson Elementary Language Academy</td>
            <td>97.40%</td>
        </tr>
        <tr>
            <td>Lenart Elementary Regional Gifted Center</td>
            <td>97.40%</td>
        </tr>
        <tr>
            <td>Disney II Magnet School</td>
            <td>97.30%</td>
        </tr>
        <tr>
            <td>John H Vanderpoel Elementary Magnet School</td>
            <td>97.20%</td>
        </tr>
    </tbody>
</table>



Double-click **here** for the solution.

<!-- Solution:

%sql select Name_of_School, Average_Student_Attendance from SCHOOLS \
    order by Average_Student_Attendance desc nulls last limit 10 

-->


### Problem 5

##### Retrieve the list of 5 Schools with the lowest Average Student Attendance sorted in ascending order based on attendance



```sql
%%sql 
select NAME_OF_SCHOOL, AVERAGE_STUDENT_ATTENDANCE from CHICAGO_PUBLIC_SCHOOLS
order by AVERAGE_STUDENT_ATTENDANCE limit 5;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>name_of_school</th>
            <th>average_student_attendance</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Richard T Crane Technical Preparatory High School</td>
            <td>57.90%</td>
        </tr>
        <tr>
            <td>Barbara Vick Early Childhood &amp; Family Center</td>
            <td>60.90%</td>
        </tr>
        <tr>
            <td>Dyett High School</td>
            <td>62.50%</td>
        </tr>
        <tr>
            <td>Wendell Phillips Academy High School</td>
            <td>63.00%</td>
        </tr>
        <tr>
            <td>Orr Academy High School</td>
            <td>66.30%</td>
        </tr>
    </tbody>
</table>



Double-click **here** for the solution.

<!-- Solution:

%sql SELECT Name_of_School, Average_Student_Attendance  \
     from SCHOOLS \
     order by Average_Student_Attendance \
     fetch first 5 rows only

-->


### Problem 6

##### Now remove the '%' sign from the above result set for Average Student Attendance column



```sql
%%sql 
SELECT NAME_OF_SCHOOL, REPLACE(AVERAGE_STUDENT_ATTENDANCE,'%','')
FROM CHICAGO_PUBLIC_SCHOOLS LIMIT 5
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>name_of_school</th>
            <th>2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Abraham Lincoln Elementary School</td>
            <td>96.00</td>
        </tr>
        <tr>
            <td>Adam Clayton Powell Paideia Community Academy Elementary School</td>
            <td>95.60</td>
        </tr>
        <tr>
            <td>Adlai E Stevenson Elementary School</td>
            <td>95.70</td>
        </tr>
        <tr>
            <td>Agustin Lara Elementary Academy</td>
            <td>95.50</td>
        </tr>
        <tr>
            <td>Air Force Academy High School</td>
            <td>93.30</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
Use the REPLACE() function to replace '%' with ''
See documentation for this function at:
https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.sql.ref.doc/doc/r0000843.html
-->


Double-click **here** for the solution.

<!-- Hint:

%sql SELECT Name_of_School, REPLACE(Average_Student_Attendance, '%', '') \
     from SCHOOLS \
     order by Average_Student_Attendance \
     fetch first 5 rows only

-->


### Problem 7

##### Which Schools have Average Student Attendance lower than 70%?



```sql
%%sql 
select NAME_OF_SCHOOL, AVERAGE_STUDENT_ATTENDANCE
from CHICAGO_PUBLIC_SCHOOLS
where DECIMAL(REPLACE(AVERAGE_STUDENT_ATTENDANCE,'%','')) < 70
order by AVERAGE_STUDENT_ATTENDANCE;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>name_of_school</th>
            <th>average_student_attendance</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Richard T Crane Technical Preparatory High School</td>
            <td>57.90%</td>
        </tr>
        <tr>
            <td>Barbara Vick Early Childhood &amp; Family Center</td>
            <td>60.90%</td>
        </tr>
        <tr>
            <td>Dyett High School</td>
            <td>62.50%</td>
        </tr>
        <tr>
            <td>Wendell Phillips Academy High School</td>
            <td>63.00%</td>
        </tr>
        <tr>
            <td>Orr Academy High School</td>
            <td>66.30%</td>
        </tr>
        <tr>
            <td>Manley Career Academy High School</td>
            <td>66.80%</td>
        </tr>
        <tr>
            <td>Chicago Vocational Career Academy High School</td>
            <td>68.80%</td>
        </tr>
        <tr>
            <td>Roberto Clemente Community Academy High School</td>
            <td>69.60%</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
The datatype of the "Average_Student_Attendance" column is varchar.
So you cannot use it as is in the where clause for a numeric comparison.
First use the CAST() function to cast it as a DECIMAL or DOUBLE
e.g. CAST("Column_Name" as DOUBLE)
or simply: DECIMAL("Column_Name")
-->


Double-click **here** for another hint

<!--
Don't forget the '%' age sign needs to be removed before casting
-->


Double-click **here** for the solution.

<!-- Solution:

%sql SELECT Name_of_School, Average_Student_Attendance  \
     from SCHOOLS \
     where CAST ( REPLACE(Average_Student_Attendance, '%', '') AS DOUBLE ) < 70 \
     order by Average_Student_Attendance
     
or,

%sql SELECT Name_of_School, Average_Student_Attendance  \
     from SCHOOLS \
     where DECIMAL ( REPLACE(Average_Student_Attendance, '%', '') ) < 70 \
     order by Average_Student_Attendance

-->


### Problem 8

##### Get the total College Enrollment for each Community Area



```sql
%%sql 
select SUM(COLLEGE_ENROLLMENT) AS total_enrollment, COMMUNITY_AREA_NAME from CHICAGO_PUBLIC_SCHOOLS GROUP BY COMMUNITY_AREA_NAME;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>total_enrollment</th>
            <th>community_area_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>6864</td>
            <td>ALBANY PARK</td>
        </tr>
        <tr>
            <td>4823</td>
            <td>ARCHER HEIGHTS</td>
        </tr>
        <tr>
            <td>1458</td>
            <td>ARMOUR SQUARE</td>
        </tr>
        <tr>
            <td>6483</td>
            <td>ASHBURN</td>
        </tr>
        <tr>
            <td>4175</td>
            <td>AUBURN GRESHAM</td>
        </tr>
        <tr>
            <td>10933</td>
            <td>AUSTIN</td>
        </tr>
        <tr>
            <td>1522</td>
            <td>AVALON PARK</td>
        </tr>
        <tr>
            <td>3640</td>
            <td>AVONDALE</td>
        </tr>
        <tr>
            <td>14386</td>
            <td>BELMONT CRAGIN</td>
        </tr>
        <tr>
            <td>1636</td>
            <td>BEVERLY</td>
        </tr>
        <tr>
            <td>3167</td>
            <td>BRIDGEPORT</td>
        </tr>
        <tr>
            <td>9647</td>
            <td>BRIGHTON PARK</td>
        </tr>
        <tr>
            <td>549</td>
            <td>BURNSIDE</td>
        </tr>
        <tr>
            <td>1568</td>
            <td>CALUMET HEIGHTS</td>
        </tr>
        <tr>
            <td>5042</td>
            <td>CHATHAM</td>
        </tr>
        <tr>
            <td>7086</td>
            <td>CHICAGO LAWN</td>
        </tr>
        <tr>
            <td>2085</td>
            <td>CLEARING</td>
        </tr>
        <tr>
            <td>4670</td>
            <td>DOUGLAS</td>
        </tr>
        <tr>
            <td>4568</td>
            <td>DUNNING</td>
        </tr>
        <tr>
            <td>5337</td>
            <td>EAST GARFIELD PARK</td>
        </tr>
        <tr>
            <td>5305</td>
            <td>EAST SIDE</td>
        </tr>
        <tr>
            <td>4600</td>
            <td>EDGEWATER</td>
        </tr>
        <tr>
            <td>910</td>
            <td>EDISON PARK</td>
        </tr>
        <tr>
            <td>6832</td>
            <td>ENGLEWOOD</td>
        </tr>
        <tr>
            <td>1431</td>
            <td>FOREST GLEN</td>
        </tr>
        <tr>
            <td>531</td>
            <td>FULLER PARK</td>
        </tr>
        <tr>
            <td>9915</td>
            <td>GAGE PARK</td>
        </tr>
        <tr>
            <td>4552</td>
            <td>GARFIELD RIDGE</td>
        </tr>
        <tr>
            <td>2809</td>
            <td>GRAND BOULEVARD</td>
        </tr>
        <tr>
            <td>4051</td>
            <td>GREATER GRAND CROSSING</td>
        </tr>
        <tr>
            <td>963</td>
            <td>HEGEWISCH</td>
        </tr>
        <tr>
            <td>3975</td>
            <td>HERMOSA</td>
        </tr>
        <tr>
            <td>8620</td>
            <td>HUMBOLDT PARK</td>
        </tr>
        <tr>
            <td>1930</td>
            <td>HYDE PARK</td>
        </tr>
        <tr>
            <td>7764</td>
            <td>IRVING PARK</td>
        </tr>
        <tr>
            <td>1755</td>
            <td>JEFFERSON PARK</td>
        </tr>
        <tr>
            <td>4287</td>
            <td>KENWOOD</td>
        </tr>
        <tr>
            <td>7055</td>
            <td>LAKE VIEW</td>
        </tr>
        <tr>
            <td>5615</td>
            <td>LINCOLN PARK</td>
        </tr>
        <tr>
            <td>4132</td>
            <td>LINCOLN SQUARE</td>
        </tr>
        <tr>
            <td>7351</td>
            <td>LOGAN SQUARE</td>
        </tr>
        <tr>
            <td>871</td>
            <td>LOOP</td>
        </tr>
        <tr>
            <td>7257</td>
            <td>LOWER WEST SIDE</td>
        </tr>
        <tr>
            <td>1552</td>
            <td>MCKINLEY PARK</td>
        </tr>
        <tr>
            <td>1317</td>
            <td>MONTCLARE</td>
        </tr>
        <tr>
            <td>3271</td>
            <td>MORGAN PARK</td>
        </tr>
        <tr>
            <td>2091</td>
            <td>MOUNT GREENWOOD</td>
        </tr>
        <tr>
            <td>3362</td>
            <td>NEAR NORTH SIDE</td>
        </tr>
        <tr>
            <td>1378</td>
            <td>NEAR SOUTH SIDE</td>
        </tr>
        <tr>
            <td>7975</td>
            <td>NEAR WEST SIDE</td>
        </tr>
        <tr>
            <td>7922</td>
            <td>NEW CITY</td>
        </tr>
        <tr>
            <td>7541</td>
            <td>NORTH CENTER</td>
        </tr>
        <tr>
            <td>5146</td>
            <td>NORTH LAWNDALE</td>
        </tr>
        <tr>
            <td>4210</td>
            <td>NORTH PARK</td>
        </tr>
        <tr>
            <td>6469</td>
            <td>NORWOOD PARK</td>
        </tr>
        <tr>
            <td>140</td>
            <td>OAKLAND</td>
        </tr>
        <tr>
            <td>786</td>
            <td>OHARE</td>
        </tr>
        <tr>
            <td>6954</td>
            <td>PORTAGE PARK</td>
        </tr>
        <tr>
            <td>1620</td>
            <td>PULLMAN</td>
        </tr>
        <tr>
            <td>1547</td>
            <td>RIVERDALE</td>
        </tr>
        <tr>
            <td>4068</td>
            <td>ROGERS PARK</td>
        </tr>
        <tr>
            <td>7020</td>
            <td>ROSELAND</td>
        </tr>
        <tr>
            <td>4043</td>
            <td>SOUTH CHICAGO</td>
        </tr>
        <tr>
            <td>1859</td>
            <td>SOUTH DEERING</td>
        </tr>
        <tr>
            <td>14793</td>
            <td>SOUTH LAWNDALE</td>
        </tr>
        <tr>
            <td>4543</td>
            <td>SOUTH SHORE</td>
        </tr>
        <tr>
            <td>4388</td>
            <td>UPTOWN</td>
        </tr>
        <tr>
            <td>4006</td>
            <td>WASHINGTON HEIGHTS</td>
        </tr>
        <tr>
            <td>2648</td>
            <td>WASHINGTON PARK</td>
        </tr>
        <tr>
            <td>3700</td>
            <td>WEST ELSDON</td>
        </tr>
        <tr>
            <td>5946</td>
            <td>WEST ENGLEWOOD</td>
        </tr>
        <tr>
            <td>2622</td>
            <td>WEST GARFIELD PARK</td>
        </tr>
        <tr>
            <td>4207</td>
            <td>WEST LAWN</td>
        </tr>
        <tr>
            <td>3240</td>
            <td>WEST PULLMAN</td>
        </tr>
        <tr>
            <td>8197</td>
            <td>WEST RIDGE</td>
        </tr>
        <tr>
            <td>9429</td>
            <td>WEST TOWN</td>
        </tr>
        <tr>
            <td>4206</td>
            <td>WOODLAWN</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
Verify the exact name of the Enrollment column in the database
Use the SUM() function to add up the Enrollments for each Community Area
-->


Double-click **here** for another hint

<!--
Don't forget to group by the Community Area
-->


Double-click **here** for the solution.

<!-- Solution:

%sql select Community_Area_Name, sum(College_Enrollment) AS TOTAL_ENROLLMENT \
   from SCHOOLS \
   group by Community_Area_Name 

-->


### Problem 9

##### Get the 5 Community Areas with the least total College Enrollment  sorted in ascending order



```sql
%%sql 
select SUM(COLLEGE_ENROLLMENT) AS total_enrollment, COMMUNITY_AREA_NAME 
from CHICAGO_PUBLIC_SCHOOLS 
GROUP BY COMMUNITY_AREA_NAME
order by total_enrollment limit 5;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>total_enrollment</th>
            <th>community_area_name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>140</td>
            <td>OAKLAND</td>
        </tr>
        <tr>
            <td>531</td>
            <td>FULLER PARK</td>
        </tr>
        <tr>
            <td>549</td>
            <td>BURNSIDE</td>
        </tr>
        <tr>
            <td>786</td>
            <td>OHARE</td>
        </tr>
        <tr>
            <td>871</td>
            <td>LOOP</td>
        </tr>
    </tbody>
</table>



Double-click **here** for a hint

<!--
Order the previous query and limit the number of rows you fetch
-->


Double-click **here** for the solution.

<!-- Solution:

%sql select Community_Area_Name, sum(College_Enrollment) AS TOTAL_ENROLLMENT \
   from SCHOOLS \
   group by Community_Area_Name \
   order by TOTAL_ENROLLMENT asc \
   fetch first 5 rows only

-->


### Problem 10

##### List 5 schools with lowest safety score.



```sql
%%sql 
select SAFETY_SCORE, NAME_OF_SCHOOL
from CHICAGO_PUBLIC_SCHOOLS 
order by SAFETY_SCORE limit 5;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>safety_score</th>
            <th>name_of_school</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Edmond Burke Elementary School</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Luke O&#x27;Toole Elementary School</td>
        </tr>
        <tr>
            <td>6</td>
            <td>George W Tilton Elementary School</td>
        </tr>
        <tr>
            <td>11</td>
            <td>Foster Park Elementary School</td>
        </tr>
        <tr>
            <td>13</td>
            <td>Emil G Hirsch Metropolitan High School</td>
        </tr>
    </tbody>
</table>



Double-click **here** for the solution.

<!-- Solution:

%sql SELECT name_of_school, safety_score \
FROM schools \
ORDER BY safety_score \
LIMIT 5
-->


### Problem 11

##### Get the hardship index for the community area which has College Enrollment of 4368



```sql
%%sql 
select hardship_index 
   from CHICAGO_SOCIOECONOMIC_DATA CD, CHICAGO_PUBLIC_SCHOOLS CPS 
   where CD.ca = CPS.community_area_number 
      and college_enrollment = 4368
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>hardship_index</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>6.0</td>
        </tr>
    </tbody>
</table>




```python
%sql select colname, colno, typename, length from syscat.columns where tabname = 'CHICAGO_SOCIOECONOMIC_DATA';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>colname</th>
            <th>colno</th>
            <th>typename</th>
            <th>length</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>index</td>
            <td>0</td>
            <td>BIGINT</td>
            <td>8</td>
        </tr>
        <tr>
            <td>CA</td>
            <td>1</td>
            <td>DOUBLE</td>
            <td>8</td>
        </tr>
        <tr>
            <td>COMMUNITY_AREA_NAME</td>
            <td>2</td>
            <td>CLOB</td>
            <td>1048576</td>
        </tr>
        <tr>
            <td>PERCENT_OF_HOUSING_CROWDED</td>
            <td>3</td>
            <td>DOUBLE</td>
            <td>8</td>
        </tr>
        <tr>
            <td>PERCENT_HOUSEHOLDS_BELOW_POVERTY</td>
            <td>4</td>
            <td>DOUBLE</td>
            <td>8</td>
        </tr>
        <tr>
            <td>PERCENT_AGED_16_UNEMPLOYED</td>
            <td>5</td>
            <td>DOUBLE</td>
            <td>8</td>
        </tr>
        <tr>
            <td>PERCENT_AGED_25_WITHOUT_HIGH_SCHOOL_DIPLOMA</td>
            <td>6</td>
            <td>DOUBLE</td>
            <td>8</td>
        </tr>
        <tr>
            <td>PERCENT_AGED_UNDER_18_OR_OVER_64</td>
            <td>7</td>
            <td>DOUBLE</td>
            <td>8</td>
        </tr>
        <tr>
            <td>PER_CAPITA_INCOME_</td>
            <td>8</td>
            <td>BIGINT</td>
            <td>8</td>
        </tr>
        <tr>
            <td>HARDSHIP_INDEX</td>
            <td>9</td>
            <td>DOUBLE</td>
            <td>8</td>
        </tr>
    </tbody>
</table>



Double-click **here** for the solution.

<!-- Solution:
NOTE: For this solution to work the CHICAGO_SOCIOECONOMIC_DATA table 
      as created in the last lab of Week 3 should already exist

%%sql 
select hardship_index 
   from chicago_socioeconomic_data CD, schools CPS 
   where CD.ca = CPS.community_area_number 
      and college_enrollment = 4368

-->


### Problem 12

##### Get the hardship index for the community area which has the school with the  highest enrollment.



```sql
%%sql 
SELECT CD.HARDSHIP_INDEX, CPS.COMMUNITY_AREA_NAME, CPS.COLLEGE_ENROLLMENT 
FROM CHICAGO_PUBLIC_SCHOOLS CPS, CHICAGO_SOCIOECONOMIC_DATA CD
WHERE CD.CA = CPS.COMMUNITY_AREA_NUMBER 
ORDER BY COLLEGE_ENROLLMENT DESC LIMIT 1;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>hardship_index</th>
            <th>community_area_name</th>
            <th>college_enrollment</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>6.0</td>
            <td>NORTH CENTER</td>
            <td>4368</td>
        </tr>
    </tbody>
</table>




```sql
%%sql 
select ca, community_area_name, hardship_index 
from chicago_socioeconomic_data
where ca in (select community_area_number from CHICAGO_PUBLIC_SCHOOLS order by college_enrollment desc limit 1)
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>ca</th>
            <th>community_area_name</th>
            <th>hardship_index</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>5.0</td>
            <td>North Center</td>
            <td>6.0</td>
        </tr>
    </tbody>
</table>



Double-click **here** for the solution.

<!-- Solution:
NOTE: For this solution to work the CHICAGO_SOCIOECONOMIC_DATA table 
      as created in the last lab of Week 3 should already exist

%sql select ca, community_area_name, hardship_index from chicago_socioeconomic_data \
   where ca in \
   ( select community_area_number from schools order by college_enrollment desc limit 1 )

-->


## Summary

##### In this lab you learned how to work with a real word dataset using SQL and Python. You learned how to query columns with spaces or special characters in their names and with mixed case names. You also used built in database functions and practiced how to sort, limit, and order result sets, as well as used sub-queries and worked with multiple tables.


## Author

<a href="https://www.linkedin.com/in/ravahuja/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01" target="_blank">Rav Ahuja</a>

## Change Log

| Date (YYYY-MM-DD) | Version | Changed By        | Change Description                        |
| ----------------- | ------- | ----------------- | ----------------------------------------- |
| 2021-07-09        | 2.4     | Malika            | Updated connection string                 |
| 2021-05-19        | 2.3     | Lakshmi Holla     | Updated question                          |
| 2021-04-20        | 2.2     | Malika            | Added the libraries                       |
| 2020-11-27        | 2.1     | Sannareddy Ramesh | Modified data sets and added new problems |
| 2020-08-28        | 2.0     | Lavanya           | Moved lab to course repo in GitLab        |

<hr>

## <h3 align="center"> © IBM Corporation 2020. All rights reserved. <h3/>

