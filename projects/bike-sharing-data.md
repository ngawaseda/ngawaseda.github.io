---
layout: page
title: How do we ride shared bikes?
permalink: /bike-sharing-data
---
<h4 style="text-align: center;">An analysis on Chicago's Divvy bike sharing program</h4>
<div class="demo-container">
  <img src="/assets/images/bike_sharing/bike_riding.gif" alt="demo">
</div>


# Rationale
The sharing economy in the eco-conscious era inevitably led to the rise of bike-sharing business. In the US, the City of Chicago cooperates with Lyft Bikes and Scooters to operate the Divvy bike-sharing service to promote bbicycling as an alternative mode of transportation. 

Since its launch in 2013, Divvy has expanded its fleet to 5,824 bikes that are monitored and secured through a network of 692 stations throughout Chicago. The bikes can be picked up from one station and returned to another station at any time. Previously, Divvy's marketing approach focused on creating general awareness and attracting a broad range of customers by offering flexible pricing options, such as single-ride passes, full-day passes, and annual memberships. Casual riders who purchase single-ride or full-day passes are referred to as casual riders or Customers, while those who purchase annual memberships are members or Subscribers. Divvy's financial analysts have found that Subscribers are more profitable than Customers. 

While offering pricing flexibility helps to attract more customers, the marketing team believes that maximizing the number of annual members will be crucial to future growth. Therefore, this analysis aims at examining historical bike trip data to understand the differences between the two sets of riders and to determine how to convert Customers into Subscribers.

---

# Data source and validation

- Data source: data is available for download [here](https://divvy-tripdata.s3.amazonaws.com/index.html) for public use under agreement terms and conditions between Lyft Bikes and Scooters, LLC and the City of Chicago.
- Geo-coverage: The program covers City of Chicago area and offers 3 flexible plans: single-ride and full-day passes (users are called Customers), and annual memberships (so-called Subscribers).
- Time coverage: The available data was dated from 2013. However, due to the changing atmosphere of the sharing economy and technology, I focus only on the last 14 months 1/2022 - 2/2023. 


# Data cleaning and transforming

- Import to TablePlus for further analysis using SQL. Convert data types as necessary.
- Filter out observations with invalid values: Several trips had stop time before start time 
- Put NA value in case of inconsistent data categorization, i.e. column "rideable_type" or bike type has "docked bike", which is the general category of all Divvy bikes. It should be either of the 2 sub-categories "electric bike" and "classic bike". 

---

# Results
#### 1. Subscibers prefer short trips on weekdays, Casual Customers prefer long trips in weekends
![Figure 1](./assets/images/bike_sharing/fig1.png)

Figure 1 shows the distribution of all bike trips in days of week by 2 types of users. There was a clear distinction between Subscribers, who rode more on weekdays, especially Tuesday to Thursday, vs Customers, who rode shared bike more in weekend. But given any day of the week, except Saturday, Subscribers always rode more trips than Customers. 

But was it true that Subscribers rode more than Customers? It was, only if we count number of trips. The relation was totally different when we look into the duration of trips too. For further analysis on different behaviors between Subscribers and Customers, I used SQL to extract data on trip duration and Python to visualize the result in Figure 2.

![Figure 2](./assets/images/bike_sharing/fig2.png)

Figure 2 gives us a closer look into max, average and total length of all trips by day of weeks and usertype. On average, Subscribers used Divvy bike for about 11 mins (Thu) to 14 minutes (Sat) per trip while this number is double for Customers, whose trips were about 25 minutes (Wed) to 33 minutes (Sun) depending on the day (Figure 2a). The longest trip for Subscribers was 26hrs (1.08 days) while for Customers was 689 hrs (nearly 29 days). 

Regarding the total duration for all trips, Divvy bikes were used the most by Customers on Sat and Sun (weekends) while used by Subscribers the most on weekdays, particularly Tue, Wed and Thu. This pattern is consistent with the trend in Figure 1. An explanation for the difference in riders' behavior is that Subscribers would use docked bike for short distance to work, hence main usage on weekdays and shorter trip length on average. Meanwhile, Customers were likely to use shared bike for longer distance and leisure purposes during weekends. If these trip duration figures could be converted into real money (meaning if we can actually bill all Customers for all their trips), the revenue would be enormous and contribute significantly to the program's profit. 

However, looking at the maximum duration per trip by Customers (up to 29 days), I believe it is difficult to charge Customer for the whole length of bike rent, considering the possibility of no return or loss, invalid payment method after the initial charge, etc. This support the strategy that converting casual riders to member customers is the key for future growth. 

#### 2. Subscribers use both types of bikes indifferently, casual Customers prefer electric bikes

Figure 3 shows the distribution of trips by bike type for 2 groups of riders. Divvy members are almost indifferent between classic and electric bike, probably for short trips and going-to-work purpose, they would use any bike available at the dock at that moment. But those use shared bikes for longer trips and leisure purpose like Customers would definitely favor electric bikes. 

![Figure 3](./assets/images/bike_sharing/fig3.png)

#### 3. Subscribers ride throughout the year, Casual Customers prefers mainly summer 
Figure 4 shows the pattern of riding Divvy bikes by month or season. There was season factor in the behavior of both groups: bicyling more in the warm summer months (May to September) and less in cold months (October to April). However, riders with membership used Divvy a bit more frequently throughout the year: even in the winter the number of trips per month by this group reached more or less 200,000, tripled that number of the casual customers in winter. 

![Figure 4](./assets/images/bike_sharing/fig4.png)

#### 4. Subscribers use bike more after lunch and in the evening, Customers ride mostly in the evening
There were 2 peaks of rides for Subscribers: around 2-3pm (probably after a late lunch) and around midnight (possibly after last trips of public transportation). In the meantime, Customers often used Divvy in the evening only. 

![Figure 5](./assets/images/bike_sharing/fig5.png)

