# NAD-A3

## Pseudocode
### Client
START CLIENT

1. SET SERVER_IP = "Server's IP address"
2. SET SERVER_PORT = "Server's port number"

3. FUNCTION ConnectToServer()
   a. CREATE a TCP socket
   b. CONNECT to SERVER_IP:SERVER_PORT
   c. RETURN the connected socket

4. FUNCTION SendLogMessage(socket, logMessage)
   a. CONVERT logMessage to bytes
   b. SEND the bytes to the server through the socket
   c. PRINT "Message sent successfully."

5. FUNCTION ShowMenu()
   a. PRINT "1. Set Log Level (INFO, WARNING, ERROR)"
   b. PRINT "2. Enter Log Message"
   c. PRINT "3. Send Log"
   d. PRINT "4. Exit"
   e. RETURN USER_INPUT

6. MAIN()
   a. CONNECT TO SERVER
   b. WHILE True
      i. DISPLAY MENU
      ii. GET USER CHOICE
      iii. IF CHOICE == 1
          - PRINT "Enter log level (INFO, WARNING, ERROR): "
          - SET logLevel = USER INPUT
      iv. IF CHOICE == 2
          - PRINT "Enter log message: "
          - SET logMessage = USER INPUT
      v. IF CHOICE == 3
          - FORMAT message as "[logLevel] logMessage"
          - CALL SendLogMessage(socket, message)
      vi. IF CHOICE == 4
          - PRINT "Exiting..."
          - CLOSE socket
          - EXIT LOOP

END CLIENT

---

### LoggingServer
START SERVER

1. SET SERVER_IP = "Server's IP address"
2. SET SERVER_PORT = "Server's port number"
3. SET LOG_FILE = "log.txt"
4. SET RATE_LIMIT = "Maximum allowed requests (e.g., 10 per second)"
5. CREATE an empty DICTIONARY to track client request counts

6. FUNCTION StartServer()
   a. CREATE a TCP socket
   b. BIND socket to (SERVER_IP, SERVER_PORT)
   c. SET socket to LISTEN mode
   d. PRINT "Server listening on SERVER_IP:SERVER_PORT"
   e. WHILE True
      i. ACCEPT new client connection
      ii. CREATE a new thread/process to handle the client

7. FUNCTION HandleClient(client_socket, client_address)
   a. PRINT "Client connected: client_address"
   b. WHILE True
      i. RECEIVE log message from client
      ii. IF client disconnected, BREAK loop
      iii. IF RateLimitExceeded(client_address), SEND error message to client, CONTINUE
      iv. WRITE log message to LOG_FILE
      v. PRINT "Log saved: log_message"
   c. CLOSE client connection
   d. PRINT "Client disconnected: client_address"

8. FUNCTION RateLimitExceeded(client_address)
   a. CHECK if client_address is in the request count dictionary
   b. IF request count exceeds RATE_LIMIT within time window
      i. RETURN True (limit exceeded)
   c. OTHERWISE, UPDATE request count and RETURN False

9. MAIN()
   a. CALL StartServer()

END SERVER
