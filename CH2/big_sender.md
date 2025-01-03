#### Import Libraries
```python
import argparse, socket
```
- **`argparse`**: This library is used to handle command-line arguments, allowing the script to accept different roles and settings when executed.
- **`socket`**: Essential for network communication, this module provides methods and settings to establish various types of sockets, including UDP.

#### Constants
```python
BUFSIZE = 65535
```
- **`BUFSIZE`**: Defines the maximum amount of data (in bytes) that the script can handle in one operation. This is set to the largest size that can be transmitted in a single UDP datagram.

#### Server Function
```python
def server(interface, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # Create UDP socket
    sock.bind((interface, port))  # Bind socket to an interface and port
    print('Listening for datagrams at {}'.format(sock.getsockname()))
    while True:
        data, address = sock.recvfrom(BUFSIZE)  # Receive data
        text = data.decode('ascii')  # Decode data to text
        print('The client at {} says: {!r}'.format(address, text))  # Print received message and sender info
```
- **Functionality**:
  - Sets up a UDP socket and binds it to a specified network interface and port, making it ready to receive data sent to this address.
  - Continuously listens for incoming datagrams and prints out the sender's information along with the message content.

#### Client Function
```python
def client(network, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # Create UDP socket
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)  # Enable broadcasting
    text = 'Broadcast datagram!'
    sock.sendto(text.encode('ascii'), (network, port))  # Send broadcast message
```
- **Functionality**:
  - Initializes a UDP socket for sending data.
  - Sets the socket option to enable broadcasting, which allows sending messages to all devices on a network segment.
  - Sends a predefined message ("Broadcast datagram!") to the specified network and port, targeting all devices that listen on that port.

#### Main Block
```python
if __name__ == '__main__':
    choices = {'client': client, 'server': server}
    parser = argparse.ArgumentParser(description='Send, receive UDP broadcast')
    parser.add_argument('role', choices=choices, help='which role to take')
    parser.add_argument('host', help='interface the server listens at;'
                                     ' network the client sends to')
    parser.add_argument('-p', metavar='port', type=int, default=1060,
                        help='UDP port (default 1060)')
    args = parser.parse_args()
    function = choices[args.role]
    function(args.host, args.p)
```
- **Setup and Execution**:
  - Configures command-line argument parsing to determine whether the script will run as a client or a server.
  - Accepts the host interface or network address and the port number as input arguments.
  - Executes the appropriate function (client or server) based on the user's role choice, passing in the necessary host and port parameters.

This script is particularly useful for network applications where messages need to be broadcast to all network participants, such as service announcements or quick network-wide alerts.
