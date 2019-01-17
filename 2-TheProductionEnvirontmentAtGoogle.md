## The Production Environment at Google, from the Viewpoint Of an SRE

- Google development environment
- Google DC is different from other common DC
- This chapter charactierizes Google DC & introduces the vocabulary used through this book.

### Hardware

- Most of Google's computer resources are running within the DC designed for Google
- Unlike general DC, Google Dc is crossing the border (?)
- To eliminate misunderstanding, this book uses the following terms:
	- **Machine**: part of harddware (or VM)
	- **Server**: A piece of software that implements the service
- **Machine** can run any **Server**. Even a mail server or Borg's server
- If you think carefully, I did not use the word "Server" and used the term "binary that accepts network connection" that works with **Machine**, but distinguishing between **Machine** and **Server** is important.
- Description of the figure Ten machines constitute a *rack*, Racks stand in a *row*, a *cluster* is composed of a plurality of racks (one or more racks), and DC accommodates a plurality of clusters.
- We call the physically close DC group *Campus*
- Machines in the DC needed to communicate with each other and created a virtual switch with tens of thousands of ports named *Jupiter*

### System Software That “Organizes” the Hardware

- In a single cluster, thousands of machines died in the year and thousands of Disk broke
- Since they are managed by software, there is no challenge for the team operating the service.
- There is a team that maintains hardware and watches infra on a Campus basis.

#### Managing Machines

- Borg, a cluster management system like Apache Mesos
- Borg manages user's job
    -   You can run server and batch processes like MapReduce
    -   A job consists of multiple tasks
- To execute the job at Borg, first select the machine to execute the task and monitor the state of the task. If execution of task fails, execute again from the beginning with another machine
- As tasks are executed beyond Machine, IP and others can not be used to reference tasks. I am doing my best at a higher level
    - I'm referring to task using Borg Naming Service
    - Queries in the form / bns // // are converted to:
- Borg also has responsibility for resource allocation to job
- Borgs reads the list of necessary resources for all jobs and distributes them to the machine in an optimal way
- If you try to use more resources than the task required, kill and restart it

#### Storage

- job can use local disk as temporary storage and can also use persistent storage
- I have a cluster file system like Luster or HDFS
- A file server where D actually holds data. Colossus is responsible for replication and encryption. I also manage where data is located. Colossus is the successor to GFS (Google File System)
- DB system of Bigtable, Spanner, etc. are listed under Colossus

#### Networking

- Google's network is managed in several ways
- OpenFlow-based SDN
- Instead of expensive hard story that smart routing can be done, it combines expensive and inexpensive switches
    - Around here, I do not know the network knowledge well ... If I read the following sentence, there is no such thing as a distribution switch, is the central switch doing its best? Is that it?
- Borg limits the bandwidth so that it limits the compute resources available to task. Thereby maximizing the average usable bandwidth
- Some services have jobs running on multiple clusters. They are moving around the world, but in order to reduce latency, we choose a DC close to the user
- GSLB load balances at the following three levels
    - Geographic graphic LB using DNS
    - User service level load balance (??)
    - RPC level load balance (see Chapter 19)
- The owner of the service has a symbolic name for the service, and there are multiple BNS hanging on it. GSLB streams traffic directly to the BNS address

#### Other System Software

##### Lock Service

- Chubby provides an interface locking mechanism like file system that can be used beyond DC
- Use Paxos for formation agreement
- Chubby is also used to decide master from job replicas
    - Is it the same feeling that the job that got locked first becomes master?
- Because it is useful for placing data that you want to persist, BNS has Chubby with correspondence of BNS and IP: Port pairs
    - I do not really know how it relates to Lock ...

##### Monitoring and Alerting

- The monitoring program called Borgmon moves and monitors a lot
- Scrape the metrics of the server you want to monitor
- We are saving data for alerts and graphs
    - Alert notification
    - Data for behavior comparison
    - Check which resources are exhausted

#### Our Software Infrastructure

- A software architecture is designed to make the best use of hardware
- Since the code is violently multithreaded, use a lot of cores in one task
- Because of dashboard display, monitoring, and debug, all servers have an HTTP server for displaying their diagnosis results and task status
    - server-status like
- All of Google's services are exchanged using the RPC infrastructure called Stubby
- Google also calls "frontend / backend" (client / server)
- RPC is transferred via protobufs (protocol buffers) (like the serialized format) like Apache's Thrift.
    - Apache Thrift is an RPC framework developed by facebook
- protobufs has many advantages over using XML to serialize structured data
    - Overwhelmingly small, overwhelmingly fast, and unambiguous
    
#### Our Development Environment

- Development speed is important, so we built our complete development environment using our infrastructure
- Apart from several groups that have an open source repository, Google software engineers are using one shared repository
- By doing this you can correct problems outside of the project and send a pull erquestian (changelist or CL)
- Even for engineers themselves, reviews are necessary. All software is reviewed by a single repository

