# Network Process Debugging

## Overview

The `/proc/[pid]/net/` directory in Linux provides various network-related information about a specific process with the given Process ID (PID). The files in this directory are helpful for debugging, monitoring, and analyzing the network behavior of a process.

## Structure of `/PROC/[PID]/NET/` Directory

The `/proc/[pid]/net/` directory contains several files that provide different types of network-related information for the process with PID `[pid]`. Some of the common files you may encounter in this directory include:

1. `tcp`: Displays TCP socket connections related to the process.
2. `udp`: Displays UDP socket connections related to the process.
3. `tcp6`: Displays IPv6 TCP socket connections related to the process.
4. `udp6`: Displays IPv6 UDP socket connections related to the process.
5. `unix`: Displays Unix domain socket connections related to the process.
6. `raw`: Displays raw socket information.
7. `sctp`: Displays SCTP (Stream Control Transmission Protocol) socket connections related to the process.

## How to Use `/proc/[pid]/net/` Directory

Here’s how you can work with the files in `/proc/[pid]/net/` directory:

### Accessing Network Connections (e.g., TCP/UDP)

To inspect the TCP and UDP connections associated with a specific process, use the `cat` command to read the corresponding files.

Example:

```
cat /proc/[pid]/net/tcp
cat /proc/[pid]/net/udp
```

Replace `[pid]` with the actual process ID. The output will display the network connections in a format that includes local and remote IP addresses, ports, connection states, and other information.

### Using `ss` Command with a Specific PID

The `ss` (socket stat) command can be used to gather socket-related information, including network connections, for a specific PID.

For example:

```
ss -p -n -a | grep [pid]
```

This will list Unix domain socket connections, which are used for inter-process communication (IPC) on the same machine.

### Understanding the File format

The files in `/proc/[pid]/net/` are often in a human-readable format but may require some interpretation. For example, the `/proc/[pid]/net/tcp` file typically displays the following columns:

- _sl_: Socket reference (sequence number).
- _local_address_: Local IP address and port.
- _rem_address_: Remote IP address and port.
- _st_: Connection state (e.g., `ESTABLISHED`, `LISTEN`, `TIME_WAIT`).
- _tx_queue_: Transmission queue.
- _rx_queue_: Receive queue.
- _tr_: Transition state.
- _uid_: User ID of the process owning the socket.

### Monitoring Process-Specific Network Traffic

If you want to monitor network traffic by a specific process in real-time, tools like `netstat`, `ss`, or `lsof` can help. For example:

```
netstat -tulnp | grep [pid]
lsof -i -p [pid]
```

This will output the list of all TCP connections associated with process `12345`. You may need to interpret the values based on the format of the data in the `tcp` file.

## Useful Notes

- You need appropriate permissions to access the `/proc/[pid]/net/` directory, especially for processes you do not own.
- The files in this directory are snapshots of the state of network sockets, so they represent real-time information but can change frequently.

By using these files and commands, you can get detailed insights into the networking activities of a particular process.

## Finding ports

To find the ports being used by a specific process through the `/proc/[pid]` directory, you'll need to look at the network-related files inside `/proc/[pid]/net/`. Specifically, files like `tcp`, `udp`, `tcp6`, and `udp6` will provide information on the ports in use by the process, both for TCP/UDP IPv4 and IPv6 connections.

Here are the steps to identify the ports used by a process from the `/proc/[pid]` directory:

###  List TCP/UDP connections

The most direct method to find the ports used by a process is to check the `/proc/[pid]/net/tcp` and `/proc/[pid]/net/udp` files, which list the TCP and UDP connections, respectively.

```
cat /proc/[pid]/net/tcp
cat /proc/[pid]/net/udp
```

Replace `[pid]` with the process ID of interest.

#### Output format

- _local_address_: Local address (IP address and port) of the connection.
- _rem_address_: Remote address (IP address and port) of the connection.
- _st_: The state of the connection (for TCP: `ESTABLISHED`, `LISTEN`, `TIME_WAIT`, etc.).
- _uid_: The user ID of the process associated with the connection.

_Important_: The addresses in the `local_address` and `rem_address` columns are typically displayed in hexadecimal. You will need to convert the hex values to human-readable IP addresses and ports.

### Interpreting the `tcp` and `udp` Files

Here’s an example of how the lines in `/proc/[pid]/net/tcp` or `/proc/[pid]/net/udp` might look:

```
sl  local_address           rem_address           st tx_queue rx_queue tr tm->when retrnsmt   uid
1:  0100007F:0050           00000000:0000          01 00000000 00000000 00 00000000 00000000  1000
2:  0100007F:0051           0100007F:0052          01 00000000 00000000 00 00000000 00000000  1000
```

In this example:

- _local_address_ (e.g., `0100007F:0050`) refers to the local address (in hex), and `:0050` represents port `80` (`0x50` in hexadecimal).
- _rem_address_ (e.g., `00000000:0000`) is typically the remote address, and `:0000` often means no established connection for UDP.
- The st column shows the socket state (`01` for `ESTABLISHED` for TCP).

#### Converting the Local Address:

For the `local_address` field, the first part is the IP address in hexadecimal, and the second part is the port in hexadecimal. For example:

- `0100007F:0050` → `0100007F` = `127.0.0.1` (local IP address), and `0050` = `80` (port number).

You can use the following to convert the hex IP to standard dotted decimal notation:

1. Reverse the byte order (for little-endian representation).
2. Convert each byte to decimal.

For example:

- `0100007F` → `7F 00 00 01` → `127.0.0.1`

The port is hexadecimal, so you can convert it similarly:

- `0050` → `80` (decimal).


## Other Useful References

Using lsof

```
lsof -i -p [pid]
```

Using SS

```
ss -tulnp | grep [pid]
```


