# Marketing-Email-Campaign-Logistic Regression
The campany ran an email campaign to let the user know about a new feature implemented on the site. Emails got sent out to a random subset and in a random way. From the marketing team perspective, a success is if the user clicks on the link inside of the email. This link takes the user to the company site. Now the marketing manager wants to the performace of this email campaign and what drives the performance.

## Data Explore
1. 100,000 emails got sent out to users.
* Link_clicked_rate, which is our conversion rate, is 2.1%
* Email_open_rate is 10.3%

2. There were two types of email_text. 
* long_email count: 50,276
* short_email count: 49,724

3. There were two types of email_version.
* generic count: 50,209
* personalized count: 49,791

4. Email got sent out to four countries
* ES: 9,967
* FR: 9,995
* UK: 19,939	
* US: 60,099

5. The logging of this email records the receiver infomation about how many items in the past this user bought
* min: 0
* max: 22

## What drives the performance -- Logistic Regression

I plan to use Logistic Regression to detect 
* What variables are important to predict the conversion rate
* To what extend, those varialbes have impact on the onversion rate

### Data processing before running model
1. Over-sampling

In the model, the dependent variable will be link_click, which is a binary result with a very imbalance shape. According the the conversion rate 0.02, we only have 1 link click out of 5 email sent. This imbalance will cause ugly prediction(recall, precision and Fscore) from RF. We need to shuffle and re-sampling the dataset to create a balance training data.

2. Create Dummy variables for categorical variables

### The result of the model
1. Confusion Matrix Check
item | Value
| --- | --- |
Recall | 0.68
precision | 0.68
F1-score | 0.68

2. Feature analysis

Among those variable whose P values are below 0.05, we have variables:
* user_past_purchases         
* email_text_short_email
* email_version_personalized
* all days exclude Friday 
* user_country_UK
* user_country_US     
* hour_10
* hour_11
* hour_12
* hour_15
* hour_16
* hour_20
* hour_23
* hour_9

which hold positive coef., which means that they have positive impact on link_click

## Actionable suggest: how to update the email campaign
Now we know those variables above have positive impact on increasing conversion rate. The updated strategy is: Instead of sending emails randomly, emails will be sent to users in those criterias above. For example:
* email_text_short_email: send email with short email format to the user
* user_past_purchases: send email to users who had bought on the site
* user_country_US: send email to users in US
* hour_11: send email to users on 11am.
* etc

### Test on the existing data to see the effectiveness
By selecting data landing in those criteria, the updated strategy increase the conversion rate by 3.3%. Running a hypothesis test could tell if this 3.3% happens by chance:

Control group: the original dataset, which is the strategy that emails got sent out randomly.

Test group: The updated dataset, which is the data selected by criterias I generated just now.

* H0: Test <= Control
* H1: Test > Control

Result: Reject H0, which means the new strategy does increase the conversion.

## Other Actionable suggest
We should run a design and implement a real A/B test to get a more accurate result. 
* Give two groups the same sample size
* Have those two groups run at the same time to avoid any seansonality effect
