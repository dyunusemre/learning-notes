# gRPC (Remote Procedure Call)
- Open-source, high performance rpc.
1. HTTP/2 Based Communication
    - Allows for multiplexed, bi-directional streaming. This makes gRPC fast, lightweight, and efficient in terms of data transfer.
    - Multiplexing requests over a single connection(parallel request over a single connection)
2. Protocol Buffers
    - gRPC uses it for message serialization. Serialize data into binary format efficient and compact.
3. Language-Neutral
    - gRPC supports multiple programming languages, making it easy to
4. Bi-directional Streaming
    - gRPC supports bi-directional streaming, allowing for more complex use cases like long-lived connections
5. It uses Protocol Buffers as interface definition language. 
6. Components of gRPC
   - Services: Defined by using protocol buffers.
   - Clients: calls gRPC servers
   - Protocol Buffers: Interface definition language and message serialization
   - Channels: Logical connections for gRPC server
   - Stubs: Provides methods to initiate calls from gRPC client. (blocking or non-blocking stubs exists)
7. Services defined in .proto files. with their request and response types.
8. Unary, Server Streaming, Client Streaming, and Bidirectional Streaming exists in gRPC
   - Unary: Client sends a request and gets a single response
   - Server Streaming: Client sends a request and gets stream of responses
   - Client Streaming: Client sends a stream of request and gets a single response
   - Bidirectional Streaming: Both client and server send stream data to each other

9. Proto files are backward compatible. adding new fields without changing existing. using reserved keyword to remove existing ones. Avoiding changing field number and field name important.
10. gRPC supports TLS
11. Pros: Efficient serialization/deserialization, multiplexing, full-duplex streaming, support for multiple languages.
12. Cons: Binary format might not be human-readable, may require more tooling compared to REST, might be overkill for simple CRUD operations.
13. Authentication can be implemented using interceptors and tokens (like JWT)
14. net.devh:grpc-spring-boot-starter project is a popular Spring Boot starter for gRPC.
   -  By default, gRPC itself uses a multi-threaded server to handle requests. This is achieved through the use of Netty, an asynchronous network application framework, which is the default transport layer for gRPC in Java.
   -  It can handle multiple requests concurrently, thanks to the underlying multi-threaded model of Netty. [Event Loop Group]
   - For Netty's NioEventLoopGroup (which is often used with gRPC), the default number of threads is the number of processors available to the JVM, which can be obtained with Runtime.getRuntime().availableProcessors() in Java
