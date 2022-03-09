# InfluxFlux

A simple Julia client to access InfluxDB based on the Flux query language.

Only supports read access

## Install

```Julia
] add https://github.com/tallakt/InfluxFlux#main
```

## Usage


```Julia
> using Dates
> using InfluxFlux
> api_key = "...."
> srv = influx_server("https://some.influxdb.endpoint.influxdata.com", "some@organization.com", api_token)
> raw = flux(srv, "buckets()") |> String
> table = flux_to_dataframe(srv, """
    from(bucket: "example-bucket")
      |> range(start: -1d)
      |> filter(fn: (r) => r._field == "foo")
      |> group(columns: ["sensorID"])
      |> mean()
  """)
> measurements_dataframe = measurement(srv, "example_bucket", "sensors", now(UTC) - Hour(1), now())
> measurements_dataframe = aggregate_measurement(srv, "example_bucket", "sensors", now(UTC) - Hour(1), now(), Minute(1))
```

