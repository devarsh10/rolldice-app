# Fetching traces using Jaeger via OpenTelemetry Collector

Note: I am using WSL2 on my windows 11 Laptop. I have tried running it on 'git bash' through my windows machine but I was getting errors which was affecting my productivity.

Before following all the steps it's recommended to give the name to the service.
`export OTEL_SERVICE_NAME=rolldice-app`. Otherwise, it will show the path where jaeger resides.


1. Running jaeger beforehand because opentelemetry depends on it.
Jaeger command:
```
docker run -d --name jaeger \
  --env COLLECTOR_ZIPKIN_HOST_PORT=:9411 \
  --env COLLECTOR_OTLP_ENABLED=true \
  --publish 16686:16686 \
  --network otel-jaeger-network \
  --network-alias jaeger \
  jaegertracing/all-in-one:latest

```

2. Running Open Telemetry using the command mentioned it its documentation itself. I just have added network and name parameter from my side

```
docker run --name otel-collector \
-v $(pwd)/collector-config.yaml:/etc/otelcol/config.yaml \
-p 4317:4317 -p 4318:4318 --rm \
--network otel-jaeger-network \
otel/opentelemetry-collector
```
3. Finally, run the node command to get the traces to jaeger 

```
node --require ./instrumentation.js app.js
```

Final results,
1. Jaeger UI
![Screenshot 2025-03-07 111412](https://github.com/user-attachments/assets/68d49dac-c214-44d8-96b5-b3268b63c7ad)


2. Open Telemetery collector logs
![Screenshot 2025-03-07 111540](https://github.com/user-attachments/assets/58080473-bddb-40fe-934a-245325b1ff84)


Things yet to be done
- I have understood the architecture and flow of instrumenting the application and sending traces, metrics and logs to any backend service. Backend service such as: Prometheus, Tempo, Jaeger.
- So, I will be using Prometheus and Grafana Tempo. 
