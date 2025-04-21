# SQL_NETFLIX_PROJECT



## Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives

- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## Schema

CREATE DATABASE NETFLIX_DB;
USE NETFLIX_DB;


CREATE TABLE NETFLIX_P (
   show_id VARCHAR(10),
	type VARCHAR(10),
    title VARCHAR (50),
    director VARCHAR (50),
    cast VARCHAR (300),
    country	VARCHAR (200),
    date_added	VARCHAR (50),
    release_year	YEAR,
    rating	VARCHAR(20),
    duration	VARCHAR(20),
    listed_in	VARCHAR(100),
    description VARCHAR(300));
    
  SELECT * FROM NETFLIX_P;
    
  ## Business Problems and Solutions
  ----- 1.Count the Number of Movies vs TV Shows
    
  SELECT TYPE,COUNT(*) FROM NETFLIX_P GROUP BY 1;
    
    
  ----- 2. Find the Most Common Rating for Movies and TV Shows
    
  SELECT TYPE,RATING,COUNT(*) AS RATING_COUNT, RANK() OVER(PARTITION BY TYPE ORDER BY COUNT(*) DESC) AS RANKS 
    FROM NETFLIX_P GROUP BY 1,2;
    
    
  ----- 3. List All Movies Released in a Specific Year (e.g., 2020)
    
  SELECT * FROM NETFLIX_P 
    WHERE release_year = '2020';
    
  ----- 4. Find the Top 5 Countries with the Most Content on Netflix
    
   SELECT 
        substring_index(country,',',1) as country_p,
		count(*) as total_content
      FROM netflix_p group by 1 order by total_content desc limit 5;
    
  ----- 5. Identify the Longest Movie
    
  select * from netflix_p where type = 'movie'
    ORDER BY substring_index(duration, ' ', 1) DESC;
    
    
   -----  6. Find Content Added in the Last 5 Years
   
   SELECT *
FROM netflix_p
WHERE STR_TO_DATE(date_added, "%d-%b-%y") >= DATE_SUB(CURDATE(),interval 5 year );

----- 7.Find All Movies/TV Shows by Director 'Rajiv Chilaka'

select * from netflix_p where director like 'rajiv chilaka' ;


----- 8.List All TV Shows with More Than 5 Seasons

SELECT *
FROM netflix_p
WHERE type = 'TV Show'
  AND left(duration, 2) > 5;
  
----- 9. Count the Number of Content Items in Each Genre

SELECT 
        substring_index(listed_in,',',1) as genre,
		count(*) as total_content
      FROM netflix_p group by 1 ;
      
-----  10.Find each year and the average numbers of content release in India on netflix.
	
   select 
    year(STR_TO_DATE(date_added, "%d-%b-%y")),
    count(*),
    count(*) / (select count(*) from netflix_p where country = 'india') * 100 as avg
	from netflix_p
    where country = 'india'
    group by 1;
    
    
   -----  11. List All Movies that are Documentaries
   
   select * from netflix_p
   where listed_in like '%Documentaries';
     
  ----- 12. Find All Content Without a Director
     
SELECT * 
FROM netflix_p
WHERE country is null;


----- 13. Find How Many Movies Actor 'Salman Khan' Appeared in the Last 10 Years

SELECT * 
FROM netflix_P
WHERE cast LIKE '%Salman Khan%'
  AND release_year > YEAR(CURRENT_DATE) - 10;
  
  ----- 14. Find the Top 10 Actors Who Have Appeared in the Highest Number of Movies Produced in India
  
  SELECT 
    substring_index(cast,',',1) AS actor,
    count(*)
    FROM NETFLIX_P
 WHERE country = 'India'
GROUP BY actor
ORDER BY COUNT(*) DESC
LIMIT 10;


----- 15. Categorize Content Based on the Presence of 'Kill' and 'Violence' Keywords

SELECT 
    category,
    COUNT(*) AS content_count
FROM (
    SELECT 
        CASE 
            WHEN description LIKE '%kill%' OR description LIKE '%violence%' THEN 'Bad'
            ELSE 'Good'
        END AS category
    FROM netflix_p
) AS categorized_content
GROUP BY category;

     
     

**Objective:** Categorize content as 'Bad' if it contains 'kill' or 'violence' and 'Good' otherwise. Count the number of items in each category.

## Findings and Conclusion

- **Content Distribution:** The dataset contains a diverse range of movies and TV shows with varying ratings and genres.
- **Common Ratings:** Insights into the most common ratings provide an understanding of the content's target audience.
- **Geographical Insights:** The top countries and the average content releases by India highlight regional content distribution.
- **Content Categorization:** Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.

This analysis provides a comprehensive view of Netflix's content and can help inform content strategy and decision-making.








  
    
    
    
    
    
    
    
    
    
