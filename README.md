# knx-gateway-nse
Nmap NSE scripts to discover KNX home automation gateways via multicast and unicast methods.

Further information:
* DIN EN 13321-2
* http://www.knx.org/

## knx-gateway-info

This script establishes a unicast connection to a specific device in order to retrieve information. This can be used to e.g. retrieve gateways information over the Internet.

## Usage

```
# nmap -sU -p3671 --script ./knx-gateway-info.nse 192.168.178.11
```

**Note**: Increase verbosity/debug to see full message contents:

```
# nmap -sU -p3671 -d --script ./knx-gateway-info.nse 192.168.178.11
```

## Sample Output

```
# nmap -sU -p3671 --script ./knx-gateway-info.nse 192.168.178.11

Starting Nmap 6.49SVN ( https://nmap.org ) at 2015-08-12 20:21 CEST
Nmap scan report for 192.168.178.11
Host is up (0.00042s latency).
PORT     STATE         SERVICE
3671/udp open|filtered efcp
| knx-gateway-info:
|   Body:
|     DIB_DEV_INFO:
|       KNX address: 15.15.255
|       Decive serial: 00ef2650065c
|       Multicast address: 0.0.0.0
|       Device friendly name: IP-Viewer
|     DIB_SUPP_SVC_FAMILIES:
|       KNXnet/IP Core version 1
|       KNXnet/IP Device Management version 1
|       KNXnet/IP Tunnelling version 1
|_      KNXnet/IP Object Server version 1
```

## knx-gateway-discover

This script uses a multicast packet to discover all local gateways. According to the KNX specification every device must support this. This script can only be used to discover local KNX gateways.

### Usage

```
# nmap -e eth0 --script ./knx-gateway-discover.nse
```

**Note**: Increase verbosity/debug to see full message contents:

```
# nmap -e eth0 -v -d --script ./knx-gateway-discover.nse
```

The script supports the following `script-args`:
* timeout: Defines how long the script waits for responses
* newtargets: Add found gateways to target list

### Sample Output

#### Default

```
# nmap -e eth0 --script ./knx-gateway-discover.nse

Starting Nmap 6.49SVN ( https://nmap.org ) at 2015-08-12 20:19 CEST
Pre-scan script results:
| knx-gateway-discover:
|   192.168.178.11:
|     Body:
|       HPAI:
|         Port: 3671
|       DIB_DEV_INFO:
|         KNX address: 15.15.255
|         Decive serial: 00ef2650065c
|         Multicast address: 0.0.0.0
|         Device MAC address: 00:05:26:50:06:5c
|         Device friendly name: IP-Viewer
|       DIB_SUPP_SVC_FAMILIES:
|         KNXnet/IP Core version 1
|         KNXnet/IP Device Management version 1
|         KNXnet/IP Tunnelling version 1
|_        KNXnet/IP Object Server version 1
```

#### Debug

```
# nmap -d -e eth0 --script ./knx-gateway-discover.nse

Starting Nmap 6.49SVN ( https://nmap.org ) at 2015-08-12 20:20 CEST
PORTS: Using top 1000 ports found open (TCP:1000, UDP:0, SCTP:0)
--------------- Timing report ---------------
  hostgroups: min 1, max 100000
  rtt-timeouts: init 1000, min 100, max 10000
  max-scan-delay: TCP 1000, UDP 1000, SCTP 1000
  parallelism: min 0, max 0
  max-retries: 10, host-timeout: 0
  min-rate: 0, max-rate: 0
---------------------------------------------
NSE: Using Lua 5.2.
NSE: Arguments from CLI:
NSE: Loaded 1 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 1) scan.
Initiating NSE at 20:20
NSE: Starting knx-gateway-discover.
NSE: Finished knx-gateway-discover.
NSE: Finished knx-gateway-discover.
NSE: Finished knx-gateway-discover.
Completed NSE at 20:20, 3.08s elapsed
Pre-scan script results:
| knx-gateway-discover:
|   192.168.178.11:
|     Header:
|       Header length: 6
|       Protocol version: 16
|       Service type: SEARCH_RESPONSE (0x0202)
|       Total length: 78
|     Body:
|       HPAI:
|         Protocol code: 01
|         IP address: 192.168.178.11
|         Port: 3671
|       DIB_DEV_INFO:
|         Description type: Device Information
|         KNX medium: KNX TP1
|         Device status: 00
|         KNX address: 15.15.255
|         Project installation identifier: 0000
|         Decive serial: 00ef2650065c
|         Multicast address: 0.0.0.0
|         Device MAC address: 00:05:26:50:06:5c
|         Device friendly name: IP-Viewer
|       DIB_SUPP_SVC_FAMILIES:
|         KNXnet/IP Core version 1
|         KNXnet/IP Device Management version 1
|         KNXnet/IP Tunnelling version 1
|_        KNXnet/IP Object Server version 1
```
