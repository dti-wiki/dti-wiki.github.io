# Basic Port Forwarding

> [!WARNING]
> Don't use this external to one host, when using plain-text protocols (unencrypted). This is not adviced.

You have a network service "pikachu" that listens on an `IP_Addr:port` and you need a port forwarding to that same service.

- may be on localhost
- may be from a different network

We can use `socat` , `ncat` , `goproxy`  or `go-gost` etc.

## socat

You can use this command listen on port `5050` and forward all to port `2020`.

```
socat tcp-l:5050,fork,reuseaddr tcp:127.0.0.1:2020
```

source: `http://www.dest-unreach.org/socat/`

## ncat

This is for NMap `Ncat` and not `nc` , you can use the below command to listen on port `8080` and the forward traffic to port `80`.

```
ncat -l localhost 8080 --sh-exec "ncat example.org 80"
```

source: `https://nmap.org/ncat/`

## goproxy

You can use below command to listen on port `1234` and forward it to port `4567` on address `10.10.0.1` (a local IP Address in your box).

```
proxy tcp -p ":1234" -T tcp -P "10.10.0.1:4567"
```

source: `https://snail007.host900.com/goproxy/manual/#/` and `https://github.com/snail007/goproxy/releases`

## go-gost

You can use below command to listen on port 1234 and forward it to port 4567 on address `10.10.0.1` (a local IP Address in your box).

```
gost -L tcp://:1234/1.1.1.1:4567
```

source : `https://gost.run/en/getting-started/quick-start/` and `https://github.com/ginuerzh/gost/releases`




