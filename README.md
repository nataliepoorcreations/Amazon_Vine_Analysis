# Amazon_Vine_Analysis

## Overview
Since my work with Jennifer on the SellBy project was so successful, I’ve been tasked with another, larger project: analyzing Amazon reviews written by members of the paid Amazon Vine program. The Amazon Vine program is a service that allows manufacturers and publishers to receive reviews for their products. Companies like SellBy pay a small fee to Amazon and provide products to Amazon Vine members, who are then required to publish a review.

In this project, I have access to approximately 50 datasets. Each one contains reviews of a specific product, from clothing apparel to wireless products. I picked one of these datasets (pet products) and used PySpark to perform the ETL process to extract the dataset, transform the data, connect to an AWS RDS instance, and load the transformed data into pgAdmin. Next, I use PySpark, Pandas, and SQL to determine if there is any bias toward favorable reviews from Vine members in your dataset. Then, I wrote a summary of the analysis for Jennifer to submit to the SellBy stakeholders.

## Resources
* Data source: https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Pet_Products_v1_00.tsv.gz
* Software: Google Colaboratory, Python, PySpark, Spark, Amazon Web Services, PostgreSQL

## Deliverable 1: Perform ETL on Amazon Product Reviews
Using my knowledge of the cloud ETL process, I created an AWS RDS database with tables in pgAdmin, picked a dataset from the Amazon Review datasets, and extracted the dataset into a DataFrame. I then transformed the DataFrame into four separate DataFrames that match the table schema in pgAdmin. Finally, I uploaded the transformed data into the appropriate tables and ran queries in pgAdmin to confirm that the data has been uploaded.

* Reading the data into a DataFrame


<img width="887" alt="Read in review dataset" src="https://user-images.githubusercontent.com/106033535/193477091-c9869732-f154-4dda-803b-c1d7300a8440.png">



* Extracting each table data I wanted from the DataFrame 'df' and created new "table" DataFrames for customers, products, review id's, and vines. 

<img width="605" alt="customer_table" src="https://user-images.githubusercontent.com/106033535/193477273-7a5fa6ef-c059-46ba-b3f2-123f303c79c4.png">


<img width="601" alt="product_table" src="https://user-images.githubusercontent.com/106033535/193477284-1fcd08f6-fd39-44b5-907a-4b0b79b14dc5.png">


<img width="757" alt="review_id_table" src="https://user-images.githubusercontent.com/106033535/193477287-75ea70d0-dbf9-428f-9c30-e6ce726c5f2c.png">


<img width="803" alt="vine_table" src="https://user-images.githubusercontent.com/106033535/193477290-e7a64d6c-a7f6-4bb1-9bca-f12bbd1ee9dc.png">


* I then setup an RDS PostgreSQL Database on the Amazon Web Services and in the PySpark program listed the connection configuration to the database, set up a connection in pgAdmin so I could view the tables locally in my desktop, and created the 4 tables through the pgAdmin interface so I could load them from the PySpark program.


<img width="1084" alt="create customer table" src="https://user-images.githubusercontent.com/106033535/193477351-48a95680-7a80-423b-a6e8-8469237e8aab.png">


<img width="624" alt="create product table" src="https://user-images.githubusercontent.com/106033535/193477354-9e63d370-b247-4458-af50-e4f3c7e56a6a.png">


<img width="1205" alt="create review id table" src="https://user-images.githubusercontent.com/106033535/193477362-7cdc0c0d-dee1-4abb-a285-d510e4ca9547.png">


<img width="894" alt="create vine table" src="https://user-images.githubusercontent.com/106033535/193477370-c752fdd2-f6a8-4bec-832e-07c72a57867b.png">



## Deliverable 2: Determine Bias of Vine Reviews
Using my knowledge of PySpark, Pandas, and SQL, I determined if there is any bias towards reviews that were written as part of the Vine program. For this analysis, I determined if having a paid Vine review makes a difference in the percentage of 5-star reviews.

* I repeated the steps above to create the DataFrames using Google Colaboratory.
* I created a new DataFrame of the data, removing any row with null values.


<img width="1251" alt="clean df" src="https://user-images.githubusercontent.com/106033535/193477890-409a4418-5e61-4d06-a471-7cfe177300e5.png">


* From the clean_df, I created the vine_df DataFrame, with columns for review_id, star_rating, helpful_votes, total_votes, vine, verified_purchase.
* I then created a new DataFrame or table to retrieve all the rows where the total_votes count is equal to or greater than 20 to pick reviews that are more likely to be helpful and to avoid having division by zero errors later on.


<img width="670" alt="votes  = to 20" src="https://user-images.githubusercontent.com/106033535/193477909-0554a1dc-6e26-435a-9012-53c44a253ee2.png">


* I filtered the new DataFrame or table created in the previous step and created a new DataFrame or table to retrieve all the rows where the number of helpful_votes divided by total_votes is equal to or greater than 50%.


<img width="1312" alt="votes  = to 50" src="https://user-images.githubusercontent.com/106033535/193477918-589c3121-1656-4e54-af58-b1008919c268.png">


* I also filtered the DataFrame or table created in the previous step, and created a new DataFrame or table that retrieves all the rows where a review was written as part of the Vine program (paid) and (unpaid). 


<img width="1089" alt="percent votes" src="https://user-images.githubusercontent.com/106033535/193477932-e66523c7-d7c5-418b-b4b4-2948dd4a5afb.png">


* Finally I determined the total number of reviews, the number of 5-star reviews, and the percentage of 5-star reviews for the two types of review (paid vs unpaid).


## Results
<img width="516" alt="total reviews" src="https://user-images.githubusercontent.com/106033535/193477984-eed6f107-fdc3-45e9-9924-574745985719.png">

* How many Vine reviews and non-Vine reviews were there? 37,945
* How many Vine reviews were 5 stars? 65
* How many non-Vine reviews were 5 stars? 20,612
* What percentage of Vine reviews were 5 stars? 38.24%
* What percentage of non-Vine reviews were 5 stars? 54.5%

## Summary
The results show that even though there is a lower number of reviewers from the Amazon Vine program, 65, compared to 37,945, the percentage of 5 star reviews from non-Vine users were about 16% more than Vine users. This tells us that the paid reviewers did give out more 5 star reviews because they were being paid either in free product or money. 
