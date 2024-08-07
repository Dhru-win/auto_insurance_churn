## Importing csv file in PostgreSQL

### Importing customer table

```sql
CREATE TABLE IF NOT EXISTS public.customer
(
    "INDIVIDUAL_ID" numeric PRIMARY KEY,
    "ADDRESS_ID" numeric NOT NULL,
    "CURRENT_ANN_AMT" double precision,
    "DAYS_TENURE" numeric,
    "CUST_ORIG_DATE" date,
    "AGE_IN_YEARS" numeric,
    "DATE_OF_BIRTH" date,
    "SOCIAL_SECURITY_NUMBER" varchar,
    CONSTRAINT customer_pkey PRIMARY KEY ("INDIVIDUAL_ID")
    FOREIGN KEY ("ADDRESS_ID") REFERENCES public.address("ADDRESS_ID")
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.customer
    OWNER to postgres;

```

### Importing address table

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

### Creating and Importing demographic table

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

