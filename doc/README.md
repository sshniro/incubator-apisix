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
[Chinese](./zh-cn/README.md)

# Introduction

APISIX (API.6) is a cloud-native microservices API gateway, delivering the ultimate performance, security, open source and scalable 
platform for all your APIs and microservices. APISIX is based on Nginx and etcd. Compared with traditional API gateways, 
APISIX has dynamic routing and plug-in hot loading, which is especially suitable for API management under micro-service system.

The document is divided into 7 main sections

- [Quick Start]() section provides a step by step guide on setting up APISIX with a secured route to access an API.
- [General]()
  - [Architecture] section explains the overall architecture of APISIX
  - [Benchmark] section contains the benchmarking results of APISIX
  - [Installations] section contains installtion details for mutiple platforms
  - [Plugin Development] section explains how to develop your custome plugins and how perform hot reloading
  - [Proxies] section contains how to setup GRPC and straming proxies with APISIX
- [FAQs]() section contains answers to some of the frequently asked question regarding APISIX.
- [Plugins]
  - [Authentication]() plugins to authenticate API calls to API Gateway.
  - [General]() pluings for general usage such as redirections or batch requests.
  - [Security]() plugins to provide additional security such as CORS, Rate limitting etc.
  - [Transformation]() plugins to transform the API requests and response.
  - [Monitoring]() plugins to export metrics and KPIs to platforms like Prometus or Skywalking.
  - [Loggers]() plugins to export access logs to log management platforms such as Kibana or Kafka.
- [Admin API]() section contains reference to the Admin API of APISIX to manage the routes.


Deploy to the Cloud
=======
### AWS

The recommended approach is to deploy APISIX with [AWS CDK](https://aws.amazon.com/cdk/) on [AWS Fargate](https://aws.amazon.com/fargate/) which helps you decouple the APISIX layer and the upstream layer on top of a fully-managed and secure serverless container compute environment with autoscaling capabilities.

See [this guide](https://github.com/pahud/cdk-samples/blob/master/typescript/apisix/README.md) by [Pahud Hsieh](https://github.com/pahud) and learn how to provision the recommended architecture 100% in AWS CDK.
