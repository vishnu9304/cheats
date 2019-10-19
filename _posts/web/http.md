#### Fragments and Queries in HTTP url:

    Example:    https://www.yahoo.com/search?q=tesla%20research
            Query: search?q=tesla%20research


    Example:    https://www.yahoo.com/search?q=tesla%20research#space%20research
            Query: #space research
            Which means under "q=tesla%20research" resource scroll to "#space" section.

#### HTTP Request Methods:

    [GET]         Retreive a resource
    [POST]        Update a resource
    [PUT]         Store a resource
    [DELETE]      Remove a resource
    [HEAD]        Reterive the headers for a resource

    telnet yahoo.com 80
    GET /yahoo.com/yahoomail.jpg    HTTP/1.1
    Host:yahoo.com

#### Safe and Unsafe:

    Safe    -    let's you to read and view resources.
    Unsafe  -    let's you to modify the resources.

#### Post/Redirect/Get Operation: To avoid duplicate post requests

    signup (GET signup) > signup page with forms (POST and update the data and REDIRECT to signed page) > signed

#### http Request Format:

    [method] [url] [version]        [GET] [http://server.com/articles/741.html] [HTTP/1.1]
    [headers]                       [HOST]: [yahoo.com]
    [headers]                       [Accept-Language]: [fr-FR]
    [headers]                       [Date]: [Fri, 10 Aug 2002 21:12:00 GMT]

#### Common Request Header:

    Referrer            The URL of the referring page. The page where the URL originated.
    User-Agent          Information about the browser.
    Accept              Preffered media types.
    Accept-Language     Preferred language.
    Cookie              Cookie information.
    If-Modified-Since   Date of last retrieval. Used for caching.
    Date                Creation timestamp for the message.

#### http Response Format:

    [version] [status] [reason]     [HTTP/1.1] [200] [ok]
    [headers]                       [Server]: [nginx]
    [headers]                       [Content-Type]: [text/html]
    [body]

#### Status Codes:

    [100-199]   Informational
    [200-299]   Successful
    [300-309]   Redirection
    [400-499]   Client Error
    [500-599]   Server Error

    [200]       [OK]                    Success.
    [301]       [Moved Permanently]     Resource Moved, don't check here again.
    [302]       [Moved Temporarily]     Resource Moved, but check here again. (Post Redirect Get Mechanism)
    [304]       [Not Modified]          Resource hasn't changed since last retrieval.
    [400]       [Bad Request]           Bad Syntax.
    [401]       [Unauthorized]          Client might need to authenticate.
    [403]       [Forbidden]             Refused Access.
    [404]       [Not found]             Resource doesn't exist.
    [500]       [Internal server error] Something went wrong during processing.
    [503]       [Service unavailable]   Server will not service the request.


### HTTP Connections
    
    How does the messages actualy moves in the network?
    When are the network connections opened?
    When are the network connections closed?

    http (Application Layer) -> tcp (Transport Layer)

#### http Layer (Browser):

        1 - Extract the host name and port number from the URL "http://mail.yahoo.com/q?s=^mail".
        2 - Creates an HTTP socket and start writing the data to the socket.

#### TCP Transport Layer:

        1 - Accepts the data and ensures the data is getting delivered to the server without getting lost or duplicated.
            Error detection / flow control and takes care of the data reliablity.
    
#### IP Network Layer:

        1 - Responsible for taking these information and moving them in the network switch/router/gateway etc.
        2 - IP is responsible for delivering the data to destination but doesn't gaurentee the delivery (TCP's job).

#### Datalink Layer:

        1 - Ethernet frames.

#### TCP Handshake:

        Before starting the actual transmission, there is a 3 steps process followed to make sure the server and the client are in agreement to transfer the data.
    
        [SYN] Seq=0
        [SYN, ACK] Seq=0 Ack=1
        [ACK] Seq=1 Ack=1
    
#### Persistent Connections: Default type of connection in HTTP/1.1

        1 - Persistent connection stays open after completion of one request response transaction.
        2 - There is always a downside, each server has a limit in number of persistent connections as a security measure.   
        3 - Attackes will perform DOS attacks by opening number of persistent connections and makes servers un-responsive. 
        4 - Since servers only accepts only finite number of persistent connections, servers are configured to close the connections after certain intervals (if idle).

        Note: The server which does not allow the persistent connection must include a http connection header called "Connection: close"
        which will not allow the client to make another request on the same request. It has to re-open a new connection.

        Parallel connections:   Making 2 different connection parallely at the same time. The server will return "Connection: close" header.
        Persistent Connections: Making more than one req/res transaction in a single connection.

#### Proxies:
    Forward and Reverse Proxy:
        Forward Proxy forwards the client requests to the internet.
        Example: Specific set of users (clients) can access twitter from company via the Forward server.
    
#### Reverse Proxy:
        Reverse sits at the server end accepts the request from internet and forwards them to servers (example: load balancing).
    

#### Cache Controls:

    With HTTP/1.1 clients and proxies generally cache the response with 200 ok response code.
    (response to the http get request)

    It will not cache PUT, POST and DELETE transaction.

    Note: Application server can influence this cache settings by using appropriate cache headers.
    
    "Cache-Control: private, max-age=0"

        Cache-Control: public       Public proxy servers can cache the response.
        Cache-Control: private      Response targeted to single user, only web browser.
        Cache-Control: no-cache     Should not be cached.    
        Cache-Control: no-store

#### HTTP Security:

    The stateful and stateless web: HTTP is designed as a stateless protocol, each request/response transaction is independent of any previous or future transactions.

    cookie are used for tracking / differentiate one user from another user.

    Non-Persistent Cookies (Session Cookies): These cookies will be used only for particular session.
    Example:
        set-cookie: GUID=07hfhjebhbwb76,
                    domain=.search.yahoo.com,       // Send this cookie only to yahoo.com and not to other websites.
                    path=/                          // Restrict cookie to specific path.
    Persistent Cookies (Will have a validity and stored to client filesystem).
        set-cookie: GUID=07hfhjebhbwb76,
                    domain=.search.yahoo.com,
                    path=/,                         
                    expires=Monday, 09-July-2019 21:20:00 GMT

#### Authentication Types:

    Basic Type:
        1.
            GET /account HTTP/1.1
            Host: starlingbank.com
        ...   
        2.
            HTPP/1.1 401 Unauthorized
            WWW-Authenticate: Basic realm="starlingbank.com"
        3.
            GET /account HTTP/1.1
            Host: starlingbank.com
            Authorization: Basic Z3fnjnjnsflr

            "Authorization" [header type]
            "Basic"         [AUTH type]
            "Z3fnjnjnsflr"  [base64 encoded value of username and password]
    
    Digest: uses md5 digets instead of base64

#### Form Authentication:

        When you try to access a secured resource, the user will be temporarily redirected to a web page with authentication form.
        If authentication is successful, the user will be redirected again to the secured resource.
    
    Open ID