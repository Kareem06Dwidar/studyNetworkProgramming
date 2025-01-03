This Python script demonstrates how to transmit data over TCP using length-prefixed blocks, ensuring that complete messages are sent and received. This method avoids issues related to partial reads, which can occur in streaming data transfers where the message boundaries must be known. Here's a detailed breakdown of each component of the script:

### Import Libraries

```python
import socket, struct
from argparse import ArgumentParser
```
- **`socket`**: Provides the necessary functions and methods for network communications.
- **`struct`**: Facilitates byte conversions by packing and unpacking structured data, particularly useful for handling fixed-size elements like message headers.
- **`ArgumentParser`**: Simplifies the process of writing user-friendly command-line interfaces.

### Global Structure Definition

```python
header_struct = struct.Struct('!I')  # messages up to 2**32 - 1 in length
```
- This line defines a struct for handling the headers of messages. `!I` specifies a network-order (big-endian) unsigned int, which will be used to prefix messages with their length, supporting messages up to approximately 4.29 billion bytes in length.

### Utility Functions

#### `recvall(sock, length)`
This function ensures that exactly `length` bytes are read from the socket. It handles partial reads which can commonly occur in network programming.

```python
def recvall(sock, length):
    blocks = []
    while length:
        block = sock.recv(length)
        if not block:
            raise EOFError('socket closed with {} bytes left in this block'.format(length))
        length -= len(block)
        blocks.append(block)
    return b''.join(blocks)
```
- **Purpose**: To read a precise number of bytes from the socket.
- **Process**: It keeps reading from the socket until the specified number of bytes is reached or the socket closes unexpectedly.

#### `get_block(sock)`
This function reads a block of data from the socket, where the block's length is prefixed as defined by `header_struct`.

```python
def get_block(sock):
    data = recvall(sock, header_struct.size)
    (block_length,) = header_struct.unpack(data)
    return recvall(sock, block_length)
```
- **Functionality**: First retrieves the length of the upcoming block, then reads exactly that amount of data.

#### `put_block(sock, message)`
Complements `get_block` by sending data with a length prefix.

```python
def put_block(sock, message):
    block_length = len(message)
    sock.send(header_struct.pack(block_length))
    sock.send(message)
```
- **Operation**: Packs the length of the message and sends it, followed by the message itself.

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
    while True:
        block = get_block(sc)
        if not block:
            break
        print('Block says:', repr(block))
    sc.close()
    sock.close()
```
- **Setup**: Prepares the server socket and waits for a connection.
- **Connection Handling**: After accepting a connection, it reads data blocks until there are no more (an empty block signifies the end).

### Client Function

```python
def client(address):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(address)
    sock.shutdown(socket.SHUT_RD)
    put_block(sock, b'Beautiful is better than ugly.')
    put_block(sock, b'Explicit is better than implicit.')
    put_block(sock, b'Simple is better than complex.')
    put_block(sock, b'')
    sock.close()
```
- **Process**: Connects to the server and sends several blocks of text, with each block's length prefixed.

### Main Execution Block

```python
if __name__ == '__main__':
    parser = ArgumentParser(description='Transmit & receive blocks over TCP')
    parser.add_argument('hostname', nargs='?', default='127.0.0.1', help='IP address or hostname (default: %(default)s)')
    parser.add_argument('-c', action='store_true', help='run as the client')
    parser.add_argument('-p', type=int, metavar='port', default=1060, help='TCP port number (default: %(default)s)')
    args = parser.parse_args()
    function = client if args.c else server
    function((args.hostname, args.p))
```
- **Configuration**: Sets up command-line arguments for running as either a client or a server. Determines the mode based on the `-c` flag and calls the appropriate function.

This script demonstrates a robust method for sending and receiving data over TCP, ensuring that message boundaries are respected, making it suitable for applications that require structured data exchange.
