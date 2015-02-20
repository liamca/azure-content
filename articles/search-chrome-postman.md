# How to use Chrome Postman with Azure Search #

[Postman](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm "Chrome Postman") is a tool provided as part of Google Chrome that allows developers to work efficient with REST based API services such as Azure Search.   This allows you to quickly create, and query your search indexes without the need of writing code.  This tutorial will explain how to leverage Postman to interact with Azure Search.
![][1]
 
## Requirements and Assumptions ##
It is assumed that you have: 

- Created an Azure Search service and have access to the URL and Admin API Key.  For more information on how to do this, please visit the Getting Started with Azure Search tutorial

## Installing Postman ##
To download Postman, visit the [Google Chrome Store](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm).  The link from this page allows you to download and install the REST client for Postman.  Once installed, you can launch Postman from the Chrome App Launcher.
![][2]
 
## Configuring Postman to Query Azure Search ##
To configure Postman, follow the steps: 

- Enter your Azure Search service URL where it says “Enter request URL here”.  
- Append to the URL: ?api-version=2014-07-31-Preview as shown below. 
- Ensure that “GET” is chosen
- Click on the Headers button
- Enter a values for:
	- api-key:  [You Azure Search Admin Key ]
	- Content-Type: application/json; charset=utf-8
- Press Send to issue the REST call to Azure Search and visualize the JSON response 

![][3]

## Creating an Azure Search index with Postman ##
Next we will expand on what we completed in the last step by issuing a REST call to create a new Azure Search index. Unlike the previous call, the index creation requires an HTTP PUT as well as a JSON document with the definition of the index schema.  For this sample we are going to create an index that will store a list of hotels.  To do this:

- Change the URL to: https://[SEARCH SERVICE].search.windows.net/indexes/trails?api-version=2014-07-31-Preview and update to reflect your search service name
- Change the request type from GET to PUT
- In the RAW body content enter to following JSON

	    {
	    "name": "trails", 
	    "fields": [
	    {"name": "id", "type": "Edm.String", "key": true, "searchable": false}, 
	    {"name": "name", "type": "Edm.String"}, 
	    {"name": "county", "type": "Edm.String"}, 
	    {"name": "elevation", "type": "Edm.Int32"}, 
	    {"name": "location", "type": "Edm.GeographyPoint"} ]
	    }
    
•	Press Send
![][4]
 
## Posting Documents to an Azure Search index with Postman ##
Now that we have an index created, we can start to load content into it.  To do this, we will be posting a group of documents in a batch.  To do this, we will take five trails from the USGS dataset: 

- Change the URL to: https://[Search Service].windows.net/indexes/trails/docs/index?api-version=2014-07-31-Preview and update to reflect your search service name
- Change the HTTP type to POST
- In the RAW body enter:

	    {
	      "value": [
		    {"@search.action": "upload", "id": "233358", "name": "Pacific Crest National Scenic Trail", "county": "San Diego", "elevation":1294, "location": { "type": "Point", "coordinates": [-120.802102,49.00021] }},
		    {"@search.action": "upload", "id": "801970", "name": "Lewis and Clark National Historic Trail", "county": "Richland", "elevation":584, "location": { "type": "Point", "coordinates": [-104.8546903,48.1264084] }},
		    {"@search.action": "upload", "id": "1144102", "name": "Intake Trail", "county": "Umatilla", "elevation":1076, "location": { "type": "Point", "coordinates": [-118.0468873,45.9981939] }},
		    {"@search.action": "upload", "id": "1509897", "name": "Burke Gilman Trail", "county": "King", "elevation":10, "location": { "type": "Point", "coordinates": [-122.2754036,47.7059315] }},
		    {"@search.action": "upload", "id": "1517508", "name": "Cavanaugh-Oso Truck Trail", "county": "Skagit", "elevation":339, "location": { "type": "Point", "coordinates": [-121.9470829,48.2981608] }}
	      ]
	    }
    
- Press Send

![][5]

## Querying the Index with Postman ##
The final step is to be able to query the index and issue a simple full text search request for the word “Trail”.  To do this:

- Enter the following in the URL: https://[Search Service].search.windows.net/indexes/trails/docs?api-version=2014-07-31-Preview&search=trail and update to reflect your search service name
- Change the HTTP request type to GET
- Press Send
 
In the Response, you should see the JSON search results returned from Azure Search.
![][6]

## Next Steps ##
Now that we have walked through all the basics of using Azure Search with Postman, there are few things to help you with the next steps:

- Postman support Collections which are a convenient way to save commonly issued requests.  These collections can also be shared with other people to be issued in their own version of Postman
- In the Azure Search documentation, make sure to make note of the HTTP request type associated with each call and change to match as appropriate in Postman



<!-- Image References -->
[1]: ./media/search-chrome-postman/full_postman_client.png
[2]: ./media/search-chrome-postman/postman.png
[3]: ./media/search-chrome-postman/configure.png
[4]: ./media/search-chrome-postman/create_index.png
[5]: ./media/search-chrome-postman/upload_documents.png
[6]: ./media/search-chrome-postman/query.png
