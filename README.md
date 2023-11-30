## Wikipedia World's Billionaires Web Scraping Analysis

Project Goal:
The goal of this project is to effectively scrape data from an online web source, transform the data using various data cleaning methods, and then conducting an analysis on the data by gaining insights from various visualizations.

In this project, the Wikipedia page called "The World's Billionaires" was used to collect data. Data from each of the tables from the years 1987-2023 was collected and then combined into one dataframe. This data was then cleaned, and multiple visualizations were created to gain insights about the dataset. This was conducted using Pandas on a Jupyter Notebook in Google Colab.

Data Extraction Code Documentation:
https://www.geeksforgeeks.org/scraping-wikipedia-table-with-pandas-using-read_html/

Data License:
The MIT License
Copyright 2023 Nithya Cheemalamarri

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (World_Billionaires.csv), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Data Source:
Wikipedia: The World's Billionaires
https://en.wikipedia.org/wiki/The_World%27s_Billionaires#

Data Description:
This data set contains information about the world's billionaires from the years 1987-2023. In the final dataset, there are 5 attributes: Name, Net Worth (USD, Billions), Age, Nationality, and Source(s) of Wealth.

Attribute 1: Name
- Data type: String
- This attribute provides the name of each of the billionaires who have made it onto the world's top billionaires list in any of the given years.

Attribute 2: Net Worth (USD, Billions)
- Data type: Float
- This column provides information about each billionaire's net worth in billions of dollars.

Attribute 3: Age
- Data type: Integer
- This column gives information about the age of the billionaire at the time they made the list of the world's top billionaires.

Attribute 4: Nationality
- Data type: String
- This column provides information about the country of the billionaire's nationality.

Attribute 5: Source(s) of Wealth
- Data type: String
- This column provides information about the company or companies that are the primary source of wealth for each billionaire.

Data Transformations:
The first step is to import the pandas library to extract data from Wikipedia and conduct data cleaning and create data visualizations for analysis.

In this analysis, information about the world's top billionaires between the years of 1987 and 2023 is collected from Wikipedia. This data contains information about the name of the billionaire, their ranking, net worth (USD), age, nationality, and source(s) of wealth. Data across the years of 1987-2023 was extracted from the tables in the Wikipedia URL: https://en.wikipedia.org/wiki/The_World%27s_Billionaires#. The Pandas library was used to extract and concatenate the data from the tables from each year into one dataframe.

First, the URL of the Wikipedia page was provided. Then, the Pandas read_html() function was used to store the information from each of the tables. Using a for loop, the stored information was parsed through, appending each table to a list called "dfs." The criteria for a table from each year was if the tables contained a column called "Name," for the name of the billionaire and a column called "Net Worth (USD)," for the net worth of the billionaire. This is necessary to ensure that the correct tables are scraped from the Wikipedia page.

Then, the information stored in the "dfs" list was stored in a dataframe called combined_df, which now stores the combined information from each of the appropriate tables.

The head() function in Pandas was used to display the first 5 rows of the dataframe to make sure the right information was collected and stored.

The dataframe called combined_df with the information from the tables in the Wikipedia page is converted to a CSV file called "World_Billionaires_Raw.csv" to store the raw, uncleaned data.

With this, the data extraction step is completed, allowing us to move onto the next step of cleaning and transforming the data.

During data extraction, excess columns such as "No. [47]," "No. [59]," "No. [60]," "No. [61]" were generated. Each of these columns contain Nan values which are not useful and clutter the dataset. To delete these columns, the location of the last relevant column, "Source(s) of wealth," is stored in a variable called last_relevant_column. The pandas get_loc() function is used for this. Then, all of the columns before and including the last relevant column are stored in a dataframe called final_data, which will be used for the rest of the data cleaning and transformation.

The first column, called "No." is also dropped from the dataset, as this was intially used to store the rank of the billionaire from each year. But for this purpose, the rank is irrelevant because information from all of the years on the Wikipedia page were combined. The pandas function .drop() was used for this. The final_data dataframe is updated after dropping this column.

In the original dataset, there is a naming discrepancy for the column that shows the companies that have provided the main source of wealth for each of the world's top billionaires. In the tables for 2022 and 2023, the column is named "Primary source(s) of wealth," and for the rest of the years, the column is named "Source(s) of wealth." Due to this difference, when the tables were combined, these columns were displayed as two separate columns, displaying NaN values in each of the columns.

To make this column uniform and to combine the data from each of these columns into one, the NaN values in the Source(s) of wealth column were filled in with the corresponding values that were in the Primary source(s) of wealth column using the fill.na() function.

Then, the Primary source(s) of wealth column was dropped using the .drop() function in Pandas.

To further clean the data, the rows containing duplicate names were dropped using the drop_duplicates() function as well as the remaining NaN values using the dropna() function on pandas. Since information about billionaires across a wide variety of years was combined, it is inevitable that some billionaires that have made the list for multiple years created duplicates in the final dataset.

Furthermore, in order to be able to create a numerical summary of the different ages of the world's top billionaires, it is essential that these values are of the int datatype. Some values in the "Age" column have the _ value, which will not be able to be converted to an int value. Just like filtering NaN values, the values with _ as an Age are filtered out. The final_data dataset is updated. Then, the remaining values are converted to an int datatype using the astype() function on pandas.

Similar to preparing the values in the "Age" column for a numerical summary, the values in the "Net worth (USD)" column also need to be converted to a numerical datatype, which in this case, is a float. In the original dataset, the values in this column have a dollar sign, the number, and the word "Billion." To convert these values to a float, the values are first converted to a string datatype, and then the values which are not a number or decimal are replaced with ''. This leaves the numbers in the column. Then, these numbers are converted to a float datatype.

Finally, the original dataset has both Wal-Mart and Walmart as source(s) of wealth. Since these are both the same company, is would be better to make these both the same world. To do this, the location of each of the companies named "Wal-Mart" are located in the dataframe. These are then replaced with "Walmart" so that all mentions of Walmart say "Walmart."

Additionally, some duplicate names were not dropped because they contained numbers after them, specifically for the billionaires for the source of wealth that is Walmart. This created discrepancies in the data. To adjust for this issue, the indices of the duplicates were collected in a list called indices_to_remove.

Then, using the pandas function drop() was used to remove all elements in the "Names" column at the indices in the list.

The final_data dataframe is displayed after all transformations are made.

The final_data dataframe is converted to a csv called "World_Billionaires.csv" using the to_csv() function.

Data Analysis:
Using the describe() function on the "Age" column in the final_data dataset, a numerical summary is generated of the different ages of the billionaires. From this numerical summary, a few insights can be gained. First, we can see that the average age of the world's top billionaires is about 67 years. The youngest billionaire who has been on the list was 35 years old, and the oldest billionaire was 94 years old. 25% of the billionaires were under 52 years old, and 75% of the billionaires were over 81%.

From this information, it is evident that a majority of the world's top billionaires are of old age, with half of the billionaires being about 70 years old.

Using the describe() function on the "Net Worth (USD)" column in the final_data dataset, a numerical summary is generated of the different net worth values in USD of the billionaires. From this numerical summary, a few insights can be gained. First, we can see that the average net worth of the world's top billionaires is about 52 billion dollars. 9.2 Billion dollars is the smallest net worth a billionaire has, and 211 Billion dollars is the largest net worth a billionaire has. 25% of the billionaires had a net worth under 19.8 Billion dollars, and 75% of billionaires had a net worth over 78.35 Billion dollars.

The first visualization is a histogram using the "Age" column in the final_data dataframe. This histogram shows the distribution of the different ages of the world's top billionaires. From this graph, ir can be seen that most of the world's top billionaires are between the ages of 70-80, with about 13 billionaires in this approximate range. It can be seen that there are less billionaires under 60 and a decreasing amount over 80 years of age.

The second visualization is a scatterplot with the "Age" column on the x-axis and "Net worth (USD)" column on the y-axis. This scatterplot demonstrates the relationship between the ages of the world's top billionaires and their net worth in billions of dollars. As seen from this plot, a clear correlation between age and net worth is not present. Most of the points are concentrated between approximately 12 Billion and 125 Billion dollars. This visualization also shows outliers, such as a 50-year-old billionaire with a net worth slightly exceeding 175 billion dollars and a 75-year-old billionaire with a net worth just above 200 billion dollars.

The third visualization is a histogram using the "Source(s) of wealth" column in the final_data dataframe. This histogram displays the top companies that provide the primary source of wealth to the world's top billionaires. It can be seen that Walmart and Microsoft are tied as the most commonly occurring sources of wealth, with 4 of the world's top billionaires in each company. The histogram demonstrates how significant Walmart and Microsoft are in terms of sources of wealth, as they are marginally more prevalent than the other companies.

The last visualization is a histogram using the "Nationality" column in the final_data dataframe. This histogram visualizes the various nationalities of the world's top billionaires. The most prevalent country of origin is the United States, with over 20 of the world's top billionaires being of this country's nationality. This is followed by France, with about 4 billionaires, which is tied with India and Hong Kong.

Issues in Data:
The original dataset contained information about the years in which the individual was identified as one of the world's billionaires. Additionally, it only contained the top n number of billionaires for each year as well as their rank in a given year. However, the processed dataset does not contain information about the year in which the billionaire was identified nor their rank in the given year. Duplicates were cleaned, thus, in the new dataset, it cannot be pinpointed as to whether or not an individual was identified across multiple years.
