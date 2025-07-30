## Basic Connectivity Tests

### Ping - Test basic connectivity

```bash
ping google.com
ping -c 4 8.8.8.8    # Linux/Mac: Send 4 packets
ping -n 4 8.8.8.8    # Windows: Send 4 packets
```

Tests if a host is reachable and measures round-trip time.

### Traceroute - Trace network path

```bash
traceroute google.com     # Linux/Mac
tracert google.com        # Windows
```

Shows the path packets take to reach a destination, including all intermediate hops.

## DNS Testing

### DNS Lookup

```bash
nslookup google.com
nslookup google.com 8.8.8.8    # Use specific DNS server
```

Queries DNS records for a domain name.

### Dig (Linux/Mac) - Advanced DNS lookup

```bash
dig google.com
dig @8.8.8.8 google.com A      # Query specific DNS server for A records
dig google.com MX              # Query MX records
dig +short google.com          # Brief output
```

More detailed DNS information than nslookup.

## Port and Service Testing

### Telnet - Test port connectivity

```bash
telnet google.com 80
telnet 192.168.1.1 22
```

Tests if a specific port is open and accepting connections.

### Netcat - Network utility

```bash
nc -zv google.com 80           # Test port without connecting
nc -zv 192.168.1.1 20-25      # Scan port range
```

Versatile tool for testing ports, creating connections, and network debugging.

### Nmap - Network scanning

```bash
nmap 192.168.1.1               # Basic host scan
nmap -p 80,443 google.com      # Scan specific ports
nmap -sS 192.168.1.0/24        # SYN scan of subnet
```

Comprehensive network discovery and security auditing tool.

## Web Testing

### cURL - Command-line web requests

```bash
curl https://google.com
curl -I https://google.com           # Headers only
curl -X POST -d "data" https://api.example.com
curl -w "%{http_code}" https://google.com    # Show HTTP status code
```

Flexible tool for making HTTP requests and testing web services.

### PowerShell Web Request

```powershell
Invoke-WebRequest -Uri https://google.com
Invoke-RestMethod -Uri https://api.example.com/data
Test-NetConnection -ComputerName google.com -Port 443
```

Windows PowerShell methods for web requests and connection testing.

## Network Interface Information

### Show network configuration

```bash
# Linux/Mac
ifconfig
ip addr show

# Windows
ipconfig
ipconfig /all
```

Displays network interface configuration and IP addresses.

### Network statistics

```bash
# Linux/Mac
netstat -tuln      # Show listening ports
ss -tuln           # Modern replacement for netstat

# Windows
netstat -an        # Show all connections
netstate -an | findstr "LISTENING" # filter by status
netstat -b         # Show process names
```

Shows network connections, routing tables, and interface statistics.

## Route Testing

### Show routing table

```bash
# Linux/Mac
route -n
ip route show

# Windows
route print
```

Displays the system's routing table.

### Test specific route

```bash
# Linux/Mac
ip route get 8.8.8.8

# Windows
Test-NetConnection -ComputerName 8.8.8.8 -DiagnoseRouting
```

Shows which route will be used to reach a destination.

## Bandwidth and Performance

### Wget - Download and test speed

```bash
wget https://example.com/file.zip
wget --spider https://example.com    # Check if URL exists without downloading
```

Downloads files and can test download speeds.

### Speedtest (if installed)

```bash
speedtest-cli
```

Tests internet connection speed to nearby servers.

## Quick Troubleshooting Workflow

1. **Test basic connectivity**: `ping 8.8.8.8`
2. **Check DNS resolution**: `nslookup domain.com`
3. **Test specific port**: `telnet domain.com 80`
4. **Trace network path**: `traceroute domain.com`
5. **Check local network config**: `ipconfig` or `ifconfig`