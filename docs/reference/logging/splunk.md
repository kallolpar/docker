<!--[metadata]>
+++
title = "Splunk logging driver"
description = "Describes how to use the Splunk logging driver."
keywords = ["splunk, docker, logging, driver"]
[menu.main]
parent = "smn_logging"
weight = 2
+++
<![end-metadata]-->

# Splunk logging driver

The `splunk` logging driver sends container logs to
[HTTP Event Collector](http://dev.splunk.com/view/event-collector/SP-CAAAE6M)
in Splunk Enterprise and Splunk Cloud.

## Usage

You can configure the default logging driver by passing the `--log-driver`
option to the Docker daemon:

    docker --log-driver=splunk

You can set the logging driver for a specific container by using the
`--log-driver` option to `docker run`:

    docker run --log-driver=splunk ...

## Splunk options

You can use the `--log-opt NAME=VALUE` flag to specify these additional Splunk
logging driver options:

  - `splunk-token` required, Splunk HTTP Event Collector token
  - `splunk-url` required, path to your Splunk Enterprise or Splunk Cloud instance
      (including port and schema used by HTTP Event Collector) `https://your_splunk_instance:8088`
  - `splunk-source` optional, event source
  - `splunk-sourcetype` optional, event source type
  - `splunk-index` optional, event index
  - `splunk-capath` optional, path to root certificate
  - `splunk-caname` optional, name to use for validating server
      certificate; by default the hostname of the `splunk-url` will be used
  - `splunk-insecureskipverify` optional, ignore server certificate validation

Below is an example of the logging option specified for the Splunk Enterprise
instance. The instance is installed locally on the same machine on which the
Docker daemon is running. The path to the root certificate and Common Name is
specified using an HTTPS schema. This is used for verification.
The `SplunkServerDefaultCert` is automatically generated by Splunk certificates.

    docker run --log-driver=splunk \
        --log-opt splunk-token=176FCEBF-4CF5-4EDF-91BC-703796522D20 \
        --log-opt splunk-url=https://localhost:8088 \
        --log-opt splunk-capath=/opt/splunk/etc/auth/cacert.pem \
        --log-opt splunk-caname=SplunkServerDefaultCert