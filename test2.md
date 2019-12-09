# Design

## World Bank ESG Data Visualization

### Project Overview
The design of this project has four major components: database, data load, flask application, and html templates. In process order, we needed to create our database and load the data we needed. Then we can create our application and html templates to operationalize the application. More detail on each component is given below.

### General Design Thoughts:
#### Design Overview
Our application has the below design
* Load data into database with Python script
* Database that allows us to store ESG datapoint and user data
* Flask application that allows a user to register, log in, and logout
* Application also allows a logged in user to create a chart with settings or rebuild account saved charts

#### DB vs API
Our general design is making the ESG data available in our database so our flask application can quickly access it. Another option was directly pulling from the API each time we needed to render a chart, but it seemed more extensible to load the data into our DB once and then pull from the DB on demand.

#### Line Charts
We started with line charts to display data with the chartJS library.

#### Saving & Image Downloading
We save the imput information of a chart to our user's account for chart recreation. We also allow them to download a png to their computer.

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
*Reused from CS50 Finance

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

#### Helpers: Apology & Login Required
The helpers file adds two functions reused from CS50 Finance that render an apology to the user and allow us to require a login to view specific areas of our application.

#### Application Function: login
This function logs a user into our application and updates the session with their information. It checks our users table to see if the username and hashed password match and then logs the user in and redirects them to our index if so. If not, it shows an appropriate error.
*Reused from CS50 Finance

#### Application Function: register
This function allows our user to register for our application. If they don't register correctly it shows them an appropriate error. If they register correctly their information is saved in our users table and they are redirected to index/login.
*Reused from CS50 Finance

#### Application Function: logout
This function clears a user's session and logs them out.
*Reused from CS50 Finance

#### Application Function: index
This function renders our index.html homepage which displays all of the available indicators to our user.

#### Application Function: chart
##### GET
If reached by GET, our chart function renders the chart_form.html page to our user to collect their requested chart settings.

##### POST
If reached by POST (when our user submits the chart form), this function validates that the user input appropriate chart information (indicators, country(ies), minYear, and maxYear), and renders an error if they did not. If they did, then it pulls the relevant (settings indicated) information from our database to render a chart. It then formats and passes than information to chart.html for display to the user.

#### Application Function: savedChart
##### GET
If reached by GET, our savedChart function renders the savedChart.html page with the users past saved chart information.

##### POST
If reached by POST (when the user selects a chart for recreation), this function validates that the form was correctly submitted (selects 1 prior chart), and then pulls the relevant information from the DB and formats & passes it to chart.html for display to the user.

### HTML templates

#### layout.html
This file is the layout base for our othe html files in the project. We deine our broader css files for the project as well as the navbar.
*Reused from CS50 Finance

#### login.html
This file extends layout.html and includes a form for the user to log in to our application.
*Reused from CS50 Finance

#### register.html
This file extends layout.html and includes a form for the user to register for our application.
*Reused from CS50 Finance

#### apology.html
This file extends layout.html and renders our errors.
*Reused from CS50 Finance

#### index.html
This file extends layout.html and is the first page the user sees when they log in. It renders all indicators that we have access to in conjunction with our index function.

#### chart_form.html
This file extends layout.html and renders a form of chart settings for our users to submit. It is rednered with the chart get function and with the information passed through there will render a dropdown of all indicators, a list of all countries, and the min/max year option.

#### savedChart.html
This file extends layout.html and renders a list of all previous chart settings the user has used.

#### chart.html & ChartJS
This file extends layout.html and renders the chart based on the POST method of both the savedChart and chart function. It uses chartJS (more detail below) to rener a chart to the user with the data returned from the chart settings they selected. It also offers the option to download the chart rendered in the canvas element.

##### ChartJS
Chart JS is an open source library that enables the creation of JS + html charts on webpages. We use the line chart options for our chart display.
https://www.chartjs.org/
