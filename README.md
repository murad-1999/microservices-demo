<!-- <p align="center">
<img src="/src/frontend/static/icons/Hipster_HeroLogoMaroon.svg" width="300" alt="Online Boutique" />
</p> -->
![Continuous Integration](https://github.com/GoogleCloudPlatform/microservices-demo/workflows/Continuous%20Integration%20-%20Main/Release/badge.svg)

**Online Boutique** is a cloud-first microservices demo application.  The application is a
web-based e-commerce app where users can browse items, add them to the cart, and purchase them.

Google uses this application to demonstrate how developers can modernize enterprise applications using Google Cloud products, including: [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine), [Cloud Service Mesh (CSM)](https://cloud.google.com/service-mesh), [gRPC](https://grpc.io/), [Cloud Operations](https://cloud.google.com/products/operations), [Spanner](https://cloud.google.com/spanner), [Memorystore](https://cloud.google.com/memorystore), [AlloyDB](https://cloud.google.com/alloydb), and [Gemini](https://ai.google.dev/). This application works on any Kubernetes cluster.

If you’re using this demo, please **★Star** this repository to show your interest!

**Note to Googlers:** Please fill out the form at [go/microservices-demo](http://go/microservices-demo).

## Architecture

**Online Boutique** is composed of 11 microservices written in different
languages that talk to each other over gRPC.

[![Architecture of
microservices](/docs/img/architecture-diagram.png)](/docs/img/architecture-diagram.png)


---

# 🛒 Microservices Observability Stack: Online Boutique

A full-stack observability implementation for the **Google Online Boutique** microservices architecture. This project demonstrates a production-grade **LGTM** (Loki, Grafana, Tempo/Jaeger, Mimir/Prometheus) monitoring pipeline, providing 360-degree visibility into a distributed system.

## 🏗️ Architecture & Tech Stack

* **Microservices:** 11-service polyglot application (Go, Python, Node.js, C#).
* **Metrics:** **Prometheus** & **cAdvisor** for host and container resource tracking.
* **Traces:** **OpenTelemetry (OTel) Collector** exporting distributed traces to **Jaeger**.
* **Logs:** **Grafana Loki** for high-cardinality log aggregation.
* **Visualization:** **Grafana** with correlated dashboards linking metrics to traces.

---

## 🛠️ Key Implementation Details

### 1. The "Fail-Fast" Configuration

During deployment, I implemented strict environment validation. For example, the `recommendationservice` was configured with mandatory downstream dependencies (`PRODUCT_CATALOG_SERVICE_ADDR`). This ensures services crash immediately with clear logs rather than entering a "zombie" state.

### 2. Containerized Observability

* **cAdvisor:** Deployed as a privileged container with read-only mounts to `/sys`, `/var/run`, and `/var/lib/docker` to provide granular per-container CPU/Memory metrics without host-level agents.
* **OTel Collector:** Configured as the central "telemetry gateway," receiving OTLP data on port `4317` and exposing Prometheus metrics on port `8889`.

### 3. Correlated Dashboards

I built dynamic Grafana dashboards using **Data Source Variables** (`${DS_JAEGER}`, `${DS_PROMETHEUS}`). This decouples the dashboard from specific UIDs, making the entire monitoring stack portable across environments.

---

## 🚀 Troubleshooting & Resilience

* **Network Recovery:** Handled Docker network "wipes" by recreating specialized bridges for the monitoring stack.
* **Metric Mapping:** Resolved naming mismatches between OTel semantic conventions and Prometheus (e.g., mapping `http.server.duration` to `http_server_duration_seconds_bucket`).
* **Trace Visibility:** Fixed `traceID: undefined` errors by reconfiguring panels from "TraceID" lookup mode to "Search" mode for initial discovery.

---

## 📊 How to Run

1. **Clone the Repo:**
```bash
git clone <your-repo-link>

```


2. **Start the Stack:**
```bash
docker compose up -d

```


3. **Access the Tools:**
* **Grafana:** `http://localhost:3000` (Default dashboards included)
* **Jaeger UI:** `http://localhost:16686`
* **Prometheus:** `http://localhost:9090`


