# hotel_chain_analysis_python
Analyzed hotel booking and revenue data using Python, Pandas, Matplotlib, and Seaborn to uncover key business insights related to occupancy, customer behavior, booking platforms, and revenue trends. Performed extensive data cleaning and preprocessing by handling missing values, removing invalid guest entries, and treating revenue outliers 

# Business Problem Statement

The hospitality industry faces challenges in maximizing occupancy, maintaining consistent revenue growth, and understanding customer booking behavior across different cities and hotel categories. Hotels often struggle to identify underperforming properties, inefficient booking channels, seasonal demand fluctuations, and factors affecting revenue generation.

**This project aims to analyze hotel booking and revenue data to:**
- Identify trends affecting occupancy and revenue
- Evaluate hotel performance across cities and room categories
- Understand customer booking patterns and platform preferences
- Detect operational inefficiencies and revenue opportunities
- Support strategic decision-making through data-driven insights

# Business overview

This project analyzes hotel booking and revenue performance data to uncover business insights for the hospitality industry. Using Python, Pandas, and data visualization techniques, the analysis focuses on understanding booking trends, occupancy rates, revenue generation, customer behavior, and city-wise hotel performance.

The dataset includes hotel properties, booking transactions, room categories, booking platforms, and date-level information. The project explores key hospitality KPIs such as occupancy percentage, booking trends, cancellation impact, and revenue contribution across multiple cities and hotel categories.

Through exploratory data analysis and visualizations, the project identifies patterns in customer demand, seasonal performance, and operational efficiency. The findings can help hotel management teams optimize pricing strategies, improve occupancy, reduce revenue leakage, and make data-driven business decisions.


## Data Cleaning Process

During Data exploration we found that no of guests are having negative values.So we clean the negative values

```python

df_bookings[df_bookings.no_guests<=0]
```

Identify the no of guest which are having negative value

```python

df_bookings=df_bookings[df_bookings.no_guests>0]
```

Dealing with records which are having no_of_guests more than 0

During Data exploration also we find maximum revenue is having absurd value

```python

avg,std=df_bookings.revenue_generated.mean(),df_bookings.revenue_generated.std()

higher_limit=avg+3*std
higher_limit

lower_limit=avg-3*std
lower_limit

df_bookings[df_bookings.revenue_generated>higher_limit]

df_bookings=df_bookings[df_bookings.revenue_generated<higher_limit]
```

During Data exploration we also find some null values in ratings_given columns.But it is ok to have Null values in ratings_given column
because every customer will not give rating

```python

df_bookings.isnull().sum()
```

But we find null values in capacity coloumns and we replace it with median value

```python

df_agg_bookings.isnull().sum()

df_agg_bookings["capacity"].fillna(df_agg_bookings["capacity"].median(),inplace=True)
```
