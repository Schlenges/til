# AJAX
Asynchronous JavaScript and XML
AJAX is an approach or concept to web development using existing tools and technologies
that can update and change content dynamically without the page refreshing.
With AJAX, wesbites can send and request data from a server in the background without disturbing the current page
(HTML, JS, DOM, CSS, XMLHttpRequest) 
Formed around 2005, first mentioned in: http://adaptivepath.org/ideas/ajax-new-approach-web-applications/


#### XML vs JSON
```XML
<book>
  <title>The Picture of Dorian Gray</title>
  <author>Oscar Wilde</author>
  <likes>149</likes>
</book>
```
```JS
'book': {
  'title': 'The Picture of Dorian Gray',
  'author': 'Oscar Wilde',
  'likes': 149
}
```

## XMLHttpRequest (XHR)
XMLHttpRequest() objects are used to interact with servers. You can retrieve data from a URL without having to do a full page refresh, meaning you are able to update just parts of a web page without having to interrupt the user. The data you want to retrieve can be of any type (it doesn't have to be XML). Despite HTTP, it also supports other protocols such as file and ftp.

To send an HTTP request, you first have to create a new XMLHttpRequest object:
```JS
var request = new XMLHttpRequest();
```
To initialize the request object, you have to call the **open()** method, in which the request method and the url it should be sent to are defined:
```JS
request.open("GET", "http://www.example.org/example.txt")
```


```JS
const request = new XMLHttpRequest();

request.open('GET', 'https://api.github.com/zen');
request.send();
```

XMLHttpRequest.readyState tells what state the request client is in
0 UNSENT Client has been created, but open() has not been called yet
1 OPENED open() has been called
2 HEADERS_RECEIVED send() has been called and headers and status are available
3 LOADING downloading the data
4 DONE the operation is complete

to check the current state, we can add the event listener onreadystatechange to the request and access the data inside the responseText after the operation is done. To make sure that there is no problem with the url and the request actually got through, we can additionally check the status code to handle any errors.

```JS
request.onreadystatechange = function(){
  if(request.readyState == 4 && request.status == 200){
      console.log(request.responseText);
    } else {console.log("There was a problem!");}
  }
}
```
response text comes back as a string, so it has to be parsed into json via JSON.parse()
const data = JSON.parse(request.responseText);
