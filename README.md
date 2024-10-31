# otel-collector-debug
Simple boilerplate for debugging of otel collector and otlp instrumentation

## Requisite
* Docker desktop
* An otel instrumented application (you can get pre configured application from [opentelemetry.io](https://opentelemetry.io))

## Setup
in th file [otel-collector.yaml](./otel-collector/otel-collector.yaml) you can configure the verbosity of the debug logs
```yaml
exporters:
  debug:
    verbosity: detailed # none, basic, detailed
```

and what to log to the console by commenting out piplines:
```yaml
service:
  pipelines:
    # no traces will be logged in the debug logger
    # traces:
    #  receivers: [otlp]
    #  processors: [batch]
    #  exporters: [debug, spanmetrics]

    metrics:
      receivers: [otlp, spanmetrics]
      processors: [batch]
      exporters: [debug] 

    logs:
      receivers: [otlp]
      processors: [batch, resource/loki, attributes/loki]
      exporters: [debug]
```

Configure your app to send the otlp signals to the collector on localhost with port:
- 4317 for `grpc`
- 4318 for `http`
  
  ***grpc is preferred***

## Get started
run `docker compose up` or in detached mode `docker compose up -d`

all signals (trace metrics and logs) will be logged in th console of the container

