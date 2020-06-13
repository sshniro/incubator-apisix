<!--
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
-->

# Summary
- [**Name**](#name)
- [**Attributes**](#attributes)
- [**How To Enable**](#how-to-enable)
- [**Test Plugin**](#test-plugin)
- [**Disable Plugin**](#disable-plugin)


## Name

`sys` is a plugin which push Log data requests to Syslog.

This will provide the ability to send Log data requests as JSON objects.

The plugin uses [Batch-Processor](../batch-processor.md) to aggregate the logs and exports them as batches. Hence, the logs will be exported 
when it reaches the `inactive_timeout` or `buffer_duration` or `batch_max_size`.  By default the logs will be exported
in 60 seconds interval.

Optional: 

For optimal usage set the `inactive_timeout` smaller than `buffer_duration`. 
Set the `batch_max_size` to `1` if the logs do not need to be aggregated.

## Attributes

|Name           |Requirement    |Description|
|---------      |--------       |-----------|
|host           |required       | IP address or the Hostname.|
|port           |required       | Target upstream port.|
|timeout        |optional       |Timeout for the upstream to send data.|
|tls            |optional       |Boolean value to control whether to perform SSL verification|
|flush_limit    |optional       |If the buffered messages' size plus the current message size reaches (>=) this limit (in bytes), the buffered log messages will be written to log server. Default to 4096 (4KB).|
|drop_limit           |optional       |If the buffered messages' size plus the current message size is larger than this limit (in bytes), the current log message will be dropped because of limited buffer size. Default drop_limit is 1048576 (1MB).|
|sock_type|optional      |IP protocol type to use for transport layer. Can be either "tcp" or "udp". Default is "tcp".|
|max_retry_times|optional       |Max number of retry times after a connect to a log server failed or send log messages to a log server failed.|
|retry_interval|optional       |The time delay (in ms) before retry to connect to a log server or retry to send log messages to a log server, default to 100 (0.1s).|
|pool_size    |optional       |Keepalive pool size used by sock:keepalive. Default to 10.|
|batch_max_size |optional       |Max size of each batch, default is 1000|
|inactive_timeout|optional      |maximum age in seconds when the buffer will be flushed if inactive, default is 30s|
|buffer_duration|optional       |Maximum age in seconds of the oldest entry in a batch before the batch must be processed, default is 60|

## How To Enable

The following is an example on how to enable the sys-logger for a specific route.

```shell
curl http://127.0.0.1:9080/apisix/admin/routes/5 -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
      "plugins": {
            "syslog": {
                 "host" : "127.0.0.1",
                 "port" : 5044,
                 "flush_limit" : 1
            }
       },
      "upstream": {
           "type": "roundrobin",
           "nodes": {
               "127.0.0.1:1980": 1
           }
      },
      "uri": "/hello"
}'
```

## Test Plugin

* success:

```shell
$ curl -i http://127.0.0.1:9080/hello
HTTP/1.1 200 OK
...
hello, world
```

## Disable Plugin

Remove the corresponding json configuration in the plugin configuration to disable the `sys-logger`.
APISIX plugins are hot-reloaded, therefore no need to restart APISIX.

```shell
curl http://127.0.0.1:9080/apisix/admin/routes/5  -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
    "uri": "/hello",
    "plugins": {},
    "upstream": {
        "type": "roundrobin",
        "nodes": {
            "127.0.0.1:1980": 1
        }
    }
}'
```
