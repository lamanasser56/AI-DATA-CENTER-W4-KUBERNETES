# Week 4 — Kubernetes for AI Data Center Workloads

## Overview

Week 4 progressed from Kubernetes cluster inspection and Pod diagnostics to resilient Deployments, resource-aware scheduling, Helm packaging, horizontal autoscaling, and an authenticated, observable go-live service. The work combined CPU-based echo workloads for orchestration exercises with separate GPU scheduling checks and validation of the shared vLLM serving workload.

## Environment

The labs used a k3s/Kubernetes environment on `aidc-t09`. Personal exercises ran in the `lama` namespace, while shared serving and go-live work used the `team` namespace. GPU checks confirmed an NVIDIA RTX A6000 with one schedulable `nvidia.com/gpu` resource. The public endpoint `https://t09.aidc.nadir.sh` was validated during Day 5; no credential or Secret value is stored in this documentation.

## Week 4 Progression

### [Day 1 — Cluster Diagnostics and GPU Scheduling](W4D1/README.md)

Diagnosed image-pull, CPU scheduling, and container-exit failures; deployed and validated a standalone echo-serving Pod; and exercised Kubernetes GPU allocation and contention separately from the serving workload. The main takeaway was learning to use Pod state, Events, and resource accounting to distinguish configuration, runtime, and scheduling failures.

### [Day 2 — Self-Healing and Zero-Downtime Rollout](W4D2/README.md)

Replaced the standalone Pod with a two-replica Deployment behind a ClusterIP Service. Readiness and liveness probes, a controlled rolling-update strategy, and a pre-stop delay supported automatic Pod replacement and rollout tests with no failed probe requests in the recorded runs. The exercise demonstrated how controllers and health gates maintain availability during failure and change.

### [Day 3 — Resource Scheduling and GPU Contention](W4D3/README.md)

Added CPU and memory requests and limits, observed an oversized CPU request remain `Pending`, and confirmed single-GPU contention through `Insufficient nvidia.com/gpu`. CPU noisy-neighbour measurements and inspection of the shared GPU-backed vLLM engine reinforced that scheduler requests, runtime limits, and accelerator availability address different aspects of workload control.

### [Day 4 — Helm and Horizontal Pod Autoscaling](W4D4/README.md)

Packaged the echo-serving workload as a Helm chart and configured an HPA for 1–3 replicas with a 50% CPU target. Generated CPU load produced scale-out from 1 to 3 replicas, followed by scale-down to 1 after load stopped. Measurements from the shared GPU-bound vLLM engine showed that CPU utilization alone is not a reliable scaling signal for that workload; queue depth was proposed as a candidate signal, not established as a measured production threshold.

### [Day 5 — Go-Live, Authentication, and Metrics](W4D5/README.md)

Protected the `/v1` API with bearer-key authentication, exposed `team-serving` through NodePort 30800, and validated the public health, model-list, and chat-completion paths. Prometheus scraped `team-serving:8000/metrics`, its API was used to inspect known metric names, and generated requests were visible in the recorded time series. The completed integration note documented the consumer handoff without storing the key, and the supplied verifier ended with **`GREEN CHECK: PASS`**.

## Key Skills Developed

* Diagnosing Pods through status, termination details, scheduler Events, and resource availability.
* Building resilient Deployments with Services, probes, rolling updates, graceful shutdown behavior, and controller-driven recovery.
* Applying CPU, memory, and GPU requests and limits, then interpreting scheduling and contention outcomes.
* Packaging configurable workloads with Helm and validating CPU-based HPA scale-out and scale-down.
* Securing model APIs with bearer authentication while keeping credentials outside repository artifacts.
* Exposing services safely, validating external API behavior, and using Prometheus to query and observe application metrics.

## Repository Structure

```text
W4D1/  Cluster diagnostics, standalone serving, and GPU scheduling
W4D2/  Self-healing Deployment and zero-downtime rollout
W4D3/  Resource controls, contention, and shared GPU engine validation
W4D4/  Helm packaging, HPA behavior, and GPU scaling-signal analysis
W4D5/  Authenticated go-live service, Prometheus, and integration verification
```

Each directory contains its detailed README and the relevant configuration, validation, and execution evidence retained for that day's lab.

## Final Week 4 Outcome

The week established a practical progression from **cluster diagnostics → resilient workloads → resource control → packaging and autoscaling → an authenticated, observable go-live service**.
