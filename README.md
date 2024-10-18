Hello I'am Mukhlis a passionate person in data and machine learning who professional in data analysis and experience in processing large datasets. Bachelor of Electrical Engineer with almost 2 years work experience in data. Have experience at PT. Telekomunikasi Selular Indonesia as Data Analyst and now work at PT. Optimasi Data Indonesia as Data Analyst too.

---
# **WELCOME TO MY PORFOTOLIO**

This section contains my portfolio showcasing my work experience and participation in the bootcamp at Purwadhika, where I developed skills and knowledge in the field of data..


## 1. Optimasi Data Indonesia - Data Analyst
### March 2024 - Present
**1. Football Player Statistic**
   
<p align="justify">
In this project, I am responsible for preparing a use case related to football player statistics for potential clients from one of the largest football clubs in Indonesia. Data is taken from the website <a href="https://www.sofascore.com">Sofascore</a> using the available API. I tried to find freely available APIs besides Sofascore, but it is quite difficult because most API service providers for football player statistics are subscription-based. Unfortunately, Sofascore only provides player statistics based on their overall performance, making it challenging to determine the development of players from each match played. The player data collected includes players from the top 5 European leagues (English Premier League, France, Italy, Spain, and Germany) and BRI Liga 1 Indonesia.
</p>

![Alt Text](/pic/football_flow_1.jpg)

Figure 1.1
<p align="justify"> The data extraction process from the web is executed using <strong>Apache Airflow</strong> as a data orchestration tool scheduled to pull data weekly through the API while considering the match schedules between countries from FIFA. The data collected is inserted into <strong>OpenSearch</strong> for visualization. </p>

Here is the Sofascore data retrieved via the available API:
![Alt Text](/pic/sofascore_1.png)
Figure 1.2

The visualization can be adjusted using a timestamp set to one week ago, ensuring that older data will not be visualized. Below is the dashboard created:

![Alt Text](/pic/football_1.jpg)
Figure 1.3
![Alt Text](/pic/football_2.jpg)
Figure 1.4
![Alt Text](/pic/football_3.jpg)
Figure 1.5

The three dashboard images above are interactive, allowing for filtering by club name or player name.
<br>
<br>
<br>
<br>
**2. Ministry Application**

<p align="justify">
In this project, I had the opportunity to be the PIC for transferring data from a ministry's database (PostgreSQL) into a dedicated application. This data contains regional budgets that internally need to be disseminated to local governments using the application to be developed. My company was chosen to take responsibility for the application's database with our product, the <a href="https://onyx.id/">Onyx Big Data Platform</a>. Onyx leverages many open-source applications; in this project, the applications used are <strong>Apache Hadoop</strong>, <strong>Apache Airflow</strong>, <strong>Apache Hive</strong>, <strong>Apache Zeppelin</strong>, <strong>Apache Spark</strong>, and <strong>OpenSearch</strong>.
</p>

![Alt Text](/pic/application_flow.jpg)

Figure 2.1

<p align="justify">
Apache Airflow serves as the orchestration tool to schedule the data flow from the ministry's database to <strong>Apache Hive</strong> and <strong>OpenSearch</strong>. Programming is done using <strong>Python</strong> while leveraging <strong>Apache Spark</strong> for heavy data transformations. The results in Apache Airflow are in the form of <strong>DAG (Directed Acyclic Graph)</strong>, with hundreds of DAGs created. Before the production process, many simulations were conducted in the notebook, particularly using <strong>Apache Zeppelin</strong>, before finally being transformed into DAGs ready for use. Below is the flow of the created DAG.
</p>

![Alt Text](/pic/dag.png)
Figure 2.2

<p align="justify">
Data entering Apache Hive is not processed because the incoming data must match the source data as original data. Meanwhile, data processing occurs if the data enters as OpenSearch Index, including <strong>data cleansing</strong> and <strong>data transforming</strong>.
</p>

Data cleansing and data transforming are two essential stages in the data processing process, but they have different focuses and goals.

Data cleansing, or data cleaning, is the process aimed at identifying and correcting errors in the data. This process includes various activities, such as:
1. **Error Identification**: Checking the data to find anomalies, such as duplications, missing values, or format mismatches. For example, if a dataset contains empty values or typos, the initial step is to identify these issues.
2. **Removing Duplicates**: Eliminating identical entries in the dataset to ensure that each record is unique.
3. **Data Correction**: Fixing errors in the data, such as changing inconsistent date formats or correcting spelling mistakes. This is crucial to ensure that the data can be used for accurate analysis.
4. **Data Validation**: Ensuring that the collected data meets certain criteria, such as reasonable value ranges or correct formats. For example, this might involve checking whether all email addresses are in the correct format.

Data transforming, or data transformation, is the process aimed at changing the data into a format that is more suitable for analysis or further processing. This process includes various activities, such as:
1. **Normalization**: Changing the scale of data so that it can be compared consistently. For example, if we have budget data in different currencies, we may need to convert everything into one currency.
2. **Aggregation**: Combining data from multiple sources or different levels of granularity to provide a more holistic view. For instance, summing total budgets per activity or sub-activity.
3. **Feature Creation**: Generating new variables from existing data to enhance the analysis or prediction models, such as flagging extremely poor regions.
4. **Changing Data Structure**: Modifying the data format, such as converting from a table format to JSON format (or vice versa), or changing data types (e.g., from string to integer) to meet the needs of the the application.

Application result:

![Alt Text](/pic/aplikasi_1.jpg)
Figure 2.3
![Alt Text](/pic/aplikasi_2.jpg)
Figure 2.4

<br>
<br>
<br>
<br>

---

## 2. Telkomsel Orbit - Data Analyst
### Juni 2023 - Desember 2023
<p align="justify">
Telkomsel is the first place I worked professionally, which is why I mostly helped my users in working on Telkomsel Orbit projects. As a data analyst, I primarily focused on compiling reporting dashboards on a weekly and monthly basis. Here, I utilized <strong>Microsoft Excel (especially the pivot feature)</strong>, <strong>Microsoft Power Point</strong>, <strong>Tableau</strong>, <strong>Jupyter Notebook</strong>, and <strong>SQL</strong>. In addition to assisting my users, I took on the responsibility as the only person providing data needs from colleagues in the Telkomsel Orbit division using HiveQL. The data needs from my colleagues varied, ranging from raw data to analytical results that informed decisions, especially in running campaigns related to the Telkomsel Orbit product.
</p>

Here are some dashboards that I was responsible for:

![Alt Text](/pic/telkomsel_1.jpg)
**The information numbers and text in the image above are not valid to maintain Telkomsel's confidentiality.**

![Alt Text](/pic/telkomsel_2.jpg)
**The information numbers and text in the image above are not valid to maintain Telkomsel's confidentiality.**

<br>
<br>
<br>
<br>
---
## 3. Purwadhika Digital Technology School - Bootcamp Data Science
### Mei 2022 - Oktober 2022
<p align="justify">
As a graduate in electrical engineering focused on low voltage, I possess a strong foundational concept of programming. To enhance my programming skills, I am interested in pursuing a career in data, which continuously utilizes programming knowledge, leading me to choose Purwadhika and enroll in the Data Science and Machine Learning class. Here, I learned about <strong>basic programming</strong>, <strong>statistics</strong>, and <strong>machine learning</strong>. Some projects from and after graduating from Purwadhika can be accessed here:
</p>

- [Crime in Boston](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Crime%20in%20Boston)
- [Hotel Book Cancellation](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Hotel%20Cancellation)
- [Olist Store](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Olist%20Store)
- [Saudi Arabia Used Car](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Saudi%20Arabia%20Used%20Car)

---
Connect me in [LinkedIn](www.linkedin.com/in/mmukhlis10) or touch my social media in [Twitter](https://twitter.com/bobyjhow).

Thank You
