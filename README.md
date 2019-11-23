ETLing the FEC
by Ron, Bowen, Lakre, Kassaye, John, and David


Introduction: 

For our project, we decided to Extract, Transform and Load candidate, committee, individual contribution and independent expenditure data from the Federal Election Committee’s website. While deciding on our topic, we considered many different topics that we could focus on; ranging from a dataset on Jeopardy questions to a dataset of Russian energy futures. There were a lot of things that were interesting on sites like Kaggle, but we decided on something that was both practical and that had a healthy data supply for us to work with.  


Extract-Transform-Load: 

We used two main ways to extract our data.   For most tables, we directly downloaded Zip files from the FEC website. We extracted the txt files (|-delimited) from the Zip and grabbed the separate header file from the website.  

For each of the tables we used the following method to combine the txt file with the header file and upload the data into the postgresql database:

INDEP_EXPEND:  We utilized pandas (independent.ipynb) to import the txt files and assigned the column names to the dataframe.  Added index to the dataframe.   Changed data-type of date column to a date-time format.   Then exported to a new, cleaned CSV ready to import to postgresql.  David used the Jupyter Notebook to perform both the extraction and transformation of the data.  We then created the schema and uploaded the csv to postgresql.

cand_master, com_master: Lakre, Kassaye and Bowen manually added the header file (which was in Excel format) to the data in the txt, and saved it as a CSV.  We then created the schema and uploaded the csv to postgresql.  The cand_master and com_master data each had duplicate records, which prevented the addition of keys.   We identified the duplicate records, destroyed them, and re-inserted one.

ccl: Bowen downloaded the candidate-committee linkages file as a txt file.   He wrote sql code to create the schema, and uploaded the data into postgresql directly from the text file.

Individual_cont: Ron created a jupyter notebook (individual_contributions.ipynb) to extract the zip file into memory and add the txt file to a dataframe, assigning the header names in the jupyter notebook.   One of the records contained text in the date-column, so we removed that record and set all the columns to their appropriate data types.   We then used sqlalchemy to create the class to set the metadata for the table, including the primary key, and uploaded the dataframe to postgresql.   John found a faster alternative (rec_rons_incon.ipynb) to this method by extracting the dataframe to a CSV and using psycopg2 to send a command line sql function to upload the data from the csv to postgresql.


Primary Keys:  We concluded by altering the tables that did not already have primary keys to add the primary keys.

Foreign Keys:   As we attempted to add foreign keys to link tables to the candidate master, we discovered that there are many missing candidate ids from the candidate master, and the constraint rendered it impossible to create a foreign key.   For now, we have decided not to employ foreign keys, subject to future exploration on the reason for the discrepancy.

SQL Code that we used for creation of the schema, finding duplicates in cand_master and com_master, removing the duplicate records, and adding primary keys to the schema can be found in the document master_schema.sql.
