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
- Download the latest version of Logstash
    - Copy and Paste the "Basic Config" Into your Logstash.yml
        - found here: https://www.elastic.co/guide/en/logstash/current/azure-module.html#azure_best_practices
    - Update var.elasticsearch.hosts to reflect your Elasticsearch endpoint
    - Update var.kibana.host to reflect your Kibana endpoint
