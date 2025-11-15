Implementation of Microsoft Purview in Azure
Approach1:Use Azure Purview’s REST APIs for creating custom lineage
Link:https://piethein.medium.com/use-azure-purviews-rest-apis-for-creating-custom-lineage-ad8efacc6230
Pros:     We can create custom lineage via Service Principal Purview’s REST API. We can extend these objects with additional metadata, like relationships to terms, new entities and definitions. We can apply customizations, register new types or transfer metadata from other repositories
Cons:    Need GUID in interlinking purview and databricks using Curl Script, GUID should be extracted manually through hyperlink for each entity in purview. Manual Effort involved in fetching GUID and developing curl script
Access:     ATLAS API endpoint, Service Principal
Approach2: PyApacheAtlas: A Python SDK for Azure Purview and Apache Atlas
Link:https://github.com/wjohnson/pyapacheatlas
Pros:     PyApacheAtlas lets you work with the Azure Purview and Apache Atlas APIs in a Pythonic way. Supporting bulk loading, custom lineage, custom type definition and more from an SDK and Excel templates / integration.
Cons:    Need GUID in interlinking purview and databricks using Python Script, GUID should be extracted manually through hyperlink for each entity in purview. Manual Effort involved in fetching GUID and developing Python script
Access:   Using Azure-Identity and the Azure CLI to Connect to Purview. Create a Purview Client Connection Using Service Principal - Service Principal should have all types of access
Approach3: Capture and view data lineage with Unity Catalog
Link:https://learn.microsoft.com/en-us/answers/questions/1315128/can-i-import-custom-lineage-from-a-json-file-in-pu
Pros:     Use the Purview REST API to import the lineage data into the new glossary in Purview
Cons:    Purview has standard csv format, the json we generated via Postman not able to fit to the standard even while using web converters. Need custom scripting or development work involved seems complex
Approach4: Enabling Lineage in Purview(Approach 1 or 2 +python script to reduce manual effort)

Links:
https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/data-lineage
https://learn.microsoft.com/en-us/rest/api/purview/datamapdataplane/type/get-entity-def-by-name?view=rest-purview-datamapdataplane-2023-09-01&tabs=HTTP
Pros:     To minimize manual effort from Approach 1 or Approach 2 using Python script in databricks

Cons:    development effort involved

Access: Service Principal for purview, Atlas API Endpoint

Subtasks involved :

Input : Table name
Output : Lineage details will get updated in azure purview entity ( existing entity )

Data bricks pull lineage details :
send the table name we can get the lineage details.
https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/data-lineage
-Enable Service principal
-Get the token
-Call API to get lineage details

Purview Pull entity details :

-Enable Service principal
-Enable Atlas API endpoint url
-Get the token
-Call API to get entity details
Generate Access token:https://learn.microsoft.com/en-us/purview/data-gov-api-rest-data-plane?tabs=datamap
Atlas API to get entity details:https://learn.microsoft.com/en-us/rest/api/purview/datamapdataplane/type/get-entity-def-by-name?view=rest-purview-datamapdataplane-2023-09-01&tabs=HTTP
Python Notebook to update lineage details to purview 

Pull above two API details
Match both data and prepare Lineage link with guid
Prepare final required json data.
Update Lineage details to purview :https://piethein.medium.com/use-azure-purviews-rest-apis-for-creating-custom-lineage-ad8efacc6230
Enable Service principal
-Get the token
-Call API to update lineage details

Broad view of Purview Implementation:

1.> Setup the Data sources

2.> Setup proper connection to Databricks using a Service Principal

3.> Identify the Data source to be migrated

4.> Scan the source system to gather the Data Mart into Purview

5.> Set up classifications for each Table column details

6.> Setup auto classifications to run independently

7.> Setup manual lineage and/or try new ways  for auto lineage population

8.> Setup Business Glossary

9.> Tasks need a great collaboration with the other teams to gather information and curate them
