![Screenshot 2024-08-10 at 10.32.06 PM](./Screenshot%202024-08-10%20at%2010.32.06%20PM.png)

1. client starts tcp three way handshake with a SYN flag set to server

2. server responds with SYN+ACK 

3. client responds with ACK to server

4. After TCP three way handshake,  the client sends a Client Hello message which contains some details like, highest TLS version supported, A list of cypher suite supported by the client, a secret number 'A' ,  ( A=n ^ a mod p ), where 'a' is a random value 'n' , 'p' are values from standards.

5. Server responds back with a Server Hello message, including its public key, Signature, Chosen TLS version, Selected cypher suite  and a secret number 'B' ,   ( B= n ^ b mod p ), where 'b' is a random value 'n' , 'p' are values from standards.

