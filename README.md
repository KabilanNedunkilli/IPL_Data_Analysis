**IPL_Data_Analysis**
   
<img width="605" alt="image" src="https://github.com/KabilanNedunkilli/IPL_Data_Analysis/assets/104073053/c9c4125b-4aae-4676-abbf-1d9ac16472a9">'

## Steps & DAX Measures.

 1. Import Raw data from CSV file.
 2. Walkthrough the data
 3. Data Cleaning ( Like - Bengaluru and Bangalore are Same , replace them as one)
 4. Data Processing
		i. Create a Calendar table - 
		
		Calendar Table = CALENDAR(min(ipl_matches_2008_2022[match_date]),max(ipl_matches_2008_2022[match_date]))
		
		Create a new Column called year - 
		
		Year = YEAR('Calendar Table'[Date])
		
		Connect match_date from ipl_mathes to the date column in Calendar table.
5. Add Background Image and Title.
6. KPI Design -  Add a Slicer for Selecting the Season at the Top
   ► Season Winner -
			
			Title Winner =
			VAR max_date =
			    CALCULATE (
			        MAX ( 'Calendar Table'[Date] ),
			        ALLSELECTED ( ipl_matches_2008_2022 ),
			        VALUES ( ipl_ball_by_ball_2008_2022 )
			    )
			VAR title_winner =
			    CALCULATE (
			        SELECTEDVALUE ( ipl_matches_2008_2022[winning_team] ),
			        'Calendar Table'[Date] = max_date
			    )
			RETURN
			    title_winner
			  
			Title Winner =
			CALCULATE (
			    SELECTEDVALUE ( ipl_matches_2008_2022[winning_team] ),
			    ipl_matches_2008_2022[match_number] = "Final"
			)
			 

   ► Economy :
		
			ECONOMY =
			DIVIDE (
			   SUMX (
			        FILTER (
			            ipl_ball_by_ball_2008_2022,
			            ipl_ball_by_ball_2008_2022[extra_type] <> "legbyes"
			                && ipl_ball_by_ball_2008_2022[extra_type] <> "byes"
			        ),
			        ipl_ball_by_ball_2008_2022[total_run]
			    ),
			    ( COUNT ( ipl_ball_by_ball_2008_2022[overs] ) ) / 6
			)
			
		

   ► Batting Strike Rate :
				Strike Rate for Batsman =
				(
				    SUM ( ipl_ball_by_ball_2008_2022[batsman_run] )
				        / COUNT ( ipl_ball_by_ball_2008_2022[ball_number] )
				) * 100
				
			

   ► Batter Runs = CONCATENATE( SUM(ipl_ball_by_ball_2008_2022[batsman_run]), " Runs")
			

   ► Bowler Wickets = CONCATENATE(SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery]), " Wickets")

   ► Average :
				
				Average = 
				DIVIDE (
				    SUMX (
				        FILTER (
				            ipl_ball_by_ball_2008_2022,
				            ipl_ball_by_ball_2008_2022[extra_type] <> "legbyes"
				                && ipl_ball_by_ball_2008_2022[extra_type] <> "byes"
				        ),
				        ipl_ball_by_ball_2008_2022[total_run]
				    ),
				    SUM(ipl_ball_by_ball_2008_2022[iswicket_delivery])
				)
				

   ► Bowling Strike Rate : 
					Bowling Strike Rate =
					COUNT ( ipl_ball_by_ball_2008_2022[bowler] )
					    / SUM ( ipl_ball_by_ball_2008_2022[iswicket_delivery] )
					
					

   ► Matches Won On Toss Decision
				Match Won or Loss Based Upon Toss =
				CALCULATE (
				    COUNTROWS ( ipl_matches_2008_2022 ),
				    ipl_matches_2008_2022[toss_winner] = ipl_matches_2008_2022[winning_team]
				)

7. Add the images to respectively Cards.	
