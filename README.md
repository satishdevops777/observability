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
###🔹Application Metrics (Golden Signals ⭐)
- Latency (P95 / P99)
- Traffic (RPS)
- Errors (4xx / 5xx)
- Saturation

### How data gets into SignalFx
- SignalFx Smart Agent / OpenTelemetry
- K8s metrics-server
- Cloud integrations (AWS, Azure, GCP)
- App instrumentation (Java, Node, Python, etc.)

Example Alert in SignalFx 🚨
 
👉 This tells you something is wrong
But not yet why.
2️⃣ Logging & Deep Debugging with Splunk
What goes into Splunk
•	Application logs
•	Kubernetes pod logs
•	Node/system logs
•	API gateway logs
•	Error & exception traces


Example Logs in Splunk 🔍

 
Now you know:
•	Which service
•	Which dependency
•	Exact failure reason

3️⃣ How Monitoring + Observability Work Together

🚨 Incident Flow (Real World)
Step 1: Alert fires in SignalFx
High latency in payment-service

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

### ✅ Together
- = True Observability


