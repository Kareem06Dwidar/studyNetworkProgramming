### Import Libraries
```python
import argparse, socket
from datetime import datetime
```
- **`argparse`**: A standard Python library for parsing command-line arguments.
- **`socket`**: A module that provides access to the BSD socket interface, which is used for network communication.
- **`datetime`**: Module used to work with dates and times. Here, it's used to fetch the current time.

### Constants
```python
MAX_BYTES = 65535
```
- **`MAX_BYTES`**: This constant defines the maximum amount of data that can be sent at once through the socket. 65535 bytes is the maximum size that can be handled in one UDP datagram.

### Server Function
```python
def server(port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    sock.bind(('127.0.0.1', port))
    print('Listening at {}'.format(sock.getsockname()))
    while True:
        data, address = sock.recvfrom(MAX_BYTES)
        text = data.decode('ascii')
        print('The client at {} says {!r}'.format(address, text))
        text = 'Your data was {} bytes long'.format(len(data))
        data = text.encode('ascii')
        sock.sendto(data, address)
```
- **Server Setup**: 
  - A UDP socket is created using `socket.socket()`, specifying `AF_INET` for IPv4 and `SOCK_DGRAM` for UDP.
  - The socket is bound to the localhost address (`127.0.0.1`) and a specified port, making it listen for incoming messages on that IP and port.
- **Receiving and Responding**:
  - The server enters an infinite loop where it waits for data with `recvfrom()`, which blocks until a message is received.
  - Incoming data is decoded from ASCII to a string, and the server prints the message along with the sender's address.
  - The server then sends back a response indicating the length of the received data.

### Client Function
```python
def client(port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    text = 'The time is {}'.format(datetime.now())
    data = text.encode('ascii')
    sock.sendto(data, ('127.0.0.1', port))
    print('The OS assigned me the address {}'.format(sock.getsockname()))
    data, address = sock.recvfrom(MAX_BYTES)
    text = data.decode('ascii')
    print('The server {} replied {!r}'.format(address, text))
```
- **Client Setup**: 
  - Similarly creates a UDP socket.
  - Prepares a message that includes the current time, encodes it in ASCII, and sends it to the server at localhost and the specified port.
- **Sending and Receiving Data**:
  - After sending, it prints the address assigned to it by the OS.
  - It then waits to receive a response from the server and prints that response once received.

### Main Block
```python
if __name__ == '__main__':
    choices = {'client': client, 'server': server}
    parser = argparse.ArgumentParser(description='Send and receive UDP locally')
    parser.add_argument('role', choices=choices, help='which role to play')
    parser.add_argument('-p', metavar='PORT', type=int, default=1060,
                        help='UDP port (default 1060)')
    args = parser.parse_args()
    function = choices[args.role]
    function(args.p)
```
- **Argument Parsing**:
  - The script uses `argparse` to handle command-line inputs. It takes one positional argument for the role (either 'client' or 'server') and one optional argument for the port number.
- **Execution**:
  - Based on the command-line arguments, it determines whether to run the script as a client or a server and on which port.

This script is a basic demonstration of setting up a UDP client and server that can communicate over localhost. It showcases sending and receiving simple messages, handling basic networking operations through Python’s socket API.
