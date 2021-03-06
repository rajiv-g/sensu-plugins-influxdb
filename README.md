## Sensu-Plugins-influxdb

[![Build Status](https://travis-ci.org/sensu-plugins/sensu-plugins-influxdb.svg?branch=master)](https://travis-ci.org/sensu-plugins/sensu-plugins-influxdb)
[![Gem Version](https://badge.fury.io/rb/sensu-plugins-influxdb.svg)](http://badge.fury.io/rb/sensu-plugins-influxdb)
[![Code Climate](https://codeclimate.com/github/sensu-plugins/sensu-plugins-influxdb/badges/gpa.svg)](https://codeclimate.com/github/sensu-plugins/sensu-plugins-influxdb)
[![Test Coverage](https://codeclimate.com/github/sensu-plugins/sensu-plugins-influxdb/badges/coverage.svg)](https://codeclimate.com/github/sensu-plugins/sensu-plugins-influxdb)
[![Dependency Status](https://gemnasium.com/sensu-plugins/sensu-plugins-influxdb.svg)](https://gemnasium.com/sensu-plugins/sensu-plugins-influxdb)
[![Sensu Bonsai Asset](https://img.shields.io/badge/Bonsai-Download%20Me-brightgreen.svg?colorB=89C967&logo=sensu)](https://bonsai.sensu.io/assets/sensu-plugins/sensu-plugins-influxdb)

## Sensu Asset  
  The Sensu assets packaged from this repository are built against the Sensu ruby runtime environment. When using these assets as part of a Sensu Go resource (check, mutator or handler), make sure you include the corresponding Sensu ruby runtime asset in the list of assets needed by the resource.  The current ruby-runtime assets can be found [here](https://bonsai.sensu.io/assets/sensu/sensu-ruby-runtime) in the [Bonsai Asset Index](bonsai.sensu.io).

## Functionality

## Files
 * bin/check-influxdb.rb
 * bin/check-influxdb-query.rb
 * bin/metrics-influxdb.rb
 * bin/mutator-influxdb-line-protocol.rb 

## Usage - metrics-influxdb.rb
Add the following to `/etc/sensu/conf.d/influx.json` "plugin expects `influxdb` to be a top level node in the json
**metrics-influxdb**
```
{
    "influxdb": {
        "hosts"         : ["influxdb.familyguy.com"],
        "host"          : "influxdb.familyguy.com",
        "port"          : "8086",
        "username"      : "root",
        "password"      : "root",
        "database"      : "stats",
        # OPTIONAL
        "time_precision": "s",
        "use_ssl"       : false,
        "verify_ssl"    : true,
        "ssl_ca_cert"   : "path to the ca certificate file",
        "auth_method"   : "params",
        "initial_delay" : 0.01,
        "max_delay"     : 30,
        "open_timeout"  : 5,
        "read_timeout"  : 300,
        "retry"         : null,
        "prefix"        : "",
        "denormalize"   : true,
        "status"        : true
    }
}
```
Then add the following to your `/etc/sensu/conf.d/handlers.json`:
```
{
  "handlers": {
     "influx-tcp": {
       "type": "pipe",
       "command": "/opt/sensu/embedded/bin/metrics-influxdb.rb"
     }
   }
}
```

Setting "status" to true will store the metric using the status as the value and the check name as the series

## Usage - mutator-influxdb-line-protocol.rb
Add the mutator-influxdb-line-protocol.rb file into /etc/sensu/extensions/

Then add the following to your `/etc/sensu/conf.d/handlers.json`:
```
{
  "handlers": {
    "influxdb_udp": {
      "type": "udp",
      "socket": {
        "host": "localhost",
        "port": 8090
      },
      "mutator": "influxdb_line_protocol"
    }
  }
}
```

## Check configuration
To ship additional tags to your metrics via mutator-influxdb-line-protocol.rb (only available in influxdb >= 0.9), just add a tags block inside your check.

```
{
  "checks": {
    "cpu-metrics": {
      "command": "/opt/sensu/embedded/bin/metrics-cpu-pcnt-usage.rb",
      "handlers": [
        "influxdb_udp"
      ],
      "interval": 60,
      "subscribers": [
        "base"
      ],
      "type": "metric",
      "standalone": false,
      "tags": {
        "group": "operations"
      }
    }
  }
}
```

## Installation

[Installation and Setup](http://sensu-plugins.io/docs/installation_instructions.html)

## Notes
