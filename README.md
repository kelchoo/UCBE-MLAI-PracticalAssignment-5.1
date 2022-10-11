# UCBE-MLAI-PracticalAssignment-5.1
Practical Application Assignment 5.1: Will the Customer Accept the Coupon?
his is a Solution to the Practical Application 5.1 exercise of Module 5, in the Berkeley Haas, Professional Certificate in Machine Learning and AI course

Will a Customer Accept the Coupon?

Context

Imagine driving through town and a coupon is delivered to your cell phone for a restaraunt near where you are driving. Would you accept that coupon and take a short detour to the restaraunt? Would you accept the coupon but use it on a sunbsequent trip? Would you ignore the coupon entirely? What if the coupon was for a bar instead of a restaraunt? What about a coffee house? Would you accept a bar coupon with a minor passenger in the car? What about if it was just you and your partner in the car? Would weather impact the rate of acceptance? What about the time of day?

Obviously, proximity to the business is a factor on whether the coupon is delivered to the driver or not, but what are the factors that determine whether a driver accepts the coupon once it is delivered to them? How would you determine whether a driver is likely to accept a coupon?

Overview

The goal of this project is to use what you know about visualizations and probability distributions to distinguish between customers who accepted a driving coupon versus those that did not.

Data

This data comes to us from the UCI Machine Learning repository and was collected via a survey on Amazon Mechanical Turk. The survey describes different driving scenarios including the destination, current time, weather, passenger, etc., and then ask the person whether he will accept the coupon if he is the driver. Answers that the user will drive there ‘right away’ or ‘later before the coupon expires’ are labeled as ‘Y = 1’ and answers ‘no, I do not want the coupon’ are labeled as ‘Y = 0’. There are five different types of coupons -- less expensive restaurants (under $20), coffee houses, carry out & take away, bar, and more expensive restaurants ($20 - $50).

Data Description

The attributes of this data set include:

User attributes

Gender: male, female
Age: below 21, 21 to 25, 26 to 30, etc.
Marital Status: single, married partner, unmarried partner, or widowed
Number of children: 0, 1, or more than 1
Education: high school, bachelors degree, associates degree, or graduate degree
Occupation: architecture & engineering, business & financial, etc.
Annual income: less than \$12500, \$12500 - \$24999, \$25000 - \$37499, etc.
Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
Number of times that he/she buys takeaway food: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
Number of times that he/she goes to a coffee house: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
Number of times that he/she eats at a restaurant with average expense less than \$20 per person: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
Contextual attributes

Driving destination: home, work, or no urgent destination
Location of user, coupon and destination: we provide a map to show the geographical location of the user, destination, and the venue, and we mark the distance between each two places with time of driving. The user can see whether the venue is in the same direction as the destination.
Weather: sunny, rainy, or snowy
Temperature: 30F, 55F, or 80F
Time: 10AM, 2PM, or 6PM
Passenger: alone, partner, kid(s), or friend(s)
Coupon attributes

time before it expires: 2 hours or one day


2.Investigate the dataset for missing or problematic data.


3. Data Exploration

My Assumptions:

3.1. when value is 'less1' across the data, we should replace with 0 since less 1 is really 0

3.2 in addition, when its never, we should replace it with 0 so we have a consistent representation of ranges and numbers

3.3 when value is 'gt8' we should replace with '>8'

3.4 ## replace ~ with -

data = data.replace('~', '-', regex=True)
data = data.replace('less1', '0', regex=True)
data = data.replace('never', '0', regex=True)
data = data.replace('gt8', '>8', regex=True)
data = data.replace('Education&Training&Library', 'Education & Training & Library', regex=True)
data = data.replace('below21', '<21', regex=True)
data = data.replace('50plus', '>=50', regex=True)
data.rename(columns = {'passanger':'passenger'}, inplace = True)
data['passenger'] = data['passenger'].replace('Alone', 'None')
data['Bar'] = data['Bar'].replace(np.nan, '0')
data['Restaurant20To50'] = data['Restaurant20To50'].replace(np.nan, '0')

3.5 rename car column to vehicle type to better reflect driver status and specify a non driver as do not drive indicating could be a taxi or uber driver but the person is able to accept or reject coupon
data.rename(columns = {'car':'vehicletype'}, inplace = True)
data['vehicletype'] = data['vehicletype'].replace(np.nan, 'do not drive')

4. What proportion of the total observations chose to accept the coupon?
Total Accepted Coupon:7210
Total Did Not Accept Coupon:5474
Total observations:12684
Total_proportion_accepted:57%

5. Use a bar plot to visualize the coupon column.
<img width="915" alt="image" src="https://user-images.githubusercontent.com/115063137/195062054-f3bdcbda-777e-4458-a5b5-60fe6a85eb8b.png">

6. use histogram to visualize temperature
<img width="926" alt="image" src="https://user-images.githubusercontent.com/115063137/195062716-edf41cb2-dcb7-4511-b674-b3251681dc15.png">
coffee house most popular with coupon offers when temps are high
carry out and takeaways are more popular in cold temps.

Investigating the Bar Coupons
2.  Bar coupon acceptance rate
<img width="883" alt="image" src="https://user-images.githubusercontent.com/115063137/195063955-3cabd2b2-6f18-430f-a041-e4429d8f3ec0.png">

3. Conclusion: Acceptance rate was higher for those that went 3 or fewer times a month
<img width="886" alt="image" src="https://user-images.githubusercontent.com/115063137/195064115-b5f7f75b-652e-41dc-ad1f-c2b57fb368f0.png">

4. <img width="895" alt="image" src="https://user-images.githubusercontent.com/115063137/195064278-2cf7b2f5-0d60-446d-8695-9cc5b0adfae7.png">
5. <img width="893" alt="image" src="https://user-images.githubusercontent.com/115063137/195064598-6e495fdc-5633-4ff1-a46a-b45c0652e967.png">
6. <img width="912" alt="image" src="https://user-images.githubusercontent.com/115063137/195064795-6218a8df-a616-451f-b2fc-19a29b6845cf.png">

# 7.Based on these observations, what do you hypothesize about drivers who accepted the bar coupons?
## Based on the above observations, the acceptance rate of coupon increases based on the following :
## - lower income groups tend to use the coupons more for food purchases 
## - the marital status does influence the use of alcohol and therefore the singles tend to frequent bars and have a high acceptance rate
## - even the lack of a job does cause the use of more coupons to indulge in alcoholic beverages at bars as evidenced by the high unemployed acceptance of coupons
## - age and frequency of visiting bars also play a role in accepting coupons as younger patrons of bars who visit often tend to use coupons more

## Independent Investigation

## Using the bar coupon example as motivation, you are to explore one of the other coupon groups and try to determine the characteristics of passengers who accept the coupons.

<img width="891" alt="image" src="https://user-images.githubusercontent.com/115063137/195065499-55539062-edff-4c12-9369-0061ed618c4d.png">

<img width="880" alt="image" src="https://user-images.githubusercontent.com/115063137/195065587-8dc5497f-a6b2-42a5-aae6-3d45e12c3a89.png">
 
## Higher acceptance rate of Coffee House coupons based on the following factors:
## - education level : close to half of the accepted coupons patrons were having some college, or bachelors or Graduate degree
## - occupation      : students and unemployed as well as computer and math related employees and sales folks tend to congregate at coffee house to network and spend time on their books for students.
