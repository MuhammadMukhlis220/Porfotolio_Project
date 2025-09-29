
# **WELCOME TO MY PORTFOLIO**

This section contains my portfolio showcasing my work experience and participation in the bootcamp at Purwadhika, where I developed skills and knowledge in the field of data.

__Disclaimer:__
<br>
__Not all of my professional experience is listed here (such as technical support tasks), as I’ve chosen to highlight the projects and responsibilities that are most meaningful and relevant to my interests in data and machine learning__.

---

Hi, I'am Mukhlis~

I'm a passionate Data enthusiast with a Bachelor's degree in Electrical Engineering and over 2 years of professional experience in the data field.

I specialize in processing large-scale datasets, building robust data pipelines, and transforming raw data into meaningful insights. I began my career as a Data Analyst at PT Telekomunikasi Selular Indonesia, where I supported strategic decision-making through data-driven analysis. Currently, I'm working as a Data Engineer at PT Optimasi Data Indonesia, focusing on data infrastructure, pipeline optimization, and scalable data solutions.

With a strong technical foundation and business-oriented mindset, I aim to build impactful solutions through data.

**My expertise across the data lifecycle:**

![Alt Text](/pic/stack-tools.png)

Figure: My tech stack


## 1. Optimasi Data Indonesia - Data Engineer
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

<p align="justify">
From the website's page, we can use the "inspect" feature. "Inspect" allows us to view and modify the HTML, CSS, and JavaScript used to build the webpage directly. This is very useful for web developers, users, and data analysts who want to understand the structure or inspect certain elements on the page. </p>

![Alt Text](/pic/football_api_1.jpg)
Figure 1.3

From the image above, Sofascore does not only provide a single API for one player. Here is one of the API contents provided by Sofascore related to player statistics.

![Alt Text](/pic/football_api_2.jpg)
Figure 1.4

<p align="justify">
Once the API is identified, I used Python programming on Apache Airflow to fetch the data through web scraping with a weekly interval. The number of tasks is determined by the number of selected leagues (6 leagues), and all APIs are fetched simultaneously to optimize time. </p>

![Alt Text](/pic/football_airflow_1.png)
Figure 1.5

For the complete Python programming code, refer to the following block.
<details>
   <summary>Click to view the complete Python code.</summary>

   ```python
from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from airflow.operators.python import ShortCircuitOperator
from airflow.utils.dates import days_ago
import requests
import re
import subprocess
import json
import pandas as pd
from datetime import datetime, timedelta
from opensearchpy.helpers import bulk
from opensearchpy import OpenSearch
import uuid

default_args = {
    'start_date': days_ago(1)}

dag = DAG(
    'ds_persibidx_sofascore',
    default_args=default_args,
    schedule_interval='@weekly',
    catchup=False,
    tags=['PoC', 'Dev']
)

def skip_during_exception_dates(**kwargs):

    exec_date = kwargs['execution_date'].date()

    exception_ranges = [
        (datetime(2024, 11, 11).date(), datetime(2024, 11, 19).date()),  # Fifa Match Day
        (datetime(2025, 3, 17).date(), datetime(2025, 3, 25).date()), # Fifa Match Day
        (datetime(2025, 6, 2).date(), datetime(2025, 6, 10).date())   # Fifa Match Day
    ]
        
    for start_exception, end_exception in exception_ranges:
        if start_exception <= exec_date <= end_exception:
            print(f"Execution date {exec_date} is within the exception range {start_exception} to {end_exception}. Skipping task")
            return False
    
    print(f"Execution date {exec_date} is outside the exception ranges. Lanjut tasknya maang...")
    return True

def curl_url(url):

    try:
        result = subprocess.run(['curl', '-kX', 'GET', url], capture_output=True, text=True)
        result.check_returncode()
        data = json.loads(result.stdout)

        if 'error' in data:
            print(f"Error in response: {data['error']['message']}")
            return None

        return data
    
    except subprocess.CalledProcessError as e:
        print(f"An error occurred while running curl: {e}")
        return None
    
    except json.JSONDecodeError as e:
        print(f"An error occurred while parsing JSON: {e}")
        return None

def create_statistics_dataframe(statistics_data, player_team):
    """Create DataFrame from statistics data."""
    player_team = {
        'teamName' : player_team
    }
    player_team = pd.DataFrame([player_team])
    stat = pd.DataFrame([statistics_data])
    return pd.concat([player_team, stat], axis=1)

def create_player_dataframe(player_data):
    """Create DataFrame from player data."""
    player_info = {
        'playerName': player_data.get('name', 'N/A'),
        'position': player_data.get('position', 'N/A'),
        'height': player_data.get('height', 0),
        'preferredFoot': player_data.get('preferredFoot', 'N/A'),
        'country': player_data.get('country', {}).get('name', 'N/A'),
        'dateOfBirthTimestamp': player_data.get('dateOfBirthTimestamp', 0),
        'dateOfBirth': datetime.utcfromtimestamp(player_data.get('dateOfBirthTimestamp', 0)).strftime('%Y-%m-%d')
    }
    return pd.DataFrame([player_info])

def process_league(league_name, league_code, league_country, clubs, codes, tournament, **kwargs):
    gameweek = kwargs['ti'].xcom_pull(key=f'{league_name}_value', task_ids='get_match_days')

    html_content = ""
    for club, code in zip(clubs, codes):
        url = f"https://www.sofascore.com/team/football/{club}/{code}#tab:squad"
        response = requests.get(url, verify=False)
        response.raise_for_status()
        html_content += response.text

    pattern = r'<a href="/player/[^/]+/(\d+)">'
    player_codes = re.findall(pattern, html_content)
    player_codes = list(set(map(int, player_codes)))  # Remove duplicates

    dataframes = []
    for player_id in player_codes:
        statistics_url = f'https://www.sofascore.com/api/v1/player/{player_id}/unique-tournament/{tournament}/season/{league_code}/statistics/overall'
        player_url = f'https://www.sofascore.com/api/v1/player/{player_id}'

        statistics_data = curl_url(statistics_url)
        player_data = curl_url(player_url)

        if statistics_data and player_data:
            statistics = statistics_data.get('statistics', {})
            player_team = statistics_data.get('team', {}).get('name', 'N/A')
            df_statistics = create_statistics_dataframe(statistics, player_team)

            player_info = player_data.get('player', {})
            df_player = create_player_dataframe(player_info)

            if not df_statistics.empty and not df_player.empty:
                df_combined = pd.concat([df_player, df_statistics], axis=1)
                dataframes.append(df_combined)

    if dataframes:
        final_df = pd.concat(dataframes, ignore_index=True)
        #final_df['gameWeek'] = gameweek
        final_df['leagueCountry'] = league_country
        final_df['league'] = '1'
        final_df['league_name'] = league_name
        final_df['season'] = '24/25'
    else:
        final_df = pd.DataFrame()
    
    final_df['execute_time'] = datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%S')

    #feature
    stat = ["rating", "goals","assists","goalsAssistsSum", "shotsOnTarget", "shotsOffTarget", "totalShots", "goalConversionPercentage",
            "penaltiesTaken", "penaltyGoals", "attemptPenaltyMiss", "penaltyConversion", "passToAssist", "accuratePasses", "inaccuratePasses",
            "totalPasses", "accuratePassesPercentage", "accurateFinalThirdPasses", "keyPasses", "accurateLongBalls", "totalLongBalls",
            "accurateLongBallsPercentage", "accurateCrosses", "totalCross", "accurateCrossesPercentage", "shotFromSetPiece", "clearances",
            "interceptions", "blockedShots", "errorLeadToShot", "ballRecovery", "totalDuelsWon", "duelLost", "totalDuelsWonPercentage",
            "aerialDuelsWon", "aerialLost", "aerialDuelsWonPercentage", "successfulDribbles", "successfulDribblesPercentage", "saves", "cleanSheet",
            "goalsConceded","goalKicks","fouls","wasFouled","offsides","yellowCards","redCards","directRedCards","yellowRedCards","appearances",
            "matchesStarted","substitutionsIn","substitutionsOut","minutesPlayed"]

    detail = ["playerName","teamName","position","height","preferredFoot","country","dateOfBirthTimestamp","dateOfBirth","leagueCountry","league","season"]

    # feature untuk masuk ke opensearch
    feature = detail + stat + ['execute_time']

    feature = [col for col in feature if col in final_df.columns]
    final_df = final_df[feature]


    #cleansing data
    final_df = final_df.applymap(lambda x: 0 if pd.isna(x) and isinstance(x, (int, float)) else 'na' if pd.isna(x) and isinstance(x, str) else x)
    
    print(final_df.columns)
    print(final_df[['playerName', 'leagueCountry', 'league', 'season']].head())
    
    league_country_lc = league_country.lower() 
    OPENSEARCH_HOST = "x.x.x.x"
    OPENSEARCH_PORT = "x"
    OPENSEARCH_INDEX = f"ds_persibidx_league_{league_country_lc}"
    OPENSEARCH_TYPE = "_doc"
    OPENSEARCH_URL = "https://x:x@x.x.x.x:x/"
    OPENSEARCH_CLUSTER = "ONYX-analytic"
    ONYX_OS = OpenSearch(
                         hosts = [{"host": OPENSEARCH_HOST, "port": OPENSEARCH_PORT}], http_auth = ("admin", "admin"),
                         use_ssl = True, verify_certs = False, ssl_assert_hostname = False, ssl_show_warn = False
                        )
    batch = 100000
    
    for x in range(0, len(final_df), batch):
        df = final_df.iloc[x:x+batch]
        hits = [{"_op_type": "index", "_index": OPENSEARCH_INDEX, "_id": str(uuid.uuid4()), "_score": 1, "_source": i} for i in df.to_dict("records")]
        resp, err = bulk(ONYX_OS, hits, index=OPENSEARCH_INDEX, max_retries=3)
        print(resp, err)
    
    
################################################ INPUT YOUR LEAGUE HERE ################################################
leagues = {
    'ligue_1': {
        'code': 61736,
        'clubs': [
            "olympique-lyonnais",
            "rc-lens",
            "olympique-de-marseille",
            "stade-de-reims",
            "stade-rennais",
            "paris-saint-germain",
            "as-monaco",
            "stade-brestois",
            "lille",
            "nice",
            "toulouse",
            "montpellier",
            "rc-strasbourg",
            "nantes",
            "le-havre",
            "auxerre",
            "saint-etienne",
            "angers"
        ],
        'club_codes': [
            "1649",
            "1648",
            "1641",
            "1682",
            "1658",
            "1644",
            "1653",
            "1715",
            "1643",
            "1661",
            "1681",
            "1642",
            "1659",
            "1647",
            "1662",
            "1646",
            "1678",
            "1684"
        ],
        'tournament': 34,
        'country': 'France',
        'name': 'Ligue 1'
    },
    'liga_1': {
        'code': 65049,
        'clubs': [
            'borneo-fc-samarinda',
            'persib-bandung',
            'bali-united-fc',
            'madura-united',
            'dewa-united-fc',
            'psis-semarang',
            'persis-solo',
            'persija-jakarta',
            'persik-kediri',
            'barito-putera',
            'psm-makassar',
            'persebaya-surabaya',
            'pss-sleman',
            'persita-tangerang',
            'arema-fc',
            'psbs-biak',
            'semen-padang',
            'malut-united'
        ],
        'club_codes': [
            "189945",
            "64289",
            "64299",
            "86578",
            "383891",
            "189471",
            "200020",
            "64295",
            "135866",
            "86542",
            "135864",
            "76319",
            "204733",
            "86576",
            "47465",
            "266861",
            "86066",
            "491968"
        ],
        'tournament': 1015,
        'country': 'Indonesia',
        'name': 'BRI Liga 1'
    },
    'premier_league': {
        'code': 61627,
        'clubs': [
            "liverpool",
            "manchester-city",
            "arsenal",
            "chelsea",
            "aston-villa",
            "brighton-and-hove-albion",
            "newcastle-united",
            "fulham",
            "tottenham-hotspur",
            "nottingham-forest",
            "brentford",
            "west-ham-united",
            "bournemouth",
            "manchester-united",
            "leicester-city",
            "everton",
            "ipswich-town",
            "crystal-palace",
            "southampton",
            "wolverhampton"
        ],
        'club_codes': [
            "44",
            "17",
            "42",
            "38",
            "40",
            "30",
            "39",
            "43",
            "33",
            "14",
            "50",
            "37",
            "60",
            "35",
            "31",
            "48",
            "32",
            "7",
            "45",
            "3"
        ],
        'tournament': 17,
        'country': 'England',
        'name': 'Premier League'
    },
    'bundesliga': {
        'code': 63516,
        'clubs': [
            "fc-bayern-munchen",
            "rb-leipzig",
            "eintracht-frankfurt",
            "sc-freiburg",
            "bayer-04-leverkusen",
            "1-fc-union-berlin",
            "borussia-dortmund",
            "vfb-stuttgart",
            "1-fc-heidenheim",
            "1-fsv-mainz-05",
            "sv-werder-bremen",
            "vfl-wolfsburg",
            "fc-augsburg",
            "borussia-mgladbach",
            "fc-st-pauli",
            "tsg-hoffenheim",
            "holstein-kiel",
            "vfl-bochum-1848"
        ],
        'club_codes': [
            "2672",
            "36360",
            "2674",
            "2538",
            "2681",
            "2547",
            "2673",
            "2677",
            "5885",
            "2556",
            "2534",
            "2524",
            "2600",
            "2527",
            "2526",
            "2569",
            "2573",
            "2542"
        ],
        'tournament': 35,
        'country': 'Germany',
        'name': 'Bundesliga'
    },
    'serie_a': {
        'code': 63515,
        'clubs': [
            "napoli",
            "inter",
            "juventus",
            "lazio",
            "udinese",
            "milan",
            "torino",
            "atalanta",
            "roma",
            "empoli",
            "fiorentina",
            "hellas-verona",
            "bologna",
            "como",
            "cagliari",
            "parma",
            "lecce",
            "genoa",
            "monza",
            "venezia"
        ],
        'club_codes': [
            "2714",
            "2697",
            "2687",
            "2699",
            "2695",
            "2692",
            "2696",
            "2686",
            "2702",
            "2705",
            "2693",
            "2701",
            "2685",
            "2704",
            "2719",
            "2690",
            "2689",
            "2713",
            "2729",
            "2688"
        ],
        'tournament': 23,
        'country': 'Italy',
        'name': 'Serie A'
    },
    'la_liga': {
        'code': 61643,
        'clubs': [
            "barcelona",
            "real-madrid",
            "atletico-madrid",
            "villarreal",
            "osasuna",
            "athletic-club",
            "mallorca",
            "rayo-vallecano",
            "celta-vigo",
            "real-betis",
            "girona-fc",
            "sevilla",
            "deportivo-alaves",
            "espanyol",
            "real-sociedad",
            "getafe",
            "leganes",
            "valencia",
            "real-valladolid",
            "las-palmas"
        ],
        'club_codes': [
            "2817",
            "2829",
            "2836",
            "2819",
            "2820",
            "2825",
            "2826",
            "2818",
            "2821",
            "2816",
            "24264",
            "2833",
            "2885",
            "2814",
            "2824",
            "2859",
            "2845",
            "2828",
            "2831",
            "6577"
        ],
        'tournament': 8,
        'country': 'Spain',
        'name': 'La Liga'
    }
}
    
t1 = ShortCircuitOperator(
    task_id='check_exceptions',
    python_callable=skip_during_exception_dates,
    provide_context=True,
    dag=dag
)

for league_names, details in leagues.items():
    task = PythonOperator(
        task_id=f'process_{league_names}',
        python_callable=process_league,
        op_kwargs={
            'league_name': details['name'],
            'league_code': details['code'],
            'clubs': details['clubs'],
            'codes': details['club_codes'],
            'tournament': details['tournament'],
            'league_country': details['country']
        },
        provide_context=True,
        dag=dag
    )

    t1 >> task

   ```
   </details>

Once the data is ingested into OpenSearch, I created a visualization dashboard to monitor player performance statistics. The visualization can be adjusted using a timestamp set to one week ago, ensuring that older data will not be visualized. Here is the dashboard created:

![Alt Text](/pic/football_1.jpg)
Figure 1.3
![Alt Text](/pic/football_2.jpg)
Figure 1.4
![Alt Text](/pic/football_3.jpg)
Figure 1.5

The three dashboard images above are interactive, allowing for filtering by club name or player name.

While using Power BI, it looks like:

![Alt Text](/pic/football_dashboard_1.png)
Figure 1.6
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

![Alt Text](/pic/dag_1.png)
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

**Advance Analytics: RFM and Machine Learning**

Perform regional government clustering using RFM and machine learning with K-Means model to identify regions that lack synergy with the government's mission in combating extreme poverty. 

RFM can be performed manually or automatically. Manual segmentation takes a lot of time and effort, which is why automating the business process is often preferred. One such approach is to use specialized tools for region segmentation. A CDP (Customer Data Platform) is used as a source to obtain data related to region behavior, making the segmentation process more accurate. RFM is suitable for region segmentation because it considers region behavior based on the following three aspects:

1. RECENCY (R): The time since the last program
2. FREQUENCY (F): The number of programs made
3. MONETARY VALUE (M): The total amount spent for all programs

Result:

![Alt Text](/pic/kementrian_ml_1.png)

Figure 2.5

We choose the model based on **recency** and **frequency** resulting in a total of 4 groups. The fourth rank of group should be considered because all regions in there are lack synergy with the government's mission.

<br>
<br>
<br>
<br>

---

## 2. Telkomsel Orbit - Data Analyst
### June 2023 - December 2023
<p align="justify">
Telkomsel is the first place I worked professionally, which is why I mostly helped my users in working on Telkomsel Orbit projects. As a data analyst, I primarily focused on compiling reporting dashboards on a weekly and monthly basis. Here, I utilized <strong>Microsoft Excel (especially the pivot feature)</strong>, <strong>Microsoft Power Point</strong>, <strong>Tableau</strong>, <strong>Jupyter Notebook</strong>, and <strong>SQL</strong>. In addition to assisting my users, I took on the responsibility as the only person providing data needs from colleagues in the Telkomsel Orbit division using HiveQL (This company using Hadoop environment). The data needs from my colleagues varied, ranging from raw data to analytical results that informed decisions, especially in running campaigns related to the Telkomsel Orbit product.
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
### May 2022 - October 2022
<p align="justify">
As a graduate in electrical engineering focused on low voltage, I possess a strong foundational concept of programming. To enhance my programming skills, I am interested in pursuing a career in data, which continuously utilizes programming knowledge, leading me to choose Purwadhika and enroll in the Data Science and Machine Learning class. Here, I learned about <strong>basic programming</strong>, <strong>statistics</strong>, and <strong>machine learning</strong>. Some projects from and after graduating from Purwadhika can be accessed here:
</p>

- [Crime in Boston](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Crime%20in%20Boston)
- [Hotel Book Cancellation](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Hotel%20Cancellation)
- [Olist Store](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Olist%20Store)
- [Saudi Arabia Used Car](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Saudi%20Arabia%20Used%20Car)

---
Connect me in [LinkedIn](www.linkedin.com/in/mmukhlis10) or touch my social media in [Twitter](https://twitter.com/bobyjhow).
Thank You!

## Misc
In addition to my core job, I’ve worked on several mini-projects involving data processing technologies from the Apache Foundation, including:

- Apache Impala
- Apache Kudu
- Apache Superset
- Apache Sqoop
- Apache Spark (with MLlib for NLP tasks such as Named Entity Recognition and Sentiment Analysis)
- ETC

These projects reflect my passion for exploring big data ecosystems and applying machine learning techniques to real-world problems.
All of my projects are available in my GitHub repositories — feel free to explore them!
