# Project 1: Work From Home, Economic Impacts

# STEP 1: DATA PREP

The Covid-19 pandemic pushed many to work from home (WFH), but as the health crisis has receded, a question that employers and employees alike have faced is whether to return to in-person work or to maintain remote-work options. I seek to help inform this discussion by examining impacts of WFH using data available from the Bureau of Labor Statistics (BLS). In the current project I prepare the data. In a future project I will conduct the analysis.

Potential questions to explore:
* Who works from home? Differences in WFH patterns across industries and occuptions pre-pandemic (2014 to 2018) vs post-pandemic (2019 to 2023). 
* What is the productivity impact of WFH across industries and occupations? Do producivity gains, if any, reflect similar output for less hours worked or greater output for similar hours worked?
* Does remote translate into decreased costs of production (labor, rent, utilities)?
* How does the decrease in commute time associated with WFH translate to time spent in other areas of one's life? 

## Data: Series and Preparation 

* Programming language for all scripts: Python
* Development environment: Jupyter Notebook

Starter script for flattening of data, if needed, is available in the [Raw Data](https://github.com/brenprie/BLS-data-prep/tree/main/Raw%20Data) folder. 

### American Time Use Survey (ATUS)
The ATUS measures the amount of time people spend doing various activities, such as paid work, childcare, volunteering, and socializing.

1. Download flat data and series files from [https://download.bls.gov/pub/time.series/tu/](https://download.bls.gov/pub/time.series/tu/).
2. To reduce file size and processing time, focus on seriesid's of interest obtained from [https://data.bls.gov/PDQWeb/tu](https://data.bls.gov/PDQWeb/tu). Filters: 1 - gender: Both sexes, Men, Women. 2 -	age group: all 10-year age bins and 18+. 3 - labor force status: All persons. 4 - select parents: All persons. 5 - activities: Select activities (refer to pdf readme file which highlights those selected). 6 - type of days: All days (weekday/weekend combined). 7 - type of estimate: Ave hours per day. Click "Add to selection" and then "Get Data". Click "More Formatting Options". Copy/paste list of seriesid's in lower left of screen into txt file.
3. Create and run script that reads full series, select series, and data input files; strips whitespace from all headers and from seriesid's columns to facilitate merge; filters the series and data files by the select seriesid's; merges the two sets of filtered files; splits the series_title column into four columns based on its components (series, activity, age group, gender); and generates a single csv output file ready for data analysis. 

Readme, input (txt), script (ipynb), and output (csv) files: [Link](https://github.com/brenprie/BLS-data-prep/tree/main/Raw%20Data/American%20Time%20Use%20Survey). Note: version 1 files correspond to excel-based queries from the BLS website above; I later discovered the flat files, which contain more information and can be accessed more efficiently, and I thus pivoted to the use of flat files.

Script:
![Screenshot 2024-12-09 at 02 43 57](https://github.com/user-attachments/assets/e2252392-4353-4e5b-96bc-ab2dd08dac5d)

Note about APIs: I attempted API queries of the BLS, but encountered two challenges. First: exceeding 500-query limit per day per account. This was addressed with sleep timers, batched queries, and saving queries to cache; chunking data was an additional option to consider, but I dropped API analysis when it became clear that series titles, which would be key for subsequent analysis, were not included in the queries. Consult with ChatGPT suggested the missing variate may be in metadata. I later started to understand that all necessary inforamtion is contained in series files, data files, and dictionary files, something my API query was not capturing. In the end, I chose to skip APIs and work with downloads of the full files, which I preferred in any case to be able to examine content particulars.        

### Productivity
The Office of Productivity and Technology (OPT) measures how efficiently the U.S. converts inputs into the outputs of goods and services.  Measures of labor productivity compare the growth in output to the growth in hours worked and measures of total factor productivity (TFP), also known as multifactor productivity (MFP), compare growth in output to the growth in a combination of inputs that include labor, capital, energy, materials, and purchased services.

### Major Sector Quarterly Labor Productivity and Costs
1. Download relevant flat files (all series) from [https://download.bls.gov/pub/time.series/pr/](https://download.bls.gov/pub/time.series/pr/).
2. Create and run script to read and merge series and data files (after removing whitespace from headers and seriesid's columns) and save output to a single csv file. Series titles are not available in this set of series and data files; I obtained natural-English series identifiers by employing dictionaries that translate elements of the seriesid codes. In this implementation I chose to define the dictionaries in-script; in implementations below I have scripts read external dictionary files, which is a more efficient, robust, and flexible solution.  

Readme, input (txt), script (ipynb), and output (csv) files: [Link](https://github.com/brenprie/BLS-data-prep/tree/main/Raw%20Data/Major%20Sector%20Quarterly%20Labor%20Productivity%20and%20Costs).

Script:
![Screenshot 2024-12-09 at 03 41 09](https://github.com/user-attachments/assets/9f86494d-44f3-47ec-a296-cce989b0e8cf)


### Major Sector and Major Industry Total Factor Productivity
1. Download flat files (all series) from [https://download.bls.gov/pub/time.series/mp/](https://download.bls.gov/pub/time.series/mp/).
2. Create and run script to read and merge series and data files (after removing whitespace from headers and seriesid's columns) and save output to single csv file. Series titles are available in these files, but rather than split the series titles into its components, I split the series_ids and map to natural-English identifiers by calling on external dictionaries. In some cases dictionary codes begin with 0s that must be read to map correctly, so I force the script to read a specfied number of digits for each code.

Readme, input (txt), script (ipynb), and output (csv) files: [Link](https://github.com/brenprie/BLS-data-prep/tree/main/Raw%20Data/Major%20Sector%20and%20Major%20Industry%20Total%20Factor%20Productivity%20(Annual)).

Script:
![Screenshot 2024-12-09 at 04 07 02](https://github.com/user-attachments/assets/20b25146-e0ef-4987-9457-fb1e453a818f)


### Current Employment Statistics (CES)
The CES program produces detailed industry estimates of nonfarm employment, hours, and earnings of workers on payrolls...Each month, CES surveys approximately 119,000 businesses and government agencies, representing approximately 629,000 individual worksites.

1. Download flat files (all series) from [https://download.bls.gov/pub/time.series/ce/](https://download.bls.gov/pub/time.series/ce/).
2. Create and run script to read and merge series and data files, calling on several external dictionaries. In this case there multiple data files, which vary in length but some are quite large in size. To reduce file sizes and processing time, I dropped observations prior to 2014 (the CES data start in 2006) and I generated separate csv output files corresponding to each data input file; at the data analysis stage, file size can be reduced further by selecting specific variates of interest and merging the reduced datasets into one csv file for analysis and visualization. This approach allows one to examine the available metrics and investigate which offer more story-telling potential. Rather than create separate functions to process each data file, I employed a generalized function that allows for a far more compact script.

Readme, input (txt), script (ipynb), and output (csv) files: [Link](https://github.com/brenprie/BLS-data-prep/tree/main/Raw%20Data/Current%20Employment%20Statistics).

Script with prints showing variation in output file size by number of rows:
![Screenshot 2024-12-09 at 04 22 26](https://github.com/user-attachments/assets/71bbb328-1cbf-4ced-9d7c-63cc435ac937)


### Current Population Survey (CPS)
The CPS provides a wealth of information on the nation’s labor force. Key CPS measures are the unemployment rate, labor force participation rate, and employment-population ratio. The CPS can also provide insights into the impact of working from home, as it now includes questions about telework, allowing researchers to track the percentage of people working remotely and identify trends related to the practice across different demographics and industries, particularly since the pandemic significantly increased remote work rates.

1. Download flat files (all series) from [https://download.bls.gov/pub/time.series/le](https://download.bls.gov/pub/time.series/le).
2. Create and run script to read and merge series and data files, calling on numerous external dictionaries, and save output to single csv file. In this final implementation, I explored error handling to a greater degree.

Readme, input (txt), script (ipynb), and output (csv) files: [Link](https://github.com/brenprie/BLS-data-prep/tree/main/Raw%20Data/Current%20Population%20Survey).

Script:

![Screenshot 2024-12-09 at 05 07 35](https://github.com/user-attachments/assets/c4072c56-942f-4b7c-82ab-55e4a1155b90)

# STEP 2: ANALYSES

-- TO FOLLOW AT CONCLUSION OF COURSE -- 

# RESOURCES

* Series ID formats: https://www.bls.gov/help/hlpforma.htm
* ChatGPT for instruction on how to tackle tasks I've not encountered previously and for assistance debugging 
