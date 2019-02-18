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
    
###### cost

- A way to fulfill conflicting constraints in a cost-effective way
    - Split the infrastructure and provide it at multiple independent levels
    - In the example of BigTable, low latency cluster and throughput cluster
        - A low latency cluster is designed to be operated and used for services requiring low latency and high reliability
            - It is provisioned with considerable leeway to guarantee the short queue length and reduce conflicts and increase redundancy to meet strict client isolation requirements
        - Throughput clusters optimize throughput for latency, always operate and provision low redundancy
            - In fact, the cost to fulfill this requirement seems to be low, the former 10% - 50% cost
        - In the scale of BigTable, this cost reduction width appears conspicuously
- The key strategy of the infrastructure is to clearly distinguish the levels and provide services
    -  Clients can find a compromise between risk and cost (images like when they use AWS)
    - By explicitly dividing the level of service, infrastructure providers can externalize the costs necessary to service a level of clients
        - Motivation for clients to select the level of service at the lowest cost to satisfy their needs
    - In the case of Google+,
        - Important data such as user privacy concerns placed in a highly available worldwide consistent data store
        -  Supplementary data that enhances the user experience may be placed on a cheap and less reliable data store
- Note that the story so far is realized using the same software and hardware
    - Various service guarantees can be provided by adjusting the characteristics of various services
        - Characteristics include resource volume, redundancy, geographical provisioning constraints, infrastructure software configuration
   
##### Motivation for error budget

- In other chapters of the book we are discussing how tension between the product development team and the SRE team occurs
    - Teams are evaluated with different metrics
        - Development team will be evaluated at development speed
        - The SRE team is evaluated with reliability
    - Asymmetry of information between teams amplifies tension
        - The development team is focused on writing code and releasing it
        - The SRE team is looking at the reliability of the service and the state of the product
- Typical examples of tension
    - Fault tolerance of software
        - How to endure the software against unexpected events
            - If it is too weak it is too brittle to use
            - Those that no one wants to use if it is too strong (there is stability)
- test
    - If it is too little, it will be embarrassing to stop functioning and leakage of personal information
    - Too much will miss the market
- Frequency of push
    - All push is dangerous. How to tackle to reduce risk or do other work
- Canary duration and size
    - Testing a new release with a subset of typical workload is called canarying
    - How long to wait, how big is the canary (size of the subset)
- Approach to team asymmetry
    - It seems to be where the boundary between risk and labor exists
    - It is difficult to prove that the balance is optimal
        - It simply becomes a problem of the negotiation skills of the involved engineers
    - Such decisions should not be made by politics, fear or hope
- The ultimate goal is to determine the objective metrics with the agreement of both teams
    - By doing this you will be able to reproduce negotiations
    - "Data based decisions are good"
    
###### Form an error budget

- In order to make decisions based on objective data, both teams need to cooperate and define quarterly error budget based on SLO
    -  Error budget is a clear metric showing how unstable the service during the quarter
- Practice
    - Define SLO (product seems to be written in Chapter 4). SLO determines how much service (uptime) is running every quarter
    - uptime is measured by a neutral third party (it seems to be a monitoring system)
    - The difference between these two numbers is an error budget
    - New functions will be released if uptime is above SLO

###### advantage

- The primary advantage of error budgeting is to provide a common incentive for the development team and the SRE team
    - This allows us to find an appropriate balance between innovation and reliability
- It is used to adjust the release speed
    - I hope I fulfilled the SLO
    - Release as long as you need to expand the error budget, or if you infringe the SLO or you submit additional resources
    - Even if you do not stop releasing completely, there is also a way to slow down the release or rewind to the point where the error budget recovers
- For example, if the development team omits the test and wishes to speed up the release cycle, if the SRE team can afford, the error budget is an indicator
    - If the error budget is large, it should be issued, and if it is small, the development team should take time to test or slow down the cycle
    - The development team will be able to autonomously. Because we can recognize and manage risks thanks to the error budget.
- In case of network trouble or something?
    - It can also be charged
    - Everyone is responsible for uptime
- Error budget also helps to emphasize the cost of excessively high reliability goals
    - In such a case, lower the SLO