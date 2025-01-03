### Import Libraries
```python
import argparse, socket
```
- **`argparse`**: This library is used for parsing command-line arguments to the script, allowing users to specify whether the script should run as a client or a server, and to provide necessary network addresses or port numbers.
- **`socket`**: Essential for TCP/IP networking, this module enables the creation of socket objects necessary for connections between devices on a network.

### Helper Function: `recvall`
```python
def recvall(sock, length):
    data = b''
    while len(data) < length:
        more = sock.recv(length - len(data))
        if not more:
            raise EOFError('was expecting %d bytes but only received %d bytes before the socket closed' % (length, len(data)))
        data += more
    return data
```
- **Purpose**: Ensures that the specified amount of data (in this case, 16 bytes) is completely received before proceeding. TCP sockets might receive data in chunks, so this function loops until all required data is gathered.
- **Error Handling**: Raises an `EOFError` if the socket closes before all data is received, indicating a possible connection issue or data transmission error.

### Server Function
```python
def server(interface, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.bind((interface, port))
    sock.listen(1)
    print('Listening at', sock.getsockname())
    while True:
        print('Waiting to accept a new connection')
        sc, sockname = sock.accept()
        print('We have accepted a connection from', sockname)
        print('  Socket name:', sc.getsockname())
        print('  Socket peer:', sc.getpeername())
        message = recvall(sc, 16)
        print('  Incoming sixteen-octet message:', repr(message))
        sc.sendall(b'Farewell, client')
        sc.close()
        print('  Reply sent, socket closed')
```
- **Setup**: Initializes a TCP server socket, binds it to a specified interface and port, and listens for incoming connections.
- **Connection Handling**: Accepts new connections, receives a 16-byte message, and sends a fixed response. Each connection is closed after handling.

### Client Function
```python
def client(host, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((host, port))
    print('Client has been assigned socket name', sock.getsockname())
    sock.sendall(b'Hi there, server')
    reply = recvall(sock, 16)
    print('The server said', repr(reply))
    sock.close()
```
- **Setup**: Creates a TCP client socket and connects to the server at the specified host and port.
- **Data Transmission**: Sends a greeting message to the server and waits for a reply, which is expected to be 16 bytes long. The connection is closed after receiving the response.

### Main Execution Block
```python
if __name__ == '__main__':
    choices = {'client': client, 'server': server}
    parser = argparse.ArgumentParser(description='Send and receive over TCP')
    parser.add_argument('role', choices=choices, help='which role to play')
    parser.add_argument('host', help='interface the server listens at; host the client sends to')
    parser.add_argument('-p', metavar='PORT', type=int, default=1060, help='TCP port (default 1060)')
    args = parser.parse_args()
    function = choices[args.role]
    function(args.host, args.p)
```
- **Configuration**: Allows the user to specify the role (`client` or `server`) and relevant network settings via command-line arguments. The script dynamically selects the function to execute based on user input.

This script exemplifies basic TCP operations, highlighting the protocol's capability for reliable communication by ensuring complete data transmission and orderly conduct of sessions. It's a great tool for understanding the basics of TCP communication in network programming.
