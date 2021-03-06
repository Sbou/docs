---
id: version-oryOS.17-hydra
title: ORY Hydra
original_id: hydra
---

In this document you will find benchmark results for different endpoints of ORY
Hydra. All benchmarks are executed using
[rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks
run against the in-memory storage adapter of ORY Hydra. These benchmarks
represent what performance you would get with a zero-overhead database
implementation.

We do not include benchmarks against databases (e.g. MySQL, PostgreSQL or
CockroachDB) as the performance greatly differs between deployments (e.g.
request latency, database configuration) and tweaking individual things may
greatly improve performance. We believe, for that reason, that benchmark results
for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark
data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All
benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using
flows such as the OAuth 2.0 Client Credentials Grant, ORY Hydra validates the
client credentials using BCrypt which causes (by design) CPU load. CPU load and
performance depend on the BCrypt cost which can be set using the environment
variable `BCRYPT_COST`. For these benchmarks, we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	2.7472 secs
  Slowest:	0.0729 secs
  Fastest:	0.0003 secs
  Average:	0.0266 secs
  Requests/sec:	3640.1013

  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.008 [897]	|■■■■■■■■■■■■■■■■■■■
  0.015 [1374]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.022 [1833]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.029 [1905]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.037 [1562]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.044 [1155]	|■■■■■■■■■■■■■■■■■■■■■■■■
  0.051 [700]	|■■■■■■■■■■■■■■■
  0.058 [399]	|■■■■■■■■
  0.066 [146]	|■■■
  0.073 [28]	|■


Latency distribution:
  10% in 0.0083 secs
  25% in 0.0158 secs
  50% in 0.0255 secs
  75% in 0.0362 secs
  90% in 0.0463 secs
  95% in 0.0519 secs
  99% in 0.0613 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0003 secs, 0.0729 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0026 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0061 secs
  resp wait:	0.0265 secs, 0.0003 secs, 0.0707 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0061 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	19.9146 secs
  Slowest:	0.4157 secs
  Fastest:	0.0174 secs
  Average:	0.1956 secs
  Requests/sec:	502.1430

  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.017 [1]	|
  0.057 [207]	|■■
  0.097 [272]	|■■■
  0.137 [1713]	|■■■■■■■■■■■■■■■■
  0.177 [759]	|■■■■■■■
  0.217 [4165]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.256 [848]	|■■■■■■■■
  0.296 [1552]	|■■■■■■■■■■■■■■■
  0.336 [400]	|■■■■
  0.376 [56]	|■
  0.416 [27]	|


Latency distribution:
  10% in 0.1116 secs
  25% in 0.1623 secs
  50% in 0.1979 secs
  75% in 0.2234 secs
  90% in 0.2839 secs
  95% in 0.2957 secs
  99% in 0.3241 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0174 secs, 0.4157 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0102 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0486 secs
  resp wait:	0.1955 secs, 0.0173 secs, 0.4156 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0014 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading
from which negatively impacts performance. Performance will be better if IDs and
secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.4152 secs
  Slowest:	0.0248 secs
  Fastest:	0.0001 secs
  Average:	0.0038 secs
  Requests/sec:	24085.6346

  Total data:	4820000 bytes
  Size/request:	482 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [4914]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1377]	|■■■■■■■■■■■
  0.007 [1757]	|■■■■■■■■■■■■■■
  0.010 [1086]	|■■■■■■■■■
  0.012 [584]	|■■■■■
  0.015 [192]	|■■
  0.017 [70]	|■
  0.020 [15]	|
  0.022 [2]	|
  0.025 [2]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0028 secs
  75% in 0.0065 secs
  90% in 0.0095 secs
  95% in 0.0113 secs
  99% in 0.0146 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0248 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0075 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0095 secs
  resp wait:	0.0034 secs, 0.0001 secs, 0.0228 secs
  resp read:	0.0002 secs, 0.0000 secs, 0.0098 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.3832 secs
  Slowest:	0.0309 secs
  Fastest:	0.0001 secs
  Average:	0.0036 secs
  Requests/sec:	26097.8584

  Total data:	4800000 bytes
  Size/request:	480 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [5677]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.006 [2081]	|■■■■■■■■■■■■■■■
  0.009 [1232]	|■■■■■■■■■
  0.012 [582]	|■■■■
  0.015 [248]	|■■
  0.019 [50]	|
  0.022 [50]	|
  0.025 [49]	|
  0.028 [21]	|
  0.031 [9]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0015 secs
  75% in 0.0058 secs
  90% in 0.0093 secs
  95% in 0.0119 secs
  99% in 0.0203 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0309 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0112 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0187 secs
  resp wait:	0.0030 secs, 0.0000 secs, 0.0272 secs
  resp read:	0.0003 secs, 0.0000 secs, 0.0188 secs

Status code distribution:
  [200]	10000 responses



```
