# HTTP

1. HTTP is a communication protocol between client and server over TCP. because TCP is reliable. ordering and correctness with HTTP/3; http started to support over UDP with QUIC
   1. #### Client is a user-agent
   2. #### HTTP is extensible
      
      - HTTP Headers
   3. #### HTTP is stateless
      
      - There is no link between 2 requests
      - It's based on request-response communication
   4. #### HTTP Methods
      
      1. *GET*[IDEMPOTENT]: Retrieve data from a specific resource.
      2. *POST*: Submit data to a specific resource.
      3. *PUT*[IDEMPOTENT]: Update a specific resource with provided data.
      4. *DELETE*[IDEMPOTENT]: Delete a specific resource.
      5. *HEAD*[IDEMPOTENT]: Retrieve meta-information without the response body.
      6. *OPTIONS*: Describes the communication options.
      7. *PATCH*: Apply partial modifications to a resource
   5. #### HTTP Some Status Codes [1xx, 2xx, 3xx, 4xx, 5xx]
      
      1. 200 OK: Successful request.
      2. 404 Not Found: The resource could not be found.
      3. 500 Internal Server Error: Server encountered an unexpected condition.
      4. 403 Forbidden: The request was valid, but the server is refusing to respond to it.
      5. 304 Not Modified: Resource has not been modified since the last request.
   6. #### Notes:
      
      - Connection is controlled by transport layer. But HTTP application layer protocol. HTTP must be reliable so choose sent over TCP
      - ***What can be controlled by HTTP***
        
        1. Caching
        2. Cookies: makes stateless http stateful data.
           1. session management
           2. shopping cart
           3. login
      - ***HTTP Flows***
        
        1. Open a TCP connection or use existing one. to send a request or several and get a response.
        2. Send an HTTP messages
        3. Read the HTTP response
        4. Close or resuse connection for further requests.
      - ***Requests contain***
        METHOD TYPE / PATH / VERSION OF PROTOCOL
        HEADERS
        BODY
        
        Version of protocol: HTTP/1.1 HTTP/2 HTTP/3
        Method types: GET PUT POST DELETE HEAD OPTIONS
        Path: URL
        Headers: Request Headers [Accept,Accept Language] General Headers [2] Content Headers
        Body:
      - ***Response contain***
        VERSION OF PROTOCOL /  STATUS CODE / STATUS MESSAGE
        HEADERS
        BODY
        
        Version of protocol:
        Status code: 2xx[Success], 4xx[fail] , 3xx[redirection], 5xx [server issues]
        Headers:
        Body:
   7. #### HTTP has 3 different connections
      
      1. **short-lived** connection HTTP / 1.0 default
         - Tcp handshake and send request -> response close tcp
      2. **persistent** connection[keep alive connection] HTTP /1.1 default
         1. Many request in a opened connection HTTP /1.1
         2. Saving the time for handshake of tcp
         3. Keep-Alive header to specify connection time or idle.
      3. **Pipelening** also available in HTTP/1.1
         1. over the same persistent connection pipeline sends successive request without waiting an answer
         2. so it will decrease the latency
         3. not all http method types can be pipelened only idempotent ones
         4. Despite its design to improve performance, pipelining has not been widely adopted. This is mainly because of issues like **head-of-line blocking** where if one response gets delayed, all subsequent responses (even if they're ready) must wait.
      4. **Multiplexing** with HTTP/2
         1. Multiplexing in the context of HTTP/2 is a technique that allows multiple request and response messages to be in transit simultaneously over a single connection
         2. Responses can return out-of-order, ensuring that a delay in one response doesn't block others.
         3. Multiplexing effectively addresses the "**head-of-line blocking**" issue that persisted with pipelining in HTTP/1.1.
   8. #### HTTP upgrade
      
      1. WebSocket uses HTTP Upgrade in HTTP/1.1 with Upgrade request [Header]

# REST and HTTP Relation

1. Protocol Independence.means that REST can technically be used over any application protocol, not just HTTP. However, HTTP has become the de facto standard for implementing RESTful services because of its ubiquity and simplicity.
2. HTTP Methods and RESTful Operations: RESTful principles closely align with specific HTTP methods to represent CRUD (Create, Read, Update, Delete) operations
3. One of the core tenets of REST is that each request from a client to a server must contain all the information needed to understand and process the request. This statelessness is also an inherent feature of the HTTP protocol.
4. Resource-Oriented: In a RESTful architecture, everything is a resource, and these resources are addressed using URIs (Uniform Resource Identifiers). In the context of HTTP, these are URLs.
5. State Representation: REST dictates that communication between the client and server is done through representations, like JSON or XML. The appropriate representation is transmitted over HTTP based on headers like Accept or Content-Type.
6. Idempotence and Safety: As mentioned previously, idempotence and safety are characteristics of HTTP methods. This perfectly aligns with REST, where certain operations (like reading data) should be safe and have no side effects, while others (like updating or deleting data) should be idempotent.
7. Status Codes: RESTful services use standard HTTP status codes to indicate the outcome of the API call. For example, a 404 Not Found might indicate a requested resource doesn't exist, while a 201 Created indicates successful resource creation
8. HATEOAS (Hypermedia as the Engine of Application State): An advanced RESTful concept, it means that the response from the server not only has data but also provides information on the next possible actions the client can take. This is realized in HTTP responses through hypermedia (links) embedded in the representation.

# REST Bests

1. Web services that follow the principles of REST and use HTTP as the underlying protocol for communication
2. Use HTTP Methods Appropriately:
   1. GET for retrieving data.
   2. POST for creating data.
   3. PUT for updating or replacing data.
   4. PATCH for partially updating data.
   5. DELETE for removing data.
3. Use Meaningful and Consistent URIs: URIs should represent resources (nouns), not actions. For instance:
   1. /users (for a collection of users)
   2. /users/123 (for a specific user with ID 123)
4. Use Plural Nouns for Resources: For instance, /users instead of /user.
5. Statelessness
   1. Ensure that each request from a client to a server contains all the information needed to understand and process the request, without relying on any stored context on the server.
6. Use HTTP Status Codes Correctly: Utilize standard HTTP status codes to indicate the success or failure of an API request:
   1. 2xx for successful operations.
   2. 4xx for client errors (e.g., 400 Bad Request, 404 Not Found).
   3. 5xx for server errors (e.g., 500 Internal Server Error).
7. Version Your API: This helps in making non-breaking changes in the future. You can include the version in the URI (/v1/users) or use request headers
8. Support Filtering, Sorting, and Pagination: Especially for endpoints that can return large amounts of data.
9. Maintain JSON as Standard Payload: While XML was popular in the past, JSON is more lightweight and has become the standard for RESTful web services.
10. Use SSL/TLS: Secure your API by using HTTPS to ensure data confidentiality and integrity. Consider implementing authentication mechanisms such as OAuth or JWT (JSON Web Tokens) for authorization and security.
11. Rate Limiting: Implement rate limiting to protect your service from abuse and to ensure fair usage.
12. Use Nested Routes for Related Resources: If a resource is related to another, you can nest them, e.g., /users/123/orders for orders of a specific user.
13. Consistent Naming and Case: Stick to a naming convention, like snake_case
14. Enable CORS (Cross-Origin Resource Sharing) if you expect web-based clients from other domains to access your API.

# WebSocket Protocol

1. WebSocket is a communication protocol(low level) in application level. Provides full-duplex communication between client and server over a single connection.
2. Primary problem that WebSocket solving is real time processing and streaming.
   1. Provides real time bidirectional communication with low latency.
3. Flow
   1. Handshake: API start with HTTP/1.1 handshake with an Upgrade Header.
   2. If both channel supports the WebSocket response will be 101 that switching to protocol to WebSocket
   3. Full-Duplex: once established full duplex communication, data can be sent both directions without waiting each others.
   4. Data transmitted by Frames
   5. Unlike HTTP connections which are stateless(request-response cycle) connection closes, but in WebSocket it remains open. which overhead the latency of establishing.
   6. Like http to https also ws to wss exists.
4. Considerations:
   1. Server-Sent Events (SSE): unidirectional
   2. Long Polling(HTTP)

## Spring Boot WebSocket with Sub Protocol STOMP

1. **Considerations**: while stomp is widely using in spring, there are alternative ways to use WebSocket like raw
2. To put it simply, STOMP provides a standardized way to structure the data and the interactions over a WebSocket connection in Spring. Using STOMP with Spring's WebSocket support makes it easier to build feature-rich real-time web applications
   1. STOMP[Simple Text Oriented Message Protocol] is a messaging SUB-PROTOCOL to be used on over raw WebSocket
   2. WebSocket bidirectional low-level protocol but it doesn't define how messages sending or receiving. Does not support broadcasting to multiple subscriber, user-specific messaging, or handling different type of messaging.
      1. **Message Multiplexing**
         1. with single WebSocket connection, send and receive messages to/from multiple destinations
      2. **User-Specific Messaging**
         1. Allows the sending specific users with SimpMessagingTemplate
      3. **Subscription Model**
         1. Clients can subscribe topics(or destinations) to receive messages. over STOMP with raw WebSocket
      4. **Message Interceptors**
         1. can be useful logging, modifying messages, validation or even security.
      5. **Easy Client Integration**
         1. STOMP clients available in many languages. SOCKJs easily conjunction with stomp
      6. **Flexible Broker Integration**
         1. while spring provides embedded brokers, also we can integrate powerful queues
   3. **Basically**
      1. We do websocket with STOMP protocol build on top of WebSocket

```
@Configuration  
@EnableWebSocketMessageBroker // We say spring that we're opening websocket port 
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
  @Override
  public void configureMessageBroker(MessageBrokerRegistry config) {
    config.enableSimpleBroker("/topic"); // this is topic destination here provided by spring we can use powerful QUEUEUs. Subscribers and Publisher will be using this topic. 
    config.setApplicationDestinationPrefixes("/app"); // we add this prefix so controllers can map messages with @MessageMapping("/example-route")  
  }

  @Override
  public void registerStompEndpoints(StompEndpointRegistry registry) {
    registry.addEndpoint("/gs-guide-websocket");  // with this register method we opened a web-socket port by using sub protocol STOMP. clients will connect this socket to receive and send messages.
  }
}

```
@Controller // also jackson serializer can be used with help of this annotation so messages converted to JSON. 
public class GreetingController {

  @MessageMapping("/hello")  // with spring any messages send to this route will run this method.
  @SendTo("/topic/greetings") // With spring, any respnose after this method executed will be send to specified topic(or destination we registered in configuration)
  public Greeting greeting(HelloMessage message) throws Exception {
    return new Greeting("Hello, " + HtmlUtils.htmlEscape(message.getName()) + "!"); // we can coverted to message in different type. 
  }
}

```
const stompClient = new StompJs.Client({
    brokerURL: 'ws://localhost:8080/gs-guide-websocket' // client opens an connection to this websocket 
});
function sendName() {
    stompClient.publish({  
        destination: "/app/hello", // client send the message to StompController which we register and open. messages will be send to the topic by the servers. 
        body: JSON.stringify({'name': $("#name").val()})
    });
}
function connect() {
    stompClient.activate(); // clients connects to the socket 
}
function disconnect() {
    stompClient.deactivate(); // clients disconnects to the socket 
    setConnected(false);
    console.log("Disconnected");
}

stompClient.onConnect = (frame) => {
    stompClient.subscribe('/topic/greetings', (greeting) => { // client subscribe to the registered topic(destination) on our websocket server so it will receive any messages that published to web socket port.
        showGreeting(JSON.parse(greeting.body).content);
    });
};
```

```

```

```

```

