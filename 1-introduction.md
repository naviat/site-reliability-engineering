# Introduction

### Introduction 

- Ben Treynor Sloss is the godmother of SRE
- In Chapter 1 Ben Treynor Sloss talks about SRE
	- Add to difference between what was done in the old IT industry
- A huge and complicate system does not work in its own !
- How should the system work? 

#### A traditional system administrator approach

- Historically the company has hired system administrators to operate complex systems
- The system admin analyzes the existing software and deploys them.
- The system administrator operates the service and responds to events and software updates that occur.
-  The method of the system administrator is well-known and there are also many imitations, so even a novice team of system administrator does not need to redevelop the wheels.
-  The system manager approach is problematic in terms of joint expenses and direct expenses
-	Since direct costs are handled by a system administrator manually, people are needed a lot due to an increase in traffic etc.
-	The increase in indirect costs is also difficult to understand, but it is also a costly part of direct costs
	- Due to the separation of dev and ops, conflicts between two teams differing in viewpoint of risk against background and solutions occur (I could not understand translations here)
	- "This split between the groups can easily become one more incentives, but also communication, goals, and eventually, this outcome is a pathology.
	- Quickly release vs Reduce release for stability etc.
	- A lot of checks at the time of deployment that correspond to all major obstacles that occurred in the past are needed, and the dev team runs to decrease launche and increase flag flips (What is flag flips?)
	- In order to reduce review, we will divide services with few functions (micro service dis?)

#### Google's Approach to Service Management:

- The explanation that SRE is what can be done when asking software engineer to design ops team is simple
- I am a software engineer and I designed my SRE team
- Fifty to sixty percent of the SRE was employed as a software engineer, the rest were software engineers, skills were less than necessary and skills were 85% - 99%, but software engineers do not have UNIX system internal or networking skills Person who has skill
- Both of the above SREs work as much as good
- Various backgrounds work in the right direction
- SRE has software skills to replace manual work with machines because manual work is boring
- SRE automates
- Otherwise it will be necessary to hire a lot of people as the service grows
- We spend 50% of time on ops and write code for the rest
- Anyway automation. Automation that will recover arbitrarily even if something happens
- Measuring and hiring new people and keeping ops 50% or less
- Only for sublinearly against the growth of service people are unnecessary
	- Nonlinear? Log n like?
- SRE has various problems
- Employment is serious
- Competition with acquisition of software engineers at Google
- When high coding skills and high system engineer skills are required, people who can hire are limited
- As the approach is not an orthodox, support by high layers is required. For example, because the error budget has risen, quitting quarterly releases, etc. will be rebounded from the product development team, so impossible without management.

#### DevOps or SREs?

- The central principle of DevOps and SRE is close
- DevOps contains a wider range. Organization, management organization, etc.

#### About the creed of SRE

- SRE redirects ops work to development team if ops work exceeds 50%
- It also allows developers to turn to create systems that do not require manual inspection
- It is a holiday-oriented on-call shift, and on average on one call it is set to two failures on average. Beyond that, enough time will not be available for convergence of itself or creation of postmortem, and it will be tiring
- Beyond that, I get tired. Conversely, if it is less than 1 time, they will waste their time
- If an important problem occurs, create postmotrtems. Make them to eliminate or reduce the next event
- blame-free postmortem culture
- error budget prevents innovation and stability conflicts
- It is useless in most cases to follow the occupancy rate of 100%
- Determine the target occupancy rate by looking at the following
	- Which level of occupancy rate the user is happy with
	- Is there an alternative service if you are not satisfied with the occupancy rate
	- What happens to how the user uses the product due to the difference in the utilization rate
- If error budget is decided, the major obstacle is not bad, it is one of the processes for innovation
	- Rather, aim for a faster launch within the range of using error budget.

##### Monitoring 

- It is weak to skip the monitoring results by email. The need for human intervention will emerge
- Ideal to recover automatically without skipping alerts
- There are three output as monitoring output: alerts, tickets, logging.

##### Change management

- A major failure of 70% is caused by change
- At the time of change, it is possible to reduce the persons involved in operation by the Implementing progressive rollout and the early detection of the problem and the automatic roll back of the change

##### Demand forecast and capacity planning

- Capacity planning has a natural increase and a non-natural increase due to events and additions of functions, and we think to be able to deal with them
- Think about lead time.
- Add non-natural demand increase to predict natural increase.
- Test the system.
- SRE will be in charge of capacity planning naturally in order to secure occupancy rate. This also represents that you are in charge of provisioning n


#### Provisioning

- Provisioning is part of the capacity planning. Even if you buy a server it will be meaningless if you can not enter by provisioning n
- Efficient use of resources is extremely important.
- Service slowdown is equivalent to reduction in capacity.
- Monitor and change the service to increase performance.

#### The End of the Beginning

- SRE is different from the traditional way of maintaining a large service.
