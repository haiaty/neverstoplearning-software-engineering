When deciding whether to use SQL, R, or Python for an analysis, consider:

Where is the data located—in a database, a file, a website?

What is the volume of data?

Where is the data going—into a report, a visualization, a statistical analysis?

Will it need to be updated or refreshed with new data? How often?

What does your team or organization use, and how important is it to conform to existing standards?


## analysys workflow


Analysis work always starts with a question.

Once the question is framed, we consider where the data originated, where the data is stored, the analysis plan, and how the results will be presented to the audience.

explore, profile, clean, shape, and analyze the data. Exploring the data involves becoming familiar with the topic, where the data was generated, and the database tables in which it is stored. Profiling involves checking the unique values and distribution of records in the data set. Cleaning involves fixing incorrect or incomplete data, adding categorization and flags, and handling null values. Shaping is the process of arranging the data into the rows and columns needed in the result set. Finally, analyzing the data involves reviewing the output for trends, conclusions, and insights. This process is cyclical.

## Data workflow

First data is generated from ssome source. 

Then data is moved to a database for analysis. 

Once the data is in a database, the next step is performing queries and analysis.



### Data warehouse, data storage, data mart and data lake

data warehouse, which is a database that consolidates data from across an organization into a central repository, and data store, which refers to any type of data storage system that can be queried.
data mart, which is typically a subset of a data warehouse, or a more narrowly focused data warehouse; and data lake, a term that can mean either that data resides in a file storage system or that it is stored in a database but without the degree of data transformation that is common in data warehouses.


# ETL 
Usually a person or team is responsible for getting data into the data warehouse. This process is called ETL, or extract, transform, load. Extract pulls the data from the source system. Transform optionally changes the structure of the data, performs data quality cleaning, or aggregates the data. Load puts the data into the database. This process can also be called ELT, for extract, load, transform—the difference being that, rather than transformations being done before data is loaded, all the data is loaded and then transformations are performed, usually using SQL. You might also hear the terms source and target in the context of ETL. The source is where the data comes from, and the target is the destination, i.e., the database and the tables within it.

