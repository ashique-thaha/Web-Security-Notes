![Screenshot 2024-08-10 at 10.32.06 PM](./Screenshot%202024-08-10%20at%2010.32.06%20PM.png)

1. client starts tcp three way handshake with a SYN flag set to server

2. server responds with SYN+ACK 

3. client responds with ACK to server

4. After TCP three way handshake,  the client sends a Client Hello message which contains some details like, highest TLS version supported, A list of cypher suite supported by the client, a secret number 'A' ,  ( A=n ^ a mod p ), where 'a' is a random value 'n' , 'p' are values from standards.

5. Server responds back with a Server Hello message, including its public key, Signature, Chosen TLS version, Selected cypher suite  and a secret number 'B' ,   ( B= n ^ b mod p ), where 'b' is a random value 'n' , 'p' are values from standards.

6. Client Generates Pre Mater Secret key (PMS) and encrypts it with server's public key and sends it to the server.

7. Server receives PMS and decrypts it using their private key only accessible  to it.

8. Client and server derives Master Secret Key by using the parameters received in the above steps, which will be used as the symmetric key for communicating each other.

9. Client sends a client finish message which includes the hash of previous handshakes, and server verifies it with expected hash.

10. Server sends a server finish message which includes the hashes of previous handshakes. client verifies it with expected hashes.

11. If everything works correctly the client and server will starts symmetric key encrypted communication.

	`note`: if the hashes doesn't match the connection will be terminated and the whole process has to restart.