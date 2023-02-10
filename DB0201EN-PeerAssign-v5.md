<center>
    <img src="https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/Logos/organization_logo/organization_logo.png" width="300" alt="cognitiveclass.ai logo"  />
</center>

<h1 align=center><font size = 5>Assignment: Notebook for Peer Assignment</font></h1>


# Introduction

Using this Python notebook you will:

1.  Understand three Chicago datasets
2.  Load the three datasets into three tables in a Db2 database
3.  Execute SQL queries to answer assignment questions


## Understand the datasets

To complete the assignment problems in this notebook you will be using three datasets that are available on the city of Chicago's Data Portal:

1.  <a href="https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01">Socioeconomic Indicators in Chicago</a>
2.  <a href="https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01">Chicago Public Schools</a>
3.  <a href="https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01">Chicago Crime Data</a>

### 1. Socioeconomic Indicators in Chicago

This dataset contains a selection of six socioeconomic indicators of public health significance and a “hardship index,” for each Chicago community area, for the years 2008 – 2012.

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
[https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2](https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)

### 2. Chicago Public Schools

This dataset shows all school level performance data used to create CPS School Report Cards for the 2011-2012 school year. This dataset is provided by the city of Chicago's Data Portal.

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
[https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t](https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)

### 3. Chicago Crime Data

This dataset reflects reported incidents of crime (with the exception of murders where data exists for each victim) that occurred in the City of Chicago from 2001 to present, minus the most recent seven days.

A detailed description of this dataset and the original dataset can be obtained from the Chicago Data Portal at:
[https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ)


### Download the datasets

This assignment requires you to have these three tables populated with a subset of the whole datasets.

In many cases the dataset to be analyzed is available as a .CSV (comma separated values) file, perhaps on the internet. Click on the links below to download and save the datasets (.CSV files):

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCensusData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01" target="_blank">Chicago Census Data</a>

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01" target="_blank">Chicago Public Schools</a>

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCrimeData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01" target="_blank">Chicago Crime Data</a>

**NOTE**: For the learners who are encountering issues with loading from .csv in DB2 on Firefox, you can download the .txt files and load the data with those:

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCensusData.txt?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01" target="_blank">Chicago Census Data</a>

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.txt?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01" target="_blank">Chicago Public Schools</a>

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCrimeData.txt?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01" target="_blank">Chicago Crime Data</a>

**NOTE:** Ensure you have downloaded the datasets using the links above instead of directly from the Chicago Data Portal. The versions linked here are subsets of the original datasets and have some of the column names modified to be more database friendly which will make it easier to complete this assignment.


### Store the datasets in database tables

To analyze the data using SQL, it first needs to be stored in the database.

While it is easier to read the dataset into a Pandas dataframe and then PERSIST it into the database as we saw in Week 3 Lab 3, it results in mapping to default datatypes which may not be optimal for SQL querying. For example a long textual field may map to a CLOB instead of a VARCHAR.

Therefore, **it is highly recommended to manually load the table using the database console LOAD tool, as indicated in Week 2 Lab 1 Part II**. The only difference with that lab is that in Step 5 of the instructions you will need to click on create "(+) New Table" and specify the name of the table you want to create and then click "Next".

<img src = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/images/LoadingData.png">

##### Now open the Db2 console, open the LOAD tool, Select / Drag the .CSV file for the first dataset, Next create a New Table, and then follow the steps on-screen instructions to load the data. Name the new tables as follows:

1.  **CENSUS_DATA**
2.  **CHICAGO_PUBLIC_SCHOOLS**
3.  **CHICAGO_CRIME_DATA**


### Connect to the database

Let us first load the SQL extension and establish a connection with the database

The following required modules are pre-installed in the Skills Network Labs environment. However if you run this notebook commands in a different Jupyter environment (e.g. Watson Studio or Ananconda) you may need to install these libraries by removing the `#` sign before `!pip` in the code cell below.



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

In the next cell enter your db2 connection string. Recall you created Service Credentials for your Db2 instance in first lab in Week 3. From your Db2 service credentials copy everything after db2:// (except the double quote at the end) and paste it in the cell below after ibm_db_sa://

<img src ="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/images/details.png">



```python
# Remember the connection string is of the format:
# %sql ibm_db_sa://my-username:my-password@my-hostname:my-port/my-db-name?security=SSL
# Enter the connection string for your Db2 on Cloud database instance below
%sql ibm_db_sa://kmx22828:p1uKv0i9qXYnCbMt@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB?security=SSL
```




    'Connected: kmx22828@BLUDB'




```sql
%%sql 
select colname, colno, typename, length from syscat.columns where tabname = 'CENSUS_DATA';
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
            <td>COMMUNITY_AREA_NUMBER</td>
            <td>0</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>COMMUNITY_AREA_NAME</td>
            <td>1</td>
            <td>VARCHAR</td>
            <td>22</td>
        </tr>
        <tr>
            <td>PERCENT_OF_HOUSING_CROWDED</td>
            <td>2</td>
            <td>DECIMAL</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PERCENT_HOUSEHOLDS_BELOW_POVERTY</td>
            <td>3</td>
            <td>DECIMAL</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PERCENT_AGED_16__UNEMPLOYED</td>
            <td>4</td>
            <td>DECIMAL</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PERCENT_AGED_25__WITHOUT_HIGH_SCHOOL_DIPLOMA</td>
            <td>5</td>
            <td>DECIMAL</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PERCENT_AGED_UNDER_18_OR_OVER_64</td>
            <td>6</td>
            <td>DECIMAL</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PER_CAPITA_INCOME</td>
            <td>7</td>
            <td>INTEGER</td>
            <td>4</td>
        </tr>
        <tr>
            <td>HARDSHIP_INDEX</td>
            <td>8</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
    </tbody>
</table>




```python
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




```python
%sql select colname, colno, typename, length from syscat.columns where tabname = 'CHICAGO_CRIME_DATA';
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
            <td>ID</td>
            <td>0</td>
            <td>INTEGER</td>
            <td>4</td>
        </tr>
        <tr>
            <td>CASE_NUMBER</td>
            <td>1</td>
            <td>VARCHAR</td>
            <td>8</td>
        </tr>
        <tr>
            <td>DATE</td>
            <td>2</td>
            <td>DATE</td>
            <td>4</td>
        </tr>
        <tr>
            <td>BLOCK</td>
            <td>3</td>
            <td>VARCHAR</td>
            <td>35</td>
        </tr>
        <tr>
            <td>IUCR</td>
            <td>4</td>
            <td>VARCHAR</td>
            <td>4</td>
        </tr>
        <tr>
            <td>PRIMARY_TYPE</td>
            <td>5</td>
            <td>VARCHAR</td>
            <td>15</td>
        </tr>
        <tr>
            <td>DESCRIPTION</td>
            <td>6</td>
            <td>VARCHAR</td>
            <td>46</td>
        </tr>
        <tr>
            <td>LOCATION_DESCRIPTION</td>
            <td>7</td>
            <td>VARCHAR</td>
            <td>33</td>
        </tr>
        <tr>
            <td>ARREST</td>
            <td>8</td>
            <td>VARCHAR</td>
            <td>5</td>
        </tr>
        <tr>
            <td>DOMESTIC</td>
            <td>9</td>
            <td>VARCHAR</td>
            <td>5</td>
        </tr>
        <tr>
            <td>BEAT</td>
            <td>10</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>DISTRICT</td>
            <td>11</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>WARD</td>
            <td>12</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>COMMUNITY_AREA_NUMBER</td>
            <td>13</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>FBICODE</td>
            <td>14</td>
            <td>VARCHAR</td>
            <td>3</td>
        </tr>
        <tr>
            <td>X_COORDINATE</td>
            <td>15</td>
            <td>INTEGER</td>
            <td>4</td>
        </tr>
        <tr>
            <td>Y_COORDINATE</td>
            <td>16</td>
            <td>INTEGER</td>
            <td>4</td>
        </tr>
        <tr>
            <td>YEAR</td>
            <td>17</td>
            <td>SMALLINT</td>
            <td>2</td>
        </tr>
        <tr>
            <td>LATITUDE</td>
            <td>18</td>
            <td>DECIMAL</td>
            <td>18</td>
        </tr>
        <tr>
            <td>LONGITUDE</td>
            <td>19</td>
            <td>DECIMAL</td>
            <td>18</td>
        </tr>
        <tr>
            <td>LOCATION</td>
            <td>20</td>
            <td>VARCHAR</td>
            <td>29</td>
        </tr>
    </tbody>
</table>



## Problems

Now write and execute SQL queries to solve assignment problems

### Problem 1

##### Find the total number of crimes recorded in the CRIME table.



```sql
%%sql 
select count(*) as total_recorded_crime from chicago_crime_data 
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>total_recorded_crime</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>533</td>
        </tr>
    </tbody>
</table>



### Problem 2

##### List community areas with per capita income less than 11000.



```sql
%%sql 
select community_area_name, per_capita_income
from census_data
where per_capita_income < 11000;
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>community_area_name</th>
            <th>per_capita_income</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>West Garfield Park</td>
            <td>10934</td>
        </tr>
        <tr>
            <td>South Lawndale</td>
            <td>10402</td>
        </tr>
        <tr>
            <td>Fuller Park</td>
            <td>10432</td>
        </tr>
        <tr>
            <td>Riverdale</td>
            <td>8201</td>
        </tr>
    </tbody>
</table>



### Problem 3

##### List all case numbers for crimes  involving minors?(children are not considered minors for the purposes of crime analysis)



```sql
%%sql 
select CASE_NUMBER, primary_type, description 
from chicago_crime_data
where description like '%MINOR%';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>case_number</th>
            <th>primary_type</th>
            <th>description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HL266884</td>
            <td>LIQUOR LAW VIOL</td>
            <td>SELL/GIVE/DEL LIQUOR TO MINOR</td>
        </tr>
        <tr>
            <td>HK238408</td>
            <td>LIQUOR LAW VIOL</td>
            <td>ILLEGAL CONSUMPTION BY MINOR</td>
        </tr>
    </tbody>
</table>



### Problem 4

##### List all kidnapping crimes involving a child?



```sql
%%sql 
select case_number, primary_type, description 
from chicago_crime_data
where primary_type like '%KIDNAPPING%';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>case_number</th>
            <th>primary_type</th>
            <th>description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HN144152</td>
            <td>KIDNAPPING</td>
            <td>CHILD ABDUCTION/STRANGER</td>
        </tr>
    </tbody>
</table>



### Problem 5

##### What kinds of crimes were recorded at schools?



```sql
%%sql 
select case_number, primary_type, description, location_description 
from chicago_crime_data
where location_description like '%SCHOOL%';
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>case_number</th>
            <th>primary_type</th>
            <th>description</th>
            <th>location_description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HL353697</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HL725506</td>
            <td>BATTERY</td>
            <td>PRO EMP HANDS NO/MIN INJURY</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>HP716225</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>HH639427</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>JA460432</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HS200939</td>
            <td>CRIMINAL DAMAGE</td>
            <td>TO VEHICLE</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HK577020</td>
            <td>NARCOTICS</td>
            <td>POSS: HEROIN(WHITE)</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HS305355</td>
            <td>NARCOTICS</td>
            <td>MANU/DEL:CANNABIS 10GM OR LESS</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>HT315369</td>
            <td>ASSAULT</td>
            <td>PRO EMP HANDS NO/MIN INJURY</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HR585012</td>
            <td>CRIMINAL TRESPA</td>
            <td>TO LAND</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HH292682</td>
            <td>PUBLIC PEACE VI</td>
            <td>BOMB THREAT</td>
            <td>SCHOOL, PRIVATE, BUILDING</td>
        </tr>
        <tr>
            <td>G635735</td>
            <td>PUBLIC PEACE VI</td>
            <td>BOMB THREAT</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
    </tbody>
</table>



### Problem 6

##### List the average safety score for each type of school.



```sql
%%sql 
select  "Elementary, Middle, or High School" AS school_type, AVG(safety_score) AS avg_saftey_score
from chicago_public_schools 
group by "Elementary, Middle, or High School";
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>school_type</th>
            <th>avg_saftey_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ES</td>
            <td>49</td>
        </tr>
        <tr>
            <td>HS</td>
            <td>49</td>
        </tr>
        <tr>
            <td>MS</td>
            <td>48</td>
        </tr>
    </tbody>
</table>



### Problem 7

##### List 5 community areas with highest % of households below poverty line



```sql
%%sql 
SELECT community_area_name, PERCENT_HOUSEHOLDS_BELOW_POVERTY 
FROM CENSUS_DATA 
ORDER BY PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC LIMIT 5
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>community_area_name</th>
            <th>percent_households_below_poverty</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Riverdale</td>
            <td>56.5</td>
        </tr>
        <tr>
            <td>Fuller Park</td>
            <td>51.2</td>
        </tr>
        <tr>
            <td>Englewood</td>
            <td>46.6</td>
        </tr>
        <tr>
            <td>North Lawndale</td>
            <td>43.1</td>
        </tr>
        <tr>
            <td>East Garfield Park</td>
            <td>42.4</td>
        </tr>
    </tbody>
</table>



### Problem 8

##### Which community area is most crime prone?



```sql
%%sql 
select count(distinct(case_number)) as num_crimes, community_area_number
from chicago_crime_data
group by community_area_number order by num_crimes desc limit 1
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>num_crimes</th>
            <th>community_area_number</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>43</td>
            <td>25</td>
        </tr>
    </tbody>
</table>



### Problem 9

##### Use a sub-query to find the name of the community area with highest hardship index



```sql
%%sql 
select community_area_name, hardship_index 
from census_data 
where hardship_index = (select MAX(hardship_index) from census_data)
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>community_area_name</th>
            <th>hardship_index</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Riverdale</td>
            <td>98</td>
        </tr>
    </tbody>
</table>



### Problem 10

##### Use a sub-query to determine the Community Area Name with most number of crimes?



```sql
%%sql 
select community_area_name, community_area_number from census_data
where community_area_number = (select community_area_number from chicago_crime_data
                               group by community_area_number
                               order by count(community_area_number) desc limit 1)
```

     * ibm_db_sa://kmx22828:***@9938aec0-8105-433e-8bf9-0fbb7e483086.c1ogj3sd0tgtu0lqde00.databases.appdomain.cloud:32459/BLUDB
    Done.





<table>
    <thead>
        <tr>
            <th>community_area_name</th>
            <th>community_area_number</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Austin</td>
            <td>25</td>
        </tr>
    </tbody>
</table>



Copyright © 2020 [cognitiveclass.ai](cognitiveclass.ai?utm_source=bducopyrightlink&utm_medium=dswb&utm_campaign=bdu). This notebook and its source code are released under the terms of the [MIT License](https://bigdatauniversity.com/mit-license?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ).


## Author(s)

<h4> Hima Vasudevan </h4>
<h4> Rav Ahuja </h4>
<h4> Ramesh Sannreddy </h4>

## Contribtuor(s)

<h4> Malika Singla </h4>

## Change log

| Date       | Version | Changed by        | Change Description                             |
| ---------- | ------- | ----------------- | ---------------------------------------------- |
| 2021-11-17 | 2.6     | Lakshmi           | Updated library                                |
| 2021-05-19 | 2.4     | Lakshmi Holla     | Updated the question                           |
| 2021-04-30 | 2.3     | Malika Singla     | Updated the libraries                          |
| 2021-01-15 | 2.2     | Rav Ahuja         | Removed problem 11 and fixed changelog         |
| 2020-11-25 | 2.1     | Ramesh Sannareddy | Updated the problem statements, and datasets   |
| 2020-09-05 | 2.0     | Malika Singla     | Moved lab to course repo in GitLab             |
| 2018-07-18 | 1.0     | Rav Ahuja         | Several updates including loading instructions |
| 2018-05-04 | 0.1     | Hima Vasudevan    | Created initial version                        |

## <h3 align="center"> © IBM Corporation 2020. All rights reserved. <h3/>

