# CricketAnalytics

## Cricket Dashboards

# Cricket Stats Data Collection

This project collects cricket player statistics data from various websites and stores it in a structured format using Python and Pandas library. 

## Data Collection

The data collection process involves web scraping using Python libraries such as `requests` and `beautifulsoup4`. The following steps were taken to collect data:

1. Scraped the ICC team rankings page for each format (Test, ODI, T20I) to get the current rankings and create a country list based on this ranking. The code for collecting ODI team rankings is shown below:

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Create an empty dataframe to store the rankings data
test_ranking = pd.DataFrame(columns=['POS','TEAM','MATCHES','POINTS','RATING'])

# Send a GET request to the ICC team rankings page for ODI
url = 'https://www.icc-cricket.com/rankings/mens/team-rankings/odi'
res = requests.get(url)

# Parse the HTML content of the response using BeautifulSoup
soup = BeautifulSoup(res.content, 'html.parser')

# Find the table that contains the team rankings and extract its rows
table = soup.find_all('table')[0]
rows = table.find_all('tr')

# Loop through the rows of the table and extract the data
for row in rows[1:]:
    data = row.find_all('td')
    if len(data) > 0:
        pos = data[0].get_text().strip()
        team = data[1].get_text().strip()
        matches = data[2].get_text().strip()
        points = data[3].get_text().strip()
        rating = data[4].get_text().strip()

        # Create a dictionary to store the row data
        row_data = {'POS': pos, 'TEAM': team, 'MATCHES': matches, 'POINTS': points, 'RATING': rating}

        # Append the row data to the dataframe
        test_ranking = pd.concat([test_ranking, pd.DataFrame(row_data, index=[0])], ignore_index=True)

# Print the ODI team rankings
print(test_ranking)
```

2. Using the country list created from the rankings data, collected batting and bowling statistics for each player from their respective tables on stats.espncricinfo.com/ci. The following code snippet demonstrates the data collection process for ODI batting statistics:### Dashboard Creation

```
# Create a dictionary to map country IDs to country names
country_id_dict = {2: 'Australia',6: 'India',5: 'New Zealand',1: 'England',7: 'Pakistan',3: 'South Africa',25: 'Bangladesh',8: 'Sri Lanka',4: 'West Indies',40: 'Afghanistan',29: 'Ireland',30: 'Scotland',9: 'Zimbabwe', 15: 'Netherlands',32: 'Nepal',  37: 'Oman',28: 'Namibia',11: 'United States',27: 'United Arab Emirates', 20: 'Papua New Guinea'}

# Create a list of country IDs
country_ID_list = [1,2,3,4,5,6,7,8,9,20,25,27,28,29,30,32,37,40]

# Create an empty dataframe to store the batting statistics data
odi_bat = pd.DataFrame(columns=['Player','TimeSpan','Mat','Inns','NO','Runs','HS','Ave','BF','SR','100','50','0','4s','6s
```

The processed data is visualized using Tableau to create interactive dashboards that provide insights into player performance, team performance, and match statistics.

### Repository Contents

* `All Formats Batting.py` - Python code for collecting and processing cricket match data
* `All Formats Bowling.py` - Python code for collecting and processing cricket match data
* `dashboard.twbx` - Tableau workbook containing interactive dashboards
* `README.md` - This file providing an overview of the repository

### Usage

To use this repository, clone or download it to your local machine. Use `data_collection.py` to collect and process data. Open `dashboard.twbx` in Tableau to view the interactive dashboards.

### Credits

Data for this project was collected from [ESPNcricinfo](https://www.espncricinfo.com/).
