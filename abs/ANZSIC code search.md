# Getting Started

The ANZSIC coder REST API lets you search for ANZSIC codes by business activities such as '**banking**', '**fishing**', '**house painting**', etc.



## Key Information

### Base URL

This service only responds to a single `GET` method:

> https://industrycoder.abs.gov.au/pocc

The ANZSIC coder service is only available over HTTPS.

### Response Format

Responses are in JSON

### Authentication

This is a public and unauthenticated service.


# Using the API

## Request encoding

We protect this service with a Web Application Firewall, which will reject requests with unencoded special characters.

Please apply appropriate URL encoding to your requests. For more information, please see this [W3C article on URL encoding](https://www.w3schools.com/Tags/ref_urlencode.asp)


# Search

The search service is available at this URL:

> https://industrycoder.abs.gov.au/pocc

The service takes a single parameter `s` which is a string containing your search terms separated by spaces.

The service will return an array of JSON objects, ordered by relevance and limited to 20 results.

| Field | Description |
| --- | --- |
| code | The 5-digit ANZSIC category code |
| parsedText | Unparsed text modified to allow efficient searching. This is what the query terms have been searched against |
| unparsedText | The natural language description of the ANZSIC category |
| score | Weighted closeness score, a decimal value between 0 and 10 |
| rank | Integer closeness ranking within the results |


## Example code

Here is a basic example webpage that uses AJAX to query the service and display the results.

Note that in this example, on the the 'code' and 'unparsedText' fields are displayed.

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
 <title>PoCC Search</title>
</head>
<body>
 <div>
 <h2>Search for a Classification</h2>
 <input type="text" id="search" size="20" />
 <input type="button" id="searchButton" value="Search" onclick="find();" />
 <ol id="results" />
 </div>
 <script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js"></script>
 <script>
 var uri = 'https://industrycoder.abs.gov.au/pocc?s=';
 function formatItem(item) {
 return item.code + ": " + item.unparsedText;
 }
 function find() {
 var id = $('#search').val();
 $.getJSON(uri + id)
 .done(function (data) {
 $('#results').text(''); //clear previous search results
 $.each(data, function (key, item) {
 $('<li>', { text : formatItem(item) }).appendTo($('#results'));
 });
 })
 .fail(function (jqXHR, textStatus, err) {
 window.alert('Error: ' + err);
 });
 }
 </script>
 <script>
 $('#search').keyup(function(event) {
 if (event.keyCode === 13) {
 $('#searchButton').click();
 }
 });
 </script>
</body>
</html>
```

