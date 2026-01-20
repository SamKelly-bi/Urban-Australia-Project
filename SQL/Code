-- CREATE DATABASE

CREATE DATABASE australian_cities;

USE australian_cities;

-- CREATE TABLES

CREATE TABLE cities(
id CHAR(3) NOT NULL PRIMARY KEY,
city_name VARCHAR(50) NOT NULL,
state_id VARCHAR(4) NOT NULL,
state_name VARCHAR(50) NOT NULL,
metro_population INT
);

CREATE TABLE livability(
id VARCHAR(10) NOT NULL PRIMARY KEY,
city_id CHAR(3) NOT NULL,
sights_landmarks INT NOT NULL,
sights_landmarks_z_score DECIMAL(4,2) NOT NULL,
nature_parks INT NOT NULL,
nature_parks_z_score DECIMAL (4,2) NOT NULL,
walk_score INT NOT NULL,
walk_z_score DECIMAL (4,2) NOT NULL,
bike_score INT NOT NULL,
bike_z_score DECIMAL(4,2) NOT NULL,
air_quality INT NOT NULL,
air_quality_z_score DECIMAL (4,2) NOT NULL,
heat_stress_days INT NOT NULL,
heat_stress_z_score DECIMAL (4,2) NOT NULL,
median_rent DECIMAL (6,2) NOT NULL,
median_rent_z_score DECIMAL (4,2) NOT NULL,
median_income DECIMAL (7,2) NOT NULL,
median_income_z_score DECIMAL (4,2) NOT NULL,
rent_income_ratio DECIMAL (4,2) NOT NULL,
rent_income_ratio_z_score DECIMAL (4,2) NOT NULL,
life_expectancy DECIMAL (4,2) NOT NULL,
life_expectancy_z_score DECIMAL (4,2) NOT NULL,
gp_per_100000 INT NOT NULL,
gp_per_100000_z_score DECIMAL (4,2) NOT NULL,
livability_z_score DECIMAL (4,2) NOT NULL
);

CREATE TABLE lovability(
id VARCHAR(10) NOT NULL PRIMARY KEY,
city_id CHAR(3) NOT NULL,
nightlife INT NOT NULL,
nightlife_z_score DECIMAL (4,2) NOT NULL,
shopping INT NOT NULL,
shopping_z_score DECIMAL (4,2) NOT NULL,
restaurants INT NOT NULL,
restaurants_z_score DECIMAL (4,2) NOT NULL,
culture INT NOT NULL,
culture_z_score DECIMAL (4,2) NOT NULL,
google_trends INT NOT NULL,
google_trends_z_score DECIMAL (4,2) NOT NULL,
museums INT NOT NULL,
museums_z_score DECIMAL (4,2) NOT NULL,
attractions INT NOT NULL,
attractions_z_score DECIMAL (4,2) NOT NULL,
lovability_z_score DECIMAL (4,2) NOT NULL
);

CREATE TABLE prosperity(
id VARCHAR(10) NOT NULL PRIMARY KEY,
city_id CHAR(3) NOT NULL,
labour_force_participation DECIMAL (4,2) NOT NULL,
labour_force_participation_z_score DECIMAL (4,2) NOT NULL,
labour_productivity INT NOT NULL,
labour_productivity_z_score DECIMAL (4,2) NOT NULL,
large_companies INT NOT NULL,
large_companies_z_score DECIMAL(4,2) NOT NULL,
airport_connectivity INT NOT NULL,
airport_connectivity_z_score DECIMAL (4,2) NOT NULL,
convention_center INT NOT NULL,
convention_center_z_score DECIMAL (4,2) NOT NULL,
university INT NOT NULL,
university_z_score DECIMAL (4,2) NOT NULL,
educational_attainment DECIMAL (4,2) NOT NULL,
educational_attainment_z_score DECIMAL (4,2) NOT NULL,
business_ecosystem DECIMAL (5,3) NOT NULL,
business_ecosystem_z_score DECIMAL (4,2) NOT NULL,
unemployment_rate DECIMAL (3,2) NOT NULL,
unemployment_rate_z_score DECIMAL (4,2) NOT NULL,
homelessness_rate DECIMAL (3,2) NOT NULL,
homelessness_rate_z_score DECIMAL (4,2) NOT NULL,
prosperity_z_score DECIMAL (4,2) NOT NULL
);

CREATE TABLE city_z_score(
id VARCHAR(10) NOT NULL PRIMARY KEY,
city_id CHAR(3) NOT NULL,
livability_z_score DECIMAL (4,2) NOT NULL,
lovability_z_score DECIMAL (4,2),
prosperity_z_score DECIMAL (4,2),
overall_z_score DECIMAL (4,2) NOT NULL,
weighted_score INT NOT NULL
);

-- IMPORT ALL TABLES VIA THE IMPORT WIZARD

-- ADD IN FOREIGN KEY CONSTRAINTS

ALTER TABLE livability
ADD CONSTRAINT fk_livability_city
FOREIGN KEY (city_id)
REFERENCES cities (id)
ON DELETE CASCADE;

ALTER TABLE lovability
ADD CONSTRAINT fk_lovability_city
FOREIGN KEY (city_id)
REFERENCES cities (id)
ON DELETE CASCADE;

ALTER TABLE prosperity
ADD CONSTRAINT fk_prosperity_city
FOREIGN KEY (city_id)
REFERENCES cities (id)
ON DELETE CASCADE;

ALTER TABLE city_z_score
ADD CONSTRAINT fk_city_z_score_city
FOREIGN KEY (city_id)
REFERENCES cities (id)
ON DELETE CASCADE;

-- ADDING UNIQUE CONSTRAINTS

ALTER TABLE livability
ADD CONSTRAINT uq_livability_city UNIQUE (city_id);

ALTER TABLE lovability
ADD CONSTRAINT uq_lovability_city UNIQUE (city_id);

ALTER TABLE prosperity
ADD CONSTRAINT uq_prosperity_city UNIQUE (city_id);

ALTER TABLE city_z_score
ADD CONSTRAINT uq_city_z_score_city UNIQUE (city_id);

-- INDEXING FOREIGN KEYS

CREATE INDEX idx_livability_city_id ON livability(city_id);
CREATE INDEX idx_lovability_city_id ON lovability(city_id);
CREATE INDEX idx_prosperity_city_id ON prosperity(city_id);
CREATE INDEX idx_city_z_score_city_id ON city_z_score(city_id);



-- 1. CITIES THAT PROVIDE THAT BEST OVERALL VALUE WITH EMPHASIS ON HOUSING AFFORDABILITY

CREATE OR REPLACE VIEW best_value_cities AS
SELECT 
    c.city_name,
    z.overall_z_score,
    l.rent_income_ratio_z_score,
    (z.overall_z_score + l.rent_income_ratio_z_score) AS value_index
FROM cities c
JOIN city_z_score z 
	ON c.id = z.city_id
JOIN livability l 
	ON c.id = l.city_id
ORDER BY value_index DESC;

SELECT * FROM best_value_cities;

-- Identifies cities that offer strong overall livability while remaining relatively affordable. 
-- Combines overall performance with housing affordability to highlight “best value” locations rather than absolute top performers.



-- 2. TOP 3 CITIES WITH ABOVE AVERAGE LOVABILITY METRICS (JOINS AND SUBQUERIES)

CREATE OR REPLACE VIEW lovability_metrics AS
SELECT
	c.city_name, 
    c.id, 
    lo.lovability_z_score
FROM lovability lo
JOIN cities c 
	ON lo.city_id = c.id
WHERE lovability_z_score > (SELECT AVG(lovability_z_score) FROM lovability)
ORDER BY lovability_z_score DESC LIMIT 3;

SELECT * FROM lovability_metrics;

-- Selects the top 3 cities with above-average lovability scores using the mean as a benchmark.
-- Identifies the cities that outperform the typical experience in cultural and lifestyle amenities.



-- 3. CITIES THAT HAD AVERAGE OR BETTER METRICS IN ALL 3 CATEGORIES (COMPARISON OPERATORS)

CREATE OR REPLACE VIEW above_average_performers AS
SELECT 
	city_name, 
	city_id, 
    livability_z_score, 
    lovability_z_score, 
    prosperity_z_score, 
    overall_z_score
FROM city_z_score
JOIN cities c 
	ON city_id = c.id
WHERE 
	livability_z_score > 0 AND lovability_z_score > 0 AND prosperity_z_score > 0
ORDER BY overall_z_score DESC;

SELECT * FROM above_average_performers;

-- Highlights cities that score above-average across all three categories.
-- Identifies consistent performers using the z-score convention where  represents the mean.



-- 4. CATEGORISING CITIES BASED ON THEIR SIGHTS AND LANDMARKS (CASE STATEMENTS)

CREATE OR REPLACE VIEW sights_landmarks_categories AS
SELECT city_id, sights_landmarks_z_score,
	CASE
		WHEN sights_landmarks_z_score >= 2 THEN 'Exceptional'
        WHEN sights_landmarks_z_score BETWEEN 1 AND 1.99 THEN 'Well Above Average'
        WHEN sights_landmarks_z_score BETWEEN 0.50 AND 0.99 THEN 'Above Average'
        WHEN sights_landmarks_z_score BETWEEN 0.01 AND 0.49 THEN 'Slightly Above Average'
        WHEN sights_landmarks_z_score = 0 THEN 'Average'
        WHEN sights_landmarks_z_score BETWEEN -0.50 AND -0.01 THEN 'Slightly Below Average'
        WHEN sights_landmarks_z_score BETWEEN -0.99 AND -0.51 THEN 'Below Average'
        WHEN sights_landmarks_z_score BETWEEN -2 AND -1 THEN 'Well Below Average'
        ELSE 'Terrible'
	END AS s_l_tiers
FROM livability
ORDER BY sights_landmarks_z_score DESC;

SELECT * FROM sights_landmarks_categories;

-- Translates sights and landmarks z-scores into qualitative performance tiers.
-- Relative differences are easier to interpret which enables non-technical users to understand how cities compare at a glance.



-- 5. SHOWING CORRELATION BETWEEN CITIES ECONOMY AND ITS OVERALL Z SCORE (JOINS)

CREATE OR REPLACE VIEW economic_metrics AS 
SELECT 
	c.city_id, 
    p.large_companies_z_score,
    p.business_ecosystem_z_score, 
    c.overall_z_score
FROM prosperity p
JOIN city_z_score c 
	ON c.city_id = p.city_id
ORDER BY overall_z_score DESC;

SELECT * FROM economic_metrics;

-- Displays key economic indicators alongside overall city performance.
-- Allows for further analysis of how economic strength aligns with overall outcomes without explicitly calculating statistical correlation.



-- 6. COMPARING KEY METRICS AGAINST THE AVERAGE (JOINS AND WINDOW FUNCTIONS)

CREATE OR REPLACE VIEW key_metrics_vs_average AS
SELECT 
	c.city_name, 
	c.id, 
    c.metro_population, 
    avg(c.metro_population) OVER() AS avg_metro_population, 
    l.rent_income_ratio, 
    avg(l.rent_income_ratio) OVER() AS avg_rent_to_income_ratio,
    lo.museums,
    avg(lo.museums) OVER() AS avg_number_of_museums,
    p.labour_productivity,
    avg(p.labour_productivity) OVER() AS avg_labour_productivity,
    z.overall_z_score
FROM cities c
JOIN livability l 
	ON c.id = l.city_id
JOIN lovability lo 
	ON c.id = lo.city_id
JOIN prosperity p 
	ON c.id = p.city_id
JOIN city_z_score z 
	ON c.id = z.city_id
ORDER BY overall_z_score DESC;

SELECT * FROM key_metrics_vs_average;

-- Compares selected city-level metrics against their dataset-wide averages.
-- Allows each city to be evaluated in both absolute terms and relative to the typical city profile.



-- 7. RANKING CITIES BY ALL CATEGORIES (RANK FUNCTION)

CREATE OR REPLACE VIEW z_score_ranks AS
SELECT 
	c.city_name, 
	c.id, 
    z.livability_z_score, 
    RANK() OVER(ORDER BY z.livability_z_score DESC) AS livability_rank,
    z.lovability_z_score, 
    RANK() OVER(ORDER BY z.lovability_z_score DESC) AS lovability_rank,
    z.prosperity_z_score, 
    RANK() OVER(ORDER BY z.prosperity_z_score DESC) AS prosperity_rank,
    z.overall_z_score,
    RANK() OVER(ORDER BY z.overall_z_score DESC) AS overall_rank
FROM cities c
JOIN city_z_score z 
	ON c.id = z.city_id
ORDER BY overall_z_score DESC;

SELECT * FROM z_score_ranks;

-- Ranks cities across individual dimensions and overall performance using z-score ordering.
-- Enables a clear comparison of a cities relative standing.
-- Note the almost perfect correlation between a cities Prosperity metric rank and its overall rank.



-- 8. DETECTING OUTLIERS

CREATE OR REPLACE VIEW livability_outliers AS
SELECT
    c.city_name,
    'Sights & Landmarks' AS metric,
	l.sights_landmarks_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(sights_landmarks_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Nature & Parks' AS metric,
    l.nature_parks_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(nature_parks_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Walk Score' AS metric,
    l.walk_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(walk_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Bike Score' AS metric,
    l.bike_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(bike_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Heat Stress' AS metric,
    l.heat_stress_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(heat_stress_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Median Rent' AS metric,
    l.median_rent_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(median_rent_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Median Income' AS metric,
    l.median_income_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(median_income_z_score) >= 2

UNION ALL

SELECT 
	c.city_name,
    'Rent-Income-Ratio' AS metric,
    l.rent_income_ratio_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(rent_income_ratio_z_score) >= 2

UNION ALL

SELECT 
	c.city_name,
    'Life Expectancy' AS metric,
    l.life_expectancy_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(life_expectancy_z_score) >= 2

UNION ALL

SELECT 
	c.city_name,
    'GPs Per 100000' AS metric,
    l.gp_per_100000_z_score AS z_score
FROM cities c
JOIN livability l
	ON c.id = l.city_id
WHERE ABS(gp_per_100000_z_score) >= 2;



CREATE OR REPLACE VIEW lovability_outliers AS
SELECT 
	c.city_name,
	'Nightlife' AS metric,
	lo.nightlife_z_score AS z_score
FROM
	cities c
JOIN lovability lo 
	ON c.id = lo.city_id
WHERE
	ABS(nightlife_z_score) >= 2 

UNION ALL 
    
SELECT 
	c.city_name,
	'Shopping' AS metric,
	lo.shopping_z_score AS z_score
FROM cities c
JOIN lovability lo 
	ON c.id = lo.city_id
WHERE
        ABS(shopping_z_score) >= 2 
        
UNION ALL 
    
SELECT 
	c.city_name,
	'Restaurants' AS metric,
	lo.restaurants_z_score AS z_score
FROM cities c
JOIN lovability lo 
	ON c.id = lo.city_id
WHERE ABS(restaurants_z_score) >= 2 

UNION ALL 

SELECT 
	c.city_name,
	'Culture' AS metric,
	lo.culture_z_score AS z_score
FROM cities c
JOIN lovability lo 
	ON c.id = lo.city_id
WHERE ABS(culture_z_score) >= 2 

UNION ALL 

SELECT 
	c.city_name,
	'Google Trends' AS metric,
	lo.google_trends_z_score AS z_score
FROM cities c
JOIN lovability lo 
	ON c.id = lo.city_id
WHERE ABS(google_trends_z_score) >= 2

UNION ALL 

SELECT
	c.city_name,
    'Museums' AS metric,
    lo.museums_z_score AS z_score
FROM cities c
JOIN lovability lo 
	ON c.id = lo.city_id
WHERE ABS(museums_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Attractions' AS metric,
    lo.attractions_z_score AS z_score
FROM cities c
JOIN lovability lo 
	ON c.id = lo.city_id
WHERE ABS(attractions_z_score) >= 2;



CREATE OR REPLACE VIEW prosperity_outliers AS
SELECT 
	c.city_name,
    'Labour Force Participation' AS metric,
    p.labour_force_participation_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(labour_force_participation_z_score) >= 2

UNION ALL

SELECT 
	c.city_name,
    'Labour Productivity' AS metric,
    p.labour_productivity_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(labour_productivity_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Large Companies' AS metric,
    p.large_companies_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(large_companies_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Airport Connectivity' AS metric,
    p.airport_connectivity_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(airport_connectivity_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Convention Center' AS metric,
    p.convention_center_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(convention_center_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'University' AS metric,
    p.university_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(university_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Education' AS metric,
    p.educational_attainment_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(educational_attainment_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Business Ecosystem' AS metric,
    p.business_ecosystem_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(business_ecosystem_z_score) >= 2

UNION ALL

SELECT
	c.city_name,
    'Unemployment Rate' AS metric,
    p.unemployment_rate_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(unemployment_rate_z_score) >= 2

UNION ALL 

SELECT
	c.city_name,
    'Homelessness Rate' AS metric,
    p.homelessness_rate_z_score AS z_score
FROM cities c
JOIN prosperity p
	ON c.id = p.city_id
WHERE ABS(homelessness_rate_z_score) >= 2;

SELECT * FROM livability_outliers
ORDER BY z_score DESC;

SELECT * FROM lovability_outliers
ORDER BY z_score DESC;

SELECT * FROM prosperity_outliers
ORDER BY z_score DESC;

-- Identifies metrics with extreme z-scores (±2 or greater), flagging cities that significantly outperform or underperform the norm.

