# Project 1: Work From Home, Productivity and Work-Life Balance Impacts -- STEP 1: Data Prep

The Covid-19 pandemic pushed many to work from home (WFH), but as the health crisis has receded, a question that employers and employees alike have faced is whether to return to in-person work or to maintain remote-work options. I seek to help inform this discussion by examining impacts of WFH using data available from the Bureau of Labor Statistics (BLS). In the current project I prepare the data. In a future project I will conduct the analysis.

Potential questions to explore:
* Who works from home? Differences across industries and occuptions pre-pandemic (2014 to 2018) vs post-pandemic (2019 to 2023). 
* How do changes in commute time pre- vs. post-pandemic translate to time spent in other areas of one's life?
* What is the productivity impact of WFH across different industries and occupations? Do producivity gains, if any, reflect similar output for less hours worked or greater output for similar hours worked?
* What is the impact of remote work employers’ costs of labor, rent, and utilities? 

## Data: Series and Preparation

### Readme, input (txt), script (ipynb), and output (csv) files by BLS data set: 
* [American Time Use Survey folder on repo](https://github.com/brenprie/Project-1-Work-From-Home/tree/brenprie/Raw%20Data/American%20Time%20Use%20Survey). Note: "Version 1" files are based on Excel-based queries from the BLS user interface; I pivoted to use of BLS flat files, which contain all data, as the main implementation.
* [Major Sector Quarterly Labor Productivity And Costs folder on repo](https://github.com/brenprie/Project-1-Work-From-Home/tree/main/Raw%20Data/Major%20Sector%20Quarterly%20Labor%20Productivity%20and%20Costs). Note: for the moment, dictionaries included for sake of reference only.
* [Major Sector and Major Industry Total Factor Productivity folder on repo](https://github.com/brenprie/Project-1-Work-From-Home/tree/main/Raw%20Data/Major%20Sector%20and%20Major%20Industry%20Total%20Factor%20Productivity%20(Annual))
* [Current Population Survey folder on repo](https://github.com/brenprie/Project-1-Work-From-Home/tree/main/Raw%20Data/Current%20Population%20Survey)
* [Current Employment Statistics folder on repo](https://github.com/brenprie/Project-1-Work-From-Home/tree/main/Raw%20Data/Current%20Employment%20Statistics)
* Starter script for flattening of data, if needed, is available in the [Raw Data](https://github.com/brenprie/Project-1-Work-From-Home/tree/main/Raw%20Data) folder. 

### American Time Use Survey (ATUS)
The ATUS measures the amount of time people spend doing various activities, such as paid work, childcare, volunteering, and socializing.

1. Download flat data and series files from [https://download.bls.gov/pub/time.series/tu/](https://download.bls.gov/pub/time.series/tu/). Data file contains series title.
2. Obtain seriesid's of interest from [https://data.bls.gov/PDQWeb/tu](https://data.bls.gov/PDQWeb/tu). Copy/paste into txt file.
    1 -	gender:	Both sexes, Men, Women. 2 -	age group: all 10-year age bins and 18+. 3 - labor force status: All persons. 4 - select parents: All persons. 5 - activities: Select activities. 6 -	type of days: All days (weekday/weekend combined). 7 - type of estimate: Ave hours per day.
3. With assistance of ChatGPT, create and run script that reads files, spits series title into separate components, merges data, and generates a single csv file ready for data analysis. 

### Productivity
The Office of Productivity and Technology (OPT) measures how efficiently the U.S. converts inputs into the outputs of goods and services.  Measures of labor productivity compare the growth in output to the growth in hours worked and measures of total factor productivity (TFP), also known as multifactor productivity (MFP), compare growth in output to the growth in a combination of inputs that include labor, capital, energy, materials, and purchased services.

#### Major Sector Quarterly Labor Productivity and Costs
1. Download relevant flat files (all series) from [https://download.bls.gov/pub/time.series/pr/](https://download.bls.gov/pub/time.series/pr/).
2. With assistance of ChatGPT, create and run script to read and merge series and data files and save output to single csv file. Series titles are not available in these files, so I obtain natural-English series identifiers by employing dictionaries that translate elements of the seriesid codes. In this implementation I chose to define the dictionaries in-script; in implementations below I have script read external dictionary files, which is a more efficient, robust, and flexible solution (learning curve).  

#### Major Sector and Major Industry Total Factor Productivity
1. Download flat files (all series) from [https://download.bls.gov/pub/time.series/mp/](https://download.bls.gov/pub/time.series/mp/).
2. With assistance of ChatGPT, create and run script to read and merge series and data files and save output to single csv file. Series titles are available in these files, but rather than split the series titles into elements, I split series_ids into elements and map to natural-English identifiers by reference to external dictionaries.  

### Current Employment Statistics (CES)
The CES program produces detailed industry estimates of nonfarm employment, hours, and earnings of workers on payrolls...Each month, CES surveys approximately 119,000 businesses and government agencies, representing approximately 629,000 individual worksites.

1. Download flat files (all series) from [https://download.bls.gov/pub/time.series/ce/](https://download.bls.gov/pub/time.series/ce/).
2. With assistance of ChatGPT, create and run script to read and merge series and data files, calling on several external dictionaries. In this case there multiple data files, which vary in length but some are quite large in size. To reduce file size and processing time, I dropped observations prior to 2014 and I generated separate csv output files corresponding to each data input file; those who analyze the csv data can significnatly reduce file size further by selecting specific variates of interest and merging the reduced datasets into one csv file for analysis and visualization. This approach gives greatest opportunity to examine available series and investigate which offer more story-telling potential. Rather than create separate functions to process each data file, I employed a generalized function that allows for a far more compact script.

### Current Population Survey (CPS)
The CPS provides a wealth of information on the nation’s labor force. Key CPS measures are the unemployment rate, labor force participation rate, and employment-population ratio. The CPS can also provide insights into the impact of working from home, as it now includes questions about telework, allowing researchers to track the percentage of people working remotely and identify trends related to the practice across different demographics and industries, particularly since the pandemic significantly increased remote work rates.

1. Download flat files (all series) from [https://download.bls.gov/pub/time.series/le](https://download.bls.gov/pub/time.series/le).
2. With assistance of ChatGPT, create and run script to read and merge series and data files, calling on numerous external dictionaries, and save output to single csv file. In this final implementation, I explored error handling to greater degree.

### Sample Script: Script for CES with prints showing variation in output file size by number of rows:

![Screenshot 2024-11-29 at 15 17 53](https://github.com/user-attachments/assets/158eb5ab-8b51-4e03-8d2b-e841c65ab9a3)


## Resources
* Series ID formats: https://www.bls.gov/help/hlpforma.htm
* ATUS footnotes: https://www.bls.gov/tus/footnote.htm
