# log-parser

Single Bash script that summarises an nginx/apache access log: top client IPs, status codes and
classes, top requests, transferred bytes. No dependencies beyond `bash` and `awk`.

## Run

```sh
bash bin/log-parser samples/access.log
bash bin/log-parser -n 3 -o ips,status samples/access.log
cat samples/access.log | bash bin/log-parser -o status
```

Options: `-n TOP` rows per top-list (default 10), `-o SECTIONS` subset of `ips,status,paths,bytes`,
`-h` help. Reads stdin when no file is given; exits 2 on a bad option or an unreadable file.

Lines that carry no `"METHOD path ..."` request or no 1xx-5xx status are counted as `malformed`
and skipped, so a mixed or truncated log still produces a report.

## Sample output

```
lines=20 parsed=19 malformed=1

== Top 3 client IPs ==
       5  10.0.0.42
       4  10.0.0.17
       4  192.168.1.8
```

Tested with Bash 3.2.57 and the system `awk` on macOS; `shellcheck` 0.11.0 clean.
