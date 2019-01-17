# azure-logstash-monitoring-module
A walkthrough of setting up your Azure Account to sync up with the Monitoring plugin --- https://www.elastic.co/guide/en/logstash/current/azure-module.html#azure_best_practices



Step 1: 
- Create an Azure Resource Group
    - Give it a name
    - Try to keep Regions consistent for simplicity
    - Create it
    
Step 2:
- Create an Azure Storage Account
    - Attach the account to your new resource group
    - Give  it a name
    - Choose "BlogStorage" for Account kind
    - Leave replication as RA-GRS
    - Create it
    
Step 3:
- Create an Event Hub
    - Give it a name
    - Select the recommended pricing tier
    - Select your resource group  to add the event hub into
    - Give it a location
    - Create it

Step 4:
- Go to Azure Monitor (on the left pane)
    - Click Activity Log
    - At the top, click export to Event Hub
    - Select the proper storage account to send it to
    - Select the proper event hub namespace 
    - Select RootManageSharedAccessKey for the event hub policiy name
    - Pick your retention
    - Click save at the top

Step 5:
- Go to your Event Hub Namespace you created before
    - On the left panel select Diagnostic settings, turn on diagnostics
    - Give it a name
    - Archive it to your  storage account
    - Stream it to your event hub (use the default drop down options)
    - Send to Log Analytics
        - Create a new workspace. Give it a name, choose your subscription, send it ot your exisiting resource group, choose your location, your pricing, and hit OK
    - Select the boxes for "Archive Log", "OperatioinalLogs", "AutoscaleLogs", and "AllMetrics". 
    - Give each option a few days retention - your choice.
    - Save it
    

Step 6:
- Download the latest version of Logstash
    - Copy and Paste the "Basic Config" Into your Logstash.yml
        - found here: https://www.elastic.co/guide/en/logstash/current/azure-module.html#azure_best_practices
    - Update var.elasticsearch.hosts to reflect your Elasticsearch endpoint
    - Update var.kibana.host to reflect your Kibana endpoint
    - Update your var.input.azure_event_hubs.storage_connection
        - This can be found at:    Your Storage Account -> Access Keys -> Connection String
        
    - Update your var.input.azure_event_hubs.threads --- 
        - This should equal the number of  Event Hubs plus one (or more). I have just left mine at 5 for now and it handles things just  fine.
        
    - Update your var.input.azure_event_hubs.event_hub_connections:
        - To find this go to:     Your Event Hub Namespace -> Event Hubs -> insights-operational-logs -> shared access policies -> Create a SAS police  (give it a name, select Manage, Send, and Listen)
        - Click into your SAS  you created, and copy connection string-primary key and paste it into your .yml
        
    - Update in the Cloud Settings section  of Logstash.yml your cloudid and cloud.auth information
    
    
Step 7:
- Run bin/logstash --setup
    - Once running, create an Elastic Stack Azure  deployment in the Azure Marketplace
    - Give it a good 10-15 min and you should see some activity start to populate in Kibana (you may already see some activity logs populated in Kibana  from your previous set up as well)
    
    
Step 8:
- Go the [Azure Monitor] User Activity Dashboard as this populates the most interesting info.



Still working to get the other dashboards populated, so this is the best one for now.

Thanks!
