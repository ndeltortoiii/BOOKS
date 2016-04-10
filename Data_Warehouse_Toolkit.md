# The Data Warehouse Toolkit 

## Overview
1. Dimensional Modeling Primer
  * Vocabulary
2. Retail Sales
  * 4 Step Process
  * Fundamentals
3. Inventory
  * DW Bus Architecture
  * Conformed
    * dimensions
    * facts
4. Procurement
  * How to handle changing dimension attributes
5. Order Mgmt
  * Biz Process
  * Fact tables
    1. transaction
    2. periodic snap
    3. accumulating snap
6. CRM
  * customer dimensions
7. Accounting
  * consolidated dimensional models
8. HR Mgmt
  * Dimension table behaving like fact tables
  * audit and keyword dimensions
9. Financial Services
  * heterogeneous products
10. Telecommunications and utilities
  * data model design review
11. Transportation
  * related fact tables
12. Education
  * accumulating snapshot fact tables
13. Health care
  * bridge table
14. electronic commerce
  * model clickstream data
15. Insurance
  * layered modeling techniques
16. Building the data warehouse
  * lifecycle of typical dw project iteration

* www.kimballuniversity.com
* Dimensional Modeling
  1. Key Terminology
  2. Architectual conecpts
  3. Fundamental Techniques
  
## Chapter 1: Dimensional Modeling Primer (Vocabulary)
* Concepts
  * Business Drive Goals of a Data Warehouse
  * Data warehouse publishing
  * major components of the overall data warehouse presentation area
  * Fact an dimension table terminology
  * Myths surrounding dimensional modeling
  * Common data warehousing pitfalls to avoid
  
* Assets
  1. IN: Operational System of Record (One record at a time)
  2. OUT: Data Warehouse
* Slicing and Dicing
* Same name should mean same thing, common definitions
* **Decision Support System** The original label that predates the data warehouse is till the best description of waht we are designing
* If the business community has not embraced the data warehouse and continued to use it actively six months after training, then we have failed the acceptance test
* Correct job description for a data warehouse manager id, **"Publisher of the right data."**
* Responsibilities:
  1. Understand your users by business area, job responsibilities, and computer tolerance
  2. Determine the decisions the business users want to make with the hellp of the data warehouse
  3. Identify the best users who make effective, high-impact decisons using the data warehouse
  4. Find potential new users and make them aware of the data warehouse
  5. Choose the most effective, actionable subset of the data to present in the data warehouse, drawn from the vast universe of possible data in your organization
  6. Make the user interfaces and applications simple and template driven, explicitly matching to the user's cognitive processing profiles
  7. Make sure the data is accurate and can be trusted, labeling it consistently across the enterprise
  8. Continuously monitor the accuracy of the data and the context of the delivered reports
  9. Search fo rnew dat sources, and continously adapt the data warehouse to changing data profiles, reporting requiremetns, and business priorities
  10. Take a portion of the credit fo rthe business decisions made using the data warehouse, and use these sucesses to justify your staffing, software and hardware expenditures.
  11. Publish the data on a regular basiss
  12. Maintain the trust of busienss users
  13. Keep your business users, executive sponsors, and boss happy

* Components of a Data Warehouse
  1. Operational Source Systems: Transactions of Business
    * Maintain little historical data
    * Priority of source system is processing performance and availablility
  2. Data Staging Area: ETL
    * Your kitchen staff should not be responding to customer inquiries example
    * The key architectural requirement for the data staging area is that it is off-limits to business users and does not provide query and presentation services
    * Potential Transofrmations
      1. Correcting misspelling
      2. Resolving domain conflicts
      3. deailing with missing elements
      4. parsing into standard formats
      5. **combining data from multiple sources**
      6. deduplicating data
      7. assigning warehouse keys
    * Data staging area activities
      1. sorting
      2. sequential processing
      3. one-to-one and many-to-one business rules
    * Physically creating a normalized set of tables in your staging area
    * normalized databases are excluded from the presentation area, which should be strictly dimensionally structured
    * Load QA Dimension Tables --> Data Mart --> Index Data and Aggregation
    * Communicating the nature of any changes that have occurred in the underlying dimensions
  3. Data Presentation: Query by users / Business Units Data Warehosue
    * **Listen carefully to the CEO's emphasis on product, market, and time as they describe their business**
      * Start with GMV break down from there
    * Data Mart
      * Wedge of overall presentation pie
      * Single business process
      * Cubes of data
    * Insist that the data be presented, stored, and accessed in dimenional schemas
    * Modeling types:
      * Third-normal-form (3NF) = Entity-Relationship (ER) Model = Normalized Models = RDBMS 
      * Dimensional Modeling = business process modeling
        * Package in format resilience to change
        * Atomic data
          * Withstand ad hoc queries
          * can't be locked in normalized models
          * Aggregated navigation
          * Fine grain data in presnentation area
        * Data Mart
          * Common dimension and fact tables --> conformed
          * 20 or more similar data marts
          * several fact tables
          * 5-15 dimension tables shared between fact tables
        * Different physical implications
          1. Relational DB --> Dimensions model tables (Star schemas)
          2. Multi-dimensional OLAP --> Cubes
    * Examples
      * A marketing department that is unhappy because it can't access the system directly (And still doesn't know whether the company is profitable in Schenectady)
      * A restless CIO who is determined to make some changes if things don't improve dramatically
  4. Data Access Tools: Capabilities provided to Business Unit for decision making
    * Be able to query
    * Perceptor: The majority of the business user base likely will acces sthe data via prebuilt parameter-driven analytic applications
* Metadata: Information that is not actual data itself
  * Support the disparate needs of teh DW technical, administrative, and business user
  * Staging metadata to facilitate extraction process
    * transformation and loading process, 
    * staging file and target table layouts
    * transformation and cleaning rules
    * conformed dimension and fact definitions
    * aggregation definitions
    * ETL transmission schedules
    * run-log results
  * Examples
    1. system tables
    2. partition settings
    3. indexs
    4. view definitions
    5. DBMS-level security privileges and grants
    6. Business names and definitions
    7. constraint filters
    8. application template specifications
    9. access an dusage statistics
    10. Security settings
* Operational Data Store (ODS): Operational Reporting
  * Real time interactions (CRM)
  * Options
    1. 3rd Physical system betwee ops and data warehouse
    2. Hot partition of DW itself
  * Store atomic data
* Fact Table: Business measures | Numerical performance measurements of business held
  * Largest part of any data mart (Don't duplicate around enterprise)
  * Grains --> What the scope of the measurement is 
  * A row in the fact tabel corresponds to a measurement.  A measuremetn is a row in a fact table. All the measurement in a fact table must be at the same grain
  * Semiadditive facts can be added only along some of the dimensions, and nonadditive facts simply can't be added at all.
    * nonadditive facts forced to use counts or averages if we wihs ot summarize the rows
    * Most useful facts in a fact table are numerica and additive
  * Text Not unique --> Dimension table
  * Text unique per row --> Fact Table
    * Put text measures into dimension --> correlated to other text
  * Only introudce true activity
  * Fact takes 90% of space consumed
  * Many Rows, few columns
  * Referential integrity: Fact Table keys match Primary Key in Dimension table
  * Composite or concatenated: Primary key for fact table
    * Many-to-many = fact table
    * Don't want to use ROWID
* Dimension Tables: Textual descriptions of business --> Many columns or attributes
  * **"by"** words must be available as dimension attributes
  * Provide attributes with verbose business terms | Quality of values | Populating values
  * Attributes = real words
  * Numeric data
    * Fact Table: takes on lots of values and clculations
    * Dimension table: descriptions / constant part in constraints
  * Reducee code in dimension table --> Verbose text attributes
  * Create sepertate table dimensions
  * Snowflake: hierarchical relationships
* Fact and Dimension Tables become a star join schema
  * Evaluate arbitrary n-way joins: heavily indexted dimension tables, and then attacking the fact table all at once with the Cartesion product of the dimension table keys satisfying the user's constraints
  * Can add new, unanticipated fats to the fact table, assuming that th elevel of detail is consistent with the existing fact table
    * add new rows or alter table with NO reload
  * Dimension support report label 
  * Fact supply numeric values 
  * Diagram represents multiple business process
* How to change form existing ER diagram
  1. Separate ER diagram into discrete busines processes
  2. Model each 1 seperately
  3. separate many-to-many --> numeric --> fact table
  4. Denormalize remianing tables 
* What historically is needed in data mart
* should be organized around busienss process
* 1 source should not feed into multiple data marts
* **Process centric**
* designers will struggle with fact tables that have been prematurely aggregated based on teh designer's unfortunate belief
* Common pitfalls
  1. User acceptance
  2. Assuming underlying data and technology is static
  3. load only summarized data into the presentation area's dimensional structures
  4. populate dimensional models on a standalone bases without regard to data architecture
  5. making queriable data in presentation area overly complex
  6. pay more attention to backroom operational performance and ease of development than front-room query perfromance and ease of use
  7. Run out of budget before building a viable presentation area based on dimensional models
  8. Tactical multi-year project instead of manageable iterative developments
  9. fail to embrace management visionary as the business sponsor of the data warehouse
  10. not focusing on the business's requirements and goals
  
## Chapter 2: 4 Step Process | Fundamentals
* Concepts
  * Four Step process for designing dimensional models
  * Transaction level fact tables
  * additive and non-additive facts
  * Sample dimension table attributes
  * Causal dimensions, such as promotion
  * Degenerate dimensions, such as the transaction ticket number
  * extending an exising dimension model
  * snowflaking dimension attributes
  * avoding the "too many dimensions" trap
  * surrogate keys
  * market basket analysis
* Four-Step Dimensional Design Process
  1. Select business process
    * supported by source data collection sysstem
    * Not organized by business department
    * Not by function
      1. raw materials 
      2. purchasing
      3. orders
      4. shipments
      5. invoicing
      6. inventory
      7. general ledger
     * Publish data once
  2. Declare grain of business process
    * Exactly what a fact table represents
    * **"How do you describe a single row on the fact table?"**
  3. Choose dimensions
    * **"How do business people describe data that results from process."**
    * Discrete text attributes
  4. Identify numeric facts for fact table row
    * **"What are we measuring"**
    * True to step 2 grain
    * Interview Business Persons
 * Retail Case Study
   1. Select the business process 
     * stock keeping units (SKU) and universal product codes (UPC) same grain
     * Data collection points
     * some of the most significant management decisioins have to do with pricing and promotions
     * The business process we're going to model is POS retail sales
       1. What are business requirements
       2. Available data
   2. Declare the grain
     * Preferably you should develop a dimensional models for the most atomic information captured by a business process.  Atomic daa is the most detailed information collected; such data cannot be subdivided further
     * aggregated data good for performance tuning
     * individual line item
   3. Choose the dimensions
     * Dimensions
       1. date
       2. product
       3. store
       4. promotion
     * If the additional dimension violates the grain by causing additional fact rows to be generated, then the grain statement must be revised to accommodate this dimension
   4. Identify the facts
     * Facts collected
       1. the quantity
       2. per unit sales price
       3. the sales dollar amount
     * store calculations that are fact in database
     * storing it eliminates the possibility of user error
     * refer to gross profit consistently
     * all users access the data via this view and no users with ad hoc query tools can sneak 
     * percentage and ratios, such as gross margin are nonadditive
     * ratio of the sums, not the sum of the ratios
     * sales dollars and sales quantities before dividing the total dollars by the total quantity sold
     * to estimate the number of rows in our largest table, the fact table
     * by dividing the chan's annual gross revenue by the average item selling price
* Date Dimension
  * build the date dimension tables in advance
  * Don't be cryptic
    * simply populating the holidat indicator with a Y or an N would be far less useful
    * The selling season column is set to the name of the  retailing seaon
  * Non standard date attributes
    * SQL date functions do not support filtering by attributes such as weekdays versus veekends, holidays, fiscal periods, seasons, or major events
    * Look them up in a date dimension table  
* Time dimension table
  * Join the date and time table through the fact table
* Product Dimension 
  * The product dimension is almost always sourced from the operational product master file
  * Even more than one explicit hierarchy in our product dimension table
  * A recommended partial product dimension
  * Each attribute is a rich source for constraining and constructing row headers
    * Viewed in this manner, we see that drilling down is nothing more than asking for a row header that provides mor information
    * product dimension is one of the tow or three primary dimensions in nearly every data part
      * Great care should be taken to fill the dimensions with as many descriptive attributes as possible
* Store Dimensions
  * Because of this, we can roll stores up to any geographic attribute, such as zip code, country, and state in the United States
  * It is not uncommon to reprsent multiple hierarchies in a dimension table.  Ideally, the attributes names and values should be unique across the multiple hierarchies
  * if a numeric column is a constant attribute and is used as a report constraint or row header more often than it is used as an additive element in a summation
    * Use dimension table 
    > CREATE VIEW FIRST_OPEN_DATE (FIRST_OPEN_DAY_NUMBER, FIRST_OPEN_MONTH ...)
      > AS SELECT DAY_NUMBER, MONTH, ...
      > FROM DATE
    * System will act as if there is another physical copy of the date dimension table called FIRST_OPEN_DATE
      * Will have relaled all columns in the view so that they cannot be confused with the columns from the primary date dimension
 * Promotion Dimension
   * Lift: whether the products under promotion experienced a gain in sales during the promotional period
     * baseline values an be estimated from prior sales history and, in some cases, with the help of sophisticated mathematical models
   * various possible causal conditions are highly correlated
   * Create one row in the promotion dimension for each combination of promotion conditions that occurs
   * The separated dimensions may be more understandable to the business community if users think of these mechanisms separately.  This would be revealed during the business requirements interviews
   * No NULL values in Fact Tables
   * **You must avoid null keys in the fact table.  A proper design includes a row in the corresponding dimension table to identify that the dimension is not applicable to the measurement**
* Promotion coverage factless fact table
  * **What products were on promootion but did not sell?**
  * Factless fact table --> relationship between involved keys
    * Load one row in the fact table for each product on promotion in a store each day
    * Two Step process
      1. query the promotion coverage table to determine the univers of products that were on promotion on a given day
      2. Determine what products sold from the POS sales fact table
    * If you're working with data in a multidimensional online analytical processing (OLAP) cube environment, it is often easier to answer the question regarding what didn't sell because the cube typically contains explicit cells for non behavior
* Degenerate Transaction Number Dimension
  * traditional parent-child database vs. dimensional model headers in a dimension table
  * POS transaction number as a ~~degenerate dimension~~
  * Degenerate dimension represents the unique identifier of the parent
    1. Order numbers
    2. Invoice number
    3. Bill of lading numbers
* Retail Schema in Action
  * Cross Tab Report
* Retail Schema Extensibility
  * we must avoid null keys in the fact table
  * Any descriptive attribute that has a single value in th epresence of the fact table measurements is a god candidate to be added to an existing dimension or be its own dimension
  * New dimension attributes: If new attributes are available only after a specific point in time, then "Not Available" or its equivalent should be populated in teh old dimension records
  * New measured facts: It is almost always a mistake to mix grains in the same fact table
  * Dimension becoming more granular: allow them to roll up perfectly in a many-to-one relationship
* Resisting Comfort Zone Urges 
  * Dimension Normalization (Snowflaking
    * Happens back in the staging area, long before the data is loaded into th epresentation area's dimensional schema
    * Redundant attributes are removed fromthe flat, denomrmalized dimension table and placed in normalized secondary dimension tables
    * 
     
    
    
    
    
    
    
