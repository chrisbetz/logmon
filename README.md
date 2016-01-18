# logmon

A complete logging/monitoring setup based upon Riemann, an Elasticsearch Cluster, ElasticHQ, Kibana and/or Grafana.

# Aim of this project

This project brings together the different parts necessary to build a complete logging/monitoring pipeline. For me, that's everything from gathering information (i.e. a Riemann Appender for those still bound to log4j), to processing it (using Riemann), storing it (using an Elasticsearch cluster), and making it available (using Kibana and/or Grafana.)

It surely isn't as complete as commercial-grade projects, but it's a good starting point besides all those tutorials you need to wire together yourself. And with your help, it'll even get better :)

# Overview

This projects is the common brace around a set of projects, which are available stand alone. Feel free to take only parts if you need to.

  * The **log4-riemann-appender** is an appender you can use in your project to write stuff directly to Riemann.
  * **riemann.osv** is a project setting up Riemann to be run as an OSv unikernel appliance.
  * **riemann-elastic** is used inside **riemann.osv** to write from Riemann directly into Elasticsearch
  * **elasticsearch.osv** bundles the latest (as of time of writing this) version of Elasticsearch (ES)
    into an OSv unikernel appliance, together with the required configuration to kick it of as a set of nodes to form a cluster. With this, you can run a complete cluster of elasticsearch on your dev notebook inside VirtualBox, as the unikernel appliances do add very little overhead.
  * **elasticHQ.osv** is a unikernelized version of ElasticHQ, a management UI for Elasticsearch, ready to be connected to your ES cluster.

This project does not (yet) include unikernelized versions of Kibana and Grafana, which both bring serverside components in their latest versions. Any support on this would really be appreciated.

# Infrastructure / Runtime

The projects contain everything necessary to be run inside VirtualBox. It will startup a VM for Riemann (naming it riemann), an ES cluster consisting of three machines (es-master-1, es-master-2, and es-master-3), as well as an ElasticHQ instance. All of these machines are connected by an 'internal network' named 'virtualcloud'. es-master-1 exposes Port 9200 over NAT to the host, the riemann machine will expose its ports also (5555-5558). Also, each machine gives access to the OSv API port (8000), mapped to different host ports, so you can access these. That's not production ready, obviously.

So you write logs and metrics to the exposed Riemann ports (which should basically be done adding the log4-riemann-appender and the apropriate config), and point your Grafana / Kibana (whatever you like better) to your ES cluster.

# Setup

## Riemann

## Elasticsearch cluster

## ElasticHQ

## Your application / log4j, metrics


# Test

# Deploy


# What's missing

Currently, riemann-elastic uses the HTTP-REST API exposed by Elasticsearch (esp. since Elastisch currently does not support native API to ES 2.x). This makes it way harder than necessary to setup API round robin access for failover. So, currently, only one ES node is configured to be written to from Riemann, and if that host goes down, your logging goes down completely (at least for the part going over this Riemann instance). I'll need to bring native-2.0-support to Elastisch, incorporate it into riemann-elastic, and alter configuration accordingly, but that will take some more time.
