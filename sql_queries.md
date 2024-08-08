## Importing csv files in PostgreSQL

### Creating customer table and importing data

```sql
CREATE TABLE IF NOT EXISTS public.customer
(
    individual_id numeric PRIMARY KEY,
    address_id numeric NOT NULL,
    current_ann_amt double precision,
    days_tenure numeric,  -- Days 
    cust_orig_date date,
    age_in_years numeric,
    date_of_birth date,
    social_security_number varchar,
    FOREIGN KEY (address_id)
		REFERENCES public.address(address_id)
)


```

### Creating address table and importing data

```sql
CREATE TABLE IF NOT EXISTS public.address(
	
	"ADDRESS_ID" varchar PRIMARY KEY, -- Unique ID for a specific address character
	"LATITUDE" double precision ,-- Lattitude of the address
	"LONGITUDE"  double precision,-- Longitude of the address
	"STREET_ADDRESS" text, -- Mailing Address of the address
	"CITY" varchar,  -- City
	"STATE" varchar, -- State
	"COUNTY" varchar -- County
);
```

### Creating demographic table and Importing data

```sql
CREATE TABLE IF NOT EXISTS public.demographic (

	INDIVIDUAL_ID numeric, -- Unique ID for a specific insurance customer
	INCOME numeric, -- Estimated Income for the Household associated with the individual
	HAS_CHILDREN numeric, -- Flag, 1 indicates the individual has children in the home, 0 otherwise.
	LENGTH_OF_RESIDENCE numeric, -- Estimated number of years the individual has lived in their current home.
	MARITAL_STATUS varchar, -- Estimated marital status. Married or Single.
	HOME_MARKET_VALUE numeric,-- Estimate value of home.
	HOME_OWNER bool, -- Flag, 1 individual owns primary home, 0 otherwise.
	COLLEGE_DEGREE bool, -- Flag, 1 individual has a college degree or more, 0 otherwise.
	GOOD_CREDIT  bool, -- Flag, 1 individual has FICO greater than 630, 0 otherwise.
	FOREIGN KEY ("INDIVIDUAL_ID") REFERENCES public.customer("INDIVIDUAL_ID")
)
```

### Creating and importing termination table

```sql

CREATE TABLE IF NOT EXISTS public.termination(

	individual_id numeric,
	acct_suspd_date date -- date account was suspended
	FOREIGN KEY individual_id REFERENCING customer(individual_id)
)

```


## Questions to analyze

1. Analyze in what year/month customers left the most.

#### Query 
```sql
-- Churn by year
SELECT 
	EXTRACT(YEAR FROM acct_suspd_date) AS year_left,
	count(*) AS num_customers_left
FROM
	termination
GROUP BY
	year_left
ORDER BY 
	year_left

-- Churn by month

SELECT 
	EXTRACT(MONTH FROM acct_suspd_date) AS month_left,
	count(*) AS num_customers_left
FROM
	termination
GROUP BY
	month_left
ORDER BY 
	month_left

```
#### Answer

248,462 customers left in the year 2022 which is almost 12 times the customer leaving compared to last year i.e. 20,797 in the year 2021.

Customer left the company in a uniform pattern through out the years. Number of customers leaving per month is in the range 22,200 to 22,700.
1. Who are the churned customers
	* 
2. calculate churn rate over different periods of time monthly quaterly, yearly