# Service Level Objectives

- It is impossible to properly manage the service without understanding which behavior is really a problem, how to measure and evaluate those behaviors

- For that purpose define the service level and tell it to the user

    - Regardless of whether users use public APIs or internal APIs
- Decide SLI, SLO, SLA with understanding of intuition and experience and what the user is seeking

- These measurements explain the basic attributes of important metrics, what values ​​to retrieve, how to react when expected service can not be provided

- By choosing the right metrics, the correct action can be taken when what is going wrong, and the SRE team can trust that the service is working normally

- This chapter talks about the framework that Google uses for modeling, selecting, and analyzing metrics

- If you do not give a concrete example, it will be too abstract so let's take the Shakespeare service described in Chapter 1 as an example


## Service Level Terminology

- SLA thinks it is familiar, but SLI and SLO are also important
- In general usage, the word SLA has various meanings depending on the context
    - I think that it is better to separate them for clarity
    
### Indicators

### Objectives

### Column The Global Chubby Planned Outage

### Agreements

## Indicators in Practice

### What Do You and Your Users Care About?

### Collecting Indicators

### Aggregation

### Column A Note on Statistical Fallacies

### Standardize Indicators

## Objectives in Practice

### Defining Objectives

### Choosing Targets

#### Don’t pick a target based on current performance

#### Keep it simple

#### Avoid absolutes

#### Have as few SLOs as possible

#### Perfection can wait

------------------------------------

### Control Measures

### SLOs Set Expectations

#### Keep a safety margin

#### Don’t overachieve

### Agreements in Practice



