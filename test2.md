# Design

## World Bank ESG Data Visualization

### Project Overview
The design of this project has four major components: database, data load, flask application, and html templates. In process order, we needed to create our database and load the data we needed. Then we can create our application and html templates to operationalize the application. More detail on each component is given below.

### Database Design (project.db)
Our database includes 6 tables, by far the largest is the indicator_values table which contains the majority of our ESG data.

#### Table: countries
This table includes all of the country specific information from the World Bank country information API: http://api.worldbank.org/v2/country.

#### Table: indicators
This table includes all of our ESG indicator information from the World Bank ESG indicator API: https://api.worldbank.org/v2/source/75/indicators

#### Table: indicator_values
This table includes all of our ESG indicator datapoints (which are country/year specific). This table contains approximately 950K rows.

#### Table: users
This table includes all of our users and their hashed passwords.

#### Table: saved_charts
This table includes a given user's saved chart information including the indicator, minYear, and maxYear.

#### Table: saved_countries
This table includes a given user's saved chart information for country settings with both a user ID and chart ID.

### Loading the Data (api_load.py)
In our data loading script, we have 11 functions (including our main function).

#### Function: create_connection
When attempting to expand the volume of data I used for the project, I found that CS50's DB functions were not fast enough to load the data efficiently, so I created this function to use a faster connection. I believe the tradeoff here is more complexity, less error handling and no warnings that were baked into CS50's db.execute, but because our functions were relatively fixed, I accepted this tradeoff.

#### Function: insert_country
This function inserts a row of country information into our countries table.

#### Function: insert_indicator_values
This function inserts a row of indicator values into our indicator_values table.

#### Function: insert_indicators
This function inserts a row of indicator information into our indicators table.

#### Functions: truncate_countries, truncate_indicator_values, truncate_indicators
These functions truncate their respective tables upon data refresh.

#### Function: import_countries
This function pages through the countries API and inserts all of the received rows into our countries table.

#### Function: import_indicator_values
This function pages through the indicator values API for a given indicator and writes all of the received rows to our indicator_values table.

#### Function: import_indicator
This function pages through the ESG indicators api for all indicators and writes the rows to the indicators table.

#### Function: main
This function calls import_countries and import_indicators to get all macro information. It then calls import_indicator_values for all indicators in our DB.

### Flask Application & Helpers (application.py)

### HTML templates

#### chart.html & ChartJS
