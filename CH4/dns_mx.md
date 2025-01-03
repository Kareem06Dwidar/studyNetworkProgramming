This Python script performs DNS lookups specifically to find the mail servers associated with a given domain. It utilizes the `dnspython` library to handle DNS queries. The script resolves MX records for the domain and attempts to find the IP addresses of these mail servers. Let’s break down the script into its core components and functionalities:

### Import Libraries

```python
import argparse, dns.resolver
```
- **`argparse`**: This module is used for parsing command-line arguments. In this script, it captures the domain name for which mail server information is required.
- **`dns.resolver`**: Part of the `dnspython` package, it's used to perform DNS queries.

### Functions

#### `resolve_hostname(hostname, indent='')`
This function is designed to print the IP address associated with a hostname. It attempts to resolve A, AAAA, and CNAME records:

- **A Records**: IPv4 addresses that map a domain name to an IP address.
- **AAAA Records**: IPv6 addresses that serve the same purpose as A records but for IPv6.
- **CNAME Records**: Canonical Name records that map an alias name to the true or canonical domain name.

**Flow**:
1. **A Record Query**: The function first tries to resolve the hostname to an A record. If found, it prints each address and returns.
2. **AAAA Record Query**: If no A record is found, it checks for an AAAA record, printing the addresses if any are found.
3. **CNAME Record Query**: If neither A nor AAAA records are found, it attempts to resolve a CNAME. If a CNAME is present, it recursively calls itself with the canonical name to resolve the final A or AAAA records.
4. **Error Handling**: If no records are found, it prints an error message.

#### `resolve_email_domain(domain)`
This function resolves the MX records for a domain, which are used to route emails. It also handles cases where no MX records are found by falling back to A, AAAA, or CNAME records:

- **MX Record Handling**: First, it queries the MX records. If present, it sorts them by preference (lower values are preferred), then resolves each mail server's hostname to find its IP address.
- **Fallback**: If no MX records are found, it attempts to resolve the domain directly as if it were an A, AAAA, or CNAME record to handle situations where mail is handled by the domain itself.

**Flow**:
1. **MX Query**: Attempt to fetch MX records. Handle `NXDOMAIN` exception, which occurs if the domain does not exist.
2. **Record Processing**: If MX records are found, they are processed and resolved to IP addresses. If not, it calls `resolve_hostname` to try resolving the domain directly.
3. **Error and Fallback Handling**: If no records are resolved, it notifies the user and attempts other means of resolution.

### Main Execution Block

```python
if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Find mailserver IP address')
    parser.add_argument('domain', help='domain that you want to send mail to')
    resolve_email_domain(parser.parse_args().domain)
```
- **Setup**: Configures the command-line argument parser to accept a single positional argument: the domain.
- **Execution**: After parsing the command line, it calls `resolve_email_domain` to perform the MX record lookup and subsequent resolutions.

### Summary
This script provides a practical tool for querying DNS to find the mail servers for a specific domain, demonstrating how to handle different types of DNS records and follow DNS resolution chains. It’s particularly useful for administrators and developers needing to troubleshoot email delivery issues or to verify mail server configurations.
