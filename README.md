# California-Alcohol-Consumption-s-Influence-on-Marijuana-Usage
In this research, I used the difference-in-differences design to analyze how the alcohol consumption differences between California and the control states changed after the introduction of treatment. The treatment is the legalization of recreational marijuana in 2018.

Research topic: Effects of Recreational Marijuana Legalization on Alcohol Consumption in California

Data:
Source: National Institute of Alcohol Abuse and Alcoholism, Surveillance Report #120.
Time period: 2001 - 2022 (inclusive)
Key Data: Alcohol Consumption data separated by year and state

Data Set-Up:
After accessing the Surveillance Report #120, create an Excel file. The report is in PDF format, therefore the data must be manually entered onto the Excel file. 
1st sheet: Year column (2001-2022), California alcohol consumption column, Average consumption of control states column (copied from sheet 2)
2nd sheet: Year column (2001-2022), California alcohol consumption column, Consumption columns of all control states separately, Average consumption of control states column (summation of the all control states annual data divided by 18)

*Note: Individual state data can be found in each respective state's section on the report. 

Methodology:
	Difference-in-differences regression model:
	y ~ did | state + year

How to run:
1. Download the data file (Alcohol Consumption Data.xlsx) and the RMD file into the same directory
2. Open RMD
3. Install packages, which are # in comments in the first coding section
4. Select run all code
