# Index lifecycle management 

By leveraging Index Lifecycle Management in Kibana, you can automate the management of your indices, optimize resource allocation, and ensure efficient data retention and deletion. 
It simplifies the process of maintaining your Elasticsearch indices and helps you adhere to specific retention and data lifecycle requirements.


## Content 

### [Prerequisites](#requisite)
### [Index Lifecycle Policy](#ILMP)
### [Creation of Policy]
### [Phases]
### [Conditions and functions]

### [Selection on Index]()

## Prerequisites

### - Active Data source.
### - Connection from Data source to Kibana. 
### - Index Pattern from Data Source.


## Creating Index Lifecycle Policy <p id = "ILMP">

Index Lifecycle Management (ILM) is a feature allows actions like rollover, retention, and deletion. 

step-1 go to the Management section and click on "Index Lifecycle Policies.

step-2 go to the Management section and click on "Index Lifecycle Policies.

There will list of default policies , you can edit those or you can also attach a policy to automate when and how to transition an index through its lifecycle by clicking on create policy

## index lifecycle policy defines the lifecycle stages for your indices
  These stages include the following 
  
### 1- Hot Phase 
### 2- Warm Phase 
### 3- Cold Phase
### 4- Delete Phase

step-5 We can specify conditions and actions for each stage, 

including index rollover, shared allocation, and retention periods.


Rollover Conditions 

allows you to create new indices automatically based on certain conditions, such as index size or time

In the Index Lifecycle Policies settings, you can define the rollover conditions for transitioning from one stage to another.

