

JavaScript Object Notation (JSON) is a simple data transfer format that can be used to serialise any data. 

It can be processed directly by JavaScript interpreters. It is commonly employed in Ajax applications as an alternative to the XML format, originally used for data transmission.

In a typical situation, when a user performs an action, client-side JavaScript uses XMLHttpRequest to communicate the action to the server.

The server returns a lightweight response containing data in JSON format. The client-side script then processes this data and updates the user interface accordingly.

For example, an Ajax-based web mail application may contain a feature to show the details of a selected contact. 
When a user clicks a contact, the browser uses XMLHttpRequest to retrieve the details of the selected contact, which are returned using JSON:

```json
  {
   “name”:”abcd”,
   ”id”:”1234”,
   ”email”:”abcd@nothing.com”
  }
```


The client-side script uses the JavaScript interpreter to consume the JSON response and updates the relevant part of the user interface based on its contents. 

A further location where you may encounter JSON data in today’s applications is as a means of encapsulating data within conventional request parameters. 

For example, when the user updates the details of a contact, the new information might be communicated to the server using the following request:

```http
  POST /contacts HTTP/1.0
  Content-Type: application/x-www-form-urlencoded
  Content-Length: 89

  Contact={“name”:”abcd”,”id”:”1234”,”email”:”abcd@nothing.com”}
  &submit=update
```

