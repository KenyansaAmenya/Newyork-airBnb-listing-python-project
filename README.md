# Newyork-airBnb-listing-python-project

# Project overview
This project performs Exploratory Data Analysis (EDA) on New York Airbnb data to analyse trends and patterns in rental listings. I used  the following libraries: Pandas, Numpy, Matplotlib, and Seaborn for cleaning, visualization, and analysis.

# Objective
The goal of this project is to:

* Analyze room types, prices, and availability across different neighborhoods.
* Understand host behavior and listing patterns.
* Detect potential outliers in prices.
* Provide recommendations for guests and hosts based on insights.

# Dataset
The dataset contains 20,765 entries and 22 features, including:

* id: Unique identifier for each listing
* name: Title of the Airbnb listing
* host_name: Name of the host
* neighborhood_group: Group (borough) where the listing is located
* latitude/longitude: Geolocation of listings
* price: Nightly rental price
* room_type: Type of accommodation (e.g., entire home, private room)
* reviews_per_month: Average monthly reviews for the listing
* availability_365: Number of available days in the year

# Steps and Workflow

## 1. Importing all libraries/dependancies
```python
pip install pandas
pip install numpy
pip install seaborn matplotlib
```
### Loading all the dependancies
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib inline
```
## 2. Loading the datasets
```python
data = pd.read_csv('datasets.csv', encoding_errors='ignore')
```

## 3. Initial exploration to see if the dataset has loaded
```python
pd.head() # views the first five rows
&
pd.tail() # views the last five rows

data.shape

data.info()

# creating a statistical report/ summary of the data sets
data.describe()
```
## 4. Data Cleaning
Handle missing data: price, neighborhood, and beds columns had null values.
Fix data types: Converted last_review to a datetime object.
Remove outliers: Listings with prices > $1,000 were capped to avoid skewed visualizations.

```python
data.isnull().sum() # to check null valuesin the columns.

# for the missing values we either drop the rows or replace the missing values
# dropping all missing values rows
data.dropna(inplace=True)
# data.fillna()

# looking & dealing with duplicated data
data.duplicated().sum()

data[data.duplicated()]

# deleting all duplicated rows
data.drop_duplicates(inplace=True)
data.duplicated().sum()

# if you want to format data or data types  use
data.dtypes # to view

# type casting
# how to change
data['id'] = data['id'].astype(object)
data['host_id'] = data['host_id'].astype(object)
data.dtypes
# end of data cleaning
```

## 5. EDA (Exploratory Data Analysis)
### 1. Room type distribution:

* Visualized the count of each room type using bar plots.
* Identified Entire home/apt as the most common room type.

```python
# identifying outliers in price
# create a new data frame

df = data[data['price'] < 1500]

sns.boxplot(data=df, x='price')
plt.show()

sns.histplot(data = df, x='price')
plt.show()

plt.figure(figsize=(8, 5))
sns.histplot(data = df, x='price', bins=100)
plt.title('Price Distribution')
plt.ylabel("frequency")
plt.show()
```
### 2. Neighborhood group insights:

* Analyzed price variations by boroughs.
* Manhattan had the highest average prices.

```python
# average price per bed
df.groupby(by='neighbourhood_group')['price per bed'].mean()
```

### 3. Availability trends:

Used heatmaps to show correlations among price, availability_365, number_of_reviews, and beds.

```python
# Price distribution

plt.figure(figsize=(6, 3))
sns.histplot(data = df, x='availability_365')
plt.title('availability_365 Distribution')
plt.ylabel("frequency")
plt.show()
```

### 4. Price distribution:

* Used histograms to show the distribution of prices.
* Majority of the listings were priced between $50 - $300.

```python
# ['price per bed']
# creating a new column

df['price per bed']= df['price']/df['beds']
df.head()

```

### 5. Host listings:

* Analyzed hosts with multiple listings using boxplots to identify key contributors.

```python
# Price dependency on neighbourhood

sns.barplot(data=df, x='neighbourhood_group', y='price', hue='room_type')
plt.show()

```

### 6. Review behavior:

Used pair plots to show relationships between number of reviews, price, and availability.

```python
# Lets analyze number of review and see if they have any relationship with price

plt.figure(figsize=(8, 5))
plt.title("Locality and Review Dependency")
sns.scatterplot(data=df, x='number_of_reviews', y='price', hue='neighbourhood_group')
plt.show()

```

### 7. Data Visualization
* Pairplot: To see correlations among price, availability, and number of reviews.
* Heatmap: Showing correlations among numerical features.
* Histograms and Boxplots: To detect outliers in price.
* Bar Charts: Displaying room types and neighborhood group distributions.

# Key Findings and Insights
## Price Trends:

* Manhattan has the most expensive listings, followed by Brooklyn.
* Entire homes/apartments cost significantly more than private or shared rooms.

## Room Type Distribution:

* Entire homes/apartments are the most common, but private rooms offer budget-friendly options.

## utliers in Price:

* Few listings priced at $10,000+ were detected, indicating the need to filter such extreme values.

## Availability Patterns:

* Listings with high availability tend to have lower prices and more reviews, likely due to better guest experience.

## Host Behavior:

* Some hosts manage multiple listings, indicating a trend toward professional hosting.

# Recommendations
## For Guests:

* Look for listings with high availability and good reviews for a better experience.
* Private rooms in Brooklyn offer affordable stays compared to Manhattan.

## For Hosts:

* Improve availability and review response rates to attract more bookings.
* Manage pricing effectively to compete within the borough's market.

# Future Work

* Use machine learning to predict prices based on room type and location.
* Perform sentiment analysis on reviews to better understand guest experiences.
* Create an interactive dashboard using Plotly or Tableau for live monitoring.

# Conclusion
This project offers valuable insights into the New York Airbnb market, helping both guests and hosts make informed decisions. By using EDA techniques, we identified key trends and developed actionable recommendations. Future improvements can involve advanced analytics and predictive modeling to further enhance the findings.

# Project done by Felix Amenya