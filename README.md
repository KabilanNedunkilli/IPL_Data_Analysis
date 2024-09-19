# IPL Data Analysis

![Dashboard Image](https://github.com/KabilanNedunkilli/IPL_Data_Analysis/assets/104073053/c9c4125b-4aae-4676-abbf-1d9ac16472a9)

## Overview
This project involves a comprehensive data analysis of IPL (Indian Premier League) matches from 2008 to 2022 using Power BI. The Power BI dashboard provides detailed insights into team performances, player statistics, match outcomes, and other key metrics. The analysis helps stakeholders and team management make data-driven decisions to evaluate player form, team strategies, and match outcomes.

## Features
- **Team Performance Analysis**: Breakdown of wins, losses, and points across seasons.
- **Player Insights**: Visual representation of player performance statistics, including runs, wickets, and strike rates.
- **Match Outcomes**: Analysis of match results based on venues, toss decisions, and team head-to-head comparisons.
- **Trend Analysis**: Graphical representation of trends over time, including season-wise performance.
- **Filter Options**: Interactive filters allowing users to focus on specific teams, players, seasons, or venues for in-depth analysis.

## Tech Stack
- **Tool**: Power BI
- **Data Source**: IPL matches dataset (2008-2022)
- **Data Transformation**: Power Query for cleaning and preprocessing raw data.
- **Visualizations**: Bar charts, line graphs, pie charts, and slicers for dynamic data exploration.

## Prerequisites
- **Power BI Desktop**: Ensure Power BI Desktop is installed to open and interact with the dashboard.
- **IPL Dataset**: The IPL dataset used for analysis is included in this repository.

## Setup Guide
1. Clone this repository:
   ```bash
   git clone https://github.com/KabilanNedunkilli/IPL_Data_Analysis.git
   cd IPL_Data_Analysis
   ```

2. Open the Power BI file (`IPL_Data_Analysis.pbix`) in Power BI Desktop.

3. Explore the interactive dashboard to gain insights into IPL matches, players, and team performance.

## Steps & DAX Measures

1. **Import Raw Data**: Import the IPL data from a local database or CSV file.
2. **Data Exploration**: Walk through the dataset to understand the structure and relationships.
3. **Data Cleaning**: Standardize values (e.g., replace "Bengaluru" with "Bangalore").
4. **Data Processing**: 
   - **Create Calendar Table**: 
     ```DAX
     Calendar Table = CALENDAR(MIN(ipl_matches_2008_2022[match_date]), MAX(ipl_matches_2008_2022[match_date]))
     ```
   - **Create Year Column**: 
     ```DAX
     Year = YEAR('Calendar Table'[Date])
     ```
   - **Establish Relationship**: Connect `match_date` from `ipl_matches_2008_2022` to the `date` column in the Calendar table.
5. **Design the Dashboard**:
   - Add a background image and title.
   - Include a slicer for selecting the season at the top.

6. **KPI Design**:

   - **Title Winner**:
     ```DAX
     Title Winner = 
     CALCULATE(
         SELECTEDVALUE(ipl_matches_2008_2022[winning_team]),
         ipl_matches_2008_2022[match_number] = "Final"
     )
     ```

   - **Economy**:
     ```DAX
     ECONOMY = 
     DIVIDE(
         SUMX(
             FILTER(
                 ipl_ball_by_ball_2008_2022,
                 ipl_ball_by_ball_2008_2022[extra_type] <> "legbyes" && 
                 ipl_ball_by_ball_2008_2022[extra_type] <> "byes"
             ),
             ipl_ball_by_ball_2008_2022[total_run]
         ),
         COUNT(ipl_ball_by_ball_2008_2022[overs]) / 6
     )
     ```

   - **Batting Strike Rate**:
     ```DAX
     Strike Rate for Batsman = 
     (SUM(ipl_ball_by_ball_2008_2022[batsman_run]) / 
     COUNT(ipl_ball_by_ball_2008_2022[ball_number])) * 100
     ```

   - **Batter Runs**:
     ```DAX
     Batter Runs = CONCATENATE(SUM(ipl_ball_by_ball_2008_2022[batsman_run]), " Runs")
     ```

   - **Bowler Wickets**:
     ```DAX
     Bowler Wickets = CONCATENATE(SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery]), " Wickets")
     ```

   - **Average**:
     ```DAX
     Average = 
     DIVIDE(
         SUMX(
             FILTER(
                 ipl_ball_by_ball_2008_2022,
                 ipl_ball_by_ball_2008_2022[extra_type] <> "legbyes" &&
                 ipl_ball_by_ball_2008_2022[extra_type] <> "byes"
             ),
             ipl_ball_by_ball_2008_2022[total_run]
         ),
         SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery])
     )
     ```

   - **Bowling Strike Rate**:
     ```DAX
     Bowling Strike Rate = 
     COUNT(ipl_ball_by_ball_2008_2022[bowler]) / 
     SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery])
     ```

   - **Matches Won Based on Toss Decision**:
     ```DAX
     Match Won or Loss Based Upon Toss = 
     CALCULATE(
         COUNTROWS(ipl_matches_2008_2022),
         ipl_matches_2008_2022[toss_winner] = ipl_matches_2008_2022[winning_team]
     )
     ```

7. **Add Images**: Include relevant images to visualize the data on respective cards for better understanding.

## Dashboard Overview
- **Home Page**: Provides an overview of the IPL season statistics, including total matches, teams, and top players.
- **Team Performance**: Compare team performances across multiple seasons with detailed match outcomes.
- **Player Statistics**: Analyze top performers in batting and bowling, with filters for specific players or teams.
- **Match Analysis**: Insights into match results based on toss decisions, home and away performance, and venue-specific outcomes.
- **Trends & Comparisons**: Interactive visualizations highlighting trends and key comparisons between seasons, players, and teams.

## Customization
- **Data Source Update**: You can update the data with new IPL seasons by refreshing the dataset and adjusting the visualizations accordingly.
- **Additional Metrics**: Users can add new KPIs, custom measures, or adjust existing visualizations as per their needs.

## Use Cases
- **Team Management**: Evaluate team performance and player form to make strategic decisions.
- **Fan Engagement**: Provide fans with insights into match outcomes, top performers, and team statistics.
- **Broadcast Analysis**: Leverage the data for pre-match or post-match analysis on sports shows or digital platforms.

- PFA Power BI Service Link: https://app.powerbi.com/reportEmbed?reportId=f17892d0-6dc8-44c4-8588-5d64cc61f6ff&autoAuth=true&ctid=2e16eeee-a116-4526-a9dd-8f93116f54c0

## Contributions
Contributions are welcome! Feel free to fork this repository, create issues, or submit pull requests to enhance the project.
