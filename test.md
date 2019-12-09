# Implementation

## World Bank ESG Data Visualization

### Project Overview
This project is designed to allow the user to create charts to help visualize the different data sets included in the World Bank's 'Environment, Social and Governance (ESG) Data.'
The world bank dataset is broken down into 'indicators' which are individual data sets for given metrics. The ESG dataset contains 67 indicators.
https://datacatalog.worldbank.org/dataset/environment-social-and-governance-data

### Loading the Data & Starting the Application

#### api_load.py
This first step of this project is to load the data from the World Bank Databank APIs into our database. We do this by running 'python api_load.py' which will run a file that will load all country information, indicator information, and indicator values into our database. This process will load roughly one million rows of information into the three respective tables (countries, indicators, indicator_values) in our database.
*Note this process typically takes approximately 5-10 minutes

#### Starting the Application
We use Flask to run this application, so running this application will require using the 'flask run' command in the implementation directory.


### Using the Application

#### Registering & Logging In
Once the application is running, we will see the login page. If an account has not already been created, then we will need to 'Register' with a new username and password. Otherwise, the user can log into the application using their username and password.

#### Homepage
When we've logged in, we will be redirected to our homepage which contains a descrption of all of the indicators we have access to. We will also see the 'New Chart', 'Saved Charts', and 'Log Out' Options in the header bar.

#### New Chart
If we navigate to the 'New Chart' page, we will see a number of options for our new chart. On this page we can choose four components of our chart:
* Indicator: We will choose the indicator we want to chart here. This dropdown contains all indicators we have access to.
* Country: We see a list of countries (and macro-regions) to choose from. We can choose as many of these countries/regions to compare as we like.
* Min Year: This is the first year our chart will include.
* Max Year: This is the last year our chart will include.

If the user does not choose any of the above fields, they will see an error. They will also see an error if they chose a Min Year that is greater than the Max Year. They are limited in the input to the correct range of eyars.

Once the user fills out the required fields and selects "Chart!", they will be sent to a page that contains their chart image with their settings applied. On this page they have the option to save their chart as an image. Also, upon submitting the form, their chart is also saved to their history.

#### Saved Charts
If we navigate to the "Saved Charts" page, we will see all of the charts that the logged in user has created and what settings were used in that chart.

If they choose any of the given charts and select "re-Chart!" that chart will be rendered with the saved settings. As with the original chart display, they have the option to save it as an image.
