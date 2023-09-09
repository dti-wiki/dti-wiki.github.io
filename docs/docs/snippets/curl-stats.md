# Collect HTTP Stats using cURL

cURL supports formatted output for the details of the request (see the [cURL manpage](https://curl.se/docs/manpage.html) for details, under `-w, –write-out <format>` ).
Lets try to collect the time statistics of HTTP connection. Times below are in seconds.

## Prerequisite

we need a http-delay statistics file.

Lets create a directory and file for this requirement.

```
mkdir -vp ~/projects/dti-wiki/http-delay
```

```
touch ~/projects/dti-wiki/http-delay/curl-stats.txt

```
Lets create the contents for the statistics file.

paste the below content to: `~/projects/dti-wiki/http-delay/curl-stats.txt`

```
     time_namelookup:  %{time_namelookup}s\n
        time_connect:  %{time_connect}s\n
     time_appconnect:  %{time_appconnect}s\n
    time_pretransfer:  %{time_pretransfer}s\n
       time_redirect:  %{time_redirect}s\n
  time_starttransfer:  %{time_starttransfer}s\n
                     ----------\n
          time_total:  %{time_total}s\n
```

## Execution

We can execute the cURL as below to collect the HTTP statistics.

```
curl -w "@${HOME}/projects/dti-wiki/http-delay/curl-stats.txt" -o /dev/null -s "http://wordpress.com/"
```

We can create this cURL command as a bash script.

create a file : `~/bin/httpstats` with below content.

```
#!/bin/bash

curl -w @- -o /dev/null -s "$@" <<'EOF'
    time_namelookup:  %{time_namelookup}\n
       time_connect:  %{time_connect}\n
    time_appconnect:  %{time_appconnect}\n
   time_pretransfer:  %{time_pretransfer}\n
      time_redirect:  %{time_redirect}\n
 time_starttransfer:  %{time_starttransfer}\n
                    ----------\n
         time_total:  %{time_total}\n
EOF
```

Now lets give execute permissions for `~/bin/httpstats` file:

```
chmod +x ~/bin/httpstats
```

Then we can execute the command like

```
httpstats http://wordpress.org
```


# Other useful oneliners

Collect `total time` taken:

```
curl -o /dev/null -s -w 'Total: %{time_total}s\n'  https://www.google.com
```

```
curl -o /dev/null -s -w 'Establish Connection: %{time_connect}s\nTTFB: %{time_starttransfer}s\nTotal: %{time_total}s\n'  https://www.google.com
```
