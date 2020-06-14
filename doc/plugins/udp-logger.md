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

`udp-logger` is a plugin which push Log data requests to UDP servers.

This will provide the ability to send Log data requests as JSON objects to Monitoring tools and other UDP servers.

This plugin provides the ability to push Log data as a batch to you're external UDP servers.

The plugin uses [Batch-Processor](../batch-processor.md) to aggregate the logs and exports them as batches. Hence, the logs will be exported
when it reaches the `inactive_timeout` or `buffer_duration` or `batch_max_size`.  By default the logs will be exported
in 60 seconds interval.

Optional:

For optimal usage set the `inactive_timeout` smaller than `buffer_duration`.
Set the `batch_max_size` to `1` if the logs do not need to be aggregated (Will be exported immediately without buffering).

## Attributes

|Name           |Requirement    |Description|
|---------      |--------       |-----------|
|host           |required       | IP address or the Hostname of the UDP server.|
|port           |required       | Target upstream port.|
|timeout        |optional       |Timeout for the upstream to send data.|
|name           |optional       |A unique identifier to identity the batch processor|
|batch_max_size |optional       |Max size of each batch, default is 1000|
|inactive_timeout|optional      |Maximum age in seconds when the buffer will be flushed if inactive, default is 30s|
|buffer_duration|optional       |Maximum age in seconds of the oldest entry in a batch before the batch must be processed, default is 60s|

## How To Enable

The following is an example on how to enable the udp-logger for a specific route.

```shell
curl http://127.0.0.1:9080/apisix/admin/routes/5 -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -X PUT -d '
{
      "plugins": {
            "udp-logger": {
                 "host": "127.0.0.1",
                 "port": 3000,
                 "batch_max_size": 1,
                 "name": "udp logger"
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

Remove the corresponding json configuration in the plugin configuration to disable the `udp-logger`.
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

# Example Format

The following shows an exported log format for a POST request.

```json
{
  "upstream": "35.153.61.143:443",
  "start_time": 1592089631177,
  "client_ip": "127.0.0.1",
  "service_id": "",
  "route_id": "5",
  "request": {
    "querystring": {},
    "size": 220,
    "uri": "\\/get",
    "url": "http:\\/\\/127.0.0.2:9080\\/get",
    "headers": {
      "content-type": "application\\/x-www-form-urlencoded",
      "apikey": "auth-one"
    },
    "method": "POST",
    "body": "data=testdata"
  },
  "response": {
    "headers": {
      "access-control-allow-origin": "*"
    },
    "status": 405,
    "size": 461
  },
  "latency": 511.99984550476,
  "consumer": {
    "id": "jack",
    "username": "jack"
  }
}
```

