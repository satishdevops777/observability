# observability
 - Observability is the capability of a system to explain its internal state using telemetry data like logs, metrics, and traces.

👉 If your system breaks at 3 AM, observability tells you what broke, where, why, and what to do next.

## Why Observability Exists
### Traditional monitoring answers:
- ❓ Is the system up?
- ❓ Is CPU high?
### Observability answers:
- ✅ Which service is slow?
- ✅ Which user request failed?
- ✅ What change caused the outage?
- ✅ Is this infra, app, or dependency related?

## The Three Pillars of Observability 🧱
### 1️⃣ Metrics (What is happening?)
- Numeric values over time
- Fast, cheap, good for alerts
### Examples
- CPU usage
- Memory usage
- Request rate (RPS)
- Error rate
- Latency (P95, P99)
### Tools
- Prometheus
- Grafana
- CloudWatch
- SignalFx

### 2️⃣ Logs (Why did it happen?)

<img width="868" height="511" alt="image" src="https://github.com/user-attachments/assets/d599ac15-f8c3-4b6e-98c5-a83a794c8c7e" />

- Detailed, contextual records
- Best for debugging
```ERROR payment-service timeout while calling bank-api```
### Tools
- Splunk
- ELK / OpenSearch
- CloudWatch Logs

### 3️⃣ Traces (Where did it happen?)
- Follow a request across services
- Shows latency & bottlenecks
- User → API Gateway → Auth → Order → Payment → DB

### Tools
- Jaeger
- Zipkin
- OpenTelemetry
- X-Ray

```“Observability allows us to understand system behaviour and diagnose unknown failures using metrics, logs, and traces, reducing MTTR and improving reliability.”```

## Splunk and SignalFx
- Splunk = logs (deep details, forensics)
- Signal = metrics + monitoring (real-time health, alerts)
### Together, they answer:
- Is something wrong? → SignalFx
- Where is it wrong? → SignalFx
- Why did it happen? → Splunk
 <img width="673" height="217" alt="image" src="https://github.com/user-attachments/assets/3deaa414-f7a8-4c21-a444-c53312b02fc4" />


### Components You’re Using
### Splunk
- Centralized log storage & search
- Root cause analysis
- Incident forensics
### SignalFx
- Metrics, dashboards, alerts
- Real-time monitoring
- APM & infrastructure visibility (Now part of Splunk Observability Cloud)

## 1️⃣ Monitoring with SignalFx (Metrics-Driven)
### What you monitor in SignalFx
### 🔹 Infrastructure Metrics
- CPU, memory, disk, network
- Node health
- Load average

### 🔹 Kubernetes Metrics
- Pod restarts
- Pod CPU/memory throttling
- Node pressure
- Deployment replica health

### Application Metrics (Golden Signals ⭐)
- Latency (P95 / P99)
- Traffic (RPS)
- Errors (4xx / 5xx)
- Saturation

### How data gets into SignalFx
- SignalFx Smart Agent / OpenTelemetry
- K8s metrics-server
- Cloud integrations (AWS, Azure, GCP)
- App instrumentation (Java, Node, Python, etc.)

### Example Alert in SignalFx 🚨
 <img width="542" height="166" alt="image" src="https://github.com/user-attachments/assets/da39c50a-574e-44cd-9bdf-0b1b5f7f1c88" />

👉 This tells you something is wrong
- But not yet why.
- 
## 2️⃣ Logging & Deep Debugging with Splunk
### What goes into Splunk
- Application logs
- Kubernetes pod logs
- Node/system logs
- API gateway logs
- Error & exception traces


### Example Logs in Splunk 🔍
<img width="455" height="128" alt="image" src="https://github.com/user-attachments/assets/d0d17cb3-fcb8-4c8b-9ad4-b04e18303978" />

### Now you know:
- Which service
- Which dependency
- Exact failure reason

## 3️⃣ How Monitoring + Observability Work Together
### 🚨 Incident Flow (Real World)
- Step 1: Alert fires in SignalFx
- High latency in payment-service

### Step 2: Open SignalFx dashboard
You see:
- CPU normal ❌
- Memory normal ❌
- Latency high ✅
- Error rate rising ✅
👉 Problem is application or dependency, not infra

### Step 3: Pivot to Splunk (Logs)
- service=payment-service environment=prod ERROR
- DB connection pool exhausted

### Step 4: Root Cause
- Traffic spike
- DB max connections reached
- App retries increased latency

### Step 5: Fix
- Scale DB
- Increase pool
- Add rate limit / caching
- MTTR drops drastically

## 4️⃣ Correlation (This Is Observability 🔥)

| SignalFx     | Splunk          |
| ------------ | --------------- |
| Alert        | Error logs      |
| Metric spike | Stack trace     |
| Dashboard    | Request context |
| SLO breach   | Root cause      |

💡 Same service name, host, pod, request ID

## 5️⃣ SLOs with SignalFx + Splunk
### SignalFx
- Tracks SLI metrics (latency, error rate)
- Calculates SLO compliance
- Alerts on burn rate

### Splunk
- Explains why SLO was violated
- Provides evidence for RCA

## 6️⃣ How You’d Explain This in an Interview 🎤
- “We use SignalFx for real-time monitoring and alerting based on metrics like latency, error rate, and saturation. When an alert fires, we correlate it with logs stored in Splunk to perform root cause analysis. This combination allows us to detect issues early, reduce MTTR, and handle both known and unknown failure scenarios effectively.”


## 7️⃣ Summary (One Screen 🧠)

### ✅ SignalFx
- Monitoring
- Dashboards
- Alerts
- SLOs

### ✅ Splunk
- Logs
- Debugging
- RCA
- Audit & forensics

### ✅ Togethe = True Observability

## 1️⃣ Latency Percentiles: P95 & P99 (the “slow users” story)
- Latency = time taken to serve a request
- Example: /checkout API took 320 ms

### What does P95 mean?
- P95 latency = 95% of requests are faster than this value
- The slowest 5% are slower than this

### What does P99 mean?
- P99 latency = 99% of requests are faster than this value
- The slowest 1% are slower than this

## 2️⃣ Tracing (following a single request 🔍)
- Metrics say “something is slow
- Tracing says “THIS is where it’s slow”

### What is a Trace?
- A trace follows one request as it travels through multiple services.

### Example Trace Scenario
- User clicks “Place Order”
```
  User
 ↓ 50ms
API Gateway
 ↓ 80ms
Auth Service
 ↓ 900ms  ❌
Payment Service
 ↓ 120ms
Database
```
🧠 From this trace:
- Total latency = ~1.15s
- Payment Service is the bottleneck
- Now you know where to look

### How tracing + P99 work together
- P99 alert fires → “Some users are very slow”
- You open a trace from a slow request
- You see exact service & dependency causing delay

## 3️⃣ Error Rate (how often things fail ❌)
### What is error rate?
- Error rate = percentage of failed requests
```
Error Rate = (Failed Requests / Total Requests) × 100
```
### What counts as an error?
- Usually:
 - HTTP 5xx → server errors
 - Sometimes 4xx (depends on design)
 - Timeouts
 - Dependency failures

## 4️⃣ SLIs (Service Level Indicators)
- SLI = what you measure
- These are raw metrics.
### Common SLIs (Golden Signals ⭐)

| SLI Type     | Example               |
| ------------ | --------------------- |
| Latency      | P95 < 500 ms          |
| Availability | % successful requests |
| Error rate   | < 1%                  |
| Throughput   | Requests per second   |
| Saturation   | CPU / memory usage    |

```SLI: Percentage of requests completed under 500 ms```

## 5️⃣ SLOs (Service Level Objectives)
- SLO = the target you promise internally
- Built on top of SLIs.
```
99.9% of checkout requests
must complete under 500 ms
over a rolling 30 days
```
- 0.1% requests can be slow
- Error budget exists

## 6️⃣ SLAs (Service Level Agreements)
- SLA = legal / customer-facing promise
- If you break it → 💸 penalties


## How We get Metrics, Traces and Logs?

```
Application
 ├─ Metrics + Traces → OpenTelemetry SDK
 │                     ↓
 │              OTel Collector
 │                     ↓
 │                SignalFx
 │
 └─ Logs → Fluent Bit / Splunk Forwarder → Splunk
````

### 1️⃣ Deploy SignalFx OpenTelemetry Collector on Servers / VMs

### When you do this
- Legacy apps
- EC2 / VM workloads
- Non-Kubernetes services

### Step 1: Download the Collector

```
curl -LO https://github.com/signalfx/splunk-otel-collector/releases/latest/download/splunk-otel-collector_linux_amd64.deb
sudo dpkg -i splunk-otel-collector_linux_amd64.deb
```

### Step 2: Configure the Collector
### File: /etc/otel/collector/agent_config.yaml

```yaml
receivers:
  otlp:
    protocols:
      grpc:
      http:

processors:
  batch:
  resource:
    attributes:
      - key: service.environment
        value: prod
        action: upsert

exporters:
  signalfx:
    access_token: ${SPLUNK_ACCESS_TOKEN}
    realm: us0   # or eu0, etc.

service:
  pipelines:
    metrics:
      receivers: [otlp]
      processors: [resource, batch]
      exporters: [signalfx]

    traces:
      receivers: [otlp]
      processors: [resource, batch]
      exporters: [signalfx]
```

### Step 3: Start the service
```
sudo systemctl enable splunk-otel-collector
sudo systemctl start splunk-otel-collector
```
✅ Your VM can now receive metrics & traces.


### 2️⃣ Deploy SignalFx OpenTelemetry Collector on Kubernetes
- Best practice
 - DaemonSet → node-level metrics
 - Deployment → app-level telemetry aggregation

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: otel-collector
spec:
  selector:
    matchLabels:
      app: otel-collector
  template:
    metadata:
      labels:
        app: otel-collector
    spec:
      containers:
      - name: otel-collector
        image: quay.io/signalfx/splunk-otel-collector:latest
        env:
        - name: SPLUNK_ACCESS_TOKEN
          valueFrom:
            secretKeyRef:
              name: splunk-secret
              key: token
        - name: SPLUNK_REALM
          value: us0
```

### 3️⃣ How we instrument applications (THIS IS KEY)
- Metrics don’t magically appear.
- Instrumentation = code + runtime hooks

- Java application (most common)
 - Step 1: Add Java agent (no code change!)
```
java -javaagent:/otel/opentelemetry-javaagent.jar \
     -Dotel.service.name=payment-service \
     -Dotel.exporter.otlp.endpoint=http://otel-collector:4317 \
     -jar app.jar
```
### What you get automatically:
- HTTP latency
- DB calls
- Error counts
- Full traces

### What telemetry is generated

| Signal | Example                 |
| ------ | ----------------------- |
| Metric | request.duration        |
| Metric | request.count           |
| Trace  | user → API → DB         |
| Span   | payment-service latency |
