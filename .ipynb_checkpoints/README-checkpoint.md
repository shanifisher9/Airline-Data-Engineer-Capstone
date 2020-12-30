# Data Engineering Capstone
### Throughout the year there have been many challenges due to COVID-19 and many business/industries got affected. One industry was the transportation industry and I was curious to learn more about the industry and how things work. Therefore, I decided that I wanted to create a data warehouse of all information about flights and specifically during this time period. There wasn't a lot information that openly available but I found some.  Additionally, I though it would be interesting to lookg at the data of other methods of transportation and how they got got affected but I wasn't able to find that data in the time I had but hoping collect that information in the future. Once all of this data is together in a data warehouse it would be really easy to analyse and see how the transportation industry is doing and coping with these new changes.

### While I was working on this project I realized that there are many things I didn't know about the airline industry and it was really interesting to learn and explore it.
### The purpose of the data warehouse is to be able to build and help solve business questions. Some example of questions are: where are most airlines flying out of? Should a specific airline fly out of another airport? Where are most flights going? Why are most people traveling? Are people travelling to big populations or small populations? Using this data I'm hoping some of these questions could be answered and others as well.

### I created this data warehouse for analytical purposes so people could visualize the airline industry. Additionally, this data could be used as a source of truth so people can see what is going on in the industry. It was difficult finding all the data and in the future I would be interested to scrape data from different websites but that wasn't possible for me to do right now.


### Reason For Using Specific Tools

##### I chose to use AWS S3 and Redshift for this project because this data is big and needs cloud computing. It would take a very long time to run the data on a local machine and may even crash. The data is big but I am hoping to continue adding data to it and it made sense to start using Redshift and AWS then having to transfer to it later on. I used Redshift as the Data Warehouse because it is easily scalable and has the ability to adapt to more machines I didn't think it was necessary for me to create a Data Lake for this project because I wasn't using any raw data or adding random data to this project. I looked at many dataframes but didn't include any that didn't offer more information. I used pandas to analyse and clean the data and then python to run the scripts. I decided to store the data in S3 because it is easily accessable to people vs having the data on one local computer. It also made transferring the data to Redshift a more seamless process.

### Steps Processing Data:
1. Importing the data into Python using Pandas and looking at the structure of the data. Additionally, deleting extra columns, looking for duplicates, and missing values. Any data transformation that was necessary was done at this step. 
2. Mapping Out and Creating The Data Model. - Looking at the data and deciding how I want to structure it.
3. Writing the data to S3 as CSV files. - Once the data is clean I saved the data to S3 as CSV files.
4. Creating, Dropping already existing data tables.
5. Copying the data from S3 and importing it onto Redshift.
6. Inserting the data into the defined tables in Redshift.
7. Closing the Connection

### Updating Data
#### The data should be updated at least once a year. Airline data doesn't change that often and if one or two airlines or airports change it won't skew the data but at least every year the data should be updated in order to make sure the data never goes out of date. Fights 2020 should be updated by the end of the year ideally and by the end of 2021 I would like a complete list of the flights from 2021 from that data source. If I had included the prices or actual daily flights data then the data should be udpated at least once a day. I would love to incorporate that data eventually but for right now I would updated the data once a year.


## Data
1. airports_with_description = Dataset about airports. Data from [here](ourairports.com/data/)
2. airport_countries = The data is from the same webiste as perviouse data but has more information about the place of the airport. Data from [here](ourairports.com/data/)
3. airlines_and_countries = Data has information about airlines. Dataset from [here](https://github.com/npow/airline-codes/blob/master/airlines.json). The data can also be found [here](https://github.com/jpatokal/openflights/tree/master/data) and [here](https://openflights.org/data.html) but I used the first resource. They all get the data from Open Flights.
4. airports_past_flights = Data has information about airports and airlines of past flights. I got the data from [here](https://www.kaggle.com/flashgordon/usa-airport-dataset)
5. airline_codes = Data has more information about airline codes. Data from [here](https://www.census.gov/foreign-trade/reference/codes/aircarrier/acname.txt)
6. flights_2020 = Data has flights from 2020 and has airlines and airports which is important to connect the rest of the data together. Data from [here](https://www.transtats.bts.gov/DL_SelectFields.asp).

# Star Schema
### Steps In Order to Create Data Model

1. Drop existing tables if they exist
2. Create data tables for cleaned data and data model tables.
3. Upload the cleaned data to S3
4. Get data from S3 and move to redshift
5. Run Redshift cluster to create and insert data into my designated tables.
6. Run report and query the data to get the information needed

#### I used a star schema data model because I want to get ad-hoc queries and information from my data. I knew that this would be the best option for what I needed and was able to connect all of my data using this method. Then my data is readily available.

## Data Dictonary

### Fact Table: Airport
##### Airport: Airport Name
##### Region: Country And State the airport is located in ex: US-TX - United States, Texas
##### Country: Country the airport is located in ex: US - United States
##### Airport_IATA: A Three-letter Geocode defined by the International Air Transport Association ex: ORD - Chicago O'Hare International Airport

### Dimension Table: Airport_Flights_2020
##### Airline Primary KEY: Airline Name 
##### Airport_IATA_Destination:A Three-letter Geocode defined by the International Air Transport Association Of Where the flight is leaving
##### Airport_IATA_Arriving:A Three-letter Geocode defined by the International Air Transport Association Of Where the flight is arriving
##### Airline_IATA: IATA codes, are two-character codes assigned by the International Air Transport Association ex: AA - American Airlines
##### Distance: How far the flight is travelling
##### Passengers: How many passengers on the flight
##### Year: Year of the flight
##### Month: Month of the flight

### Dimension Table: Airlines
##### Airline_IATA Primary Key: IATA codes, are two-character codes assigned by the International Air Transport Association ex: AA - American Airlines
##### Airline: Name of Airline
##### ICAO: The International Civil Aviation Organization (ICAO) gave each airline a specific code to go by
##### Country: Country The Airline is From
##### Active: If the Airline is currently active

### Dimension Table: Past_Flights:
##### Airport_destination PRIMARY KEY: 3 Letter abbrevation(IATA) which airport the flight is leaving from
##### Airport_arriving:3 Letter abbrevation(IATA) which airport the flight is arriving to
##### Date: Date of the flight
##### Seats: Amount of seats on the flight
##### Distance: Distance of the flight

### Dimension Table: Airport_Description
##### Airport PRIMARY KEY: Airport Name
##### Airport_IATA: A Three-letter Geocode defined by the International Air Transport Association
##### Latitude:Latitude of the airport
##### Longitude: Longidute of the airport
##### City: City of the airport
##### Region: Country And State the airport is located in ex: US-TX - United States, Texas
##### Country: Country of the airport
##### Continent: Continent of the airport
##### Airport_type: What type of airport ex: Small, Large, Helipad
##### Elevation: What elevation is the airport
##### GPS_code: The GPS code of the airport

### Dimension Table: Countries
##### Country PRIMARY KEY: Country Name
##### Country_abbreviation: Country name abbreviated ex: US - United States
##### Continet: Continent of the country
##### Wikipedia_link: Wikipedia Link in case you want more information about the country

### Dimension Table: Population
##### Airport PRIMARY KEY: Three-letter Geocode defined by the International Air Transport Association
##### City: City of the population
##### Population: Amount of people in the city


## Future Work:

#### For the future of this project I would really love to incorportate Airflow to schedule tasks and grab data from my souces and get any new data. I have heard that AWS came out with a tool similar to Airflow and I would really love to try that and incorporate that with this project in the future. Additionally, I would want to scrape data off of websites to add in my data warehouse and would like to use Airlow to accomplish this. If I had a pipeline that would run everyday at 7am and I would want it to scrape data off of the websites and then import it into an S3 bucket which would then copy the data into Redshift and create tables. Airflow would also update and get new information about flights but might need to backfill because the data might be older than the date it runs. 
#### If the data was increased by 100x I still think I might be able to run it on my local computer but it might be too big. If that was the case and I don't want my computer to crash and I would use Spark to run as a virtual machine so it could clean my data and write it to S3. I might need another server on Redshift but that would depend on how fast I want the data to run. At all costs I am trying to avoid my computer crashing.
#### If the data needed to be accessed by 100+ people at one time I would keep the data in Redshift. Redshift could handle many people since it's cloud computing I might need to increase the number of my machine and would want to make a list of who's queries should run first. However, for the main purpose I would leave the data in Redshift.
##### On a side note I would also really like if I would be able to expand this data and not only have airport data but all transportation data. For example: train, bus, bike rental and car rentals. All of this data together could give people a really great understanding of all methods of transportation and how often and fast things run espeically during COVID-19. It would also be cool to get price data for airlines but also other methods of transportation because that data seems like there is so much to learn from it and would be really cool to incorporate it in this project. The trouble is finding that data but if I could find the data and make it readily available that would be ideal.
