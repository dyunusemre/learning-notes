1. # **What are Microservices**
   
   1. Microservices are independently releasable services that are modeled around a business domain
   2. independent deployability is key
   3. They are technology agnostic
   4. Changes inside a microservice boundary (as shown in Figure 1-1) shouldn’t affect an upstream consumer
      1. #### **Key Concepts of Microservices**
         
         1. Independent Deployability
            1. To ensure independent deployability, we need to make sure our microservices are loosely coupled: we must be able to change one service without having to change anything else
            2. This means we need explicit, well-defined, and stable contracts between services. Some implementation choices make this difficult—the sharing of databases
   5. Modeled Around a Business Domain
2. # **How to Model Microservices**
   
   1. **A good microservice boundary**
      1. Information Hiding
      2. Cohesion
      3. Coupling
   2. **Types of coupling**
      1. domain coupling
      2. pass-thorugh coupling
      3. common coupling
      4. content coupling
   3. **DDD**
      1. Ubiquitous Lang
      2. Aggregate
      3. Bounded Context
      4. How to map aggregate and bounded context
      5. Event storming
   4. **DDD Case for microservice**
   5. **Alternatives to business domain boundaries**
      1. Volatility
      2. Data
      3. Technology
      4. Organizational
   6. **Mixing  Models and Exceptions**
3. # **Splitting to Monolith**
   
   1. **Incremental Migration**
4. # **Microservice Communication Styles**
   
   1. ### **From in-process to inter-process**
      
      1. ###### **Performance**
         
         1. the needs to serialize to send over(transmitted) in network
         2. caution of payload size
      2. ###### **Changing interface**
         
         1. in-process interface changing is easy so IDE might you help you refactor
         2. inter-process interface change should be backward compatible
      3. ###### **Error handling**
         
         1. Crash Failure : Every
         2. Omission Failure: sent something but didn't get response. or expected firing messsage from a response but it stopped
         3. Timing failure: something happened too late or early
         4. Response failure: we got a response but seems wrong.
         5. Arbitrary failure:
            - network timeouts
            - downstream microservices may be unavailbe temporarly
            - containers might got killed
            - database can collapsed
         
         - Notes : HTTP is an example of a protocol that understands the importance of this. Every HTTP response has a code, with the 400 and 500 series codes being reserved for errors
   2. ### **Inter-Process Communication**
      
      1. ***Synchronous blocking***: A microservice makes a call to another microservice and blocks operation waiting for the response.
      2. ***Asynchronous nonblocking***: The microservice emitting a call is able to carry on processing whether or not the call is received.
      3. ***Request-response***: A microservice sends a request to another microservice asking for something to be done. It expects to receive a response informing it of the result.
      4. ***Event-driven***:Microservices emit events, which other microservices consume and react to accordingly. The microservice emitting the event is unaware of which microservices, if any, consume the events it emits
      5. ***Common data***:Not often seen as a communication style, microservices collaborate via some shared data source.
         
         - **Notes**: for a technology choice
           
           - reliable communication,
           - acceptable latency,
           - volume of communication
           - request-response: both synchronous and asynchronous implementations are still available or an event-driven style is mostly common.
             
             1. ###### *Pattern: Synchronous Blocking*
                
                1. **Advantages**
                2. **Disadvantages**
                   1. Cause Temporal Coupling. between to specific instance of microservice.
                      1. Retry or buffer it try it later
                   2. if network is slow temporarly, it will block the service because response needed
                3. **Where to Use It**
                   1. problamatic when blocking chain is exists
                4. blocks until the call has completed, and potentially until a response has been received
                5. a synchronous blocking call is one that is waiting for a response from the downstream process
             2. #### *Pattern: Asynchronous Nonblocking*
                
                1. Communication through common data: The upstream microservice changes some common data, which one or more microservices later make use of
                2. Request-response: A microservice sends a request to another microservice asking it to do something. When the requested operation completes, whether successfully or not, the upstream microservice receives the response. Specifically, any instance of the upstream microservice should be able to handle the response
                3. Event-driven interaction : A microservice broadcasts an event, which can be thought of as a factual statement about something that has happened. Other microservices can listen for the events they are interested in and react accordingly.
                   1. **Advantages**
                      1. the microservice making the initial call and the microservice (or microservices) receiving the call are decoupled temporally. This means we avoid the concerns of temporal decoupling that we discussed in
                   2. **Disadvantages**
                      1. The main downsides of nonblocking asynchronous communication, relative to blocking synchronous communication, are the level of complexity and the range of choice.
                   3. **Where to Use It**
                      1. Long-running processes are an obvious candidate
                      2. situations in which you have long call chains that you can’t easily restructure could be a good candidate
             3. ##### *Sub Pattern: Communication Through Common Data*
                
                1. Notes: This pattern is used when one microservice puts data into a defined location, and another microservice (or potentially multiple microservices) then makes use of the data
                2. ###### **Implementation:**
                   
                   1. To implement this pattern, you need some sort of persistent store for the data
                      
                      1. A filesystem
                      2. robust distributed memory store
                   2. Two common examples of this pattern are the data lake and the data warehouse.
                      
                      1. data lake sources upload raw data in whatever format they see fit
                      2. warehouse itself is a structured data store
                   3. Advantages:
                      
                      1. If you can read or write to a file or read and write to a database, you can use this pattern
                   4. Disadvantages
             4. #### *Sub Pattern: Request-Response Communication*
                
                1. ###### **Notes**:
                   
                   1. Sometimes, though, you just need to make sure something gets done.** **
                   2. With request-response, a microservice sends a request to a downstream service asking it to do something and expects to receive a response with the result of the request.
                   3. This interaction can be undertaken via a synchronous blocking call, or it could be implemented in an asynchronous nonblocking fashion.
                2. ###### **Implementation**: Synchronous Versus Asynchronous:
                   
                   1. Request-response calls like this can be implemented in either a blocking synchronous or a nonblocking asynchronous style
                   2. With a synchronous call, what you’d typically see is a network connection being opened with the downstream microservice, with the request being sent along this connection. The connection is kept open while the upstream microservice waits for the downstream microservice to respond
                   3. With an asynchronous request-response, with a nonblocking asynchronous interaction, the microservice that receives the request needs either to know implicitly where to route the response or else to be told where the response should go. When using a queue, we have the added benefit that multiple requests could be buffered up in the queue waiting to be handled. This can help in situations in which the requests can’t be handled quickly enough
                3. ###### **Where to Use It**
                   
                   1. They also fit really well in situations where a microservice wants to know if a call didn’t work so that it can carry out some sort of compensating action, like a retry
             5. #### *Sub Pattern: Event-Driven Communication*
                
                1. ###### **Notes**:
                   
                   1. Event-driven communication looks quite odd compared to request-response calls. Rather than a microservice asking some other microservice to do something, a microservice emits events that may or may not be received by other microservices
                   2. An event is a statement about something that has occurred, nearly always something that has happened inside the world of the microservice that is emitting the event
                   3. The microservice emitting the event has no knowledge of the intent of other microservices to use the event, and indeed it may not even be aware that other microservices exist
                   4. It emits the event when required, and that is the end of its responsibilities.
                   5. The intent behind an event could be considered the opposite of a request. The event emitter is leaving it up to the recipients to decide what to do
                   6. With request-response, the microservice sending the request knows what should be done and is telling the other microservice what it thinks needs to happen next.
                   7. This of course means that in request-response, the requester has to have knowledge of what the downstream recipient can do
                   8. implying a greater degree of domain coupling
                   9. With event-driven collaboration, the event emitter doesn’t need to know what any downstream microservices are able to do, and in fact may not even know they exist—as a result, coupling is greatly reduced
                   10. The distribution of responsibility we see with our event-driven interactions can mirror the distribution of responsibility we see with organizations trying to create more autonomous teams
                   11. An event is a fact—a statement that something happened, along with some information about exactly what happened. A message is a thing we send over an asynchronous communication mechanism, like a message broker
                   12. The message is the medium; the event is the payload
                2. ###### **Implementation**:
                   
                   1. There are two main aspects we need to consider here: a way for our microservices to emit events, and a way for our consumers to find out those events have happened.
                   2. Traditionally, message brokers like RabbitMQ, Kafka try to handle both problems.
                   3. Producers use an API to publish an event to the broker
                   4. The broker handles subscriptions, allowing consumers to be informed when an event arrives.
                   5. These brokers can even handle the state of consumers—for example, by helping keep track of what messages they have seen before
                   6. These systems are normally designed to be scalable and resilient, but that doesn’t come for free. It can add complexity to the development process, because it is another system you may need to run to develop and test your services
                   7. But once it is, it can be an incredibly effective way to implement loosely coupled, event-driven architectures.
                   8. Another approach is to try to use HTTP as a way of propagating events
                   9. Atom is a REST-compliant specification that defines semantics (among other things) for publishing feeds of resources.
                3. ###### **What’s in an Event?**
                   
                   1. With an event, we are broadcasting a fact that other parties might be interested in
                   2. but as the microservice emitting an event can’t and shouldn’t know who receives the event,
                4. ###### **Just an ID**
                   
                   1. One option is for the event to just contain an identifier for the newly registered custome
                   2. about the Customer microservice, adding additional domain coupling.
                   3. While domain coupling, as we discussed in Chapter 2, is on the looser end of the coupling spectrum
                5. ###### **Fully detailed events**
                   
                   1. which I prefer, is to put everything into an event that you would be happy otherwise sharing via an API
                   2. In addition to the fact that events with more information can allow for looser coupling
                   3. meaning that these events could be used as part of an event sourcing
                   4. This could help you as part of implementing an auditing system
                   5. Firstly, if the data associated with an event is large, we might have concerns about the size of the event
                   6. the default maximum size for a message in Kafka is 1 MB
                   7. Ultimately, if you’re venturing into a space in which you are starting to worry about the size of your events, then I’d recommend a hybrid approach in which some information is in the event but other (larger) data can be looked up if required.
                   8. I might want to limit which microservices can see personally identifiable information (or PII), payment card details, or similar sensitive data
                   9. Another consideration is that once we put data into an event, it becomes part of our contract with the outside world.We have to be aware that if we remove a field from an event, we may break external parties. Information hiding is still an important concept in event-driven collaboration
                6. ###### **Where to Use It**
                   
                   1. Event-driven collaboration thrives in situations in which information wants to be broadcast
                   2. In a situation in which you are focusing on loose coupling more than other factors, event-driven collaboration is going to have obvious appeal.
                   3. remember that our microservice architecture can (and likely will) contain a mix of different styles of interaction
                7. ###### **Proceed with Caution**
                   
                   1. Event-driven architectures seem to lead to significantly more decoupled, scalable systems
                   2. But these communication styles do lead to an increase in complexity
                   3. For example, when considering long-running async request-response
                      1. we have to think about what to do when the response comes back. Does it come back to the same node that initiated the request?
                      2. If so, what happens if that node is down?
                      3. If not, do I need to store information somewhere so I can react accordingly?
                      4. Short-lived async can be easier to manage if you’ve got the right APIs, but even so, it is a different way of thinking for programmers who are accustomed to intra-process synchronous message calls.
                   4. The associated complexity with event-driven architectures and asynchronous programming in general leads me to believe that you should be cautious in how eagerly you start adopting these ideas
                   5. Ensure you have good monitoring in place, and strongly consider the use of correlation IDs, which allow you to trace requests across process boundaries
                   6. We also have to be honest, though, about the integration styles that we might consider “simpler”—the problems associated with knowing whether things worked or not is not limited to asynchronous forms of integration. With a synchronous, blocking call, if you get a time-out, did this happen because the request got lost and the downstream party didn’t receive it? Or did the request get through, but the response got lost? What do you do in that situation? If you retry, but the original request did get through what then? (Well, this is where idempotency comes in, a topic we cover in Chapter 12.)
5. # **Implementing Microservice Communication**
   
   1. ### Looking for the Ideal Technology: **SOAP? XML-RPC? REST? gRPC?**
   2. ### **Make Backward Compatibility Easy**
      
      1. When making changes to our microservices, we need to make sure we don’t break compatibility with any consuming microservices
      2. Simple operations like adding new fields shouldn’t break clients. We also ideally want the ability to validate that the changes we have made are backward-compatible
   3. ### **Make Your Interface Explicit**
      
      1. is explicit. This means that it is clear to a consumer of a microservice as to what functionality that microservice exposes
      2. Explicit schemas can go a long way in helping ensure that the interface a microservice exposes is explicit
      3. explicit schema, as well as there being enough supporting documentation to be clear about what functionality a consumer can expect a microservice to provide.
   4. ### Keep Your APIs Technology Agnostic
      
      1. The one certainty is change
      2. it is very important to ensure that you keep the APIs used for communication between microservices technology agnostic
   5. ### Make Your Service Simple for Consumers
      
      1. our clients full freedom in their technology choice
   6. ### Hide Internal Implementation Detail
      
      1. We don’t want our consumers to be bound to our internal implementation, as it leads to increased coupling that in turn means that if we want to change something inside our microservice, we can break our consumers by requiring them to also change
   7. ### Technology Choices
      
      - *Remote procedure calls*
        - Frameworks that allow for local method calls to be invoked on a remote process. Common options include SOAP and gRPC
      - *REST*
        - An architectural style where you expose resources (Customer, Order, etc.) that can be accessed using a common set of verbs (GET, POST). There is a bit more to REST than this, but we’ll get to that shortly
      - *GraphQL*
        - A relatively new protocol that allows consumers to define custom queries that can fetch information from multiple downstream microservices, filtering the results to return only what is needed.
      - *Message brokers*
        1. Middleware that allows for asynchronous communication via queues or topics.
      
      1. #### **Remote Procedure Calls**
         
         1. **Refers to the technique of making a local call and having it execute on a remote service somewhere**
         2. **Requires an explicit schema such as SOAP(WSDL). or gRPC(IDL)**
         3. **Java RMI**
            1. Typically, using an RPC technology means you are buying into a serialization protocol. The RPC framework defines how data is serialized and deserialized. For instance, gRPC uses the protocol buffer serialization format for this purpose
            2. RPC frameworks that have an explicit schema make it very easy to generate client code
            3. The ease of generation of client-side code is one of the main selling points of RPC
         4. **Challenges**
            1. it’s not without its downsides
         5. **Technology coupling**
            1. Some RPC mechanisms, like Java RMI, are heavily tied to a specific platform
            2. Thrift and gRPC have an impressive amount of support for alternative languages
            3. In a way, this technology coupling can be a form of exposing internal technical implementation details. For example, the use of RMI ties not only the client to the JVM but the server as well.
            4. To be fair, there are a number of RPC implementations that don’t have this restriction—gRPC, SOAP, and Thrift are all examples
            5. JAVA RMI is really fragile to change method signature becuase you need re-generate for all consumers.
            6. Java RMI, for example, has a number of issues regarding brittleness and limited technology choices
            7. SOAP is pretty heavyweight from a developer perspective, especially when compared with more modern choices
         6. **Where to use it**
            1. such as gRPC, are excellent
            2. Make sure your clients aren’t oblivious to the fact that a network call is going to be made
            3. gRPC would be at the top of my list
            4. Built to take advantage of HTTP/2
            5. it has some impressive performance characteristics and good general ease of use
            6. I also appreciate the ecosystem around gRPC, including tools like Protolock, which is something we’ll discuss later in this chapter when we discuss schemas.
            7. the need to compile client-side code against a server-side schema can be problematic. In that case, some form of REST over HTTP API would likely be a better fit
      2. #### **REST**
         
         1. Representational State Transfer (REST) is an architectural style inspired by the web
         2. You can think of a resource as a thing that the service itself knows about, like a Customer. The server creates different representations of this Customer on request. How a resource is shown externally is completely decoupled from how it is stored internally. A client might ask for a JSON representation of a Customer, for example, even if it is stored in a completely different format. Once a client has a representation of this Customer, it can then make requests to change it, and the server may or may not comply with them.
         3. Most important when thinking about REST is the concept of resources
         4. REST itself doesn’t really talk about underlying protocols, although it is most commonly used over HTTP
         5. make implementing REST over HTTP easier
            1. ###### REST and HTTP
               
               1. HTTP itself defines some useful capabilities that play very well with the REST style
               2. For instance, the HTTP verbs (e.g., GET, POST, and PUT) already have well-understood meanings in the HTTP specification as to how they should work with resources
               3. The REST architectural style actually tells us that these verbs should behave the same way on all resources
               4. HTTP specification happens to define a bunch of verbs we can use. GET retrieves a resource in an idempotent way
               5. POST creates a new resource
               6. This means we can avoid lots of different createCustomer or editCustomer methods
               7. Instead, we can simply POST a customer representation to request that the server create a new resource, and then we can initiate a GET request to retrieve a representation of a resource
               8. there is one endpoint in the form of a Customer resource in these cases, and the operations we can carry out on it are baked into the HTTP protocol
               9. many monitoring tools already have lots of support for HTTP out of the box. These building blocks allow us to handle large volumes of HTTP traffic and route them smartly
               10. We also get to use all the available security controls with HTTP to secure our communications
               11. Note that HTTP can be used to implement RPC too. SOAP, for example, gets routed over HTTP, but it unfortunately uses very little of the specification. Verbs are ignored, as are simple things like HTTP error codes
               12. gRPC has been designed to take advantage of the capabilities of HTTP/2, such as the ability to send multiple request-response streams over a single connection
               13. . But of course, when using gRPC, you’re not doing REST just because you’re using HTTP!
               14. Hypermedia as the engine of application state
                   1. Another principle introduced in REST that can help us avoid the coupling between client and server is the concept of hypermedia as the engine of application state (often abbreviated as HATEOAS, and boy, did it need an abbreviation).
                   2. The idea behind HATEOAS is that clients should perform interactions with the server (potentially leading to state transitions) via these links to other resources
                   3. A client doesn’t need to know where exactly customers live on the server by knowing which URI to hit
                   4. The theory is that, by using these controls to decouple the client and serve
               15. Challenges
                   1. In terms of ease of consumption, historically you wouldn’t be able to generate client-side code for your REST over HTTP application protocol like you can with RPC implementations
                   2. The overhead of HTTP for each request may also be a concern for low-latency requirements.
                   3. All mainstream HTTP protocols in current use require the use of the Transmission Control Protocol (TCP) under the hood, which has inefficiencies compared with other networking protocols, and some RPC implementations allow you to use alternative networking protocols to TCP such as the User Datagram Protocol (UDP)
                   4. If you decide to adopt a HATEOAS-style of REST
               16. Where to use it
                   1. Due to its widespread use in the industry, a REST-over-HTTP-based API is an obvious choice for a synchronous request-response interface if you are looking to allow access from as wide a variety of clients as possible.
                   2. REST-based APIs excel in situations in which you want large-scale and effective caching of requests. It’s for this reason that they are the obvious choice for exposing APIs to external parties or client interfaces
                   3. although you can construct asynchronous interaction protocols over the top of REST-based APIs
                   4. So for use at the perimeter, it works fantastically well, and for synchronous request-response-based communication between microservices, it’s great.
      3. #### **Message Brokers**
         
         1. a message is a generic concept that defines the thing that a message broker sends. A message could contain a request, a response, or an event
         2. Rather than one microservice directly communicating with another microservice, the microservice instead gives a message to a message broker, with information about how the message should be sen
            1. ##### Topics and queues
               
               1. A sender puts a message on a queue, and a consumer reads from that queue. With a topic-based system, multiple consumers are able to subscribe to a topic, and each subscribed consumer will receive a copy of that message
               2. A consumer could represent one or more microservices—typically modeled as a consumer group
               3. When a message is put into the queue, only one member of the consumer group will receive that message; this means the queue works as a load distribution mechanism
               4. With topics, you can have multiple consumer groups
               5. A large part of the distinction between the two is that when a message is sent over a queue, there is knowledge of what the message is being sent to. With a topic, this information is hidden from the sender of the message—the sender is unaware of who (if anyone) will end up receiving the message.
               6. Topics are a good fit for event-based collaboration, queues would be more appropriate for request/response communication
            2. ##### Guaranteed delivery
               
               1. The properties they provide vary, but the most interesting feature is that of guaranteed delivery
               2. Guaranteed delivery describes a commitment by the broker to ensure that the message is delivered.
               3. It’s not a problem if the downstream destination is unavailable—the broker will hold on to the message until it can be delivered
               4. This can reduce the number of things an upstream microservice needs to worry about
               5. Compare that to a synchronous direct call—for example, an HTTP request: if the downstream destination isn’t reachable, the upstream microservice will need to work out what to do with the request; should it retry the call or give up?
               6. For guaranteed delivery to work, a broker will need to ensure that any messages not yet delivered are going to be held in a durable fashion until they can be delivered
               7. Often, the promise of guaranteed delivery can be undermined if the broker isn’t set up correctly. As an example, RabbitMQ requires instances in a cluster to communicate over relatively low-latency networks; otherwise the instances can start to get confused about the current state of messages being handled, resulting in data loss
            3. ##### Trust
               
               1. brokerın delivery guaranteed prensibini unutma ama kendin de anla
            4. ##### Other characteristics
               
               1. With Kafka, for example, ordering is guaranteed only within a single partition
               2. Some brokers provide transactions on write—for instance, Kafka allows you to write to multiple topics in a single transaction
               3. Another, somewhat controversial feature promised by some brokers is that of exactly once delivery
               4. One of the easier ways to provide guaranteed delivery is allowing the message to be resent
               5. but some brokers go further by guaranteeing exactly once delivery
               6. A very simple example would be for each message to have an ID, which a consumer can check each time a message is received, If a message with that ID has already been processed, the newer message can be ignored
            5. ##### Choices
               
               1. Popular examples include RabbitMQ, ActiveMQ, and Kafka
               2. AWS, for example, has Simple Queue Service (SQS), Simple Notification Service (SNS), and Kinesis, all of which provide different flavors of fully managed brokers
            6. ##### Kafka
               
               1. Part of that popularity is due to Kafka’s use in helping move large volumes of data around as part of implementing stream processing pipelines. This can help move from batch-oriented processing to more real-time processing
               2. There are a few characteristics of Kafka that are worth highlighting. Firstly, it is designed for very large scale
               3. Kafka is built to allow for multiple consumers and producers
               4. Another fairly unique feature of Kafka is message permanence.
               5. With a normal message broker, once the last consumer has received a message, the broker no longer needs to hold on to that message.
               6. With Kafka, messages can be stored for a configurable period
               7. This can allow consumers to reingest messages that they had already processed, or allow newly deployed consumers to process messages that were sent previously
      4. #### **Serialization Formats**
         
         1. With gRPC, for example, any data sent will be converted into protocol buffer format
            1. ##### Textual Formats
               
               1. REST APIs most often use a textual format for the request and response bodies
               2. gRPC works—using HTTP underneath but sending binary protocol buffers
               3. JSON has usurped XML as the text serialization format of choice, JSON became popular partly as a result of the backlash against XML, compactness and simplicity when compared to XML as another winning factor
               4. Avro is an interesting serialization format. It takes JSON as an underlying structure and uses it to define a schema-based format
            2. ##### Binary Formats
               
               1. the world of binary serialization protocols is where you want to be if you start getting worried about payload size or about the efficiencies of writing and reading the payloads
               2. Protocol buffers have been around for a while and are often used outside the scope of gRPC—they probably represent the most popular binary serialization format for microservice-based communication.
            3. ##### Schemas
               
               1. we should use schemas to define what our endpoints expose and what they accept
               2. If you’re working with raw XML, you’d use XML Schema Definition (XSD)
               3. if you’re working with raw JSON, you’d use JSON Schema
               4. SOAP works through the use of a WSDL
               5. while gRPC requires the use of a protocol buffer specification.
               6. for two key reasons. Firstly, they go a long way toward being an explicit representation of what a microservice endpoint exposes and what it can accept, This makes life easier both for developers working on the microservice and for their consumers, they certainly can help reduce the amount of documentation required
               7. I like explicit schemas, though, is how they help in terms of catching accidental breakages of microservice endpoints.
            4. ##### Structural Versus Semantic Contract Breakages
               
               1. structural breakages and semantic breakages
                  1. A structural breakage is a situation in which the structure of the endpoint changes in such a way that a consumer is now incompatible
                     1. this could represent fields or methods being removed, or new required fields being added
                  2. A semantic breakage refers to a situation in which the structure of the microservices endpoint remains the same but the behavior changes in such a way as to break consumers’ expectations.
            5. ##### Should You Use Schemas?
               
               1. By using schemas and comparing different versions of schemas, we can catch structural breakages
               2. Catching semantic breakages requires the use of testing
               3. if you have schemas but decide not to compare schema changes for compatibility, then the burden of catching structural breakages before you get to production also falls on testing
               4. having an explicit schema more than offsets any perceived benefit of having schemaless communication.
               5. The main argument for schemaless endpoints seems to be that schemas need more work and don’t give enough value
               6. lot of what schemas provide is an explicit representation of part of the structure contract between a client and a server. They help make things explicit and can greatly aid communication between teams as well as work as a safety net
               7. both client and server are owned by the same team—I am more relaxed about not having schemas.
      5. #### **Handling Change Between Microservices**
         
         1. it’s rarely a query regarding what sort of numbering scheme you should use and more about how you handle changes in the contracts between microservices
      6. #### **Avoiding Breaking Changes**
         
         1. ##### Expansion changes
            
            1. Add new things to a microservice interface; don’t remove old things.
               1. Probably the easiest place to start is by adding only new things to a microservice contract and not removing anything else
         2. ##### Tolerant reader
            
            1. When consuming a microservice interface, be flexible in what you expect.
            2. of implementing a reader able to ignore changes we don’t care about is what Martin Fowler calls a tolerant reader.
         3. ##### Right technology
            
            1. Pick technology that makes it easier to make backward-compatible changes to the interface.
               1. At the simple end of the spectrum, protocol buffers, the serialization format used as part of gRPC, have the concept of field number. Each entry in a protocol buffer has to define a field number, which client code expects to find. If new fields are added, the client doesn’t care
               2. Avro allows for the schema to be sent along with the payload, allowing clients to potentially interpret a payload much like a dynamic type.
               3. the REST concept of HATEOAS is largely all about enabling clients to make use of REST endpoints even when they change by making use of the previously discussed hypermedia links
         4. ##### Explicit interface
            
            1. Be explicit about what a microservice exposes. This makes things easier for the client and easier for the maintainers of the microservice to understand what can be changed freely.
            2. Having an explicit schema makes it clear to consumers what they can expect, but it also makes it much clearer to a developer working on a microservice as to what things should remain untouched to ensure you don’t break consumers.
            3. Put another way, an explicit schema goes a long way toward making the boundaries of information hiding more explicit—what’s exposed in the schema is by definition not hidden
            4. Having an explicit schema for RPC is long established and is in fact a requirement for many RPC implementations. REST, on the other hand, has typically viewed the concept of a schema as optional, to the point that I find explicit schemas for REST endpoints to be vanishingly rare
            5. OpenAPI specification gaining traction, and with the JSON Schema specification also gaining in maturity.
         5. ##### Catch accidental breaking changes early
            
            1. Have mechanisms in place to catch interface changes that will break consumers in production before those changes are deployed
            2. . We have Protolock for protocol buffers
            3. highlight consumer-driven contract testing, which explicitly helps in this area—Pact being an excellent example of a tool aimed specifically at this problem
      7. #### **Managing Breaking Changes**
         
         1. ##### Lockstep deployment
            
            1. Require that the microservice exposing the interface and all consumers of that interface are changed at the same time.
               1. lockstep deployment flies in the face of independent deployability.
               2. we need to give our consumers time to upgrade to the new interface. That leads us on to the next two options I’d consider.
         2. ##### Coexist incompatible microservice versions
            
            1. Run old and new versions of the microservice side by side.
            2. This can be an additional source of complexity.
            3. This would probably mean I have to branch the codebase for my service and that is always problematic
            4. it means I need smarts to handle directing consumers to the right microservice
            5. consider any persistent state our service might manage
            6. Customers created by either version of the service need to be stored and made visible to all services, no matter which version was used to create the data in the first place. This can be an additional source of complexity
            7. Coexisting concurrent service versions for a short period of time can make perfect sense, especially when you’re doing something like a canary release (we’ll be discussing this pattern more in “On to Progressive Delivery”).
         3. ##### Emulate the old interface
            
            1. Have your microservice expose the new interface and also emulate the old interface.
               1. The thing we want to avoid is forcing consumers to upgrade in lockstep with us
               2. as we always want to maintain the ability to release microservices independently of each other
               3. So if we want to release a breaking change, we deploy a new version of the service that exposes both the old and new versions of the endpoint
               4. Once all the consumers are no longer using the old endpoint, you can remove it along with any associated code
               5. For RPC, things can be a little trickier. I have handled this with protocol buffers by putting my methods in different namespaces—for example, v1.createCustomer and v2.createCustomer—but when you are trying to support different versions of the same types being sent over the network, this approach can become really painful.
         4. ##### Which Approach Do I Prefer?
            
            1. the same team manages both the microservice and all consumers, I am somewhat relaxed about a lockstep release in limited situations
            2. My general preference is to use emulation of old endpoints wherever possible.
            3. The challenges of implementing emulation are in my opinion much easier to deal with than those of coexisting microservice versions.
         5. ##### The Social Contract
            
            1. Keeping the old interface lying around can have a cost, and ideally you’d like to turn it off and remove associated code and infrastructure as soon as possible
            2. On the other hand, you want to give consumers as much time as possible to make a change
            3. in many cases the backward-incompatible changes you are making are often things that have been asked for by the consumers and/or will actually end up benefiting them
            4. As with schemas, having some degree of explicitness in how backward-incompatible changes will be made can greatly simplify things.
            5. You don’t necessarily need reams of paper and huge meetings to reach agreement on how changes will be handled
               1. How will you raise the issue that the interface needs to change?
               2. How will the consumers and microservice teams collaborate to agree on what the change will look like?
               3. Who is expected to do the work to update the consumers?
               4. When the change is agreed on, how long will consumers have to shift over to the new interface before it is removed?
            6. Remember, one of the secrets to an effective microservice architecture is to embrace a consumer-first approach
            7. I’ve heard from Netflix that they had issues (at least historically) with old set-top boxes using older versions of the Netflix APIs. These set-top boxes cannot be upgraded easily, so the old endpoints have to remain available unless and until the number of older set-top boxes drops to a level at which they can have their support disabled.
         6. ##### Tracking Usage
            
            1. Making sure you have logging in place for each endpoint your microservice exposes can help
            2. This could be something as simple as asking consumers to put their identifier in the user-agent header when making HTTP requests
            3. you could require that all calls go via some sort of API gateway where clients need keys to identify themselves.
         7. Extreme Measures
      8. #### **DRY and the Perils of Code Reuse in a Microservice World**
         
         1. DRY more accurately means that we want to avoid duplicating our system behavior and knowledge.
         2. that behavior is duplicated in many parts of your system, it is easy to forget everywhere you need to make a change. which can lead to bugs.
         3. So using DRY as a mantra in general makes sense.
            1. Sharing Code via Libraries
               1. One thing we want to avoid at all costs is overly coupling a microservice and consumers such that any small change to the microservice itself can cause unnecessary changes to the consumer
               2. If your use of shared code ever leaks outside your service boundary, you have introduced a potential form of coupling. Using common code like logging libraries is fine, as they are internal concepts that are invisible to the outside world.
               3. Rather than make this code shared, the company copies it for every new service to ensure that coupling doesn’t leak in.
               4. The really important point about sharing code via libraries is that you cannot update all uses of the library at once.
               5. So if you are using libraries for code reuse across microservice boundaries, you have to accept that multiple different versions of the same library might be out there at the same time.
               6. You can of course look to update all of these to the last version over time, but as long as you are OK with this fact, then by all means reuse code via libraries.
            2. Client libraries
               1. . The argument is that this makes it easy to use your service and avoids the duplication of code required to consume the service itself.
      9. #### **Service Discovery**
         
         1. ##### Domain Name System (DNS)
         2. ##### Dynamic Service Registries
            
            1. The downsides of DNS as a way of finding nodes in a highly dynamic environment have led to a number of alternative systems, most of which involve the service registering itself with some central registry, which in turn offers the ability to look up these services later on
            2. 1. ZooKeeper
                  1. ZooKeeper provides a hierarchical namespace for storing information. Clients can insert new nodes in this hierarchy, change them, or query them
               2. Consul
                  1. supports both configuration management and service discovery
                  2. Consul’s killer features is that it actually provides a DNS server out of the box
                  3. Consul also builds in other capabilities that you might find useful, such as the ability to perform health checks on nodes
                  4. Consul uses a RESTful HTTP interface for everything from registering a service to querying the key/value store or inserting health checks
               3. etcd and Kubernetes
                  1. Kubernetes. etcd has capabilities similar to those of Consul, and Kubernetes uses it for managing a wide array of configuration information.
                  2. the way service discovery works on Kubernetes is that you deploy a container in a pod, and then a service dynamically identifies which pods should be part of a service by pattern matching on metadata associated with the pod
                  3. Requests to a service will then get routed to one of the pods that make up that service
               4. Rolling your own
         3. ##### Don’t Forget the Humans!
      10. #### Service Meshes and API Gateways
          
          1. API gateway sits on the perimeter of your system and deals with north-south traffic. Its primary concerns are managing access from the outside world to your internal microservices
          2. A service mesh, on the other hand, deals very narrowly with communication between microservices inside your perimeter
          3. Put (very) simply, service meshes and API gateways can work as proxies between microservices.
          4. This can mean that they may be used to implement some microservice-agnostic behavior that might otherwise have to be done in code, such as service discovery or logging.
          5. If you are using an API gateway or a service mesh to implement shared, common behavior for your microservices, it’s essential that this behavior is totally generic—in other words, that the behavior in the proxy bears no relation to any specific behavior of an individual microservice.
             1. API Gateways
                1. the API gateway’s main concern in a microservices environment is mapping requests from external parties to internal microservices
                2. API gateways can be used to implement mechanisms like API keys for external parties, logging, rate limiting, and the like
                3. Much of the time, all an API gateway is actually being used for is to manage access to an organization’s microservices from its own GUI clients (web pages, native mobile applications) via the public internet
                4. Kubernetes natively handles networking only within the cluster and does nothing about handling communication to and from the cluster itself
                5. Where to use them
             2. Service Meshes
                1. common functionality associated with inter-microservice communication is pushed into the mesh.
                2. Common features implemented by service meshes include mutual TLS, correlation IDs, service discovery and load balancing, and more
                3. Service meshes can also be incredibly useful in implementing standard behavior across microservices created by different teams
                4. use of a service mesh, especially on Kubernetes, has increasingly become an assumed part of any given platform you might create for self-service deployment and management of microservices.
                5. Making it easy to implement common behavior across microservices is one of the big benefits of a service mesh
                6. Many service mesh implementations use the Envoy proxy for the basis of these locally running processes.
          6. API gateways and service meshes are primarily used to handle HTTP-related calls.
      11. #### Documenting Services
          
          1. Explicit Schemas
             1. As we’ve already discussed, schemas help show the structure, but they don’t go very far in helping communicate the behavior of an endpoint, so good documentation could still be required to help consumers understand how to use an endpoint
             2. if you decide not to use an explicit schema, your documentation will end up doing more work
             3. I’ve already introduced OpenAPI as a schema format, but it also is very effective in providing documentation
             4. Kubernetes, Ambassador’s developer portal is especially interesting. Ambassador is already a popular choice as an API gateway for Kubernetes, and its Developer Portal has the ability to autodiscover available OpenAPI endpoints
             5. The AsyncAPI format started off as an adaptation of OpenAPI
          2. The Self-Describing System
6. # **Chapter 6. Workflow**
   
   1. ## Database Transactions
      
      1. ### ACID Transactions
         
         1. #### Atomicity
            
            1. Ensures that the operations attempted within the transaction either all complete or all fail. If any of the changes we’re trying to make fail for some reason, then the whole operation is aborted, and it’s as though no changes were ever made.
               1. A microservice is free to use an ACID transaction for operations to its own database, for example. It’s just that the scope of these transactions is reduced to state change that happens locally within that single microservice.
               2. But fundamentally, we have to accept that by decomposing this operation into two separate database transactions, we’ve lost guaranteed atomicity of the operation as a whole.
               3. This lack of atomicity can start to cause significant problems, especially if we are migrating systems that previously relied on this property
               4. Normally, the first option that people start considering is still using a single transaction, but one that now spans multiple processes—a distributed transaction
               5. most common algorithms for implementing distributed transactions, the two-phase commit
         2. #### Consistency
            
            1. When changes are made to our database, we ensure it is left in a valid, consistent state.
         3. #### Isolation
            
            1. Allows multiple transactions to operate at the same time without interfering. This is achieved by ensuring that any interim state changes made during one transaction are invisible to other transactions.
         4. #### Durability
            
            1. Makes sure that once a transaction has been completed, we are confident the data won’t get lost in the event of some system failure.
               1. All relational database systems I’ve ever used dO
      2. ### Distributed Transactions—Two-Phase Commits
         
         1. The two-phase commit algorithm (sometimes shortened to 2PC) is frequently used in an attempt to give us the ability to make transactional changes in a distributed system, where multiple separate processes may need to be updated as part of the overall operation.
            1. a voting phase and a commit phase
         2. any worker says the change cannot take place, perhaps because the requested state change violates some local condition, the entire operation aborts.
         3. It’s important to highlight that the change does not take effect immediately after a worker indicates that it can make the change. Instead, the worker is guaranteeing that it will be able to make that change at some point in the future
         4. To guarantee that the change to VERIFIED can be made later, Worker A will likely have to lock the record to ensure that other changes cannot take place.
         5. If any workers didn’t vote in favor of the commit, a rollback message needs to be sent to all parties to ensure that they can clean up locally, which allows the workers to release any locks they may be holding
         6. If all workers agreed to make the change, we move to the commit phase
         7. It’s important to note that in such a system, we cannot in any way guarantee that these commits will occur at exactly the same time.
         8. Coming back to our definition of ACID, isolation ensures that we don’t see intermediate states during a transaction. But with this two-phase commit, we’ve lost that guarantee
         9. very often just coordinating distributed locks
         10. Managing locks and avoiding deadlocks in a single-process system isn’t fun. Now imagine the challenges of coordinating locks among multiple participants. It’s not pretty
         11. The more participants you have, and the more latency you have in the system, the more issues a two-phase commit will have.
         12. especially if the scope of locking is large, or if the duration of the transaction is large.
         13. What Google has managed to achieve with Spanner is impressive,
      3. Distributed Transactions—Just Say No
         
         1. I strongly suggest you avoid the use of distributed transactions like the two-phase commit to coordinate changes in state across your microservices
         2. the first option could be to just not split the data apart in the first place.
         3. you have pieces of state that you want to manage in a truly atomic and consistent way, and you cannot work out how to sensibly get these characteristics without an ACID-style transaction, then leave that state in a single database, and leave the functionality that manages that state in a single service (or in your monolith)
      4. ### Sagas
         
         1. Unlike a two-phase commit, a saga is by design an algorithm that can coordinate multiple changes in state, but avoids the need for locking resources for long periods of time
         2. Using sagas comes with the added benefit of forcing us to explicitly model our business processes, which can have significant benefits.
         3. addresses how best to handle operations known as long lived transactions (LLTs).
         4. Instead, the authors of the paper suggest we should break down these LLTs into a sequence of transactions, each of which can be handled independently.
         5. The idea is that the duration of each of these “sub” transactions will be shorter, and will modify only part of the data affected by the entire LLT
         6. We can break a single business process into a set of calls that will be made to collaborating services—this is what constitutes a saga
         7. , we don’t have atomicity at the level of the saga itself. We do have atomicity for each individual transaction inside the overall saga, as each one of them can relate to an ACID transactional change if needed
         8. each service, any state change can be handled within a local ACID transaction
         9. #### **Saga Failure Modes**
            
            1. The original saga paper describes two types of recovery: backward recovery and forward recovery.
            2. Backward recovery involves reverting the failure and cleaning up afterwards—a rollback. For this to work, we need to define compensating actions that allow us to undo previously committed transactions.
            3. you may expect that any failure mode triggers a backward recovery, a forward recovery, or perhaps a mix of the two
            4. It’s really important to note that a saga allows us to recover from business failures, not technical failures. For example, if we try and take payment from the customer but the customer has insufficient funds, then this is a business failure that the saga should be expected to handle
            5. The saga assumes the underlying components are working properly—that the underlying system is reliable, and that we are then coordinating the work of reliable components.
         10. #### **Saga rollbacks**
             
             1. With an ACID transaction, if we hit a problem, we trigger a rollback before a commit occurs. After the rollback, it is like nothing ever happened: the change we were trying to make didn’t take place. With our saga, though, we have multiple transactions involved, and some of those may have already committed before we decide to roll back the entire operation
             2. If these steps had all been done in a single database transaction, a simple rollback would clean it all up. However, each step in the order fulfillment process was handled by a different service call, each of which operated in a different transactional scope. There is no simple “rollback” for the entire operation.
             3. Instead, if you want to implement a rollback, you need to implement a compensating transaction. A compensating transaction is an operation that undoes a previously committed transaction
             4. It’s worth calling out the fact that these compensating transactions may not behave exactly as those of a normal database rollback. A database rollback happens before the commit, and after the rollback, it is as though the transaction never happened. In this situation, of course, these transactions did happen. We are creating a new transaction that reverts the changes made by the original transaction, but we can’t roll back time and make it as though the original transaction didn’t occur.
             5. Because we cannot always cleanly revert a transaction, we say that these compensating transactions are semantic rollbacks.
                
                1. Example: one of our steps may have involved sending an email to a customer to tell them their order was on the way. If we decide to roll that back, we can’t unsend an email!5 Instead, our compensating transaction could cause a second email to be sent to the customer, informing them that there was a problem with the order and it has canceled.
             6. Reordering workflow steps to reduce rollbacks
             7. Mixing fail-backward and fail-forward situations
                
                1. If for whatever reason we can’t dispatch the package (perhaps the delivery firm we use doesn’t have space in its vans to take an order today), it seems very odd to roll the whole order back. Instead, we’d probably just retry the dispatch (perhaps queuing it for the following day), and if that fails, we’d require human intervention to resolve the situation.
         11. #### **Implementing Sagas**
             
             1. Orchestrated sagas more closely follow the original solution space and rely primarily on centralized coordination and tracking. These can be compared to choreographed sagas, which avoid the need for centralized coordination in favor of a more loosely coupled model but can make tracking the progress of a saga more complicated
                1. ##### *Orchestrated sagas*
                   
                   1. Orchestrated sagas use a central coordinator (what we’ll call an orchestrator from now on) to define the order of execution and to trigger any required compensating action.
                   2. our central Order Processor, playing the role of the orchestrator, coordinates our fulfillment process. It knows what services are needed to carry out the operation, and it decides when to make calls to those services. If the calls fail, it can decide what to do as a result. In general, orchestrated sagas tend to make heavy use of request-response interactions between services: the Order Processor sends a request to services (such as a Payment Gateway) and expects a response to let it know if the request was successful and provide the results of the request.
                   3. Having our business process explicitly modeled inside the Order Processor is extremely beneficial. It allows us to look at one place in our system and understand how this process is supposed to work. That can make the onboarding of new people easier and help impart a better understanding of the core parts of the system.
                   4. First, by its nature, this is a somewhat coupled approach. Our Order Processor needs to know about all the associated services, resulting in a higher degree of domain coupling. While domain coupling is not inherently bad, we’d still like to keep it to a minimum, if possible. Here, our Order Processor needs to know about and control so many things that this form of coupling is hard to avoid
                   5. If logic has a place where it can be centralized, it will become centralized!
                2. ##### *Choreographed sagas*
                   
                   1. A choreographed saga aims to distribute responsibility for the operation of the saga among multiple collaborating services
                   2. If orchestration is a command-and-control approach, choreographed sagas represent a trust-but-verify architecture.
                   3. Conceptually, events are broadcast in the system, and interested parties are able to receive them
                   4. When the Payment Taken event is fired by the Payment Gateway, it causes reactions in both the Loyalty and Warehouse microservices. The Warehouse reacts by dispatching the package, while the Loyalty microservice reacts by awarding points.
                   5. Typically, you’d use some sort of message broker to manage the reliable broadcast and delivery of events. It’s possible that multiple microservices may react to the same event, and that is where you would use a topic.
                   6. They need to know only what to do when a certain event is received—we’ve drastically reduced the amount of domain coupling
                   7. The flip side of this is that it can be harder to work out what is going on. With orchestration, our process was explicitly modeled in our orchestrator
                   8. The lack of an explicit representation of our business process is bad enough, but we also lack a way of knowing what state a saga is in, which can deny us the chance to attach compensating actions when required.
                   9. One of the easiest ways of doing this is to project a view regarding the state of a saga by consuming the events being emitted. If we generate a unique ID for the saga, what is known as a correlation ID, we can put it into all of the events that are emitted as part of this saga. When one of our services reacts to an event, the correlation ID is extracted and used for any local logging processes, and it’s also passed downstream with any further calls or events that are fired
                   10. We could then have a service whose job it is to just vacuum up all these events and present a view of what state each order is in, and perhaps programmatically carry out actions to resolve issues as part of the fulfillment process if the other services couldn’t do it themselves
                   11. I consider some form of correlation ID essential for choreographed sagas like this
                3. ##### *Mixing styles*
                   
                   1. when managing the packaging and dispatch of an order, we may use an orchestrated flow even if the original request was made as part of a larger choreographed saga
                   2. If you do decide to mix styles, it’s important that you still have a clear way to understand what state a saga is in, and what activities have already happened as part of a saga. Without this, understanding failure modes becomes complex, and recovery from failure is difficult
                   3. we’ll look at concepts such as correlation IDs and log aggregation and how they can help in this regard
                4. ##### *Should I use choreography or orchestration (or a mix)*?
                   
                   1. I am very relaxed in the use of orchestrated sagas when one team owns implementation of the entire saga
                   2. If you have multiple teams involved, I greatly prefer the more decomposed choreographed saga, as it is easier to distribute responsibility for implementing the saga to the teams
                   3. It’s worth noting that as a general rule, you’ll be more likely to gravitate toward request-response–based calls with orchestration, whereas choreography tends to make heavier use of events.
7. # **Chapter 7. Build**
   
   1. A Brief Introduction to Continuous Integration
      
      1. With CI, the core goal is to keep everyone in sync with each other, which we achieve by frequently making sure that newly checked-in code properly integrates with existing code
      2. CI server detects that the code has been committed, checks it out, and carries out some verification such as making sure that the code compiles and that tests pass
      3. We get fast feedback as to the quality of our code, through the use of static analysis and testing
      4. CI is a key practice that allows us to make changes quickly and easily, and without which the journey into microservices will be painful
      5. Do you check in to mainline once per day?
         
         1. You need to make sure your code integrates. If you don’t check your code together with everyone else’s changes frequently, you end up making future integration harder.
      6. Do you have a suite of tests to validate your changes?
         
         1. Without tests, we just know that syntactically our integration has worked, but we don’t know if we have broken the behavior of the system. CI without some verification that our code behaves as expected isn’t CI.
      7. When the build is broken, is it the #1 priority of the team to fix it?
      8. Branching Models
         
         1. The problem is that when you work on a feature branch, you aren’t regularly integrating your changes with everyone else. Fundamentally, you are delaying integration. And when you finally decide to integrate your changes with everyone else, you’ll have a much more complex merge
         2. The alternative approach is to have everyone check in to the same “trunk” of source code. To keep changes from impacting other people, techniques like feature flags are used to “hide” incomplete work. This technique of everyone working off the same trunk is called trunk-based development
         3. A branch-heavy approach is still common in open source development, often through adopting the “GitFlow” development model
   2. Build Pipelines and Continuous Delivery
      
      1. This build pipeline concept gives us a nice way of tracking the progress of our software as it clears each stage, helping give us insight into the quality of our software.
      2. We create a deployable artifact, the thing that will ultimately be deployed into production, and use this artifact throughout the pipeline.
      3. s whatever checks are carried out at a stage in the pipeline, it can then move on to the next step. If it doesn’t pass a stage, our CI tool can let us know which stages the build has passed and can get visibility about what failed. If we need to do something to fix it, we’d make a change and check it in, allowing the new version of our microservice to try and pass all the stages before being available for deployment. In Figure 7-2, we see an example of this: build-120 failed the fast test stage, build-121 failed at the performance tests, but build-122 made it all the way to production.
      4. Continuous deployment can therefore be considered an extention of continuous delivery.
         
         1. Without continuous delivery, you can’t do continuous deployment. But you can do continuous delivery without doing continuous deployment.
      5. Artifact Creation
         
         1. it can theoretically introduce problems if the build configuration isn’t exactly the same each time
         2. the artifact you verify should be the artifact you deploy!
         3. Build your deployable artifact once and once only, and ideally do it pretty early in the pipeline.
         4. this artifact is stored in an appropriate repository—this could be something like Artifactory or Nexus
   3. Mapping Source Code and Builds to Microservices
      
      1. One Giant Repo, One Giant Build
      2. This model can work perfectly well if you buy into the idea of lockstep releases
      3. for example, changing behavior in the User service in Figure 7-5—all the other services get verified and built.
      4. Pattern: One Repository per Microservice (aka Multirepo)
         
         1. 
         2. Any change to the User source code repository triggers the matching build, and if that passes, I’ll have a new version of my User microservice available for deployment. Having a separate repository for each microservice also allows you to change ownership on a per-repository basis
      5. Pattern: Monorepo
      6. Defining ownership
8. Chapter 8. Deployment
   
   1. From Logical to Physical
      
      1. 
      2. This logical view of our microservices can hide a wealth of complexity when it comes to actually running them on real infrastructure
      3. Multiple Instances
         
         1. if we assume that in this situation we’re using some form of HTTP-based API, a load balancer would be enough to handle routing of requests to different instances
         2. In practice, this means that any solution you deploy should be distributed across multiple availability zones.
      4. The Database
         
         1. so any database used by a microservice for managing its state is considered to be hidden inside the microservice.
         2. don’t share databases
         3. One of our major concerns is that when sharing a database across multiple different microservices, the logic associated with accessing and manipulating that state is now spread across different microservices.But here the data is being shared by different instances of the same microservice.
      5. Database deployment and scaling
         
         1. A common example would be to split load for reads and writes between a primary node and one or more nodes that are designated for read-only purposes
         2. All read-only traffic goes to one of the read replica nodes, and you can further scale read traffic by adding additional read nodes
         3. it’s more difficult to scale writes by adding additional machines (typically sharding models are required, which adds additional complexity), so moving read-only traffic to these read replicas can often free up more capacity on the write node to allow for more scaling.
         4. both be served by the same underlying database engine and hardware, as shown in Figure 8-6. This can have significant benefits—it allows you to pool hardware to serve multiple microservices, it can reduce licensing costs, and it can also help reduce the work around management of the database itself.
         5. run from the same hardware and database engine, they are still logically isolated databases
         6. AWS’s Relational Database Service (RDS), for example, can automatically handle concerns like backups, upgrades, and multiavailability zone failover, and similar products are available from the other public cloud providers.
   2. Principles of Microservice Deployment
      
      1. Isolated Execution
      2. Focus on Automation
      3. Infrastructure as Code (IAC)
      4. Zero-Downtime Deployment
      5. Desired State Management
         
         1. Prerequisites
         2. GitOps
   3. Deployment Options
      
      1. Physical machine
         
         1. A microservice instance is deployed directly onto a physical machine, with no virtualization.
      2. Virtual machine
         
         1. A microservice instance is deployed on to a virtual machine.
      3. Container
         
         1. A microservice instance runs as a separate container on a virtual or physical machine. That container runtime may be managed by a container orchestration tool like Kubernetes.
            1. Isolated, differently
               
               1. Microsoft reacted to this by creating a cut-down operating system called Windows Nano Server
            2. Docker
               
               1. We get our isolation but at a manageable cost. We also hide underlying technology,
               2. Containers as a concept work wonderfully well for microservices,
               3. When it comes to implementing concepts like desired state management, though, we’ll need something like Kubernetes to handle it for us.
            3. Application Containers
      4. Application container
         
         1. A microservice instance is run inside an application container that manages other application instances, typically on the same runtime.
      5. Platform as a Service (PaaS)
         
         1. A more highly abstracted platform is used to deploy microservice instances, often abstracting away all concepts of the underlying servers used to run your microservices. Examples include Heroku, Google App Engine, and AWS Beanstalk.
      6. Function as a Service (FaaS)
         
         1. A microservice instance is deployed as one or more functions, which are run and managed by an underlying platform like AWS Lambda or Azure Functions. Arguably, FaaS is a specific type of PaaS, but it deserves exploration in its own right given the recent popularity of the idea and the questions it raises about mapping from a microservice to a deployed artifact.
      7. 
      8. 
   4. Kubernetes and Container Orchestration
      
      1. Kubernetes has in the last couple of years come to dominate this space.
      2. Containers are created by isolating a set of resources on an underlying machine.
      3. Tools like Docker allow us to define what a container should look like and create an instance of that container on a machine.
      4. The various container orchestration platforms also handle desired state management for us, ensuring that the expected state of a set of containers (microservice instances, in our case) is maintained.
      5. They also allow us to specify how we want these workloads to be distributed, allowing us to optimize for resource utilization, latency between processes, or robustness reasons.
      6. Without such a tool, you’ll have to manage the distribution of your containers,
         1. A Simplified View of Kubernetes Concepts
            
            1. This is where you say, “I want four of these pods,” and Kubernetes handles the rest
            2. With a deployment, you can do things like issue rolling upgrades (so you replace pods with a newer version in a gradual fashion to avoid downtime), rollbacks, scaling up the number of nodes, and more
            3. So, to deploy your microservice, you define a pod, which will contain your microservice instance inside it; you define a service, which will let Kubernetes know how your microservice will be accessed; and you apply changes to the running pods using a deployment
         2. Multitenancy and Federation
         3. Platforms and Portability
         4. Helm, Operators, and CRDs, Oh My!
            
            1. Consider the need to run Kafka on your Kubernetes cluster. You could create your own pod, service, and deployment specifications and run them yourself. But what about managing an upgrade to your Kafka setup? What about other common maintenance tasks you might want to deal with, like upgrading running stateful software?
            2. A number of tools have emerged that aim to give you the ability to manage these types of applications at a more sensible level of abstraction
   5. Feature Toggles
      
      1. This is most commonly used as part of trunk-based development, where functionality that isn’t yet finished can be checked in and deployed but still hidden from end users, but it has lots of applications outside of this.
   6. Canary Release
      
      1. with a canary rollout the idea is that a limited subset of our customers see new functionality.
      2. Another technique is to have two different versions of a microservice running side by side, and use the toggle to route to either the old or the new version.
      3. We could configure the percentage of our traffic seeing the new functionality,and over a period of a week we gradually increased this until everyone saw the new functionality.
   7. Parallel Run
      
      1. With a parallel run you do exactly that—you run two different implementations of the same functionality side by side, and send a request to the functionality to both implementations.
      2. With a microservice architecture, the most obvious approach might be to dispatch a service call to two different versions of the same service and compare the results.
      3. I explore the parallel run pattern in a lot more detail in Chapter 3 of my book Monolith to Microservices
9. # **Chapter 9. Testing**
   
   1. Types of Tests
      
      1. 
      2. At the bottom of the quadrant, we have tests that are technology facing—that is, tests that aid the developers in creating the system in the first place.
      3. The top half of the quadrant includes those tests that help the nontechnical stakeholders understand how your system works, which we call business-facing tests.
      4. If you currently carry out large amounts of manual testing, I would suggest you address that before proceeding too far down the path of microservices, as you won’t get many of their benefits if you are unable to validate your software quickly and efficiently
      5. When done well, manual exploratory testing is to a great extent about discovery.
      6. 
   2. Test Scope
      
      1. Unit Tests
         
         1. The primary goal of these tests is to give us very fast feedback about whether our functionality is good.
         2. Unit tests are also important for supporting refactoring of code, allowing us to restructure our code as we go secure in the knowledge that our small-scoped tests will catch us if we make a mistake.
      2. Service Tests
         
         1. Service tests are designed to bypass the user interface and test our microservices directly
         2. a service test would test an individual microservice’s capabilities.
         3. The cause of the test failure should be limited to just the microservice under test.
         4. To achieve this isolation, we need to stub out all external collaborators so only the microservice itself is in scope
         5. but if you decide to test against a real database or to go over networks to stubbed downstream collaborators, test times can increase
         6. These tests also cover more scope than a simple unit test, so when they fail, it can be harder to detect what is broken than with a unit test.
      3. End-to-End Tests
      4. Trade-Offs
         
         1. Unit tests are small in scope, so when they fail we can find the problem quickly. They are also quick to write and really quick to run. As our tests get larger in scope, we get more confidence in our system, but our feedback starts to suffer as the tests take longer to run. They are also more costly to write and maintain
   3. Implementing Service Tests
      
      1. The service and end-to-end tests are the ones that are more interesting, especially in the context of microservices, so that’s what we’ll focus on next.
      2. We then need to configure the stubs to send back responses to mimic the real-world microservices.
         1. Mocking or Stubbing
            
            1. When using a mock, I actually go further and make sure the call was made. If the expected call is not made, the test fails. Implementing this approach requires more smarts in the fake collaborators that we create, and if overused it can cause tests to become brittle.
            2. As noted, however, a stub doesn’t care if it is called 0, 1, or many times.
         2. A Smarter Stub Service
   4. Implementing (Those Tricky) End-to-End Tests
   5. Contract Tests and Consumer-Driven Contracts (CDCs)
      
      1. With contract tests, a team whose microservice consumes an external service writes tests that describe how it expects an external service will behave
      2. This is less about testing your own microservice and more about specifying how you expect an external service to behave
      3. The contract tests are in effect an explicit, programmatic representation of how the consumer (upstream) microservice expects the producer (downstream) microservice to behave.
      4. A good practice here is to have someone from the producer and consumer teams collaborate on creating the tests
      5. It could be argued, in fact, that implementing CDCs is just making more explicit the communication between the teams that must already exist. In cross-team collaboration, CDCs are an explicit reminder of Conway’s law
      6. CDCs sit at the same level in the test pyramid as service tests, albeit with a very different focus
   6. Developer Experience
10. # **Chapter 10. From Monitoring to Observability**
    
    1. As I’ve shown so far, I hope, breaking our system up into smaller, fine-grained microservices results in multiple benefits. It also, as we’ve also covered in some depth, adds significant sources of new complexity.
       
       1. Single Microservice, Single Server
          
          1. So what should we look for?
             1. First, we’ll want to get information from the host itself. CPU, memory—all of these things can be useful. Next, we’ll want to have access to the logs from the microservice instance itself. If a user reports an error, we should be able to see the error in these logs, hopefully giving us a way to work out what went wrong. At this point, with our single host, we can probably get by with just logging locally to the host and using command-line tools to look at the log.
       2. Single Microservice, Multiple Servers
       3. Multiple Services, Multiple Servers
          
          1. Aggregation of information—metrics and logs—play a vital part in making this happen
    2. Observability Versus Monitoring
       
       1. In practice, the more observable a system is, the easier it will be for us to understand what the problem is when something goes wrong.
       2. Monitoring, on the other hand, is something we do. We monitor the system. We look at it. Things will start to go wrong if you focus on just the monitoring—the activity—without thinking about what you expect that activity to achieve
       3. With a highly observable system, you’ll have a collection of external outputs that you can interrogate in different ways—the concrete outcome of having an observable system is that you can ask questions of your production system that you never would have thought to ask before.
          1. The Pillars of Observability? Not So Fast
             
             1. (metrics, event, logs, and traces)
             2. Observability is the extent to which you can understand what the system is doing based on external outputs. Logs, events, and metrics might help you make things observable, but be sure to focus on making the system understandable rather than throwing in lots of tools.
             3. Fundamentally, we can see any piece of information we can get from our system—any of these external outputs—generically as an event.
             4. We can project from this event stream a trace (assuming we can correlate these events), a searchable index, or an aggregation of numbers.
    3. Building Blocks for Observability
       
       1. We will be covering a number of building blocks that can help improve the observability of your system architecture
          1. Log aggregation
             1. Collecting information across multiple microservices, a vital building block of any monitoring or observability solution
                1. Processes (like our microservice instances) log to their local filesystem. A local daemon process periodically collects and forwards this log to some sort of store that can be queried by operators.
                2. you should view implementing a log aggregation tool as a prerequisite for implementing a microservice architecture.
                3. log aggregation can be incredibly valuable, especially when used with another concept we’ll cover shortly, correlation IDs
                   1. Correlating log lines
                      
                      1. When the first call is made, you generate a unique ID that will be used to correlate all subsequent calls related to the request. we generate this ID in the Gateway, and it is then passed along as a parameter to all subsequent calls.
                      2. You will, of course, need to ensure that each service knows to pass on the correlation ID
                      3. 
                      4. Once you have log aggregation, get correlation IDs in as soon as possible. Easy to do at the start and hard to retrofit later, they will drastically improve the value of your logs.
                   2. Timing
                   3. Implementations
                      
                      1. send logs to Elasticsearch, using Kibana as a way to slice and dice the resulting stream of logs.
                      2. 
          2. Metrics aggregation
             1. Capturing raw numbers from our microservices and infrastructure to help detect problems, drive capacity planning, and perhaps even scale our applications
          3. Distributed tracing
             1. Tracking a flow of calls across multiple microservice boundaries to work out what went wrong and derive accurate latency information
          4. Alerting
             1. What should you alert on? What does a good alert look like?
          5. Semantic monitoring
             1. Thinking differently about the health of our systems, and about what should wake us up at 3 a.m.
          6. Testing in production
             1. A summary of various testing in production techniques
                1. A/B testing
                   
                   1. With an A/B test, you deploy two different versions of the same functionality, with users seeing either the “A” or the “B” functionality.
                2. Canary release
                   
                   1. A small portion of your user base gets to see the new release of functionality.
                3. Parallel run
                   
                   1. With a parallel run, you execute two different equivalent implementations of the same functionality side by side
                4. Smoke tests
11. # **Chapter 11. Security**
12. # **Chapter 12. Resiliency**
    
    1. ## Resiliency actually means.These four concepts are:
       
       1. ### Robustness
          
          1. The ability to absorb expected perturbation
             1. Robustness is the concept whereby we build mechanisms into our software and processes to accommodate expected problems
       2. ### Rebound
          
          1. The ability to recover after a traumatic event
       3. ### Graceful extensibility
          
          1. How well we deal with a situation that is unexpected
       4. ### Sustained adaptability
          
          1. The ability to continually adapt to changing environments, stakeholders, and demands
    2. ## Failure Is Everywhere
       
       1. failure becomes inevitable
       2. Even for those of us not thinking at extreme scale, if we can embrace the possibility of failure we will be better off
       3. Understanding the things that are likely to fail is key to improving the robustness of our system
    3. ## Degrading Functionality
    4. ## Stability Patterns
       
       1. ### **Time-Outs**
          
          1. Adding time-outs to HTTP workers(client, server, pool). It's essentials. It's important!!
          2. You might end-up blocking whole systems. Resource exhausting. at the end if your workers which gets inbound requests start to wait indefinitely your application server might down.
          3. How you long you wait until a page load? 3-6 sec? consider this when you setting timeouts.
          4. Don’t just think about the time-out for a single service call; also think about a time-out for the overall operation, and abort the operation if this overall time-out budget is exceeded.
       2. ### **Retries**
          
          1. You should think about overall operation execution time. Again consider page load how much takes?
          2. So you can try 1 or 2 or 3 according your operation. also you need to add threshold time before retry.!
       3. ### **Bulkheads**: means if a part of the ship goes wrong don't let it to effect all part of the ship
          
          1. Thread or connection pools, Isolation to microservices, async communication, throttle requests[Rate Limiters]
          2. Separation of concerns can also be a way to implement bulkheads. By teasing apart functionality into separate microservices, we reduce the chance of an outage in one area affecting another.
          3. separate connection pools for each downstream connection
          4. They can also give you the ability to reject requests in certain conditions to ensure that resources don’t become even more saturated; this is known as load shedding
          5. Separation of concerns can also be a way to implement bulkheads. By teasing apart functionality into separate microservices, we reduce the chance of an outage in one area affecting another.
       4. ### **Circuit Breakers**
          
          1. Suggestion from the author of the book circuit breakers for all your synchronous downstream calls
          2. not only to protect the consumer from the downstream problem but also to potentially protect the downstream service from more calls that may be having an adverse impact
          3. With a circuit breaker, after a certain number of requests to the downstream resource have failed the circuit breaker is blown. All further requests that go through that circuit breaker fail fast while the breaker is in its (open) state,  After a certain period of time, the client sends a few requests through to see if the downstream service has recovered, and if it gets enough healthy responses it resets the circuit breaker(closed).
          4. While the circuit breaker is open, you have some options. One is to queue up the requests and retry them later on
             1. do not If this call is being made as part of a synchronous call chain, however, it is probably better to fail fast
             2. do if you’re carrying out some work as part of an asynchronous job
          5. When our circuit breaker(open) we can show users or client our service not available now. or not responding well.
             1. But rest of the system kept healthy. all in a fully automated way
          6. Circuit breakers help our application fail fast—and failing fast is always better than failing slow.
          7. The circuit breakers allow us to fail before wasting valuable time (and resources) waiting for an unhealthy downstream microservice to respond.
          8. If a microservice we will rely on as part of an operation is currently unavailable, we can abort the operation before we even start.
       5. ### **Isolation**
          
          1. Isolation also applies in terms of how we move from the logical to the physical. Consider two microservices that appear to be entirely isolated from one another. They don’t communicate with each other in any way. A problem with one of them shouldn’t impact the other, right? But what if both microservices are running on the same host, and one of the microservices starts using up all the CPU, causing that host to have issues?
          2. Isolation, like so many of the other techniques we have looked at, can help improve the robustness of our applications, but it’s rare that it does so for free.Deciding on the acceptable trade-offs around isolation versus cost and increased complexity, like so many other things, can be vital.
       6. ### **Redundancy**
          
          1. Having more copies of something can help when it comes to implementing redundancy, but it can also be beneficial when it comes to scaling our applications to handle increased load.
       7. ### **Middleware**
          
          1. role of middleware in the form of message brokers to help with implementing both request-response and event-based interactions.
          2. One of the useful properties of most message brokers is their ability to provide guaranteed delivery.
          3. You send a message to a downstream party, and the broker guarantees to deliver it
             1. the message broker software will have to implement things like retries and time-outs on your behalf
                - Now, in the case of our specific example with AdvertCorp, using middleware to manage request-response communication with the downstream turnip system might not actually have helped much. We would still not be getting responses back to our customers. The one potential benefit would be that we’d be relieving resource contention on our own system, but this would just shift to an increasing number of pending requests being held in the broker. Worse still, many of these requests asking for the latest turnip prices might relate to user requests that are no longer valid. An alternative could be to instead invert the interaction and use middleware to have the turnip system broadcast the last turnip ads, and we could then consume them. But if the downstream turnip system had a problem, we still wouldn’t be able to help the customer looking for the best turnip prices
          4. So using middleware like message brokers to help offload some robustness concerns can be useful, but not in every situation.
       8. ### **Idempotency**
          
          1. In idempotent operations, the outcome doesn’t change after the first application, even if the operation is subsequently applied multiple times
          2. If operations are idempotent, we can repeat the call multiple times without adverse impact. This is very useful when we want to replay messages that we aren’t sure have been processed, a common way of recovering from error.
          3. This mechanism works just as well with event-based collaboration and can be especially useful if you have multiple instances of the same type of service subscribing to events. Even if we store which events have been processed, with some forms of asynchronous message delivery there may be small windows in which two workers can see the same message. By processing the events in an idempotent manner, we ensure this won’t cause us any issues.
          4. Some people get quite caught up with this concept and assume it means that subsequent calls with the same parameters can’t have any impact, The key point here is that it is the underlying business operation that we are considering idempotent, not the entire state of the system.
          5. such as GET and PUT, are defined in the HTTP specification to be idempotent, but for that to be the case, they rely on your service handling these calls in an idempotent manner. If you start making these verbs nonidempotent, but callers think they can safely execute them repeatedly, you may get yourself into a mess.
    5. ## **CAP Theorem**
       
       1. we have three things we can trade off against each other: consistency, availability, and partition tolerance.
       2. Specifically, the theorem tells us that we get to keep two in a failure mode.
          1. Consistency is the system characteristic by which we will get the same answer if we go to multiple nodes.
          2. Availability means that every request receives a response.
          3. Partition tolerance is the system’s ability to handle the fact that communication between its parts is sometimes impossible.
       3. 

