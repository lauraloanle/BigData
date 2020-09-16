

# **SEC Financial Statement Report Analysis**
Working with Big Data - Group 12
 
## **Group Members:**
Laura Le, Jennifer Nguyen, Vibhu Verma, Sally Zhang
 

## **Executive Summary** 
In this project, we investigate public companies’ financial filings data from the SEC database. The most important findings are:

California is the state with the highest number of public companies that file with the SEC.

Balance sheets are the most important financial statements.

Regarding disclosure, Investors and the SEC care the most about a company’s income taxes and any future commitment or contingencies. 

Businesses usually choose to end their business operations cycle at the end of a quarter. 

Deep Down, Inc. is the company with the highest current assets in 2018. 


## **Introduction**
The dataset we picked for the project is SEC Financial Statement Dataset. The SEC Financial Statement Dataset provides the text and detailed numeric information of all financial statements. The Dataset is extracted from corporate annual and quarterly reports filed with the SEC using XBRL since January 2009.

The dataset covers all XBRL filings since 2009, it includes 12.6k companies. In total 239.7k annual and quarterly reports, 192.5 million data points, and 20GB+ compressed in Parquet. 

7 files (each up to 30GB, in total around 150GB) we accessed using Athena Query are: 

**1. Company_submission**

The submissions data set contains summary information about an entire EDGAR submission.

**2.report_presentation_section**

This table contains all the sections within a given report. To point out,  we use the following statement(section) type abbreviation:

* I     -  Statement of Income

* IP    -  Statement of Income (Parenthetical)

* CI    -  Statement of Comprehensive Income

* CIP   -  Statement of Comprehensive Income (Parenthetical)

* B     -  Balance Sheet

* BP    -  Balance Sheet (Parenthetical)

* C     -  Statement of Cash Flows

* CP    -  Statement of Cash Flows (Parenthetical)

* SE    -  Statement of Equity

* SEP   -  Statement of Equity (Parenthetical)


**3. Data_point**

This table contains all the data points and values since the first XBRL filing in 2009.

**4. Data_point_snapshot**

The table contains the line item sequence for each report section. The data points grouping and relationship can be found here. There are many important uses with this table. 

**5. Data_point_revision**

Segment table will provide more information for the segment defined.  

**6. Report_presentation_line_item**

This information in this table is derived from a reported data_point table. Each data point will have only one last effective value at its earliest reporting date.

**7. Segment**

This table will archive all the revised/restated values. 

## **Objectives** 

Digitizing the audit is an ongoing effort. The use of Machine Learning Methods to create systematic models in audit work is in great demand. The ultimate next step in the process is to replace conventional audit procedures by more efficient data-driven procedures. Machine Learning in audit is not a widely used application since there are certain limitations in doing so: for example: different audit reports from different agents would differ significantly. Using supervised learning to assess whether or not an account is materially misstated using independent data shifts the burden of auditing the data of the dependent data to auditing the independent data.

Machine-learning techniques are very powerful tools that the auditor can employ to reach his audit objectives. Even though these techniques have so far not been widely used as part of a financial statement audit, in other areas they have proven their added value. The barriers and roadblocks that keep auditors from their use can be overcome if needed.


## Exploratory Analysis 

The 7 tables contain numerous information of the company filling data. And by taking some explanatory analysis to our data will help us gain some insights.
We are very interested to see the results of the following questions:

1. What are the states that have the highest number of business financial fillings submitted over the recorded period of time？

2. What are the top 20 report sections that appear the most in 10Ks and 10Qs and which statement do they belong to?

3. What is the distribution of companies based on fiscal year end month?

4.  Top 10 companies have highest current assets for the year 2018?

5. What are the companies that have the highest current assets? 



### **1.  Most Popular States for Public Companies**
* As we know that all companies, foreign and domestic, are required to file registration statements, periodic reports, and other forms electronically through EDGAR. The CIK is a unique identifier assigned by the SEC to all companies and people who file disclosures with the SEC. People can check 10-K (annual) and 10-Q (quarterly) reports, proxies, and others.
To answer this question, we want to use only those annual reports (10-K) to avoid redundancy within our calculation of popular business locations. One interesting thing we noticed is that there are in total 175 states recorded and we only have 51 states in the United States. Why? After a brief investigation of the Securities and Exchange Commission website, we noticed that EDGAR State and Country Codes includes not only states within the U.S., but also international. Searches on State and Country codes will return results based on the code used by the filer at the time of the filing, which can be accessed by the following link.
* For example, F4 represents CHINA and A6 represents ONTARIO, CANADA.
EDGAR State and Country Codes Form: 
https://www.sec.gov/edgar/searchedgar/edgarstatecodes.htm

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/TopStates.png?raw=true) 
 
### **2. What are the top 20 report sections that appear the most across 10Ks and 10Qs and which statement do they belong to?**

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Jennifer_EDA1.png?raw=true) 


* There are 1170159 distinct report sections that exist across the submitted financial reports. This large number can be explained by the variation in naming a standard section. For example, Balance Sheet can be listed as `Balance Sheet` or `Consolidated Balance Sheet`.
Now, let's see which sections are the most common across reports.The higher the frequency of appearance a section has, the more important it might be to financial reporting overall.

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Jennifer_EDA2.png?raw=true) 

* As we can see from the results above, `Document and Entity Information` is the most common section. This is expected since almost every single financial report will have a section to introduce the company and the purpose of the report. The second most common section in a financial report is `Disclosure - Income Taxes`. Therefore, this may imply that besides the section that includes an introduction of the entity and the purpose of the report, `Income Taxes`, `Commitments and Contingencies`, and `Subsequent Events` are the most essential sections in a report. Within this top 20, the sections that have `Disclosure` in their names give us some clues about the information that investors most care about regarding reporting transparency.
 
* From this top 20, we can also see the most essential/important financial statements. They are Balance Sheet and Statement of Cash Flows, with Balance Sheet ranking higher in the level of importance. (We can see under `statement_type` column, B and BP appear 4 times, with a total section frequency of 161K+ times while C only appears once with a section frequency of 42851 times.)
 
### **3. What is the distribution of companies based on fiscal year end month?**

* `Fiscal_year_end_month` is the indicator of the end of a business operations cycle for each type of business or industry. For example, if the company is in the retail business, then it is reasonable to set a fiscal year end date after the Holiday season (probably December 30th). While for a pooling service company, for example, the business cycle ends at the end of the summer. Thus, it would be reasonable for the company to set the fiscal year end date on September 30th.
* The distribution of companies across months will present insights into what business cycles are the most prominent across 12.6K+ companies that submitted financial filings to the SEC over the last 10 years.


![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Jennifer_EDA3.png?raw=true) 


* The result shows that the majority of companies chose December as the fiscal_year_end_month. The second, third, and fourth most popular month to end the fiscal year belong to June, March and September, respectively. We can see that business cycles are often quite in sync with the annual quarter division as March marks the end of Q1, June marks the end of Q2, September marks the end of Q3, and December marks the end of Q4.
* Besides months that are the end of the quarters, January is the fiscal_year_end_month with the most public companies.


### **4. Top 10 companies have highest current assets for the year 2018?**

* Current assets represent all the assets of a company that are expected to be conveniently sold, consumed, used, or exhausted through standard business operations within one year. Current assets appear on a company's balance sheet, one of the required financial statements that must be completed each year
* In the data_point table, we queried and sorted out the top 10 companies that have the highest assets which are reported at the end of the year 2018. The reason we want to check the assets of companies until at the end of 2018 is that in this dataset, the last day is 2019-02-22 which means only two months of 2019 are recorded and it would be more accuracy to compare the full results of the most recent year. 


![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Laura%20-%20Q4%20-%20Pic1.png?raw=true) 
 
* Based on the cik numbers assigned, we do right join between the table of company_submission and the table above based on the cik as the primary key to find out names of top 10 companies having the highest current assets for the year 2018.

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Laura%20-%20Q4%20-%20Pic2.png?raw=true) 

* Top 10 resulted companies are Deep Down, Inc ; Amerinac Holding Corp ; Precision Aerospace Components, Inc ; Mam Software Group, Inc ; Boxlight Corp ; American Shared Hospital Services ; International Tower  Hill Mines Ltd ; Isoray, Inc ; Qualstar Corp ; Image Sensing Systems Inc.


### **5. Annual reports of these top 5 companies in the above list from 2010-02-25 to 2019-02-22.?**

* Annual reports provide information on the company's mission and history and summarize the company's achievements in the past year. 
* Top 5 companies that have highest current assets by the order are :

1. Deep Down, Inc

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Laura%20-%20Q5%20-%20Pic1.png?raw=true)

2. Amerinac Holding Corp

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Laura%20-%20Q5%20-%20Pic2.png?raw=true)

3. Precision Aerospace Components, Inc

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Laura%20-%20Q5%20-%20Pic3.png?raw=true)

4. Mam Software Group, Inc 

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Laura%20-%20Q5%20-%20Pic4.png?raw=true)

5. Boxlight Corp

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Laura%20-%20Q5%20-%20Pic5.png?raw=true)

* By reviewing their annual reports, we have an overview of timing that companies calculate and submit reports, their locations (states, address) and their contact information to have a general understanding about these companies.


## **Methodology**

#### **How the data was sourced and ingested**
You can find more details about the database’s table structure and the data dictionary within the original [Github Repository](https://github.com/secdatabase/SEC-XBRL-Financial-Statement-Dataset)
How large is the dataset: 

As of August 31, 2019, the dataset covers:

* All XBRL filings since 2009.

* 12.6k companies.

* 239.7k annual and quarterly reports.

* 192.5 million data points.

* 20GB+ compressed in Parquet.


The data sets have been optimized in both table structures and storage format to be used specifically in AWS big data ecosystem. SECDatabase.com evaluated multiple cloud platforms, and found that Athena, Redshift, and Glue really unleash the productivities. 

The detailed data table contents can be found here: https://github.com/secdatabase/SEC-XBRL-Financial-Statement-Dataset/blob/master/docs/Data_Dictionary.md

To access all the data we want, we use Amazon Athena Query to create tables and load to our S3 buckets. The process did not take long since each results by Athena would automatically stored to designated output location even it is a large file. 

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/AthenaQuery.png?raw=true)

Once we’ve got all the tables loaded to S3 buckets, we are ready to move on to the next step. 

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/Buckets.jpeg?raw=true)


### **How the dataset was cleaned and prepared for further analysis**

This dataset including all 7 tables is structured data and is already quite clean to do data analysis. After we have had all of the data tables stored in S3, we loaded each table into our environment using spark.read.csv command. Then, using spark.sql, we can query the data to explore the dataset further.
### **How the dataset was model -  The technique we used**
The SEC structured financial statements data set redistricted us to use consistent vocabulary of variable tags (DataPoint_Name) ['Asset', 'CashAndCashEquivalentsAtCarryingValue', 'CommonStockSharesAuthorized', 'LiabilitiesAndStockholdersEquity', 'NetIncomeLoss', 'RetainedEarningsAccumulatedDeficit', 'StockholdersEquity']

Firstly, we used the above 7 data point names to create features for all the ‘cik’ values which correspond to each company name the district under SEC financial data set. Combining all these features we tried to make 4 clusters which would give us companies showing similar behavior/ or having similar values for the above variable tags.
We can also see the centroids for each of these clusters. Finally we joined each of the ‘cik’ values to the corresponding company name from the company submission table to get a better picture well for clusters. 



DATA PROCESSING

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/ML1.png)

MODEL

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/ML2.png)


### **Did you just visualize the dataset, and if so, why?**

We created some basic visualization in the EDA part when exploring rankings of top companies, top locations by using bar charts to represent information, data and  makes it easier to identify patterns and trends than looking through thousands of rows on a dataframe.
Since the purpose of data analysis is to gain insights, data is much more valuable when it is visualized.




## **Results & Conclusion**
### **EDA**

By observing where most companies' business locations are located, we found that California holds the highest number. California has the famous Silicon Valley, for example, Facebook is in Menlo Park and Alphabet is in Mountain View – several are located in downtown San Francisco. These include McKesson, which is the highest-revenue company in the Bay Area, and Wells Fargo, one of the largest banks by total assets.

We have observed that the Balance sheet is the most essential financial statement, followed by the Statement of Cashflow. Moreover, we also find that the majority of public companies that report to the SEC end their fiscal year in December, followed by June. 

Besides that, having substantially more current assets  indicates that a business should be able to meet its short-term obligations and it would help the business to overcome some unexpected crisis such as the Covid 19. And with the information from this  dataset, we analyze the top 10 companies having highest assets calculated by the end of the year 2018 and provide the annual reports of these companies so anyone  who is interested can take a look and get general information about them.


### **Machine Learning**
We are successfully able to classify all the results into four clusters and we can see the predictions for the ‘cik’ and ‘company_name’ in our final results. The first 10 results show that all the values are belonging to the first cluster. 
As this is an unsupervised machine learning technique there is no scope of validation Apart from finding the best number of clusters for our dataset. 
Few more additional possible steps but added in ‘Suggestions and Future work’

RESULTS

![alt text](https://github.com/gwu-bigdata/project-group-12/blob/master/ref_images/ML3.png)





## **Challenges**


* Trouble creating DataFrames from S3

First trouble came in when we loaded all the files from S3 buckets. Our database contains 7 tables and each up to 20GB+. While we tried to load and create a dataframe to spark, our largest datatable, data_point.csv was having trouble loading the data frame. Luckily, we realized that data_point.csv has actually been divided into separate tables: data_point_snapshot.csv and data_point_revision.csv and they are more clean to do the analysis. Hence for the following analysis, we were using the data_point_snapshot.csv for the most of time. 


* Working with the clusters

Further wanting to work with each of the clusters and exploring their traits of different companies based on the features was not possible as filtering out the data for each cluster took a really long time.


* Elbow plot to get the right number of clusters:

Working with such a large data set with more than 100,000 records seems to have a challenging impact on machine learning especially when running multiple machine learning models to choose the best one. We came across this challenge when we tried to plot the elbow plot based on the Sum of Squares of data points where your machine learning code seems to run endlessly. 
 

* Difficulty to create visualization within the Spark environment:

We attempted to create multiple data visualizations under the Exploratory Data Analysis section, one for each question. However, we stumbled upon the `PyArrow is Missing` Warning Message below.

We have tried to install PyArrow using both pip install and conda forge but still failed. We have also tried to uninstall then reinstall PyArrow but failed. That’s why we have decided to show you only the tables of query results.

* The topic of this dataset

Since the dataset is about financial statements, it includes a lot of vocabulary that requires knowledge about Finance to understand thoroughly. Therefore, we had to learn about those concepts such as fiscal year and financial statements to be able to craft insightful business questions and a proposal for the ML model. 

* Inconsistent data format

In the column of numeric_value, there is a mix between full numbers and a mix of numbers with string characters which leads to the machine getting confused when executing the queries of ordering the values. Our solution is we add more conditions to the queries and apply ordering on other string_column that also contain the same value.


## **Suggestions & Future Work**

* We have a great scope in future work in regards to clustering, we can use clustering as still initial step in our EDA and further explore the traits of each cluster, we can gather a lot of information based on different ‘variable tags’ that is ‘data point name’ this can be really helpful to find companies based on similar tags, this is extremely difficult to be done manually when we have multiple features.
We can also do time series based clustering that is for every year we can make a set of clusters around that can give us good information about how various variable tags related to a company are changing over the years.

* The suggestion for future work to get more useful information, insights from this dataset is to read more financial documents to understand which fields contain useful information, which knowledge that most investor, finance analysts care about.  And based on those understanding, we will explore the dataset further and know exactly which methods are appropriate.

* In the future, when we have learned more about NLP technique, we can use text mining to mine the “footnotes” data, to learn more about the underlying issues that are common among many companies. The footnotes are important in understanding a company’s financial statements.  

* Auditors have access to vast amounts of data. They can use these to more effectively gather evidence to support their opinion. Machine-learning techniques are very powerful tools that the auditor can employ to reach his audit objectives. Even though these techniques have so far not been widely used as part of a financial statement audit, in other areas they have proven their added value. The barriers and roadblocks that keep auditors from their use can be overcome if needed.



## **Division of Labor**
1. Brainstorming ideas for project: all members
2. Writing up: all members
3. More specific coding-related or assignment execution:
* Loading and storing data in S3: Jennifer, Sally, Vibhu
* Loading data to the environment: Sally
* Exploratory Data Analysis: Jennifer, Laura, Sally
* Machine Learning: Vibhu



## **Take-aways from the course**
* Understand the importance of the Big Data. 
* Recognize “embarrassingly parallel” situations and avoid them whenever possible while taking into account the overhead created by parallel computing. 
* Understand the Map-Reduce concepts and how it can be applied while dealing with great amount of data. 
* Get to know some Big data technologies such as Hadoop and cloud-based analytics which has significant cost advantages when it comes to storing large amounts of data. 
* Getting familiar with Hadoop and PySpark, the all-in-one ecosystem which can handle the aggressive requirements with its MLlib, Structured data processing API, GraphX. 


## **References** 

Blog.Rinesi.com: [Applying machine learning to SEC filings to find anomalous companies](https://blog.rinesi.com/applying-machine-learning-to-sec-filings-to-find-anomalous-companies/)

PySpark ML: (https://spark.apache.org/docs/latest/ml-clustering.html)

Marck Vaisman: [Lab: Getting Started with Spark and PySpark](https://github.com/gwu-bigdata/lab-4-jennifernguyen281)
