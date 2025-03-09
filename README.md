________________________________________
	 Data Loading and Importing in SQL
________________________________________
		Importing Data from Files:
				CSV Files: 
		 		LOAD DATA INFILE, COPY FROM (PostgreSQL)
				Excel Files: 
		 		Use external tools like pgAdmin or MySQL Workbench, or use libraries such as pandas (Python) to load Excel into SQL databases.
				JSON Files: 
		 		JSON_TABLE(), JSON_EXTRACT(), OPENJSON(), jsonb_populate_recordset() (PostgreSQL)
				Parquet/Avro Files: 
		 		External libraries (e.g., Apache Drill or PrestoDB) for native Parquet file support
				SQL Server: 
		 		BULK INSERT, OPENROWSET() for loading from CSV and other external files
				XML Files: 
		 		XMLTABLE(), OPENXML() (SQL Server)
________________________________________
	Data Cleaning and Preprocessing in SQL
________________________________________
		Handling Missing Data:
			NULL Detection: 
	 		IS NULL, IS NOT NULL
			Remove Rows with NULL: 
	 		WHERE column_name IS NOT NULL
			Replace NULL with a Value: 
	 		COALESCE(column_name, value), IFNULL(column_name, value)
			Filling Missing Values:
			Simple Imputation: Using COALESCE() or CASE WHEN
			Advanced Imputation: Using joins with statistical processing (often using external tools in combination with SQL)
			Handling Duplicates:
			Find Duplicates: GROUP BY, HAVING COUNT(*) > 1
			Remove Duplicates:
			DISTINCT for selecting non-duplicate rows
			Window Functions (ROW_NUMBER()): Deleting rows with the highest row number in a partition of duplicates
________________________________________
	Feature Engineering:
________________________________________
		Transformations:
			Logarithmic Transformations: LOG(), LOG10()
			Square Root Transformations: SQRT()
			Normalization: MAX(), MIN(), AVG()
			Bin Data: CASE WHEN, GROUP BY for bucketing data
			Categorical Encoding: Use of CASE WHEN statements to convert categorical data into numerical form
			Creating Interaction Terms: SELECT column1 * column2 AS interaction_term
	________________________________________
 	Descriptive Statistics in SQL
________________________________________
		Measures of Central Tendency:
________________________________________
			Mean: AVG(column_name)
			Median: No native function, but can be approximated by PERCENTILE_CONT() in SQL Server, NTILE() in PostgreSQL
			Mode: Use GROUP BY column_name and ORDER BY COUNT(column_name) DESC
________________________________________
		Measures of Dispersion:
________________________________________
			Variance: VAR_SAMP(column_name) or VAR_POP(column_name)
			Standard Deviation: STDDEV_SAMP(column_name) or STDDEV_POP(column_name)
			Range: MAX(column_name) - MIN(column_name)
			Interquartile Range: Using PERCENTILE_CONT() or NTILE() to compute quantiles
			Mean Absolute Deviation: (ABS(column_name - AVG(column_name)))
		Skewness and Kurtosis:
			Skewness: No direct SQL function, but can be calculated using third-party libraries or in external applications.
			Kurtosis: Similarly, not directly available in SQL, but can be computed using statistical functions or external integrations.
	Probability Distributions in SQL
________________________________________
	Discrete Distributions:
________________________________________
			Binomial Distribution: Approximation using a combination of POW() for powers and COMBIN() for combinations.
			Poisson Distribution: EXP(-lambda) * (POW(lambda, x) / FACTORIAL(x)) can be used for custom queries.
			Geometric and Negative Binomial: Use aggregate functions like SUM() and POW() to model certain conditions.
________________________________________
	Continuous Distributions:
________________________________________
			Normal Distribution: Use cumulative probability functions (EXP(), SQRT(), PI()) to approximate normal distribution.
			Exponential Distribution: Implement EXP() in SQL queries to model the exponential decay process.
			Gamma Distribution: Use a combination of POW(), EXP() and custom SQL functions.
			Uniform Distribution: RAND() or RANDOM() for generating uniform random numbers.
	Inferential Statistics and Hypothesis Testing in SQL
			SQL aggregation functions.
________________________________________
		t-Test (for means):
________________________________________
			One-Sample t-Test: Typically done with a combination of AVG(), COUNT(), STDDEV(), and SQRT()
			Two-Sample t-Test: Implement using JOIN, AVG(), STDDEV(), and COUNT() for both datasets, and calculate t-statistic manually
________________________________________
			Chi-Square Test:
________________________________________
			Test of Independence: Use GROUP BY and COUNT() to compute observed frequencies, then calculate expected frequencies, and compute chi-square statistic manually:
			X2 = SUM((observed - expected)^2 / expected)
			SELECT SUM(POW((observed - expected), 2) / expected) AS chi_square FROM your_table;
			Z-Test:
			Z-statistic is calculated using AVG(), STDDEV(), and COUNT():
			Z = (sample_mean - population_mean) / (population_stddev / sqrt(sample_size))
________________________________________
	ANOVA:
________________________________________
			Use GROUP BY and aggregate functions to calculate group means, then compute F-statistic:
			F = (Between-group variance) / (Within-group variance)
			Use PERCENTILE_CONT() for approximating quantiles in ANOVA.
			p-Value Calculation:
			Use EXP(), LOG() to compute p-values manually based on your test statistics.
	Regression Analysis in SQL
________________________________________
		Linear Regression:
________________________________________
			Simple Linear Regression: SQL does not support built-in regression, but you can use the least squares method (via AVG() and SUM() functions):
			Slope (b) = SUM((x - AVG(x)) * (y - AVG(y))) / SUM((x - AVG(x))^2)
			Intercept (a) = AVG(y) - b * AVG(x)
			Predicted value: y_pred = a + b * x
			Multiple Linear Regression: More complex, requiring a system of linear equations and typically solved using matrix operations or in external software
________________________________________
	Logistic Regression:
________________________________________
			Logistic Regression: Typically requires external support but basic logistic regression can be approximated using odds ratio and the logit transformation:
			log(odds) = log(p / (1 - p))
		Regression with SQL Functions:
			CORR(): To compute correlation for regression analysis.
			SUM(), AVG(), COUNT(), STDDEV(): For computing regression coefficients and model fitting.
________________________________________
	Machine Learning and Statistical Modeling
________________________________________
		Clustering:
			K-Means Clustering: K-Means clustering is not natively available in SQL, but it can be implemented with loops and aggregate queries, or by using external libraries 				in Python or R and pushing the results to SQL.
________________________________________
		Decision Trees:
________________________________________
			Decision Trees are not natively supported in SQL, but it can be approximated using conditional statements (CASE WHEN), GROUP BY, and iterative splits.
			Random Forest, SVM, Neural Networks:
			These are not supported in SQL natively. You will need external machine learning tools like R, Python, or SparkML to apply these models. Afterward, results can be 					imported back into SQL for storage or querying.
________________________________________
			Cross-Validation:
________________________________________
			SQL can be used to split data into training and test sets using RAND() for random sampling, but more complex validation needs to be done outside SQL.
________________________________________
		Time Series Analysis:
________________________________________
			Moving Averages: AVG() over a window defined by PARTITION BY or using WINDOW functions like ROW_NUMBER().
			Autoregressive Models: SQL is not designed for advanced time series analysis like ARIMA, but basic trends and time windows can be identified.
			Advanced Statistical Techniques and Models in SQL
________________________________________
		Survival Analysis:
________________________________________
			Use custom queries to calculate survival functions (e.g., Kaplan-Meier) and apply log-rank tests, but SQL is not designed for detailed survival analysis, and it's 					recommended to use statistical software.
			Factor Analysis, PCA:
			Not directly possible in SQL, but can be approximated through matrix operations in external tools and then pushed back to SQL.
________________________________________
		Bayesian Methods:
________________________________________
			Use custom statistical models to implement posterior distributions and inference, but SQL does not support full-fledged Bayesian analysis.
	SQL-Based Statistical Extensions
		SQL Server: Integration with R Services or Python Services for statistical analysis.
		PostgreSQL: Integration with PL/R or PL/Python for running external statistical models.
		SQLite: Integration with RSQLite or use of external tools like Python to interface with SQLite for statistical processing.
