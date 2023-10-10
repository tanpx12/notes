# Overview
## Goal of Data science

- The final goal of DS might be classified into: 
	- Description
	- Prediction
- In order to achieve these goal, several tasks are required:
	- Data scraping 
	- Data pre-processing: cleaning, transforming, and integration
	- Machine learning
	- Visualization
- Data science may apply to any kind of data
	- Raw data
	- Text analysis
	- Image and video analysis
	- Graph analysis

**Methodlogy: insight-driven**
![[Screenshot from 2023-03-05 09-50-23.png]]
**Methodlogy: product-driven**
![[Screenshot from 2023-03-05 09-59-33.png]]
**Online platforms for DS competitions**
- KD nuggets: analytics, data science, data mining competitions
- Kaggle

## Introduction 
### What can we do with the data

**Data description**
- Data description consists of summarizing the data in an "understandable" way, either:
	- Through exploratory data analysis
	- Through data visualization

**Data segmentation**
- Data segmentation consist of grouping the similar records into homogenous groups (called cluster)
	- Records in a group have similar attribute values
	- Technically, the goal is to lean a "new" attribute from record's attributes
	- Unsupervised learning methods can be used

**Association rules**
- Association mining: discovering association rules between records, according to pre-defined criteria

**Prediction**
- Prediction consists in either: 
	- Predicting or estimating the values of an attributes for a set or records


# Data crawling and preprocessing
## Definitions
- Data scraping is a technique for extracting data from a document published in order to be read by humans

## Screen mining
- Consist in extracting the text from the displaying screen of a device
- Usual screen scraping methods use bitmap screenshots and OCR
- In some cases, a program is used to simulate the user's behaviour and to control the GUI
## Report mining
- Consists in extracting the text from the reports written and formatted to be readable by humans
### Web scraping
- Web pages are text files using a markup-based language that often contain relevant data
- However, most web pages are conceived for a final user, not for their automatic use

## Web Crawler

- A spider, also known as a robot or a crawler, is actually a program that follows , or "crawls" links throught the Internet, grabbing content from sites and adding it to the database
**Web crawler types**:
- Incremental crawler
- Distributed crawler
- Universal crawler
	- For universal search engines
	- Large-scale
	- Huge cost
	- Incremental updates
- Focused crawler
	- Focus on a particular, specialize data
**Basic crawling operation**
- Begin with known "seed" URLs
- Fetch and parse them
	- Extract URLs they point to 
	- Place the extracted URLs on a queue
- Fetch each URL on the frontier and repeat

**Crawling policy**
- Behaviour of a Web crawler is the outcome of a combinatin of policies
	- *Selection policy* states the pages to download
	- *Revisit policy* states when to check for changes to the pages
	- *Politeness policy* states how to avoid overloading the sites
	- *Parallelization policy* states how to coordinate distributed web crawlers

**Challenge**:
- Internet is huge
- Filterign interest/non-interest/malicious pages
- Content freshness
- Content deduplication
- Politeness - don't hit a server too often

**Robot.txt**
- Protocol for giving spiders "robots" limited access to a website, originally from 1994
- Website announces its request on what can(not) be crawled
	- For a server, create a file /robot.txt
	- This specifies access restrictions
## Web-scraping
- Consists in extracting relevant information from a web page in order to re-use this data in another framework and/or under another form

**Web crawling vs web scraping**
- Web crawling creates a copy of existing data
- Web scraping extract specific data for analysis
	- The final goal of web scraping is data science => it convert unstructured data available in the web into more structured data, that can be analysed

**Use cases**
- Private use: online services, comparing information from different websites
- Academic research: the web si a huge multi-domain data source that can provide researchers with huge volume of data
- Marketing analysis: the traces we leave on the web are getting more and more numerouse and allow companies to grasp our preference,... => personalizing ads

**Extraction modes**
- Semi-automatic extraction: using a software or an app to suck clean selected contents from one or several web pages
- Automatic extraction: using a software or an app to create a corpus of web-pages linked with each other

**Extraction techniques**
- XPath - most use method
- XPath uses the hierachical structure of nodes of an XML document, therefore requires a precise document structure
- HTML documents partly respect a hierachical format with XML tags

**Technical limitations**
- Inconsistent/chaotic organization of the information
- Evolutions in the information structure
- Access restricton 
- Dynamically generated contents

**Web scraping tools**
- Programming: Scrapy, beautifulsoup (Python), PhantomJS,...
- Cloud: ScrapingHub, Dexi.io,...
- Stand-alone: ParseHub, OctoParse,...
- Web-browser plugins: Data Scraper-Easy Web Scraping, Instant Data Scraper, Web Scraper
### Scrapy
- Scrapy engine: controlling the data flow between all components of the system
- Scheduler: receives requests from the engine and enqueues them for feeding them later
- Downloader: fetching web pages and feeding them to the engine
- Spider: parse responses and extract items or additional request to follow
- Item pipeline: processing the items once they have been extracted by the spiders
- Download middlewares: process request when they pass from the engine to the Downloader and vice-versa
- Spider middlewares: 
	- Specific hook that sit between the Engine and the Spiders
	- Process spider input and output

# Data cleaning and integration

## Data integration
- Provide uniform access to data available in multiple, autonomous, heterogeneous and distributed data sources
- Why data integration ?
	- To facilitate information access and reuse through a single information access point
	- Data from different complementing information systems is to be combined to gain a more comprehensive basis to satisfy the need
**Challenge**
- Physical systems
	- Various hardwares, standard
	- Distributed deployment
	- Variouse data format
- Logical structures
	- Different data models
	- Different data schemas
- Business organization
	- Data security and privacy
	- Business rules and requirements
	- Different administrative zones in the business organization

### Approaches
**Data warehouse**
- Realize a common data approach
- Data from several operational sources are extracted, transformed,  and loaded into a data warehouse
- Analysis, such as OLAP, can be performed on cubes of integrated and aggregated data
- How to load data into DW?
	- Scripts in linux shell, python,...
	- sqlldr + SQL
	- Hardcoded in Java, C#, C
	- In-house built ETL tool
	- Off-the shelf ETL tool
- ETL process
	- ETL = Extract - Transform - Load
	- 70-80% of DW is reliable ETL
	- Extract: Get the data from source system as efficiency as possible
	- Transform: Perform calculations on data
	- Load: Load the data in the target storage
- Problems: 
	- Data has ot be cleaned - different formats
	- Needs to store all the data in all the data sources that will ever be asked for -> Expensive 
	- Data needs to be updated periodically 

**Virtual integration**
![[Screenshot from 2023-03-05 16-02-57.png]]
- Architecture:
	- Leave the data in the data sources
	- For every query the mediated schema
		- Find the data source that have the data
		- Query the data sources
		- Combine results from different sources if necessary
- Challenge:
	- Designing a single mediated schema
		- Data sources might have different schemas, and might export data in different formats
		- Translation of queries over the mediated schemas to queries over the source schemas
		- Query optimization
			- No/limited/stale statistic about data source
			- Cost model to include network communication cost
			- Multiple data source to choose from
		- Query execution
			- Network connections unreliable - inputs might stall, close, be delayed, be lost
			- Query result can be cached
		- Query shipping
			- Some data sources can eb execute queries - send the sub-queries
			- Sources need to describe their query capability and also their cost models
		- Incomplete data sources
			- Data at any source might be partial, overlap with other, or even conflict
- Wrapper: 
	- Source export data in different formats 
	- Wrapper are custom-built programs that perform tranform data from  the source native format to something acceptable to the mediator
	- Can be placed either at the source or at the mediator
	- Maitainace problems
		- Have to change if source interface change
- Data source catalog
	- Contains meta-information about sources
 - Schema mediation:
	 - User pose queries over the mediated schema
	 - The data at a source is visible to the mediator is its local schema
	 - Reformulation: Queries over the mediated schema have to be rewritten as queries over the source schemas

## Data cleaning

**Why ?**
- Incomplete
	- Lacking attribute values, lacking certain attributes of interest, or containing only aggregate data
	- Different considerations between the time when the data was collected and when it is analyzed
	- Human/hardware/software bugs
- Noisy
	- Containing errors or outliners
	- Faulty data collection instruments
	- Human error at data entry
	- Error in data transmission
- Inconsistent: 
	- Different data sources
	- Functional dependency violation
- Duplicate record also need data cleaning

=> No quality data, no quality mining results

### Taxonomy of data quality problems

- Value-level
	- Missing value: value not filled in a not null attribute
	- Syntax violation: value does not satisfy the syntax rule defined for the attribute
	- Spelling error
	- Domain violation: value does not belong to the valid domain set
- Value-set and record level
	- Value-level
		- Existence of synonyms: attribute takes different values, but with the same meaning
		- Existence of homonyms: same word used with diff meaning
		- Uniqueness violation: unique attribute takes the same value more than once
		- Integrity constraint violation
	- Record-level
		- Integrity constraint violation
- Relation level
	- Heterogeneous data representations: different way of representing the same real world entity
	- Functional dependency violation
	- Existence of approximate duplicates
	- Integrity constraint violation
- Multiple relation level
	- Heterogeneous data representation
	- Existence of synonyms
	- Existence of homonyms
	- Different granularities: same real world entity represented with diff granularity levels
	- Existence of approximate duplicates
	- Integrity constraint violation

### Tasks of data cleaning
- Fill in missing values 
	- Value-level problem
- Identify/correct noisy data
	- Value-level or record-level problem
- Correct inconsistent data

**Manage missing data**
- Ignore the tuple
- Fill in the missing value manually
- Use a global constant to fill the missing value
- Use the attribute mean to fill the missing value
- Use the attribute mean for all samples of the same class to fill in the missing value: smarter
- Use the most probable value to fill the missing value: inference based such as regression, Bayesian formula, decision tree

**Manage noisy data**
- Binning method
- Clustering
- Regresssion
- Semi automated

**Manage inconsistent data**
- Manual correction using external reference
- Semi-automatic using various tools
	- To detect violation of known functional dependencies and data constraint
	- To correct redundant data


### Methodology for data cleaning
- Extraction of the individual fields that are relevant
- Standardization of record fields
- Correction of data quality problems at value level
- Correction of data quality problems at value-set level and record level
- Correction of data quality problems at relation level 
- Correction of data quality problems at multiple relation levels
- User feedback



## Data pre-processing
- Data smoothing: remove noise from data
- Data aggregation: summarization 
- Data generalization: concept hierarchy climbing
- Data normalization: scaled to fall within a small specified range
- Data reduction: Diminish the size of the data
- Attribute engineering
**Tools**
- Open refine
- Trifacta Wrangler
- Python libraries


# Exploratory data analysis
- Focus on data - its structure, outliers, and models suggested by the data
- EDA approach makes use of all the available data. In this sense is no corresponding loss of information.
	- Summary statistics 
	- Visualization 
	- Clustering and anomaly detection
	- Dimensionality reduction
## Definition
- The EDA is precisely not a set of techiques, but an attitude/phylosophy about how a data analysis should be carried out.
- EDA is an iterative process
	- Identify and prioritize relevant questions in decreasing order of importance
	- Ask question
	- Construct graphics to address questions
	- Inspect "answer" and derive new question
## Describing univariate data
**Observations and variables**
- Data is an collection of observations
- An attribute is thought of as a set of values describing some aspect across all observations, it is called a variable

**Dimensionality of data sets**
Univariate: Measurement made on one variable per subject
Bivariate: Measurement made on two variable per subject
Multivariate: Measurement made on many variable per subject

**Measures of central tendency**
- Measure of location: estimate a location parameter for the distribution to find a typical or central value that best describes the data.
- Measures of scale: characterize the spread or variability of a data set. Measures of scale are simply attempts to estimate this variability.
- Skewness and Kurtosis

**Bar chart**
- displays the relative frequencies for the different values
- or a chart presents categorical data with rectangular bars with heights or lengths proportional to the values that they present 
**Histogram plot**
- A histogram is to graphically summarize the distribution of a univariate data set.
- The histogram can be used to answer the following question:
	- What of population distribution do the data come from?
	- Where are the data located?
	- How spread out are the data?
	- Are the data symmetric or skewed?
	- Are there outliers in the data?
**Box plot**
- Displayed : the lowest value, the lower quartile (Q1), the median (Q2), the upper quartile (Q3), the highest value, and the mean
- The box plot can provide answers to the following question :
	- Is a factor significant?
	- Does the location differ between subgroup?
	- Does the variation differ between subgroup?
	- Are there any outliers?
**Skewness**
- Skewness is a measure of asymmetry. A distribution, or data set, is a symmetric if it looks the same to the left and right of the center point

## Understanding Relationships
**Scatter plot**
- Identify whether a relationship exists between two continuous variables measured on the ratio or interval scales
	- Two variables are plotted on the x-axis and y-axis.
	- Each point is a single observation.
- Scatter plot matrix:
	- A collection of scatterplots organized into a grid 
	- Each scatterplot shows the relationship between a pair of variables

**Lag plot**:
- For data values $Y_1,Y_2,...,Y_n$ the k-period (or $k^{th}$) lag of the value $Y_i$ is defined as the data point that occurred k time points before time i. That is $Lag_k(Y_i)=Y_{i-k}$. For example, $Lag_1(Y_2)=Y_1$  and $Lag_3(Y_{10})=Y_7$ 
- Lag point plots can provide answers to the following questions: 
	- Are the data random?
	- Is there serial correlation in the data?
	- What is a suitable model for the data?
	- Are there outliers in the data?

**Contour plots**:
- Show a three-dimentional surface on a two-dimentional plane. Contour lines indicate elevations that are the same.

## Identifing and understanding groups
### Motivation
- Decomposing a data set into simpler subsets helps make sense of the entire collection of observations
	- Uncover relationshops in the data such as groups of consumers who buy certain combinations of products
	- Identify rules from the data 
	- Discover observations dissimilar from those in the major identified groups
### Clustering
- A way of grouping together data samples that are similar in some way - according to some criteria
- A form of unsupervised learning - you generally don't have examples demonstrating how the data should be grouped together.
- Type of clustering
	- Hierachical clustering
	- Flat clustering
**Hierachical clustering**
- An agglomerative approach
	- Find closest two thing
	- Put them together
	- Find next closest
- Requires 
	- A defined distance
	- A merging approach
- Produces
	- A tree showing how close things are to each other

**K-mean clustering**
- A partitioning approach
	- Fix a number of cluster
	- Get centroids of each cluster
	- Assign things to closest centroid
	- Recalculate centroids
- Requires 
	- A defined distance metric
	- A number of clusters
	- An initial guess as to cluster centroid
- Produces
	- Final estimate of cluster centroid
	- An assignment of each point to clusters