# Netflix movies and TV Shows Data Analysis using SQL

![NETFLIX LOGO] (https://github.com/Madhuri244/Netflix_SQL_Project/blob/main/NETFLIX.jpg)

##Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

##Objectives
Analyze the distribution of content types (movies vs TV shows).
Identify the most common ratings for movies and TV shows.
List and analyze content based on release years, countries, and durations.
Explore and categorize content based on specific criteria and keywords.

##Dataset
The data for this project is sourced from the Kaggle dataset:
 - **Dataset Link: ** [Movies Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows/data)

##Schema
'''sql
  DROP TABLE IF EXISTS netflix;
  CREATE TABLE netflix
  (
      show_id      VARCHAR(5),
      type         VARCHAR(10),
      title        VARCHAR(250),
      director     VARCHAR(550),
      country      VARCHAR(550),
      date_added   VARCHAR(55),
      release_year INT,
      rating       VARCHAR(15),
      duration     VARCHAR(15),
      listed_in    VARCHAR(250),
  );
  '''

 ##Business Problems and Solutions

 ### 1. Count the Number of Movies vs TV Shows
    
   '''sql
   SELECT TYPE, count(*) AS TOTAL_COUNT from NETFLIX GROUP BY TYPE;
     '''

### 2. Find the Most Common Rating for Movies and TV Shows

'''sql
     SELECT TYPE, RATING FROM
    (
    SELECT TYPE, RATING,COUNT(*),
    RANK() OVER (PARTITION BY TYPE ORDER BY COUNT(*) DESC)  AS RANKING
    FROM NETFLIX
    GROUP BY 1,2
    ) AS T1 
    WHERE
    RANKING =1;
    '''

### 3.List All Movies Released in a Specific Year (e.g., 2020)

   '''sql
   SELECT * 
    FROM netflix
    WHERE release_year = 2020;
    '''

### 4. Find the Top 5 Countries with the Most Content on Netflix

   '''sql
    SELECT 
    UNNEST(STRING_TO_ARRAY(COUNTRY,',')) AS NEW_COUNTRY,
    COUNT(SHOW_ID) AS TOTAL_CONTENT
    FROM NETFLIX
    GROUP BY 1
    ORDER BY 2 DESC 
    LIMIT 5;
    '''

### 5. Identify the Longest Movie

   '''sql
   SELECT * FROM NETFLIX 
    WHERE 
    TYPE='MOVIE'
    AND 
    DURATION =(SELECT MAX(DURATION) FROM NETFLIX);
    '''

### 6. Find Content Added in the Last 5 Years
    
   '''sql
    SELECT * FROM NETFLIX 
    WHERE TO_DATE(DATE_ADDED, ' MONTH DD,YYYY') > CURRENT_DATE - INTERVAL '5 YEARS';
    '''

### 7. Find All Movies/TV Shows by Director 'George Ford'

  '''sql
  SELECT TYPE FROM NETFLIX
  WHERE DIRECTOR ILIKE  '%George Ford%';
  '''

### 8. List All TV Shows with More Than 5 Seasons
  
   '''sql
  SELECT *  FROM NETFLIX 
  WHERE 
  TYPE = 'TV Show'
  AND 
  CAST(SPLIT_PART(DURATION, '', 1) AS INTEGER) > 5 ;
  '''

### 9. Count the Number of Content Items in Each Genre
    
   '''sql
    SELECT LISTED_IN, COUNT(SHOW_ID) AS TOTAL_CONTENT,
    UNNEST(STRING_TO_ARRAY(LISTED_IN,',')) AS GENRE
    FROM NETFLIX
    GROUP BY 1;
    '''


### 10.Find each year and the average numbers of content release in PAKISTAN on netflix.
return top 5 year with highest avg content release!

  '''sql
  SELECT extract( year from TO_DATE(DATE_ADDED, 'Month DD, YYYY')) AS year, 
  count(*) as yearly_content,
  ROUND(count(*)::NUMERIC/(select count(*) FROM NETFLIX WHERE COUNTRY ='PAKISTAN')::NUMERIC * 100,2) as avg_content
  from netflix
  WHERE COUNTRY ='PAKISTAN'
  group by 1;
  '''

### 11. List All Movies that are Documentaries

   '''sql
   SELECT * FROM NETFLIX 
   WHERE LISTED_IN ILIKE '%documentaries%';
   '''

### 12. Find All Content Without a Director

   '''sql
    SELECT * FROM NETFLIX 
    WHERE  DIRECTOR IS NULL;
    '''

### 13.FIND HOW MANY MOVIES DIRECTOR 'DANIEL' APPEARED IN LAST 10  YEARS!

  '''sql
  SELECT * FROM NETFLIX 
  WHERE DIRECTOR ILIKE '%DANIEL%' 
  AND
  RELEASE_YEAR > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
  '''

### 14.FIND THE TOP 10 DIRECTORS WHO HAVE APPEARED IN THE HIGHEST NUMBER OF MOVIES PRODUCED IN CANADA?
  
 '''sql
 SELECT  UNNEST(STRING_TO_ARRAY(DIRECTOR,',') ) AS DIRECTORS, 
  COUNT(*) AS TOATL_CONTENT 
  FROM NETFLIX 
  WHERE COUNTRY ILIKE '%CANADA%' 
  GROUP BY 1 
  ORDER BY 2 DESC 
  LIMIT 10;
  '''


##Findings and Conclusion

Content Distribution: The dataset contains a diverse range of movies and TV shows with varying ratings and genres.
Common Ratings: Insights into the most common ratings provide an understanding of the content's target audience.
Geographical Insights: The top countries and the average content releases by India highlight regional content distribution.
Content Categorization: Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.
This analysis provides a comprehensive view of Netflix's content and can help inform content strategy and decision-making.



