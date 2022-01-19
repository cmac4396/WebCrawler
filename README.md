# Project 5: Web Crawler
*This project is for Networks and Distributed Systems (CS3700) at Northeastern University.*
***

Candice Mac and Mouhamadou Sissoko -- 11/19/21

This program represents a web crawler that searches through pages and links on a server.

## Important Modules
* **argparse:** used to parsing arguments from the command line.
* **socket:** used for creating sockets and connecting to server. 
* **ssl:** used to create secure connections. 
* **sys:** used to read in arguments from the command line.

* **BeautifulSoup**: used to parse HTML. 

## Key Features

### Connection Establishment and Maintenance
We decided to implement the <em>Connection: Keep-Alive</em> functionality. When establishing a 
connection, the crawler will look for failures and retry the establishment until successful.
With every request made to the server, the header <em>Connection: keep-alive</em> header is added.
When a response is read from the server, the crawler checks to see whether the server has closed the 
connection. If the connection has been closed, the crawler reestablishes the connection with the server.

### HTTP 1.1
The requests made by the crawler use the HTTP 1.1 option. Since the format of the response is a group
of headers followed by an HTML page, the crawler reads a response until the end HTML tag is reached.

### Cookies
Every time a response is received, the response is searched for new cookie attributes. If there are 
new cookie attributes in the latest response, they replace the old cookie attributes. Every time a request
is sent, it is sent with the most recent cookie attributes. 

### Status Codes
When a response is received, the behavior of the crawler changes based on the status code. The 302 (redirect) status code 
causes the crawler to send a GET request to the location the response is directing the crawler to. The 500 status code
causes the crawler to resend the request until a different code is received. The 200, 403, and 404 status codes do not 
prompt the crawler to take any action.

### Page Traversal
Traversal starts at the /fakebook/ page. On each page, hyperlinks are gathered that start with /fakebook/ and added to a 
queue. Then the page is searched for secret flags, which are stored if found. After the page has been searched for secret flags
and all the URLs on the page have been added to the queue, a GET request is sent to the next URL in the queue and the 
process for flag-finding repeats. This is a breadth-first traversal. 

After a page has been visited, its URL is stored in a list of visited URLs. Before a URL is added to the queue, the crawler
confirms that the URL has not been visited already.

## Challenges
We ran into problems with updating cookies, not realizing that the previous cookie had to be saved in the case where a response
did not specify new cookie values. The crawler also took a long time to run the page, so we had to implement <em>keep-alive</em>
so that the runtime of the program was not uncomfortably long. 

## Testing
We ran the Makefile and the webcrawler program on the Khoury server to see if the expected flags were received within
a reasonable amount of time. 
