#### Import Libraries
```python
import argparse, dns.resolver
```
- **`argparse`**: This module is used to write user-friendly command-line interfaces. The script uses `argparse` to parse command-line arguments, specifically to receive a domain name that the user wants to look up.
- **`dns.resolver`**: Part of the `dnspython` library, this module provides functions to perform DNS queries. It is used here to resolve different types of DNS records for the specified domain.

#### Function: lookup
```python
def lookup(name):
    for qtype in 'A', 'AAAA', 'CNAME', 'MX', 'NS':
        answer = dns.resolver.query(name, qtype, raise_on_no_answer=False)
        if answer.rrset is not None:
            print(answer.rrset)
```
- **Purpose**: The function `lookup` is designed to query multiple types of DNS records for a given domain name.
- **Query Types**:
  - **A**: IPv4 address record, which maps a domain to an IP address.
  - **AAAA**: IPv6 address record, similar to the A record but for IPv6 addresses.
  - **CNAME**: Canonical Name record, used to alias one name to another.
  - **MX**: Mail Exchange record, which specifies mail servers responsible for receiving email messages on behalf of a domain.
  - **NS**: Name Server record, which specifies authoritative DNS servers for the domain.
- **Query Execution**: The function iterates over a tuple of DNS record types, performing a DNS query for each type using `dns.resolver.query`. The `raise_on_no_answer=False` parameter ensures that the query does not raise an exception if no answer is found for a particular record type.
- **Output**: If the DNS query returns a record set (`answer.rrset`), it prints the record set. The record set includes the DNS record details such as the domain, record type, TTL (time-to-live), and the value (e.g., IP address for A records).

#### Main Execution Block
```python
if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Resolve a name using DNS')
    parser.add_argument('name', help='name that you want to look up in DNS')
    lookup(parser.parse_args().name)
```
- **Script Configuration**: This block is the entry point for the script. It checks if the script is being run directly (not imported) and sets up the command-line argument parsing.
- **Argument Setup**: The script defines one positional argument `name` which captures the domain name that the user wants to look up.
- **Function Call**: After parsing the arguments, the script calls the `lookup` function with the domain name provided by the user. This initiates the DNS queries for the specified domain.

### Usage
To use this script, a user would run it from the command line, specifying the domain name to look up as an argument. For example:
```bash
python dns_basic.py example.com
```
This command would trigger the script to print the DNS records for `example.com`, covering the record types A, AAAA, CNAME, MX, and NS.

### Summary
This script is a useful tool for querying and displaying various DNS record types for a domain, utilizing the powerful features of the `dnspython` library to handle DNS resolution. It exemplifies a practical application of network programming in Python for tasks related to network information gathering and diagnostics.
