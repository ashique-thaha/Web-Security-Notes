
`HTTP 1.0 :`

So here in http 1.0, tcp three way handshake occurs, the client makes a request to the server and the server responds with a response message as we know,  After the response the connection is closed. Here client will have to make multiple requests.

`HTTP 1.1`:

Here, http 1.1 allowed pipelining. in HTTP 1.1, the same TCP connection could be reused for multiple requests one by one and we will have the response from the server in the order of requests.

Eventually tcp became a bottle neck, imagine we have to get hundreds of different files, images etc. we are doing this on a single tcp channel.


even if we start several tcp connections, we could only send one request over one tcp.


`HTTP 2.0`

Here http 2.0 allows to set-up streams  on top of one tcp connection.

so basically we can imagine each streams as a tcp connection within the main one.

so http 2.0 allows multiplexing through stream. To be more specific, in each streams there will be a stream id , http headers and other request parameters. 


`HTTP 3.0`

Similar to it's predecessors, http 3.0, uses TCP at three way handshake, and the QUIC protocol uses UDP connections for faster data transmissions it uses default port 443.

even if we switched the networks, there is no need of a new tcp connection, 

as the underlying QUIC  UDP connections can change port number, ip address etc.

Facebook , google  etc are already using these protocols.