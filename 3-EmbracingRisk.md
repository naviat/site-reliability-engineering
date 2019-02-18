### Accept risk

#### Beginning

- It's different that Google is trying to build a 100% reliable service

- Extreme reliability is cost
        
    - Maximizing stability limits the speed of new product development and the speed at which products are delivered to users and increases costs.
    - Cost reduces the number of functions the team can offer
- It is not uncommon for the user to notice the difference between high reliability and enhanced reliability to the limit
    - User experience is affected by less reliable components in most cases
    - For example, mobile communication network or device
    - "Users using 99% reliable smartphones do not know the reliability difference between 99.99% and 99.999% service"
- For these reasons, we aim to balance the 'risk of unavailability of service' and 'purpose of quick innovation and efficient service operation' rather than maximizing uptime
    - By doing so, we optimize the happiness level of the whole user from the viewpoint of function, service and performance

#### Manage risk

- If the reliability of the system is low, the trust of the user falls at a stroke
    - I want to reduce opportunities for the system to fall
- However, when constructing a system empirically, the cost does not increase in proportion to the increase in reliability
    - Improvements to increase reliability are those that cost 100 times the previous measures
- There are two aspects to costing so much
    - Cost of unnecessary machine resources
        - I can not afford to keep the system offline or maintain code blocks during maintenance
    - Cost of opportunity
        - Costs occur when organizations assign engineering resources directly to users, rather than assigning them to build risk-mitigating functions and systems
        - Because these engineers are not working for the user or for the product?
- In SRE, we manage service reliability by managing risk (important)
    - Conceptualize risk as gradual change
    - Equally emphasizing incorporating higher reliability and finding the proper durability of the service
- By doing so, we can do cost / benefit analysis to identify which service to take risks
- The objective is to align the risks born by the service and the risks that the business intends to bear
- I am working on raising the reliability of the service, but I do not do it more than necessary
    - After setting a target of 99.99%, I want to exceed it, but I will not do it excessively
    - If you do too much, you miss opportunities to add new features, repayment of technical debt and operation costs
- In a sense, I see the availability goal as both a maximum and a minimum

#### Measure service risk 

- As a standard Google habit, we define a metric for expressing the state of the system we want to optimize

    - You can evaluate the current performance by tracking the improvement and worse by setting the purpose
- Regarding the risk of service, it is not clear how to put all the elements in a single metric

    - Failure of service Various influences
        - Disappearance of users, disappearance of actual damage, trust
        - Direct and indirect loss of income
        - Scratches to brands and reputation
        - Undesirable coverage?
    - Difficult to measure
- In order to make the problem traceable on various systems, pay attention to "unplanned downtime"

- Express risk tolerance in unplanned downtime tolerance level

    - Normally expressed by system availability, 99.9%
    - Expressionally, the 3-1 guy
    - Calculate acceptable downtime for one year using this formula
        - For example, a downtime of 52.56 minutes is permitted for a target of 99.99%
- As for Google, time-based availability metrics do not make sense

    - I think that it is moving somewhere in the world with a certain service
- Therefore, we use the success rate of requests (Expression 3-2)

    - Up to 250 errors are allowed if 99.99% availability goal for 2.5 M request / day
- In a typical application, not all requests are equivalent

    - Failure of new user's signup request and new mail polling request is different
    - Nevertheless, in most cases, as a value approximating unplanned downtime from the viewpoint of the end user, it is effective
- Quantifying unplanned downtime with request success rate is also effective for those not directly serving end users

    - Batch or storage system
    - On most systems success and failure are properly defined
    - Batch system example
        - To periodically extract and transform information from customer DB and insert it into data warehouse for analysis
        - You can calculate by using the number of records processed successfully and the number of codes that were not processed normally

#### Risk tolerance

- Formal Environment In safety-critical systems, it is incorporated into products and service definitions
    - At Google, there are not many definitely defined
- In order to identify the risk tolerance of a service, the SRE must cooperate with the product owner to convert the business goal into a purpose that can be engineered
    - Business goals have a direct impact on performance and reliability
    - Consumer service has a clear product owner but infrastructure service is not so
    - I will think about each case


##### Identify risk tolerance of consumer service

- Consumer service usually has a product team of the business owner role of the application
    - Google seems to be in search, map and docks respectively
    - Product manager is responsible for understanding users and business and making products successful on the market
- If there is a product team, that team is the most suitable resource to discuss the reliability requirements of the service
    - In the absence of a product team, engineers themselves often play this role
- Points to consider when evaluating the risk tolerance of services
    - How much availability is required
    - Do different types of obstacles differently affect services?
    - How can we use the cost of a service to find a service with continuous risks
    - What are the other important service metrics to consider?

###### Target level of availability

- In our service, the target level of availability depends on the functions that the service offers and the position in the market
- Issues to consider
    - What level of service does the user expect?
    - Does the service directly lead to revenue (including user's)
    - Is it a paid service or a free service
    - When competing in the market, what is the level of services provided by competitors
    - Is the service toB or toC
- Using Google Apps for Work as an example
    - Most users are enterprises
    - I rely on Google Apps to provide the necessary tools to do daily tasks
    - In other words, Google Apps outage will be not only for Google, but for user companies to be out of service
    - External quarterly availability target of 99.9%, further strengthened internal goal, determine penalties when failing to provide external goals
- In contrast to YouTube
    - When it was acquired in 2006, it was different from the life cycle of Google's business (it is still growing?
    - Since the development of quick functions was important, we set the availability goal lower

###### Types of failures

- Predicting the type of failure for a service is an important point to consider
    - How elastic is the business against service downtime
    - Whether a certain percentage of failures or all occasional outages is not good for service
    - Although errors may be numerically the same, the impact on business may be different
- A good example
    - There is a system that provides personal information as a good example of the difference between complete and partial outages
    - Contact management application
        - When rendering of a profile image intermittently fails
            - User experience is not good, SRE tackles problem repair
        - When private contact information is exposed to other users
            - Exposing private data easily loses the trust of basic users
            - As a result, full service outage is effective for debugging and cleanup phases in this case
    - Periodic outages may be allowed during the maintenance period
        - It seems that Ads Frontend a few years ago was on Google
            - Services that set up, configure, run and monitor ad campaigns
            - There is a property to be used during normal business hours
            - From time to time, it is judged that it is acceptable to stop functioning periodically in the form of a maintenance period
            - Treat this as planned downtime rather than planned downtime

###### cost

- Important factors for determining the appropriate availability target for services
    - Ads are particularly good at thinking about this trade-off because success or failure of the request becomes direct revenue and loss
- Questions to determine availability goals
    - If you try to build and operate the system by increasing the availability target of 9, how much will the income increase
    - Is additional income able to compensate for the cost to reach its credibility
    - Concrete example
        - Request has comparable value
        - Proposal for improving availability goal: 99.9% -> 99.99%
        - Increased availability by suggestion: 0.09%
        - Service revenue: $ 1 M
        - Value of improved availability: $ 900
    - Improvement is worth improvement if the cost of improving to increase one 9 fits in $ 900
    - If it does not fit, the cost will exceed the increase in revenue
- When there is no conversion function of reliability and revenue, it becomes difficult to set the target
    - There is a way to consider ISP's background error rate on the Internet
        - If the failure can be measured from the end user's point of view and the error rate of the service can be kept below the error rate of the background, it is possible to suppress the error within the noise of the user's internet connection
        - Google says there are significant differences between ISPs and protocols, but the error rate of the ISP's background seems to fall between 0.01 or 1% as a result of the measurement
        
###### Other service metrics

- It is a good idea to investigate the risk tolerance of a service by associating it with a metric other than availability
    - By understanding which metrics are important or not, degrees of freedom come out when trying to take risks
- Example of latency in Google's advertising system
    - The characteristic of when Google came lunch for searching was speed
    - Therefore, there was a key requirement of AdWords that "ads should not delay the search experience"
    - This requirement seems to remain unchanged
- AdSense example (there is a different latency goal than AdWords)
    - Do not delay slow rendering of third party pages when inserting contextually targeted ads
    - The latency goal depends on the speed at which a publisher's page is rendered
    - Adsense ads can be delayed by a few hundred milliseconds than AdWords ads
- Loose latency requirements make it possible to have many smart tradeoffs in provisioning
    - As a result, it is possible to suppress a significant cost excess against ordinary provisioning
    - Considering some insensitivity to adsense that alleviates latency change, it is possible to geographically integrate server resources and reduce operation overhead
    
##### Identify risk tolerance of infrastructure services

- Requirements for building and running infrastructure components are different from those for consumers
    - The basic difference is that you have multiple clients with different needs

###### Target level of availability

- BigTable example
    - In some consumer services, data is provided directly from the BigTable in the path of the user request
        - Low latency and high reliability are required
    - Use Bigtable as repository of data used to perform offline analysis
        - Throughput is more important than reliability
    - The risk tolerance of these two examples is quite different
- The way to satisfy the needs of both use cases is to design all infrastructure services to be highly reliable ...
    - Actually, it tends to use a lot of resources, so it is too expensive
    - To understand the different needs of different users, we turn to the ideal state of the request queue of BigTable users

###### Types of failures

- Users with low latency want BigTable's request queue to be empty
    - To allow the system to process outstanding requests as soon as they arrive
    - In fact, inefficient queuing increases latency
- Users involved in offline analysis are interested in system throughput
    - I do not want the request queue to be empty
    - In order to maximize throughput, the BigTable should not be idle while waiting for the next request
- With such a feeling, obstacles? Definition differs depending on user set
    - A user's desire for low latency? Becomes an obstacle to users desiring offline analysis