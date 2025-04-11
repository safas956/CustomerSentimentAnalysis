## Customer Sentiment Analysis

## **Overview**

Customer Satisfaction Analysis involves collecting, analyzing, and interpreting data on customer experiences and perceptions through surveys, feedback forms, ratings, and reviews. By identifying key drivers of satisfaction and dissatisfaction, businesses can make informed decisions to improve products, services, and customer interactions.

It helps retain customers, boost loyalty and advocacy, drive sales growth, and gain a competitive edge, which ultimately enhances overall business performance and customer experience.

To get started with the task of Customer Satisfaction Analysis, we need a dataset based on customer satisfaction and feedback. I found an ideal dataset for this task, which contains features like:

**CustomerID**: Unique identifier for each customer.

**Age**: Age of the customer.

**Gender**: Gender of the customer (Male/Female).

**PurchaseAmount**: Total amount spent by the customer.

**PurchaseFrequency**: Number of purchases made by the customer.

**ProductQualityRating**: Customer rating for product quality (1-5).

**DeliveryTimeRating**: Customer rating for delivery time (1-5).


**CustomerServiceRating**: Customer rating for customer service (1-5).

**WebsiteEaseOfUseRating**: Customer rating for website ease of use (1-5).

**ReturnRate**: Proportion of products returned by the customer.

**DiscountUsage**: Amount of discount used by the customer.

**LoyaltyProgramMember**: Whether the customer is a loyalty program member (Yes/No).


# **Modules**

## **1.Importing Data**

Utilized the csv file and imported the pandas library to read the data into the jupyter notebook.

```
import warnings 
warnings.filterwarnings('ignore')
import pandas as pd
data=pd.read_csv("E-commerce_NPA_Dataset.csv")
data
```

## **2. Data Exploration**

Performed Exploratory Data Analysis by checking for number of rows and columns, information of int or object or float values, descriptive statistics.

```
data.shape
data.head()
data.info()
data.describe()
```

## **3. Data Visualization**

Using Histogram, visualized customer experiences and diverse behaviours, with varying levels of satisfaction across different service aspects.
```
import matplotlib.pyplot as plt
num_columns = ['Age', 'PurchaseAmount', 'PurchaseFrequency', 'ProductQualityRating', 'DeliveryTimeRating', 
                'CustomerServiceRating', 'WebsiteEaseOfUseRating', 'ReturnRate', 'DiscountUsage']
plt.figure(figsize=(15,18))

for i, col in enumerate(num_columns, 1):
    plt.subplot(5, 2, i)
    plt.hist(data[col], bins=20, edgecolor='k', alpha=0.7)
    plt.title(f'Distribution of {col}')
    plt.xlabel(col)
    plt.ylabel('Frequency')

plt.tight_layout()
plt.show()
```
## **4. Customer Segmentation**

Segreggating the customers based on demographic and behavioral factors, and analyzing their satisfaction ratings. We have created segments based on age, gender.

```
# create age groups

bins = [18, 30, 40, 50, 60, 70]
labels = ['18-29', '30-39', '40-49', '50-59', '60-69']
data['AgeGroup'] = pd.cut(data['Age'], bins=bins, labels=labels, right=True)

# select only the numeric columns for calculation of ratings
numeric_columns = ['ProductQualityRating', 'DeliveryTimeRating', 'CustomerServiceRating', 'WebsiteEaseOfUseRating']

# calculate mean ratings by age group and gender
mean_ratings_age_gender = data.groupby(['AgeGroup', 'Gender'])[numeric_columns].mean()

# reset the index to display the dataframe
mean_ratings_age_gender.reset_index(inplace=True)
print(mean_ratings_age_gender)
```
## **5.Net Promoter Score(NPS)**

NPS is a metric used to gauge customer loyalty and satisfaction by asking customers how likely they are to recommend a company’s product or service to others on a scale of 0 to 10. Respondents are classified into three categories:

Promoters (9-10) Passives (7-8) Detractors (0-6)

The NPS is calculated by subtracting the percentage of Detractors from the percentage of Promoters. A higher NPS indicates more customer loyalty and positive word-of-mouth, which are critical for business growth.

To calculate the NPS, we will use customer service ratings as a proxy for overall satisfaction.

```
# define NPS categories based on customer service rating
data['NPS_Category'] = pd.cut(data['CustomerServiceRating'], bins=[0, 6, 8, 10], labels=['Detractors', 'Passives', 'Promoters'], right=False)

# calculate NPS
nps_counts = data['NPS_Category'].value_counts(normalize=True) * 100
nps_score = nps_counts['Promoters'] - nps_counts['Detractors']

nps_counts
```
## **6. Satisfaction Analysis**

We performed root cause analysis on customer dissatisfaction by identifying the key factors contributing to low ratings in specific areas such as product quality, delivery time, customer service, and website ease of use. We’ll analyze the characteristics of customers who provide low ratings and look for patterns that can help us understand the root causes of dissatisfaction.

```
# define low rating threshold
low_rating_threshold = 2

# create subsets for low ratings in different aspects
low_product_quality = data[data['ProductQualityRating'] <= low_rating_threshold]
low_delivery_time = data[data['DeliveryTimeRating'] <= low_rating_threshold]
low_customer_service = data[data['CustomerServiceRating'] <= low_rating_threshold]
low_website_ease_of_use = data[data['WebsiteEaseOfUseRating'] <= low_rating_threshold]

# plot the characteristics for each low rating subset
plt.figure(figsize=(20, 15))

# age distribution for low ratings
plt.subplot(2, 2, 1)
plt.hist([low_product_quality['Age'], low_delivery_time['Age'], low_customer_service['Age'], low_website_ease_of_use['Age']], bins=10, label=['Product Quality', 'Delivery Time', 'Customer Service', 'Website Ease of Use'])
plt.title('Age Distribution for Low Ratings')
plt.xlabel('Age')
plt.ylabel('Frequency')
plt.legend()

# purchase amount distribution for low ratings
plt.subplot(2, 2, 2)
plt.hist([low_product_quality['PurchaseAmount'], low_delivery_time['PurchaseAmount'], low_customer_service['PurchaseAmount'], low_website_ease_of_use['PurchaseAmount']], bins=10, label=['Product Quality', 'Delivery Time', 'Customer Service', 'Website Ease of Use'])
plt.title('Purchase Amount Distribution for Low Ratings')
plt.xlabel('Purchase Amount')
plt.ylabel('Frequency')
plt.legend()

# purchase frequency distribution for low ratings
plt.subplot(2, 2, 3)
plt.hist([low_product_quality['PurchaseFrequency'], low_delivery_time['PurchaseFrequency'], low_customer_service['PurchaseFrequency'], low_website_ease_of_use['PurchaseFrequency']], bins=10, label=['Product Quality', 'Delivery Time', 'Customer Service', 'Website Ease of Use'])
plt.title('Purchase Frequency Distribution for Low Ratings')
plt.xlabel('Purchase Frequency')
plt.ylabel('Frequency')
plt.legend()

# return rate distribution for low ratings
plt.subplot(2, 2, 4)
plt.hist([low_product_quality['ReturnRate'], low_delivery_time['ReturnRate'], low_customer_service['ReturnRate'], low_website_ease_of_use['ReturnRate']], bins=10, label=['Product Quality', 'Delivery Time', 'Customer Service', 'Website Ease of Use'])
plt.title('Return Rate Distribution for Low Ratings')
plt.xlabel('Return Rate')
plt.ylabel('Frequency')
plt.legend()

plt.tight_layout()
plt.show()
```

### Learning Outcomes

•	Customers giving low ratings span a wide age range, with notable peaks around ages 30-40 and 50-60, which suggests age-related dissatisfaction trends.

•	Purchase amount and frequency distributions reveal that low ratings are not limited to low spenders or infrequent buyers; even high spenders and frequent buyers express dissatisfaction, which shows service quality issues.

•	Return rate distribution shows that higher return rates correlate with low ratings, particularly for product quality.

•	Website ease of use, which indicates dissatisfaction with product and website experiences.

**To enhance customer satisfaction, we recommend focusing on:**

•	Customer experience improvements based on age groups to better meet their expectations. For example, simplify digital navigation for older users or refine product appeal for younger demographics.

•	Invest in stricter quality control and customer feedback loops to reduce return rates. Use return data to identify and correct recurring product issues.

•	Since dissatisfaction exists even among loyal and high-value customers, implement personalized outreach and loyalty incentives to retain these segments and restore trust.

•	Enhance customer service responsiveness and empower support teams to resolve issues more effectively to prevent dissatisfaction from escalating.


## Conclusion
This project successfully demonstrates my ability to solve real-world e-commerce problems and I was able to exhibit my data analytical skills through various python libraries.
