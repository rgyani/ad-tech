# How does Real-Time Bidding (RTB) for Online Ads Work?

When you access a website, the display ads you see, how are they choosen?  
Either/and,
1. based on your recent browsing history (**Retargeting**)
2. The website owner (**publisher**) could have a direct aggrement with an advertiser to show their ad for a set price and set amount of time
3. The publisher could use an ad-network and sell their inventory on a cost per thousand (CPM) basis
4. The publisher could be using a highly effective, beneficial and very exciting platform known as **Real-Time Bidding (RTB)**

## The Main components of a RTB process

## 1. The Publisher
Simply put, publisher is the website the users visit. However, the publisher has the make ad slots available on their website.  
This ad slot is known as **inventory**

## 2. The Supply-Side Platform (SSP)
The SSP is designed to allow the publishers to sell their inventory on a number of different competing ad exchanges in an efficient, automated way.

## 3. The Ad Exchange
The Ad Exchange is like the stock exchange for ads, which facilitate the buying and selling of ads.  
The three largest ad exchanges – DoubleClick, AdECN, and RightMedia – are owned by Google, Microsoft, and Yahoo! respectively.

## 4. The Demand-Side Platform (DSP)
The DSP stores the display ads (aka creatives) that the advertisers would like to appear on the websites. DSPs optimize the media spends and conversions by using advanced algorithms and technologies to 
1. Avoid fraudulent sites: An estimated $6Billion is stolen each year via online advertising fraud
2. Transparency and Reporting Capabilities

## 5. The advertiser
The advertiser is simply the company or agency representing a number of different advertisers.


# The Real-Time Bidding process

The RTB process starts when a internet user accesses a website/app. 
1. The **publisher's** site sends a message to the **Supply-Side Platform** saying there is an impression available
2. The **Supply-Side Platform** analyzes the information about the user's location, web history, age, gender etc and sends this information to the **ad-exchange**
3. The **ad-exchange** starts an **auction** by passing this information to multiple **Demand-Side Platforms** and asking them to bid on the impression.
4. The **Demand-Side Platform* calculates what that particular impression is worth to them and sends back a bid.
5. The highest bid wins and then the ad is fetched from the **DSP**

This whole proces is repeated for every available impression on a web page every time a user accesses a page, and also refreshes a page.  
So the DSP can/should keep track of when the last inventory was shown to this user to avoid repeat displays of same ad. (**frequency capping**)


### The beauty of the solution

The most remarkable part about this RTB is the speed of auctions.  

**Each transaction takes around 100 milliseconds (1/10th of a second)**

To put that into perspective, it takes about 300 milliseconds to blink.

If the DSP doesn't respond to the ad request within a certain time frame (typically between 120ms — 300ms), they’ll be timed out and won’t be able to purchase the ad space. 
This equals lost opportunities, and often, sales and conversions.


#### It is estimated, down the line, more than 60% of total mobile and online display advertising will be RTB-based


### The Tech Stack

Disclaimer, I dont work in AdTech (yet) so this is based on my reading, thinking and discussions with some people who work in AdTech

1. SQL databases + Cloud Storage for creatives. I would assume S3, cloudfront and RDS on AWS tech stack here.  
Advantages:
   1. Less time managing the infrastrure
   2. The obvious advantage of having instances in the same datacenter as SSPs, hence multi-region deployments
2. Autoscaling EC2 instances to run the DSP servers code with maybe, custom AMIs.  
Advantages:
   1. Removal and replacement of stale/crashed nodes
   2. Scaling up-down the servers during peak/off peak times
3. NoSQL, Cache database like DynamoDB/Redis for frequency capping, user profiles lookups
4. Logs, Redshift for generating reports

Some other solutions I can think of, using BloomFilters and/or Count-Min Sketch for validating campaigns (distributed if-exists) scenarios.  
e.g say we need to check is a particular campaign is allowed for a user on a particular website or not  
now the CampaignID+WebsiteID becomes a key, and we can use 
1. Bloomfilters behind a load balancer to quickly remove false negatives
2. or Count-Min sketch (again behind a load balancer for HA)


Some other Terminologies:

### Banker
Usually, in DSP, the chances are high that the user may overspend during the bidding. This is mainly due to several bids occurring in real-time.

If the user is not careful, he may cross the budget line. Banker is an important feature of the DSP which holds the user from overspending.


### Costs and Pricing
Ads in DSPs are sold on a few ways, depending on which DSP you work with. Generally, if the DSP is specifically built for performance campaigns such as app-installs, then the fee is based on CPI (Cost per Install). Most performance-based platforms use these:

* CPI (Cost Per Install)
* CPC (Cost Per Click) ~ mostly for driving traffic to landing pages
* CPA/CPL (Cost Per Action / Cost Per Lead)  ~ This is mostly for lead generation campaigns
* CPV (Cost per View) ~ This is mostly for video advertising campaigns

* However, the majority of programmatic ads are sold using the CPM (Cost Per Mille or Cost Per 1,000 Views). The CPM model is good for awareness ad campaigns.


### What’s the difference between Ad Networks & DSPs?
Ad networks allow advertisers to buy ad inventory in bulk rather than one impression at a time. Put simply, ad networks gather ad inventories from various publishers, grouping them up and then selling them as slices to advertisers. However, not all ad networks support Real-Time Bidding (RTB).

DSPs are unique as they offer the same capabilities as what ad networks used to provide, with an addition to a suite of audience targeting options. The advantage of DSPs over ad networks is that they provide advertisers with the ability to do real-time bidding on ads, serve ads to a multitude of platforms, track and optimize – all under a single interface.

Some targeting options offered by a DSP include:
* Geo-targeting
* Demographic targeting
* Keyword targeting
* Contextual targeting
* Device targeting
* Re-targeting

DSPs are also used for retargeting campaigns. This is possible because they are able to manage large volumes of ad inventories and recognize ad requests with an ideal target audience, targeted by the advertiser.


### List of DSPs

* AdColony
* Meta Audience Network
* Unity's Unified Auction
* Vungle
* AdMob
* AppLovin
* ironSource


### What is a DMP?
A data management platform (DMP) is a platform that stores audience data, usually coming from multiple sources. It allows advertisers to create target audiences for their campaign based on 1st party and 3rd party audience data.

A DMP acts as a single platform that consolidates online and offline data from various advertisers, creating demographics, behavioral and affinity segments which are then used as targeting options in digital advertising. Performance data from live campaigns are then fed back into the DMP, improving the accuracy of the data.

Most DSPs usually integrate with a DMP to provide advertisers with super sharp audience targeting options. You’ll find the options to use a DMP within a self-serve DSP interface.

[More on DMPs](https://www.knowonlineadvertising.com/programmatic-buying/data-management-platform-dmp/)