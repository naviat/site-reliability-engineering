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


