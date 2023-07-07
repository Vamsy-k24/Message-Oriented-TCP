README for MyTCP Socket Implementation
========================================

This project involves the implementation of a message-oriented TCP protocol called MyTCP, which guarantees that messages sent with a single `my_send` call will be received by a single `my_recv` call. MyTCP provides reliability, in-order delivery, and exactly-once semantics similar to TCP but with message-oriented behavior like UDP.  

Overview
--------
The implementation of MyTCP involves creating a layer of software between the MyTCP function calls and the underlying TCP socket calls. The program takes the MyTCP calls and converts them into equivalent TCP calls while performing additional bookkeeping to ensure message-oriented behavior.  

Functions Implemented
----------------------
The following functions need to be implemented as part of the MyTCP library:  
  
1. `my_socket`: Opens a standard TCP socket, creates two threads (R and S), and initializes necessary data structures.  
2. `my_bind`: Binds the socket to a specific address and port.  
3. `my_listen`: Puts the socket into listening mode to accept incoming connections.  
4. `my_accept`: Accepts a connection on the MyTCP socket by making the accept call on the TCP socket (server-side only).  
5. `my_connect`: Establishes a connection through the MyTCP socket by making the connect call on the TCP socket.   
6. `my_send`: Sends a message using MyTCP. Messages are defined as the data sent in one `my_send` call.  
7. `my_recv`: Receives a message using MyTCP. This is a blocking call and returns only when a message sent using `my_send` is received.  
8. `my_close`: Closes the MyTCP socket and cleans up all resources.  

Thread R
--------
Thread R handles messages received from the TCP socket. It waits on a `recv` call on the TCP socket, receives incoming data (which may come in multiple `recv` calls), interprets the data to form complete messages, and puts the messages in the `Received_Message` table.  

Thread S
--------
Thread S handles messages to be sent through the TCP socket. It periodically wakes up after a certain time interval (T) and checks if any messages are waiting to be sent in the `Send_Message` table. If there are pending messages, it uses one or more `send` calls on the TCP socket to send them. Each `send` call can send a maximum of 1000 bytes.  
  
Tables and Data Structures
--------------------------
The following tables and data structures are used in the MyTCP implementation:  
  
1. `Send_Message`: This table stores the messages to be sent. Each entry corresponds to one message sent using `my_send`.  
2. `Received_Message`: This table stores the received messages. Each entry contains one complete message received using `my_send`.  
  
Additional Considerations
-------------------------
1. Message boundaries: Since TCP is a byte-oriented protocol, the implementation needs to remember and send message boundaries. This allows the receiving side to reconstruct the complete message even if it arrives in multiple parts.  
2. Shared resources: The `Send_Message` and `Received_Message` tables are shared between different threads and require proper mutual exclusion to prevent data races.  
3. Maximum message length: The maximum length of a message is limited to 5000 bytes.  
4. Single MyTCP socket: For simplicity, the implementation assumes that a program will create only one MyTCP socket, and the server follows an iterative model.  
5. Blocking behavior: The blocking behavior in both `my_send` and `my_recv` can be achieved by using the `sleep` function to wait for some time before attempting again.  
  
Documentation
-------------
For detailed information on the data structures, fields, and function behaviors, please refer to the `mysocket.h` and `mysocket.c` files. The `documentation.txt` file provided with the submission contains the following information:  
- Description of all data structures used and their fields in `mysocket.c`  
- List and description of all functions implemented in `mysocket.c`  
- Sequence of function calls and table usage when a `my_send` call is made  
- Sequence of function calls and table usage when a `my_recv` call is made  
  
Building the Library
--------------------
To build the static library `libmsocket.a`, use the provided makefile. The makefile compiles the `mysocket.c` file, creates the library using the `ar` command, and generates the `libmsocket.a` file.  
  
To compile the library, execute the following command: make  


Thank you!
