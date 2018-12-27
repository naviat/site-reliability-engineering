# Preface

### In case

- If software engineering tends to focus on the design and development of software, there should be another specialized filed that focuses on the entire software lifecycle. We call it Site Reliability Engineering at Google.

- What is SREs?
	- About **"Engineer"** of Site Reliability Engineer: We apply the principles of computer science and engineering to the design and deployment of computer system. Our task is to write software, to build all the additional part needed by systems like backup and load balancing, and find ways to fit existing solutions to new problems.
	- Next, on the **"Reliability"** of the Site Reliability Engineer: Reliability is the most basic function. Since Reliability is very critical, SREs focus on finding ways to improve system design and operation. But if the system is "Reliable enough", we will instead invest in developing additional features and new products.
	- Finally, on **"Site"** of SREs: SREs focus on the "services" operating built on our distributed computing system.

- Unlike DevOps, we regard the infrastructure as a code, but we regard Reliability as the most important.
- We believe that specialized areas called SRE will work well beyond Google. We constructed this book so that general principles and more special practices are separated as much as possible.
- We also provide a description of Google's production environment and a mapping of our internal software and OSS software,
- Smaller organizations may be wondering how to fully utilize the experiences written here, but as with security, earlier it is better to care abount reliability. It is not costly to introduce and extend it first,  reather rthan introduce it from stratch later.
- Regardless of the size of the organization, even if it's not called SRE, there should already be someone who is working for SRE.
- From the historical point of the view ==> **WHO is the FIRST SRE?**
	- Margaret Hamilton who has engaged in the Apollo project.
	- "part of the culture was to learn from everyone and everything, including from that which one would least expect"
- Margaret said: "a thorough understanding of how to operate the systems was not enough to prevent human errors".

IN CASE

### How to read this book

- This book is a series of essays written by Google SRE members and former SRE members.
- It is closer to conference proceedings than an independent book.
- Each chapter is intended to be read as a part of consistent content as a whole, but you may read from where you are interested.
- You can read in any order. However, we recommend you start reading at least from chapters 2 and 3. It describes the Google production environment and an outline of how the SRE approaches risks.
- Reading from the beginning ois of course useful.
- Each chapter is grouped by theme. Principles (Part II), Practices (Part III), and Management (Part IV).
- Each part has a small introduction.
- In addition, [https://g.co/SREBook](https://g.co/SREBook) has many useful contents.