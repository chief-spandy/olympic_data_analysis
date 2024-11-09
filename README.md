# Olympic Games Analysis
In this project, I gathered and transformed Olympic Games data in SQL Server and Power BI to create a visualization of countries' historical performances in the Summer Olympic Games. 

The dashboard enables users to explore performance by selecting specific countries, sports, or competitors.


![OlympicGames](https://user-images.githubusercontent.com/61323876/135508380-0844179b-07b7-449f-ab7f-9a801c93591a.jpg)

## Data Source
The data is collected from [this](https://www.dropbox.com/s/3sxwx52o3x8ozj7/olympic_games.bak?dl=0) database backup file restored into SQL.


## Data Collection
The necessary data was first put into a SQL database and afterwards transformed using the instructions that you can see below.

You can check the source SQL file [here](https://github.com/FilipeTheAnalyst/DataAnalystProject_SQL_PBI_OlympicGamesAnalysis/blob/main/DataAnalystProject_SQL_PBI_OlympicGamesAnalysis.sql)

__Olympic Games View__

```
SELECT
         [ID]
        ,[Name] AS 'Competitor Name' -- Renamed Column
        ,CASE WHEN SEX = 'M' THEN 'Male' ELSE 'Female' END AS Sex -- Better name for filters and visualizations
        ,[Age]
		,CASE	WHEN [Age] < 18 THEN 'Under 18'
				WHEN [Age] BETWEEN 18 AND 25 THEN '18-25'
				WHEN [Age] BETWEEN 25 AND 30 THEN '25-30'
				WHEN [Age] > 30 THEN 'Over 30'
		 END AS [Age Grouping]
        ,[Height]
        ,[Weight]
        ,[NOC] AS 'Nation Code' -- Explained abbreviation
 --       ,CHARINDEX(' ', Games)-1 AS 'Example 1'
 --       ,CHARINDEX(' ', REVERSE(Games))-1 AS 'Example 2'
        ,LEFT(Games, CHARINDEX(' ', Games) - 1) AS 'Year' -- Split column to isolate Year, based on space
--        ,RIGHT(Games,CHARINDEX(' ', REVERSE(Games))-1) AS 'Season' -- Split column to isolate Season, based on space
--        ,[Games]
--        ,[City] -- Commented out as it is not necessary for the analysis
        ,[Sport]
        ,[Event]
        ,CASE WHEN Medal = 'NA' THEN 'Not Registered' ELSE Medal END AS Medal -- Replaced NA with Not Registered
  FROM [olympic_games].[dbo].[athletes_event_results]
  WHERE RIGHT(Games,CHARINDEX(' ', REVERSE(Games))-1) = 'Summer' -- Where Clause to isolate Summer Season
```

## Calculations
The following calculations were created in the Power BI reports using DAX (Data Analysis Expressions):
```
# of Competitors = DISTINCTCOUNT('Olympic Games Data'[ID])

# of Medals = 
CALCULATE (
    COUNTROWS('Olympic Games Data'),
    FILTER (
        'Olympic Games Data',
        'Olympic Games Data'[Medal] = "Bronze"
            || 'Olympic Games Data'[Medal] = "Silver"
            || 'Olympic Games Data'[Medal] = "Gold"
    )
)
```

<img width="672" alt="Olympic Games Analysis" src="https://user-images.githubusercontent.com/61323876/135506469-d1b07b50-8660-480e-ac9f-810cfa01f8b9.png">

