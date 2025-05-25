# Spotify Advanced SQL Project and Query Optimization P-6
Project Category: Advanced
[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

## Technology Stack
- **Database**: PostgreSQL
- **SQL Queries**: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
- **Tools**: pgAdmin 4 (or any SQL editor), PostgreSQL (via Homebrew, Docker, or direct installation)
- 
## Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using **SQL**. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.


```sql
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```
## Project Steps

### 1. Data Exploration
Before diving into SQL, it’s important to understand the dataset thoroughly. The dataset contains attributes such as:
- `Artist`: The performer of the track.
- `Track`: The name of the song.
- `Album`: The album to which the track belongs.
- `Album_type`: The type of album (e.g., single or album).
- Various metrics such as `danceability`, `energy`, `loudness`, `tempo`, and more.

### 2. Querying the Data
After the data is inserted, various SQL queries can be written to explore and analyze the data. Queries are categorized into **easy**, **medium**, and **advanced** levels to help progressively develop SQL proficiency.

#### Easy Queries
- Simple data retrieval, filtering, and basic aggregations.
  
#### Medium Queries
- More complex queries involving grouping, aggregation functions, and joins.
  
#### Advanced Queries
- Nested subqueries, window functions, CTEs, and performance optimization.

---

## Practice Questions

### Easy Level
1. Retrieve the names of all tracks that have more than 1 billion streams. 
```SQL
Select * FROM spotify WHERE 
stream > 1000000000
```
2. List all albums along with their respective artists.
```SQL
SELECT DISTINCT album, artist
FROM spotify
ORDER by 1

SELECT DISTINCT album
FROM spotify
ORDER by 1
```
3. Get the total number of comments for tracks where `licensed = TRUE`.
```SQL
SELECT SUM(comments) as Total_comments
FROM spotify
WHERE licensed = 'TRUE'
```
4. Find all tracks that belong to the album type `single`.
```SQL
SELECT * FROM spotify
WHERE album_type ILIKE 'single'
```
5. Count the total number of tracks by each artist.
```SQL
SELECT 
    artist,
    COUNT(*) AS total_no_songs
FROM 
    spotify
GROUP BY 
    artist
ORDER BY 2
```
### Medium Level
1. Calculate the average danceability of tracks in each album.
```SQL
SELECT 
    album,
    AVG(danceability) AS avg_danceability
FROM 
    spotify
GROUP BY 
    album
ORDER BY 
   2 DESC;
```
2. Find the top 5 tracks with the highest energy values.
```SQL
SELECT 
    track,
    artist,
    energy
FROM 
    spotify
ORDER BY 
    energy DESC
LIMIT 5;
```
3. List all tracks along with their views and likes where `official_video = TRUE`.
```SQL
SELECT
	track,
	views,
	likes 
FROM
	spotify
WHERE 
	official_video = TRUE
ORDER by 2 DESC
```
4. For each album, calculate the total views of all associated tracks.
```SQL
SELECT 
	album,
    SUM(views) AS total_views
FROM 
    spotify
GROUP BY 
    album
ORDER by 2 DESC;
```

### Advanced Level
1. Find the top 3 most-viewed tracks for each artist using window functions.
```SQL
WITH ranking_artist AS (
    SELECT
        artist,
        track,
        views,
        ROW_NUMBER() OVER (
            PARTITION BY artist
            ORDER BY views DESC
        ) AS rank
    FROM spotify
)
SELECT *
FROM ranking_artist
WHERE rank <= 3;
```
2. Write a query to find tracks where the liveness score is above the average.
```SQL
SELECT 
    track,
    artist,
    liveness
FROM 
    spotify
WHERE 
    liveness > (
        SELECT AVG(liveness)
        FROM spotify
    )
ORDER BY 
    liveness DESC;
```
3. **Use a `WITH` clause to calculate the difference between the highest and lowest energy values for tracks in each album.**
```sql
WITH energy_range AS (
    SELECT 
        album,
        MAX(energy) AS max_energy,
        MIN(energy) AS min_energy
    FROM 
        spotify
    GROUP BY 
        album
)

SELECT 
    album,
    max_energy,
    min_energy,
    max_energy - min_energy AS energy_difference
FROM 
    energy_range
ORDER BY 
    energy_difference DESC;
```
   
5. Find tracks where the energy-to-liveness ratio is greater than 1.2.
```SQL
SELECT 
    track,
    artist,
    energy,
    liveness,
    energy / liveness AS energy_to_liveness_ratio
FROM 
    spotify
WHERE 
    liveness > 0 AND  -- To avoid division by zero
    energy / liveness > 1.2
ORDER BY 
    energy_to_liveness_ratio DESC;
```
6. Calculate the cumulative sum of likes for tracks ordered by the number of views, using window functions.
```SQL
SELECT 
    track,
    artist,
    views,
    likes,
    SUM(likes) OVER (
        ORDER BY views
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS cumulative_likes
FROM 
    spotify
ORDER BY 
    views;
```







