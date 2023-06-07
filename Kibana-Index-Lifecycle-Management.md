# Index lifecycle management 

By leveraging Index Lifecycle Management in Kibana, you can automate the management of your indices, optimize resource allocation, and ensure efficient data retention and deletion. 
It simplifies the process of maintaining your Elasticsearch indices and helps you adhere to specific retention and data lifecycle requirements.


## Content 

### [Prerequisites](#requisite)
### [Index Lifecycle Policy](#ILMP)
### [Creation of Policy][policycreation]
### [Phases](#phases)
### [Conditions](#condition)
### [Monitoring and review](#mandr)

## Prerequisites <p id="requisite">

 - Active Data source.
 - Connection from Data source to Kibana. 
 - Index Pattern from Data Source.

</p>

## Creating Index Lifecycle Policy <p id="ILMP">

Index Lifecycle Management (ILM) is a feature allows actions like rollover, retention, and deletion. 

step-1 go to the Management section and click on "Index Lifecycle Policies.

step-2 go to the Management section and click on "Index Lifecycle Policies.

There will list of default policies , you can edit those or you can also attach a policy to automate when and how to transition an index through its lifecycle by clicking on create policy
</p>
 
## index lifecycle policy defines the lifecycle stages for your indices
##### These stages include the following 
<p id="phases">  
### 1- Hot Phase 
### 2- Warm Phase 
### 3- Cold Phase
### 4- Delete Phase
</p>
# Hot Phase 
  
  Hot phase in Index Lifecycle Management refers to the initial active phase of an index 
and Kibana can be used to configure and manage the ILM policies that govern this phase. 
Any index new or old when it's actively being written to and queried frequently automatically enters into the Hot Phase. 
we can set rules as how much size or duration an index can hold inside hot phase before it is sent to another phase .
  

# Warm Phase
  
  During the warm phase, the index transitions from being actively written(initially hot phase) to a phase where it is less frequently accessed but still needs to be readily available for search operations. 
  
# Cold Phase 
  
  Cold phase is a phase that comes after the "warm phase" in the lifecycle of an index
  phase where index is rarely accessed but still needs to be stored for long-term retention or compliance purposes
  The cold phase is useful for data that is infrequently accessed but still needs to be retained for historical or regulatory reasons

# Delete Phase 
  you can define a phase within an ILM policy to delete indices once they meet certain conditions or criteria.
  you can set a maximum index age or a maximum index size, beyond which the index should be deleted

  
## Conditions
  
  When creating index lifecycle policies in Kibana, 
  you can define conditions that determine when an index should transition from one stage to another. 
  The conditions are based on certain criteria such as index size, document count, or time. 
  Here are some commonly used conditions for index lifecycle policies
  including index rollover, shard allocation, and retention periods.


### Index Rollover Conditions 
  
  allows you to create new indices automatically based on certain conditions, such as index size or time
  In the Index Lifecycle Policies settings, you can define the rollover conditions for transitioning from one stage to another.


### Shards Allocation

  plays a crucial role in achieving data redundancy, high availability, and efficient search and query performance
  
### Retention periods 
  
  Retention periods refer to the length of time that data is retained or kept available within a system or organization. 
  
#  Review and monitoring 
  
  - In the Index Management page, you can filter the indices based on their assigned policies. On the left-hand side, under the "Indices"     tab, you'll find a filter option labeled "Index lifecycle policy." Click on it and select the specific policy you want to monitor.

  - Once you've applied the policy filter, you'll see a list of indices that are associated with the selected policy. Each index entry         displays information such as index name, size, document count, and the stage it is currently in (e.g., hot, warm, cold, or delete).
  
  - Click on an index name to access the index details page. Here, you can view more specific information about the index, including its       shard allocation, index settings, and the index lifecycle policy it is associated with. This page allows you to see if any actions        defined in the policy are failing or encountering errors.
  
  - To monitor the transitions of indices between different lifecycle stages, you can use the Index Management page. The "Status" column      provides information on the current stage of each index, and it will automatically update as indices transition based on the policy       conditions you've defined.
  
  - Keep an eye out for any errors or warnings displayed in the Index Management interface. Errors might indicate issues with index           lifecycle policy actions, such as failures in shard allocation or issues with index rollover.
