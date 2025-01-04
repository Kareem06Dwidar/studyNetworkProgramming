The Python script `big_sender.py` is designed to probe the Maximum Transmission Unit (MTU) of the network path to a specified host by attempting to send a large UDP datagram. It utilizes specific socket options available on Linux systems to discover the MTU. This script is an excellent tool for network administrators or developers who need to determine the MTU size, which can affect the performance and efficiency of data transmission over the network. Let's dive into a detailed explanation of each part of the script:

### Import Libraries

```python
import argparse, socket, sys
```
- **`argparse`**: This module helps to write user-friendly command-line interfaces. The script uses it to gather input from the user about the target host and port.
- **`socket`**: Facilitates low-level networking interfaces, used here to create UDP sockets and manage socket options related to network paths.
- **`sys`**: Used for accessing system-specific parameters and functions, including exiting the script for unsupported platforms.

### Constants Class: `IN`

```python
class IN:
    IP_MTU = 14
    IP_MTU_DISCOVER = 10
    IP_PMTUDISC_DO = 2
```
- These constants are defined to replace the ones previously available in the `IN` module, which Python 3.6 no longer includes. They are used with `setsockopt` to configure the socket for MTU discovery:
  - **`IP_MTU`**: Socket option used to retrieve the current MTU size.
  - **`IP_MTU_DISCOVER`**: Enables Path MTU Discovery on the socket.
  - **`IP_PMTUDISC_DO`**: Specifies that Path MTU Discovery should be performed.

### Platform Check

```python
if sys.platform != 'linux':
    print('Unsupported: Can only perform MTU discovery on Linux', file=sys.stderr)
    sys.exit(1)
```
- This block checks if the script is running on a Linux system. MTU discovery using these socket options is specific to Linux, making the script non-portable.

### Function: `send_big_datagram`

```python
def send_big_datagram(host, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    sock.setsockopt(socket.IPPROTO_IP, IN.IP_MTU_DISCOVER, IN.IP_PMTUDISC_DO)
    sock.connect((host, port))
    try:
        sock.send(b'#' * 999999)
    except socket.error:
        print('Alas, the datagram did not make it')
        max_mtu = sock.getsockopt(socket.IPPROTO_IP, IN.IP_MTU)
        print('Actual MTU: {}'.format(max_mtu))
    else:
        print('The big datagram was sent!')
```
- **Socket Configuration**: A UDP socket is created and configured to perform MTU discovery.
- **Sending Data**: Attempts to send a very large datagram (composed of 999999 hash symbols). If the datagram is too large for the network path, it triggers a `socket.error`.
- **Error Handling**: If an error occurs (likely due to the datagram exceeding the MTU), the script catches the exception, retrieves the actual MTU using `getsockopt`, and prints it.
- **Success Message**: If no exception is raised, it indicates that the datagram was sent successfully, which is unlikely but indicates no MTU issues up to that size.

### Main Execution Block

```python
if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Send UDP packet to get MTU')
    parser.add_argument('host', help='the host to which to target the packet')
    parser.add_argument('-p', metavar='PORT', type=int, default=1060,
                        help='UDP port (default 1060)')
    args = parser.parse_args()
    send_big_datagram(args.host, args.p)
```
- **Setup**: Configures and parses command-line arguments to define the target host and port.
- **Operation**: Calls `send_big_datagram` with the provided host and port from the command line.

### Summary
This script is practical for testing network configurations and understanding limitations in data packet sizes on specific network paths. Its direct approach to probing the MTU can help in optimizing network settings and troubleshooting issues related to packet fragmentation in networks.
