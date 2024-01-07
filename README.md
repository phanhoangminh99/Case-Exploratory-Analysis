<h1> Case Exploratory Analysis

# Background

As a new business analyst for an e-commerce enterprise, X, my first task is to create a detailed analysis of the company's business and operations from available database. This analysis will be presented to the Sales Manager and Operations Manager. The key elements to be covered in the analysis include a business overview, an evaluation of customer satisfaction, and 3 business recommendations on areas where the company could enhance its future performance.

# Analysis 

### Business situation of the company 
### 1. Business situation by year and month of the company 

```php
import matplotlib.ticker as mtick
from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()

ax = turnover.T.plot.bar(figsize=(15,10), xlabel="Years", ylabel="Payment_value", color=sns.color_palette('Set2'))
ax.yaxis.set_major_formatter(mtick.StrMethodFormatter('{x:,.0f}'))
ax.set_xlabel('Years',fontsize=20)
ax.set_ylabel('Payment Value', fontsize=20)
ax.set_xticklabels(ax.get_xticklabels(), rotation=0, ha='center')
plt.show()
```
![image](https://user-images.githubusercontent.com/110837675/226807037-ff44e509-7594-4bad-8253-4e75370a6508.png)

#### A. Business situation by year (Tình hình kinh doanh theo năm)
```php
import plotly.express as px

turnover_year = df.groupby('year')['payment_value'].sum().reset_index()

fig = px.bar(turnover_year, x='year', y='payment_value', title='Revenue By Month')

fig.update_layout(xaxis=dict(type='category'),width=900,height=700,font=dict(color='black'))
fig.update_traces(marker=dict(line=dict(width=50)),text=turnover_year['payment_value'],textfont=dict(color='black'))
fig.show()
```
output:

![image](https://user-images.githubusercontent.com/110837675/226799131-02b61b01-05c8-45e6-a4b2-9925f390ec04.png)

At first glance, the company's business has seen substantial growth over the years. Specifically, within one year, from 2016 to 2017, revenues saw a significant jump from $18202,78 to $2,124,535. By September 2018, the revenue had surged to $2,659,783, a 25.2% increase compared to the previous year. 

Additionally, the upward trajectory in the first figure also suggested that increased payment value of sales order over recent years has greatly contributed to the company's overall financial success.


#### B. Business situation by month

```php
import plotly.express as px
turnover_month= df.groupby('month')['payment_value'].sum().reset_index()
fig= px.line(turnover_month,x='month',y='payment_value', markers=True, title='Revenue By Month')
fig.update_traces(marker=dict(line=dict(width=10)),text=turnover_year['payment_value'],textfont=dict(color='black'))
fig.show()
```
output:

![image](https://user-images.githubusercontent.com/110837675/226800408-c21a3a11-4462-4032-9317-4da4328bb53d.png)

Monthly revenue steadily climbed from January through August, with August seeing a significant jump from previous months. 

It's important to note that we currently only have data until September 2018, which limits our ability to draw any conclusions. With that said, a clear upward trend in monthly revenue leading up to the strong August performance points to a positive development for the company. 



### 2. Top revenue-yielding product groups

```php
import plotly.graph_objs as go

fig = go.Figure(
    data=[go.Pie(labels=product['product_category_name'], values=product['payment_value'], pull=[0.09, 0])])
fig.update_layout(title="Percentage Revenue By Product_Category",width=1140, height=600, legend_title_text='Product Category Name')
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226800499-02eee2f8-bd62-4642-87a1-cdd38a08da9e.png)

The pie chart figure highlights the company’s top-performing product groups. These are the products that play a crucial role in the company’s growth because the revenue generated from them helps lift the product groups that are not performing as strongly. To make the most of these successful products, consider the following suggestions:

- Create a product ecosystem around these successful products. Specifically, focus on developing complementary products or additional accessories that can generate increased demand.  
- Offer incentives in the form of discounts or vouchers when customers buy complimentary products with those products.


  
### 3. Number of orders delivered successfully and canceled

```php
fig= px.pie(values=delivered['proportion'],names=delivered.index, hole=.3, title='Order Status')
fig.update_layout(annotations=[dict(text='Status', x=0.50, y=0.5, font_size=20, showarrow=False)])
fig.update_layout(
    width=1140,
    height=600, legend_title_text='Status')
fig.show()
```

![image](https://user-images.githubusercontent.com/110837675/226800799-a2f2e271-d275-4073-9ae8-f8d23e0f2243.png)

Canceled orders accounts for almost half of the total successfully delivered orders. It's important to find out the reasons behind these cancellations and address the issues to reduce the number of canceled orders in the future. Here are the customer IDs with the highest frequency of order cancellations:

```php
fig= px.pie(values=customer_cancel['order_status'],names=customer_cancel['customer_id'], hole=.3, title='CustomerId_Cancel')
fig.update_layout(annotations=[dict(text='Cancel', x=0.50, y=0.5, font_size=20, showarrow=False)])
fig.update_layout(
    width=1140,
    height=600,legend_title_text='Customer_id')
```


![image](https://user-images.githubusercontent.com/110837675/226801089-a5d3fdbf-c57c-47fe-9fa2-17dcaac9eb4c.png)

Having identified the customer IDs associated with the highest number of order cancellations, I recommend issuing warnings and/or penalties as a proactive measure to prevent these behaviors from happening again in the future.



### 4. Payments Methods

```php
payment_type= df.groupby('payment_type')['customer_id'].count().reset_index()
fig= px.bar(payment_type, x="payment_type",y='customer_id',title='Payment Type of customer')
fig.update_layout(xaxis=dict(type='category'), width=1000, height=700)
fig.update_traces(marker=dict(line=dict(width=0.6)))
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226801448-ac80ff3e-8f51-43fe-87ba-bffb73d80567.png)

Examining the figure, it's clear that credit card is the most commonly used payment method, reflecting the prevalent online shopping habit of using credit cards. Here are my suggestions:

- Reduce cash transactions by giving customers more incentives to make purchases via credit or debit cards. This will ensure less failed transactions as a result of insufficient cash or payment issues.
- Introduce promotions specifically tailored for credit/debit card payments.



### 5. Customer review title 

```php
tile=df['review_comment_title'].value_counts().nlargest(10).reset_index()
fig = px.treemap(tile, path=[px.Constant('Review Comment Title'),'index'], values='review_comment_title')
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226882999-87545479-1d8d-4153-a0ee-63cc66b862bd.png)


Context for labels in the figure:

 - recomendo: Recommended
 - super recomendo: Highly recommended
 - não recomendo: Not recommended
 - excelente : Excellent
 - bom: Good
 - muito bom : Very good
   

From the data table, “recomendo” and “super recomendo” are frequently rated by customers; however, there is also a high number of  "não recomendo" ratings, suggesting that a high amount of dissatisfactory products.

To address this issue, sales team should proactively engage with customers to identify the reasons behind the negative feedback. By understanding and addressing product concerns, the goal is to enhance and tailor products to better meet buyer needs, ultimately increasing sales revenue.

For products that received positive reviews, continue to maintain the quality of that product and continue to develop higher quality to earn more revenue. 

This approach ensures ongoing customer satisfaction and opens avenues for increased revenue through enhanced product quality.



### 6. Customer Satisfaction Level

```php
fig = go.Figure(data=[go.Pie(labels=star['review_score'], values=star['count'], pull=[0.1, 0.2, 0, 0, 0])])
fig.update_layout(title="Number of star customer evaluate")
fig.update_layout(height=600, width=1130,legend_title_text='Star')
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226801881-81e6d259-88f7-4eea-85dc-a801d5ee5dbb.png)

For product groups that are rated 1 and 2 star, a reevaluation is needed. It's crucial to understand the reasons behind these low ratings and identify the specific product groups or seller IDs associated with such reviews. This information will be useful for product quality improvement, ensuring it aligns more closely with customer needs and preferences.



### 7. List of product groups rated 1 and 2 star 

```php

star12 = df.loc[(df['review_score'] == 1) | (df['review_score'] == 2)]
star12 = pd.crosstab(star12.product_category_name, star12.review_score).sort_values(by=1, ascending=False)
star12 = star12.nlargest(columns=[1,2], n=3)

fig = px.bar(star12, x=star12.index, y=[1,2], labels={'x': 'Product Category', 'y': 'Number of Reviews'})
fig.update_layout(title="Number of 1- and 2-Star Reviews By Product Category", legend_title_text='star')
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226807507-4a83294f-9d88-441b-a40e-7060f864493c.png)




### 8. List of seller ids with 1 and 2 star reviews.

```php
star12 = df.loc[(df['review_score'] == 1) | (df['review_score'] == 2)]
star12= star12.groupby('seller_id')['review_score'].count().sort_values(ascending=False).nlargest(5).reset_index()
fig = go.Figure(data=[go.Pie(labels=star12['seller_id'], values=star12['review_score'],pull=[0, 0.1, 0, 0, 0])])
fig.update_layout(title="1 And 2 Rated Star Of Seller Id")
fig.update_layout(height=600, width=1130, legend_title_text='Seller_id')
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226802119-772f396f-b894-4a64-8a3b-c67db9bf3a81.png)

These are the top 5 seller IDs with the highest ratings of 1 and 2 points. To address the issue, here are a few suggested measures: 
- Categorize the reasons behind bad reviews
- Seek explanations from seller and compensate customers who had bad experiences with the products (through vouchers, discounts, flexible product return policy, etc.)
- Ask sellers to present improvement plans or issue warnings. Consistent low ratings may result in banning the seller ID. 



### 9. Customers with the most orders.

```php
df['payment_installments']= df['payment_installments'].apply(lambda x:'pay one' if x ==1 else 'pay installments')
top_10_customers = df['customer_id'].value_counts().head(10)
top_10_df = df[df['customer_id'].isin(top_10_customers.index)]
fig = px.histogram(top_10_df, x='customer_id', color='payment_installments',title='Customer ID have most order')
fig.update_layout(width=1150, height= 700)
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226802365-743c07ea-c439-4629-8994-4e8f525025ed.png)



### 10. List of customers who bring the highest revenue for the company.

```php
df_sorted = df.groupby('customer_id')['payment_value'].sum().sort_values(ascending= False).reset_index().nlargest(columns='payment_value',n=10)
df_sorted['cumulative_percent'] = 100 * df_sorted['payment_value'].cumsum() / df_sorted['payment_value'].sum()
# create the bar chart for the payment values
bar_chart = go.Bar(x=df_sorted['customer_id'], y=df_sorted['payment_value'])

# create the line chart for the cumulative percentage
line_chart = go.Scatter(x=df_sorted['customer_id'], y=df_sorted['cumulative_percent'], yaxis='y2', name='% Cumulative')
# set the layout for the chart
layout = go.Layout(title='Top 10 Customers Who Bring Highest Revenue', yaxis=dict(title='Payment Value'), yaxis2=dict(title='% Cumulative', overlaying='y', side='right'), width=1150, height=600, margin=dict(l=50, r=50, b=50, t=50), plot_bgcolor='#f8f8f8')
# create the figure and plot the chart
fig = go.Figure(data=[bar_chart, line_chart], layout=layout)
fig.show()
```

![image](https://user-images.githubusercontent.com/110837675/226802529-83dae0be-4855-4ec9-89de-b671e407bec5.png)

This figure features the top 10 customers with the highest number of orders at the company. It is imperative to consistently nurture these customers to prevent potential attrition and ensure their continued loyalty to our brand.

Some effective strategies include offering milestone-based vouchers and sending follow-up emails to customers with prolonged inactivity. 

These approaches capitalize on the influential role of satisfied customers in generating word-of-mouth marketing, underscoring the significance of customer retention for attracting new business.



### 11. Percentage of customers paying in a lump sum and one-time payment. 

```php
fig= px.pie(values=tile['payment_installments'],names=tile['index'], hole=.3, title='Paying in a lump sum and one-time payment')
fig.update_layout(annotations=[dict(text='Payments', x=0.50, y=0.5, font_size=20, showarrow=False)])
fig.update_layout(
    width=1140,
    height=600, legend_title_text='Payments')
fig.show()
```
![image](https://user-images.githubusercontent.com/110837675/226803861-9f8c26ab-8901-43b1-9cfd-98fb825a95af.png)


- The figures reveal a near parity between one-time payments and installment payments. However, for customers opting for installments, it's crucial to issue reminders about timely debt and interest payments. This proactive approach is aimed at preventing inconvenience for both parties, avoiding the necessity of repeated calls and messages. Failure to adhere to timely payments may lead to increased interest and potential disturbances for both the company and the customer.

### 12. Cities that bring in the highest revenue for the company 

```php
# create the bar chart for the payment values
bar_chart = go.Bar(x=data_sorted['customer_city'], y=df_sorted['payment_value'])

# create the line chart for the cumulative percentage
line_chart = go.Scatter(x=data_sorted['customer_city'], y=data_sorted['cumulative_percent'], yaxis='y2', name='% Cumulative')
# set the layout for the chart
layout = go.Layout(title='Top 10 City That Bring The Highest Revenue', yaxis=dict(title='Payment Value'), yaxis2=dict(title='% Cumulative', overlaying='y', side='right'), width=1150, height=600, margin=dict(l=50, r=50, b=50, t=50), plot_bgcolor='#f8f8f8')
# create the figure and plot the chart
fig = go.Figure(data=[bar_chart, line_chart], layout=layout)
fig.show()
```
Output:

![image](https://user-images.githubusercontent.com/110837675/226806929-250ca3f4-31a0-4148-bb92-cc12ec381595.png)

The company's revenue mainly comes from these cities. To optimize efficiency, strategically placing warehouses in these locations is recommended. This approach not only saves on transshipment costs but also ensures faster delivery for customers, contributing to savings on their shipping expenses.


# Conclusion 

- In three years, the company has witnessed significant development, evidenced by the increase in revenue and the number of sales orders. However, a closer examination reveals fluctuations in monthly revenue, particularly with a decline towards the end of the year. This calls for a more in-depth review.

- It is recommended to allocate resources strategically around product groups that yield the highest revenues for the company. Additionally, prioritizing excellent customer care for high-revenue contributors is crucial, requiring the establishment of a dedicated team for regular communication and personalized incentives, such as special vouchers. Furthermore, attention to installment customers, including timely reminders for debt and interest payments, remains vital for overall customer satisfaction and financial stability.

- Some areas for improvement include addressing low ratings for certain products and seller IDs. Company should collect more customer feedback to ensure that the seller list is.of high quality. 

- Lastly, a focused approach to installment customers, involving regular checks and timely reminders, can be complemented with targeted promotions to incentivize one-time payments. This comprehensive strategy aims to strengthen customer relationships, enhance product quality, and ultimately contribute to sustained business growth.

  
