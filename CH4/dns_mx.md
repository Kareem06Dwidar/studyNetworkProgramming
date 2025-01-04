#### Import Libraries

```python
import argparse, dns.resolver
```
- **`argparse`**: Manages command-line arguments to take a domain name as input.
- **`dns.resolver`**: Facilitates making DNS queries to resolve various DNS record types.

#### Define `resolve_hostname` Function

```python
def resolve_hostname(hostname, indent=''):
    "Print an A or AAAA record for `hostname`; follow CNAMEs if necessary."
    indent += '    '  # Increase indentation for better output readability.
    try:
        answer = dns.resolver.query(hostname, 'A')
        if answer.rrset is not None:
            for record in answer:
                print(indent, hostname, 'has A address', record.address)
            return
    except dns.resolver.NoAnswer:
        pass

    try:
        answer = dns.resolver.query(hostname, 'AAAA')
        if answer.rrset is not None:
            for record in answer:
                print(indent, hostname, 'has AAAA address', record.address)
            return
    except dns.resolver.NoAnswer:
        pass

    try:
        answer = dns.resolver.query(hostname, 'CNAME')
        if answer.rrset is not None:
            record = answer[0]
            cname = record.target.to_text(omit_final_dot=True)
            print(indent, hostname, 'is a CNAME alias for', cname)
            resolve_hostname(cname, indent)  # Recursively resolve CNAME.
            return
    except dns.resolver.NoAnswer:
        print(indent, 'ERROR: no A, AAAA, or CNAME records for', hostname)
```
- This function attempts to resolve `hostname` to its A, AAAA, or CNAME records and prints the results. It handles exceptions if a particular record type is not found (using `dns.resolver.NoAnswer`).

#### Define `resolve_email_domain` Function

```python
def resolve_email_domain(domain):
    "For an email address `name@domain` find its mail server IP addresses."
    try:
        answer = dns.resolver.query(domain, 'MX', raise_on_no_answer=False)
        if answer.rrset is not None:
            records = sorted(answer, key=lambda record: record.preference)
            print('This domain has', len(records), 'MX records')
            for record in records:
                name = record.exchange.to_text(omit_final_dot=True)
                print('Priority', record.preference)
                resolve_hostname(name)  # Resolve each MX record's hostname.
        else:
            print('This domain has no explicit MX records')
            print('Attempting to resolve it as an A, AAAA, or CNAME')
            resolve_hostname(domain)
    except dns.resolver.NXDOMAIN:
        print('Error: No such domain', domain)
```
- This function resolves MX records for the given `domain`. If no MX records are found, it attempts to resolve it as an A, AAAA, or CNAME. It handles the `NXDOMAIN` exception which occurs if the domain does not exist.

### Main Execution Block

```python
if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Find mailserver IP address')
    parser.add_argument('domain', help='domain that you want to send mail to')
    resolve_email_domain(parser.parse_args().domain)
```
- Sets up command-line argument parsing to get a domain from the user.
- Calls `resolve_email_domain` to perform the DNS lookups based on the input domain.

### Summary
This script effectively demonstrates how to handle DNS queries for different record types, particularly focusing on MX records which are crucial for understanding email server configuration. By resolving and following potential CNAME records recursively and handling the absence of direct A or AAAA records, the script provides comprehensive DNS resolution capabilities.
