# carbon-emission-analysis

<img width="640" height="427" alt="image" src="https://github.com/user-attachments/assets/af543174-3099-4a3a-bcb4-db10d760535f" />

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## Data Source
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

<img width="687" height="487" alt="image" src="https://github.com/user-attachments/assets/122465b7-2d0a-461d-998c-4d355d520b9a" />

## Data Exploring
### Table 'product_emissions'
```sql
SELECT *
FROM product_emissions pe 
LIMIT 5;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|

### Table 'industry_groups'
```sql
SELECT *
FROM industry_groups ig 
LIMIT 5;
```
|id|industry_group|
|--|--------------|
|1|"Consumer Durables, Household and Personal Products"|
|2|"Food, Beverage & Tobacco"|
|3|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|
|4|"Mining - Iron, Aluminum, Other Metals"|
|5|"Pharmaceuticals, Biotechnology & Life Sciences"|

### Table 'companies'
```sql
SELECT *
FROM companies c 
LIMIT 5;
```
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|

### Table 'countries'
```sql
SELECT * 
FROM countries c 
LIMIT 5;
```
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|


INSERT INTO product_emissions (id,company_id,country_id,industry_group_id,`year`,product_name,weight_kg,carbon_footprint_pcf,upstream_percent_total_pcf,operations_percent_total_pcf,downstream_percent_total_pcf) VALUES
	 ('10056-1-2014',82,28,2,2014,'Frosted Flakes(R) Cereal',0.7485,2,'57.50','30.00','12.50'),
	 ('10056-1-2015',82,28,15,2015,'"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"',0.7485,2,'57.50','30.00','12.50'),
	 ('10222-1-2013',83,28,8,2013,'Office Chair',20.68,73,'80.63','17.36','2.01'),
	 ('10261-1-2017',14,16,25,2017,'Multifunction Printers',110.0,1488,'30.65','5.51','63.84'),
	 ('10261-2-2017',14,16,25,2017,'Multifunction Printers',110.0,1818,'25.08','4.51','70.41');
```

```sql
INSERT INTO industry_groups (industry_group) VALUES
	 ('"Consumer Durables, Household and Personal Products"'),
	 ('"Food, Beverage & Tobacco"'),
	 ('"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"'),
	 ('"Mining - Iron, Aluminum, Other Metals"'),
	 ('"Pharmaceuticals, Biotechnology & Life Sciences"');
```
```sql
INSERT INTO countries (country_name) VALUES
	 ('Australia'),
	 ('Belgium'),
	 ('Brazil'),
	 ('Canada'),
	 ('Chile');
```
```sql
INSERT INTO companies (company_name) VALUES
	 ('"Autodesk, Inc."'),
	 ('"Casio Computer Co., Ltd."'),
	 ('"Cisco Systems, Inc."'),
	 ('"CNX Coal Resources, LP"'),
	 ('"Coca-Cola Enterprises, Inc."');
```

###
```sql
SELECT *, COUNT(*) AS count_duplicate
  FROM product_emissions
  GROUP BY
      		id,
      		company_id,
      		country_id,
      		industry_group_id,
      		year,
      		product_name,
      		weight_kg,
      		carbon_footprint_pcf,
      		upstream_percent_total_pcf,
      		operations_percent_total_pcf,
      		downstream_percent_total_pcf
    HAVING COUNT(*) > 1
    LIMIT 10;
```

| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf                       | operations_percent_total_pcf                     | downstream_percent_total_pcf                     | count_duplicate | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -----------------------------------------------: | -----------------------------------------------: | -----------------------------------------------: | --------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2               | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2               | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                                            | 17.36                                            | 2.01                                             | 2               | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                                            | 5.51                                             | 63.84                                            | 2               | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                                            | 4.51                                             | 70.41                                            | 2               | 
| 10261-3-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 2274                 | 20.05                                            | 3.61                                             | 76.34                                            | 2               | 
| 10324-1-2016 | 15         | 16         | 19                | 2016 | KURALON  fiber                                                  | 1500      | 10000                | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10418-1-2013 | 84         | 9          | 19                | 2013 | Portland Cement                                                 | 1000      | 1102                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10661-1-2014 | 85         | 28         | 11                | 2014 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10661-1-2015 | 85         | 28         | 6                 | 2015 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 


### Which products contribute the most to carbon emissions?
```sql
SELECT 
	product_name,
	ROUND(AVG(carbon_footprint_pcf),2) AS "carbon emission"
FROM product_emissions pe 
GROUP BY product_name
ORDER BY ROUND(AVG(carbon_footprint_pcf),2) DESC;
```
| product_name                                                                                                                       | carbon emission | 
| ---------------------------------------------------------------------------------------------------------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00      | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00      | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00      | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00      | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00       | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00       | 
| TCDE                                                                                                                               | 99075.00        | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00        | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00        | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00        | 


### hat are the industry groups of these products?
```sql
SELECT 
	product_name,
	industry_groups.industry_group,
	ROUND(AVG(carbon_footprint_pcf),2) AS "carbon emission"
FROM product_emissions pe 
JOIN industry_groups ON pe.industry_group_id = industry_groups.id
GROUP BY (pe.product_name)
ORDER BY ROUND(AVG(carbon_footprint_pcf),2) DESC
LIMIT 10;
```
| product_name                                                                                                                       | industry_group                     | carbon emission | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00      | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00      | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00      | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00      | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00       | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00       | 
| TCDE                                                                                                                               | Materials                          | 99075.00        | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00        | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00        | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00        | 


### What are the industries with the highest contribution to carbon emissions?
```sql
SELECT 
	i.industry_group,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emission"
	  
FROM product_emissions pe 
JOIN industry_groups AS i ON pe.industry_group_id = i.id
GROUP BY (i.industry_group)
ORDER BY ROUND(AVG(carbon_footprint_pcf),2) DESC
LIMIT 10;
```

| industry_group                                   | carbon emission | 
| -----------------------------------------------: | --------------: | 
| Electrical Equipment and Machinery               | 9801558.00      | 
| Automobiles & Components                         | 2582264.00      | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.00        | 
| Capital Goods                                    | 258712.00       | 
| Materials                                        | 577595.00       | 
| "Mining - Iron, Aluminum, Other Metals"          | 8181.00         | 
| Energy                                           | 10774.00        | 
| Chemicals                                        | 62369.00        | 
| Media                                            | 23017.00        | 
| Software & Services                              | 46544.00        | 


### What are the companies with the highest contribution to carbon emissions?
```sql
SELECT 
	company_name,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emission"
	  
FROM product_emissions pe 
JOIN companies c ON pe.company_id = c.id
GROUP BY (c.company_name)
ORDER BY ROUND(AVG(carbon_footprint_pcf),2) DESC
LIMIT 10;
```
| company_name                           | carbon emission | 
| -------------------------------------: | --------------: | 
| "Gamesa Corporación Tecnológica, S.A." | 9778464.00      | 
| "Hino Motors, Ltd."                    | 191687.00       | 
| Arcelor Mittal                         | 167007.00       | 
| Weg S/A                                | 160655.00       | 
| Daimler AG                             | 1594300.00      | 
| General Motors Company                 | 137007.00       | 
| Volkswagen AG                          | 655960.00       | 
| Waters Corporation                     | 72486.00        | 
| "Daikin Industries, Ltd."              | 105600.00       | 
| CJ Cheiljedang                         | 94817.00        | 

### What are the countries with the highest contribution to carbon emissions?
```sql
SELECT 
	c.country_name,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emission"
	  
FROM product_emissions pe 
JOIN countries AS c ON pe.industry_group_id = c.id
GROUP BY (c.country_name)
ORDER BY ROUND(AVG(carbon_footprint_pcf),2) DESC
LIMIT 10;
```
| country_name | carbon emission | 
| -----------: | --------------: | 
| Indonesia    | 9801558.00      | 
| Colombia     | 2582264.00      | 
| Chile        | 72486.00        | 
| Finland      | 258712.00       | 
| Malaysia     | 577595.00       | 
| Canada       | 8181.00         | 
| Ireland      | 10774.00        | 
| France       | 62369.00        | 
| Netherlands  | 23017.00        | 
| Sweden       | 46544.00        | 

### What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT 
	ig.industry_group,
	SUM(t.avg_carbon_footprint) AS total_industry_emissions  
FROM (
  	  SELECT
  			industry_group_id,
  			product_name,
  			AVG(carbon_footprint_pcf) AS avg_carbon_footprint
  	  FROM product_emissions
  	  GROUP BY industry_group_id, product_name
) AS t
JOIN industry_groups ig ON t.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY total_industry_emissions DESC
LIMIT 10;
```

| industry_group                                   | total_industry_emissions | 
| -----------------------------------------------: | -----------------------: | 
| Electrical Equipment and Machinery               | 9801558.0000             | 
| Automobiles & Components                         | 2318945.3333             | 
| Materials                                        | 408449.8333              | 
| Technology Hardware & Equipment                  | 252039.7667              | 
| Capital Goods                                    | 177108.7500              | 
| "Food, Beverage & Tobacco"                       | 92965.5000               | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.0000               | 
| Chemicals                                        | 44939.0000               | 
| Software & Services                              | 23683.0000               | 
| Media                                            | 14139.3333               | 


### Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```sql
SELECT 
    ig.industry_group AS 'Industry Group',
    ROUND(SUM(CASE WHEN pe.year = 2013 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2013 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2014 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2014 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2015 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2015 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2016 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2016 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2017 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2017 Emission'
FROM 
    product_emissions AS pe
LEFT JOIN 
    industry_groups AS ig ON ig.id = pe.industry_group_id
GROUP BY 
    ig.industry_group
ORDER BY 
    '2017 Emission',
    '2016 Emission',
    '2015 Emission',
    '2014 Emission',
    '2013 Emission';
```

  | Industry Group                                 | 2013 Emission | 2014 Emission | 2015 Emission | 2016 Emission | 2017 Emission | 
| ---------------------------------------------: | ------------: | ------------: | ------------: | ------------: | ------------: | 
| "Food, Beverage & Tobacco"                     | 4995.00       | 2685.00       | 0.00          | 100289.00     | 3162.00       | 
| Food & Beverage Processing                     | 0.00          | 0.00          | 141.00        | 0.00          | 0.00          | 
| Capital Goods                                  | 60190.00      | 93699.00      | 3505.00       | 6369.00       | 94949.00      | 
| Technology Hardware & Equipment                | 61100.00      | 167361.00     | 106157.00     | 1566.00       | 27592.00      | 
| Materials                                      | 200513.00     | 75678.00      | 0.00          | 88267.00      | 213137.00     | 
| Consumer Durables & Apparel                    | 2867.00       | 3280.00       | 0.00          | 1162.00       | 0.00          | 
| "Textiles, Apparel, Footwear and Luxury Goods" | 0.00          | 0.00          | 387.00        | 0.00          | 0.00          | 
| Software & Services                            | 6.00          | 146.00        | 22856.00      | 22846.00      | 690.00        | 
| Chemicals                                      | 0.00          | 0.00          | 62369.00      | 0.00          | 0.00          | 
| Semiconductors & Semiconductor Equipment       | 0.00          | 50.00         | 0.00          | 4.00          | 0.00          | 
