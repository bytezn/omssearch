# Exploring your Infrastructure with simple OMS Log Searches by Lawrance Reddy
## 
http://www.cloudlogic.expert/single-post/2016/09/05/The-Software-Defined-Datacentre


This repo contains samples of OMS searches that you could use to explore your hybrid infrastructure. Using OMS Log Analytics, these custom searches provide some cool
examples and shows how easy it is to query complex environments with infrastructure, applications, and many other hidden problematic secrets. 

You are free to download these searches, use them and change them as you see fit.


The Sky is the limit with OMS Analytics. Search Anything, Search Any Source, and Apply corrective action or proactive alerting to search results.

## How to Use these Searches

1. You will require a valid Azure Subscription to test these searches.

2. You will require a configured OMS Workspace.  

3. You will need the OMS direct agent installed on Windows or Linux servers.

4. You will require the necessary OMS Solutions installed as appropriate for the search you are running.

5. Cut and paste the appropriate searches below into your OMS Workspace Search, 

6. Important, change the values to match your environment. 
 


## Measure the average CPU on 4 servers anywhere in my environment
Type=Perf CounterName="% Processor Time" (Computer="Computer") OR (Computer="Computer2") OR (Computer="Computer3") OR (Computer="Computer4") | measure avg(CounterValue) by Computer Interval 5MINUTE
or 
Type=Perf CounterName="% Processor Time" (Computer=*Computer1*) | measure avg(CounterValue) by Computer Interval 5MINUTE

## Tell me what VM "X" processor utilization is?
Type=Perf ObjectName="Capacity and Performance" CounterName="% VM Processor Usage" InstanceName=*X* | measure avg(CounterValue) by InstanceName Interval 1HOUR
 

## What is the current disk Queue length of those 4 servers causing me problems
Type=Perf (CounterName="Current Disk Queue Length") (Computer="Computer1") OR (Computer2="Computer2") OR (Computer3") OR (Computer="Computer4")  
or
Type=Perf CounterName="% Processor Time" (Computer=*Computer*)


## File Server + custom shares using DSC
This templates creates a active directory forest with  a file  server and automatically creates a custom  share.

## File server + additional user created using DSC
This template in addition to creating an active directory forest with a file server, it automatically assigns a custom user to the local users and groups.

## Backup vault + policy added to single vm deployment.
This template in addition to creating a virtual machine, will auto create a backup vault and a custom backup policy. 

## Automatic backup added to single vm
This template in addition to creating a virtual machine, will auto create a backup vault a custom backup policy and will automatically enable data protection. The following define the mandatory backup policy parameters, note that syntax for the arrays need to be exact and enclosed with square brackets.

## Data protection vault + policy added to multi-server deployment
This template will build a small datacentre, comprising of 2 domain controllers, file server and web servers with a backup vault and the associated policies. DSC configuration does the automation of each tier.

## Automatic backup added to each server in a multi-server deployment.
This template will add to the template above by adding automatic data protection to the attached VMs. DSC configuration does the automation of each tier.


## Parameters

## Vault & Policy

Mandatory parameters required to auto configure the backup vault and policy. change the parameters to match whether policy is daily, weekly or other.

"vaultName": "vmbackupvault"
"policyName":"MyPolicy" 
"scheduleRunDays": ["Monday"]       
"scheduleRunTimes": ["2016-09-12T09:00:00.000Z"]             
"weeklyRetentionDurationCount": 2 
"daysOfTheWeekForMontlyRetention":  ["First"]
"weeksOfTheMonthForMonthlyRetention":  ["First"]    
"monthlyRetentionDurationCount":  3
"monthsOfYear": ["January"]        
"daysOfTheWeekForYearlyRetention": ["Monday"]          
"weeksOfTheMonthForYearlyRetention": ["Monday"]          
"yearlyRetentionDurationCount":  2
"skuName": "Standard"

## Auto Protect

Mandatory parameters required to automatically configure the backup of the VMs being protected. use  these as parameters or variables.

 protection container
 iaasvmcontainer;iaasvmcontainerv2;my-resource-group;my-arm-vm 
 protectable item
 vm;iaasvmcontainerv2;my-resource-group;my-arm-vm
 resource ids
/subscriptions/subscriptionid/resourceGroups/resourceGroupName/p  roviders/Microsoft.Compute/virtualMachines/my-arm-vm
