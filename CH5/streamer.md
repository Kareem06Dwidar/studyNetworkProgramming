This Python script illustrates a basic TCP server and client interaction, where the client sends a series of messages to the server and then closes the socket, indicating it doesn't expect any response. It's a straightforward demonstration of a one-way data transfer using TCP sockets. Let's break down the script to understand how each component works.

### Import Libraries
```python
import socket
from argparse import ArgumentParser
```
- **`socket`**: Provides the necessary functions and methods to create sockets, enabling networking capabilities.
- **`ArgumentParser`**: Facilitates the parsing of command-line arguments, allowing the user to specify operating parameters like whether to run as a client or server.

### Server Function
```python
def server(address):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.bind(address)
    sock.listen(1)
    print('Run this script in another window with "-c" to connect')
    print('Listening at', sock.getsockname())
    sc, sockname = sock.accept()
    print('Accepted connection from', sockname)
    sc.shutdown(socket.SHUT_WR)
    message = b''
    while True:
        more = sc.recv(8192)  # arbitrary value of 8k
        if not more:
            print('Received zero bytes - end of file')
            break
        print('Received {} bytes'.format(len(more)))
        message += more
    print('Message:\n')
    print(message.decode('ascii'))
    sc.close()
    sock.close()
```
- **Socket Setup**: The server creates a TCP socket and sets options to reuse the address. It binds to the provided address and starts listening for incoming connections.
- **Connection Handling**: It accepts a connection and immediately shuts down the sending side because it only expects to receive data.
- **Data Reception**: Data is read in chunks until no more data is available (the client has closed the connection). It accumulates the received bytes and decodes them to print as a string.

### Client Function
```python
def client(address):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(address)
    sock.shutdown(socket.SHUT_RD)
    sock.sendall(b'Beautiful is better than ugly.\n')
    sock.sendall(b'Explicit is better than implicit.\n')
    sock.sendall(b'Simple is better than complex.\n')
    sock.close()
```
- **Connection Setup**: Similar to the server, the client sets up a TCP socket, connects to the server's address, and immediately shuts down its reading side as it only intends to send data.
- **Data Transmission**: Sends multiple messages encoded in bytes to the server. After sending all data, it closes the socket.

### Main Execution Block
```python
if __name__ == '__main__':
    parser = ArgumentParser(description='Transmit & receive a data stream')
    parser.add_argument('hostname', nargs='?', default='127.0.0.1',
                        help='IP address or hostname (default: %(default)s)')
    parser.add_argument('-c', action='store_true', help='run as the client')
    parser.add_argument('-p', type=int, metavar='port', default=1060,
                        help='TCP port number (default: %(default)s)')
    args = parser.parse_args()
    function = client if args.c else server
    function((args.hostname, args.p))
```
- **Command-line Arguments**: Allows the user to specify the hostname, port, and whether the script should run in client mode. Based on these inputs, it either starts the server or the client.

### Usage
To use this script:
- Start the server in one terminal window:
  ```bash
  python streamer.py
  ```
- Run the client in another terminal window to connect to the server and send the data:
  ```bash
  python streamer.py -c
  ```

This script effectively demonstrates a basic pattern where a client sends data to a server without expecting any response, useful for understanding socket programming basics and one-way data transmission patterns.
