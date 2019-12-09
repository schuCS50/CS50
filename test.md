# Implementation

## World Bank ESG Data Visualization

### Project Overview
This project is designed to allow the user to create charts to help visualize the different data sets included in the World Bank's 'Environment, Social and Governance (ESG) Data.'
https://datacatalog.worldbank.org/dataset/environment-social-and-governance-data

### Loading the Data
This first step of this project is to load the data from the World Bank Databank APIs into our database. We do this by running 'python api_load.py' which will run a file that will load all country information, indicator information, and indicator values into our database. This process will load roughly one million rows of information into the three respective tables (countries, indicators, indicator_values) in our database.

### Using the Application

