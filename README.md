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

## Some Important Business Query

### Business Question no 1

- Average Occupancy rate per city

 ```python

 df=pd.merge(df,df_hotels,on="property_id")

fig=df.groupby("city_x")["occ_pct"].mean().sort_values().plot(kind="bar")
```
![City Occupancy Chart](https://github.com/polonimmo115a/hotel_chain_analysis_python/blob/main/city_occupancy_chart.png)

**conclusion:** Bangalore is having least avg occupancy rate

**Why it is matter:**

- **Market demand assesment:** it reveals whether the destination is a high demand tourist hub or an emerging business district
- **Pricing & Revenue Strategy:** if a city has a consistently high occupancy rate,hotels can raise their average daily rate(ADR).Conversely low
  occupancy cities may require promotional discounts to stay competitive
- **Operational efficiency:** it guides staffing levels

**Business Recommendation:**

**For cities with high average occupancy**

- Implement dynamic pricing
- Focus on upselling(focus on maximizing revenue per available room)
- Target high value segments(shift marketing budget towards high spending corporate clients rather than relying on deep discount booking channels

**For cities with low average occupancy**

- introduce value add packages
- establish corporate and travel agent partnerships
- develop loyalty programs

### Business Question no 2
- When was the occupancy is better?weekend or weekdays?

  ```python

   df = pd.merge(df, df_date, left_on="check_in_date", right_on="date")

  df.groupby("day_type")["occ_pct"].mean().round(2)
  ```

**Conclusion:** weekend occupancy is better

**Why it is matter:**

- **Profit margin analysis** weekend guest typically spend more on premium food and beverage,spas and room upgrade making them more proftable
- **Resource allocation** it dictates the budgeting for housekeeping and staff scheduling
- **Dynamic Pricing** it helps to determine whether a hotel can apply premium rates on high demand days or not

**Business Recommendation:**

- **Mid week coporate packages** target the sluggish days(monday to thursday) by offering work from hotel packages,reliable wifi bundles and corporate loyalty programs
- **Data driven staffing**

### Business Question no 3
- Revenue realized per city

 ```python

  df_bookings_all=pd.merge(df_bookings,df_hotels,on="property_id")
df_bookings_all.head()

df_bookings_all.groupby("city")["revenue_realized"].sum().sort_values(ascending=False)
```
**Conclusion:** Mumbai performs good but hyderabad and delhi are not performing well

**Why it is matter:**

- It is a critical business question because it reveals actual cash flow and financial health of hotel across different locations,highlighting the market demand

**Business Recommendation:**

- **Optimize the pricing strategy** cities with higher realized-revenue should have premium pricing while lower revenue-realized cities can use promotional discounts
- **Change in cancellation policy** if cancellation are high then implement stricter cancellation policies and give early bird booking discounts
- **Relocate the marketing budgets**

### Business Question no 4
- Show month by month revenue

  ```python

  df_date["date"] = pd.to_datetime(df_date["date"])

  df_bookings_all["check_in_date"] = pd.to_datetime(df_bookings_all["check_in_date"],format='mixed')

  df_bookings_all = pd.merge(df_bookings_all, df_date, left_on="check_in_date", right_on="date")

  df_bookings_all.groupby("mmm yy")["revenue_realized"].sum()
  ```

 **Conclusion:** May month is having higher revenue genarated compare to other months

 **Why it is matter:** 
 - it reveals highly predictable seasonal demand trends,uncover cyclical booking patterns and highlights the impact of local events.It empowers the manager to align the roomrates,optimize the staffing and stabilize the cash flow throughtout the year

 **Business Recommendation:**
 - implement tiered pricing
 - introduce stategic packages for low revenue months
  
### Business Question no 5
-Revenue realized per hotel type

```python

df_bookings_category=pd.merge(df_bookings,df_hotels,on="property_id")
df_bookings_category.head(3)

df_bookings_category.groupby("category")["revenue_realized"].sum()
```

 **Conclusion:** business class is having less revenue over luxury class 
